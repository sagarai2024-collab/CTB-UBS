# CTB Work Item Checklist

**Project:** CTB (Swiss Bank Project)
**Last Updated:** 2026-04-25

---

## Summary

| Category | Items | Total |
|---|---|---|
| Access & Setup | #1, #8 | 2 |
| Architecture | #2 | 1 |
| Build & Configuration | #3, #4 | 2 |
| Authentication & Authorization | #5, #6 | 2 |
| Pipeline | #7, #9, #10, #11 | 4 |
| Deployment | #12, #13, #14 | 3 |
| Integration | #15, #16 | 2 |
| **Total** | | **16** |

---

## Detailed Work Items

### Access & Setup

| # | Work Item | Status | Owner | Notes |
|---|---|---|---|---|
| 1 | CTB repository access | ⬜ Pending | | Request access to CTB GitLab/Azure DevOps repository |
| 8 | Dev server access | ⬜ Pending | | Provision developer server credentials and VPN/jump-host access |

---

### Architecture

| # | Work Item | Status | Owner | Notes |
|---|---|---|---|---|
| 2 | DD2 — Three-tier conversion | ⬜ Pending | | Convert existing application to three-tier architecture (Presentation / Business Logic / Data) per DD2 design document |

---

### Build & Configuration

| # | Work Item | Status | Owner | Notes |
|---|---|---|---|---|
| 3 | Locally build application and package configuration | ⬜ Pending | | Validate local build succeeds; confirm NuGet/Maven package config aligns with Nexus feed |
| 4 | Agnes roles profile creation | ⬜ Pending | | Create required Agnes identity profiles and assign roles for service accounts |

---

### Authentication & Authorization

| # | Work Item | Status | Owner | Notes |
|---|---|---|---|---|
| 5 | Authentication and authorization code changes for Windows application | ⬜ Pending | | Update Windows app auth layer to meet UBS security standards (token handling, session management) |
| 6 | AGS / GSR request (CTB) | ⬜ Pending | | Submit and get approval for AGS access group and GSR service request for CTB environment |

---

### Pipeline

| # | Work Item | Status | Owner | Notes |
|---|---|---|---|---|
| 7 | Creating pipeline | ⬜ Pending | | Define initial CI/CD pipeline YAML in Azure Pipelines / GitLab CI |
| 9 | Pipeline setup — Application + Puppet for server config | ⬜ Pending | | Integrate Puppet manifests for server configuration alongside application pipeline stages |
| 10 | Build in pipeline, publish to Nexus | ⬜ Pending | | Configure pipeline to compile, package, and publish artifacts to Nexus repository |
| 11 | DNS and SSL certification for API | ⬜ Pending | | Register DNS entries and obtain/configure SSL certificates for API endpoints |

---

### Deployment

| # | Work Item | Status | Owner | Notes |
|---|---|---|---|---|
| 12 | Dev1 server connect — Artifact registration, Agent user installation | ⬜ Pending | | Connect to Dev1 server; register build agent user; install pipeline agent |
| 13 | Deploy in Dev1 | ⬜ Pending | | Execute first deployment of application to Dev1 environment |
| 14 | Automatically showing Nexus repository | ⬜ Pending | | Verify that Dev1 environment auto-resolves and pulls artifacts from Nexus on deployment |

---

### Integration

| # | Work Item | Status | Owner | Notes |
|---|---|---|---|---|
| 15 | ADT config | ⬜ Pending | | Configure ADT (Application Deployment Tool) settings for the migrated application |
| 16 | MERS implementation | ⬜ Pending | | Implement MERS (Monitoring / Event / Reporting System) integration for the application |

---

## Status Key

| Symbol | Meaning |
|---|---|
| ⬜ Pending | Not yet started |
| 🔄 In Progress | Work underway |
| ✅ Done | Completed and verified |
| ❌ Blocked | Blocked — awaiting dependency or approval |

---

## How to Update This Checklist

Replace the status symbol in the **Status** column:

```
⬜ Pending     →  not started
🔄 In Progress →  currently active
✅ Done        →  completed
❌ Blocked     →  blocked
```
