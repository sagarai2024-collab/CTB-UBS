# 📜 Changelog

All notable changes to this project will be documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and [Semantic Versioning](https://semver.org/).

---

## [Unreleased]

### Changed — 2026-07-04 (checklist expanded 16 → 19 items)
- **`docs/CTB-Checklist.md`** restructured to **19 work items** (was 16):
  - **New #10** — Puppet module content rules: do not upload services, `.cs` files, or `.sln` paths into the Puppet module (server config only; binaries come from Nexus).
  - **New #14** — Manual deploy step: application files + Puppet module deployed manually on Dev1 before pipeline automation.
  - **New #19** — TE1 server access for deployment.
  - **#6** now covers **ACG / GSR request + IIS application pool permission** (was "AGS / GSR").
  - **#7** now requires Nexus **snapshot and release uploads both passing**.
  - **#12** (DNS/SSL) now requires the certificate **fingerprint (thumbprint)** for binding registration.
  - **#17** (ADT config) scoped explicitly to the **Windows application**.
  - **#18** (MERS) expanded into 4 workstreams: OpenTelemetry via **BBS** request (MERS EM1 → Grafana logs + IIS/CPU/disk metrics), Grafana alerting via **Big Panda** request, **CID-5** label validation, and MERS auth **token creation on the app team side**.
  - Items renumbered: old #10→#11, #11→#12, #12→#13, #13→#15, #14→#16, #15→#17, #16→#18.
- **`README.md`** — progress counters, phase table, prerequisites, environments (added **TE1**), and monitoring stack (OpenTelemetry / Grafana / Big Panda) updated to match.
- **`docs/Architecture.md`** / **`docs/Pipeline-Guide.md`** — item cross-references updated to new numbering; Puppet module content rule and SSL fingerprint requirement documented.

### Changed
- **`pom.xml`** clarified for **.NET Windows application** packaging — Maven used only as the upload mechanism to Nexus, not for compilation. Artifact: `CTB_integracao_dotnet_app` (packaging=zip).
- **`settings.xml`** updated with comments explaining its role in a .NET project (authenticates `mvn deploy:deploy-file` to push the .NET ZIP to Nexus). NuGet restore is handled separately via `nuget.config`.
- **`.gitlab-ci.yml`** build stages now use `dotnet` (validate/build/test) instead of `mvn`. Maven is invoked only at the `nexusupload` stage. Build flow: `dotnet restore → build → publish → zip (.NET output + MSI + config) → mvn deploy:deploy-file`.
- **`.gitlab-ci.yml`** mirrors enterprise structure:
  - `include:` shared CI templates for id_tokens.
  - Container image: Maven 3.6.3.
  - Stages: `validate → build → test → Zippingpackage → nexusupload → notify`.
  - `.fetch-eva-secrets` + `.get-deploy-eva-credentials` hidden jobs (Vault auth via OIDC).
  - `Zippingpackage` zips application files (`bin/`, `config/`, `scripts/` — not puppet manifests).
  - `Nexus_Snapshot_Upload` / `Nexus_Release_Upload` jobs use `mvn deploy:deploy-file`.
- All UBS-internal URLs replaced with `<PLACEHOLDER>` markers — to be substituted only inside enterprise environment.

### Added
- Initial repository scaffolding for CTB (Swiss bank project) server migration.
- `README.md` with project overview, architecture diagram, tech stack, and quick links.
- `azure-pipelines.yml` — Azure DevOps multi-stage pipeline (validate → build → test → publish → deploy → notify).
- `.gitlab-ci.yml` — GitLab CI mirror of the same pipeline stages.
- `docs/Architecture.md` — DD2 three-tier architecture, auth flow, deployment topology with mermaid diagrams.
- `docs/Pipeline-Guide.md` — pipeline operations, secrets, branching strategy, troubleshooting.
- `docs/CTB-Checklist.md` — 16 CTB work items with phased progress dashboard, gantt roadmap, and acceptance criteria.
- `.gitignore` — .NET, IDE, secrets, and pipeline cache rules.

---

## [0.1.0] — 2026-04-25

### Added
- Project initialised by Sagarika (Technical Lead).
- Documented all 16 CTB work items across 4 delivery phases.
