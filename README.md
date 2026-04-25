# CTB UBS

**Project:** CTB (Swiss Bank Project)
**Platform:** Azure DevOps / GitLab
**Type:** Server Migration — Cut To Build (CTB)

---

## Overview

This repository contains all artefacts, configuration, and documentation for the CTB (Swiss bank project) server migration. The scope covers a full Cut-To-Build (CTB) lifecycle — from initial environment access and three-tier architecture conversion, through CI/CD pipeline setup, Nexus artifact publishing, DNS/SSL provisioning, and deployment to the Dev1 server.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Application | Windows Application (.NET) |
| Architecture | Three-tier (DD2 conversion) |
| CI/CD | Azure Pipelines / GitLab CI |
| Artifact Registry | Nexus Repository |
| Server Configuration | Puppet |
| Identity / Auth | Agnes, AGS, GSR |
| Integration | MERS, ADT |
| DNS / SSL | Enterprise certificate authority |

---

## Repository Structure

```
CTB-UBS/
├── README.md                  ← This file
└── docs/
    └── CTB-Checklist.md       ← Full CTB work item tracker (16 items)
```

---

## Prerequisites

Before starting work, ensure the following access and tools are in place:

- CTB repository access granted (item #1 in checklist)
- Dev server access provisioned (item #8)
- Agnes roles and profile created (item #4)
- AGS / GSR request approved (item #6)
- Local build environment configured (item #3)

---

## Quick Links

| Document | Description |
|---|---|
| [CTB Checklist](docs/CTB-Checklist.md) | All 16 CTB work items with status and notes |

---

## Key Milestones

1. **Onboarding** — Repo access, Dev server access, Agnes profile setup
2. **Architecture** — DD2 three-tier conversion complete
3. **Auth** — Windows application auth/authz changes deployed
4. **Pipeline** — CI/CD pipeline live, building and publishing to Nexus
5. **DNS/SSL** — API endpoints secured with valid certificates
6. **Deployment** — Application deployed to Dev1 and verified
7. **Integration** — ADT config and MERS implementation complete

---

## Contact

| Role | Name |
|---|---|
| Technical Lead | Sagarika |
