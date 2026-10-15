# 🌿 NovaCare Biotech — Git Branch Governance & ALM Lifecycle
## Enterprise Fabric Deployment Pipeline & Branch Protection Policy

---

## 1. Branch Topology & Environment Alignment
The Git repository is structured to map 1:1 with our 3-tier isolated Microsoft Fabric workspaces:

| Branch Name | Fabric Workspace | Target Environment | Deployment Mechanism | Access Level |
| :--- | :--- | :--- | :--- | :--- |
| **`main`** | **`NC-Biotech-PROD`** | Production | Fabric Deployment Pipeline | Strictly Protected (PR Only, 2 Approvers) |
| **`test`** | **`NC-Biotech-TEST`** | User Acceptance Testing (UAT) | Automated Pipeline / PR | Protected (PR Only, 1 Approver) |
| **`dev`** | **`NC-Biotech-DEV`** | Development | Direct Git Sync from Fabric | Working Branch for Developers |
| **`feature/*`** | Local / Sandbox | Feature Development | Git checkout | Unrestricted developer branches |

---

## 2. Branch Protection Rules

### 2.1 Rules for `main` (Production)
* **Require a Pull Request before merging**: Enabled.
* **Require approvals**: Minimum 2 approvals (Lead Architect + Business Owner).
* **Dismiss stale pull request approvals when new commits are pushed**: Enabled.
* **Require status checks to pass before merging**:
  * CI Pipeline: Tabular Editor 2 Best Practice Analyzer (BPA) clean check.
  * Schema Validation Check: No breaking column renames/drops in Gold layer.
  * Data Pipeline validation test suite pass.
* **Require signed commits**: Enabled.
* **Require linear history / Squash merge**: Enabled.
* **Do not allow bypassing the above settings**: Enabled (Enforce for Administrators).

### 2.2 Rules for `test` (Staging / UAT)
* **Require a Pull Request before merging**: Enabled.
* **Require approvals**: Minimum 1 approval (Peer Developer).
* **Require status checks to pass**: Automated integration tests.

---

## 3. Fabric Workspace Git Integration Workflow
1. Developers work in feature branches or sync directly to **`dev`** in `NC-Biotech-DEV`.
2. To promote to UAT, open a Pull Request: `dev` $\rightarrow$ `test`.
3. Merging to `test` triggers the Fabric Deployment Pipeline to sync `NC-Biotech-TEST`.
4. After business validation, open a Pull Request: `test` $\rightarrow$ `main`.
5. Merging to `main` deploys production artifacts to `NC-Biotech-PROD`.
