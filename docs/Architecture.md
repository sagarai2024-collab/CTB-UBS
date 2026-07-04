# 🏛️ Architecture — CTB UBS

**Document version:** 1.0
**Last updated:** 2026-04-25
**Owner:** Sagarika

---

## 1. Overview

The CTB application is being migrated from a legacy 2-tier monolith into a hardened **three-tier (DD2)** architecture. The redesign separates concerns cleanly across **Presentation**, **Business Logic**, and **Data** layers, with auth centralised through the bank's enterprise identity stack (Agnes / AGS / GSR).

---

## 2. Three-Tier (DD2) Design

```mermaid
flowchart TB
    subgraph CL["👥 Client Layer"]
        WIN[Windows Desktop App<br/>.NET 8]
    end

    subgraph T1["🌐 Tier 1 — Presentation"]
        direction TB
        LB[Load Balancer<br/>F5 BIG-IP]
        API[API Gateway<br/>HTTPS / SSL]
        LB --> API
    end

    subgraph T2["⚙️ Tier 2 — Business Logic"]
        direction TB
        BLS[Business Services]
        AUTHSVC[Auth Service<br/>Agnes / AGS]
        VAL[Validation Engine]
        BLS --> AUTHSVC
        BLS --> VAL
    end

    subgraph T3["💾 Tier 3 — Data"]
        direction TB
        DB[(Primary DB<br/>SQL Server)]
        REPLICA[(Read Replica)]
        CACHE[(Redis Cache)]
        DB -.replication.-> REPLICA
    end

    WIN -->|HTTPS 443| LB
    API -->|gRPC| BLS
    BLS -->|TDS| DB
    BLS -->|TDS| REPLICA
    BLS -->|RESP| CACHE

    style CL fill:#e3f2fd,stroke:#1976d2
    style T1 fill:#fff3e0,stroke:#f57c00
    style T2 fill:#f3e5f5,stroke:#7b1fa2
    style T3 fill:#e8f5e9,stroke:#388e3c
```

### Tier Responsibilities

| Tier | Responsibility | Tech | Hosted On |
|---|---|---|---|
| **Client** | UI, local validation | .NET 8 WinForms / WPF | End-user workstation |
| **Presentation** | API gateway, SSL termination, rate limiting | REST + gRPC | Dev1 web servers |
| **Business Logic** | Domain logic, auth orchestration, validation | .NET services | Dev1 app servers |
| **Data** | Persistence, caching, replication | SQL Server + Redis | Dev1 DB servers |

---

## 3. Authentication & Authorization Flow

```mermaid
sequenceDiagram
    autonumber
    actor U as 👤 User
    participant W as Windows App
    participant API as API Gateway
    participant A as Agnes
    participant AGS as AGS
    participant GSR as GSR
    participant BL as Business Service

    U->>W: Launch app
    W->>A: Request token (kerberos)
    A-->>W: Agnes token
    W->>API: HTTPS + Agnes token
    API->>AGS: Validate token + scope
    AGS-->>API: ✅ valid (with role)
    API->>GSR: Lookup service-role mapping
    GSR-->>API: Authorized actions
    API->>BL: Forward request + claims
    BL-->>API: Response
    API-->>W: HTTPS response
    W-->>U: Render result
```

### Auth Components

| Component | Role |
|---|---|
| **Agnes** | Identity provider — issues authentication tokens |
| **AGS** | Access Group Service — validates tokens, maps to access groups |
| **GSR** | Group Service Registry — maps groups to allowed service operations |

---

## 4. Deployment Topology — Dev1

```mermaid
flowchart LR
    subgraph DEV1["🟡 Dev1 Environment — dev1.ctb.internal"]
        direction TB

        subgraph WEB["Web Tier"]
            W1[web01]
            W2[web02]
        end

        subgraph APP["App Tier"]
            A1[app01]
            A2[app02]
        end

        subgraph DATA["Data Tier"]
            D1[(db01<br/>primary)]
            D2[(db02<br/>replica)]
            R1[(redis01)]
        end

        WEB --> APP
        APP --> DATA
    end

    NEXUS[📦 Nexus]
    PUPPET[⚙️ Puppet]
    ADT[📊 ADT]
    MERS[📡 MERS]

    NEXUS -.artifacts.-> WEB
    NEXUS -.artifacts.-> APP
    PUPPET -.config.-> WEB
    PUPPET -.config.-> APP
    PUPPET -.config.-> DATA
    ADT -.tracks.-> DEV1
    DEV1 -.metrics.-> MERS
```

---

## 5. Data Flow — Typical Transaction

```mermaid
flowchart LR
    A[Client request] --> B{SSL valid?}
    B -->|no| X1[❌ 403]
    B -->|yes| C{Auth token<br/>valid?}
    C -->|no| X2[❌ 401]
    C -->|yes| D{Role authorized<br/>via GSR?}
    D -->|no| X3[❌ 403]
    D -->|yes| E[Business Logic]
    E --> F{Cache hit?}
    F -->|yes| G[Return cached]
    F -->|no| H[Query DB]
    H --> I[Update cache]
    I --> G
    G --> J[Send to MERS]
    J --> K[✅ Response to client]
```

---

## 6. Network & Security Boundaries

| Boundary | Control | Item Ref |
|---|---|---|
| Client ↔ Presentation | TLS 1.2+, enterprise SSL cert (fingerprint registered) | #12 |
| Presentation ↔ Business | mTLS, service mesh | #12 |
| Business ↔ Data | Encrypted DB connection (TDS-over-TLS) | — |
| All → MERS | Outbound HTTPS, app-team token | #18 |
| Pipeline → Nexus | Token auth, RBAC | #11 |
| Pipeline → Dev1 | Puppet agent, signed manifests | #9, #13 |

---

## 7. Configuration Management — Puppet

Puppet manifests live in a sibling repo (`ctb-ubs-puppet`) and are applied automatically by the pipeline (item #9).

> ⚠️ **Module content rule (item #10):** the Puppet module carries **server configuration only**. Do not upload application services, `.cs` source files, or other `.sln` solution paths into the module — application binaries are always pulled from Nexus.


```puppet
# Example excerpt — node 'app01.dev1.ctb.internal'
class { 'ctb_ubs::app':
  version       => $facts['ctb_app_version'],
  nexus_url     => 'https://nexus.internal/repository/ctb-ubs-releases/',
  agnes_realm   => 'CTB.INTERNAL',
  ssl_cert_path => '/etc/ssl/ctb/api.crt',
  mers_endpoint => 'https://mers.internal/events',
}
```

---

## 8. Future State (Post-Migration)

- Move from Dev1 → SIT → UAT → PROD using the same pipeline
- Add active-active replication across data centres
- Enable canary deployments via ADT
- Integrate full distributed tracing into MERS

---

## 📚 Related Documents

- [README](../README.md)
- [CTB Checklist](./CTB-Checklist.md)
- [Pipeline Guide](./Pipeline-Guide.md)
