# 📜 Changelog

All notable changes to this project will be documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and [Semantic Versioning](https://semver.org/).

---

## [Unreleased]

### Added
- `pom.xml` — Maven build with Spring Boot, JUnit 5, Mockito, JaCoCo, SonarQube, Nexus distribution.
- `settings.xml` — Maven settings template with Nexus mirror + servers + Dev1 profile.
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
