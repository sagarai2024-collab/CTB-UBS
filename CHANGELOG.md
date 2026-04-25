# 📜 Changelog

All notable changes to this project will be documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and [Semantic Versioning](https://semver.org/).

---

## [Unreleased]

### Changed
- **`pom.xml`** rewritten for **application** artifact (`api_CTB_integracao_application`, packaging=zip), distribution to release/snapshot Nexus repos.
- **`settings.xml`** rewritten with `mirrors` + `servers` matching enterprise SSO upload pattern. Password injected from Vault at runtime.
- **`.gitlab-ci.yml`** rewritten to mirror enterprise structure:
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
