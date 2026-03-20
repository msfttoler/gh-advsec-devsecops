# 3-Day Light Proof of Concept Delivery Guide

> **Format:** 60% hands-on labs / 40% contextual presentation + discussion  
> **Duration:** 3 days × 8 hours (09:00–17:00) = 24 hours total  
> **Audience:** Security engineers, senior developers, DevOps leads, platform team members  
> **Delivery Model:** Instructor-led with each participant working on their own forked repository  
> **Objective:** Validate GHAS + GHCP + MDC capabilities through hands-on labs, remediation exercises, and agent-assisted security workflows. Produce a documented findings summary and initial remediation results.

---

## How This Builds on the 1-Day

The 1-Day delivery is **entirely contained within Day 1** of this 3-Day PoC. Participants who attended the 1-Day have already completed all of Day 1. Days 2 and 3 add:

| Capability | 1-Day (Day 1) | Added in Day 2 | Added in Day 3 |
|-----------|---------------|-----------------|-----------------|
| Secret Scanning | Full lab | — | — |
| CodeQL | Full lab | — | — |
| Dependency Review | Full lab | — | — |
| SBOM / Scorecard | Demo + quick try | Deeper analysis | — |
| Container Scanning | Full lab | — | — |
| IaC Scanning | tfsec findings review only | KICS + MSDO comparison, Terraform remediation | — |
| Kubernetes | Comparison demo | Kubesec hands-on remediation | — |
| Copilot Autofix | 1-2 fixes | — | Security Campaigns demo |
| Agents (Security, Reviewer, IaC) | Full labs | — | — |
| Agents (Pipeline, Supply Chain) | Full labs | — | — |
| Agent (Security Plan Creator) | Not covered | — | Full lab |
| Agent Orchestration (CI/CD) | Not covered | — | Full lab |
| MDC | Facilitator demo only | Full hands-on connection + dashboard | CSPM + runtime |
| DAST | Not covered | Deploy app + ZAP scan | Log forging attack demo |
| Pipeline Hardening | Not covered | Full remediation lab | — |
| IaC Remediation | Not covered | Full remediation lab | — |
| Code Remediation | 1-2 fixes via Autofix | — | Full remediation sprint (all 7+ vulns) |
| Compliance Mapping | Not covered | — | Guided exercise |
| Findings Report | Verbal recap | — | Written PoC readout document |

---

## Pre-Engagement Requirements

Everything from the 1-Day prerequisite list, plus:

### Additional Participant Requirements

| # | Prerequisite | Purpose |
|---|-------------|---------|
| 1 | Azure subscription with Defender for Cloud accessible (Contributor + Security Admin role) | MDC hands-on labs on Day 3 |
| 2 | `terraform` CLI installed locally (≥ 1.0) | Local IaC scanning and remediation on Day 2 |
| 3 | Familiarity with at least one: Terraform, Bicep, or ARM templates | IaC remediation labs |

### Additional Facilitator Requirements

| # | Prerequisite | Purpose |
|---|-------------|---------|
| 1 | Azure subscription with webapp01 deployed and accessible via HTTPS | DAST lab target on Day 2 |
| 2 | MDC connector configured and synced (24+ hours prior) | Day 3 MDC hands-on |
| 3 | Defender CSPM + Defender for App Service enabled | Day 3 CSPM lab |
| 4 | Branch `demo/pre-remediated` with all fixes applied | Day 3 before/after comparison |

---

## Day 1 — Detection: Full GHAS Capability Sweep + Agents

This is the complete 1-Day delivery. Refer to `planning/1-day-demo-delivery.md` for the full minute-by-minute timeline. Summary:

| Session | Time | Duration | Focus | Labs |
|---------|------|----------|-------|------|
| 1 | 09:00–09:20 | 20 min | Environment verification + repo tour | F-01 (subset) |
| 2 | 09:20–09:55 | 35 min | Secret Scanning & Push Protection | G-01 |
| 3 | 09:55–10:45 | 50 min | Code Scanning with CodeQL | G-02 |
| — | 10:45–11:00 | 15 min | Break | — |
| 4 | 11:00–11:35 | 35 min | Supply Chain Security | G-03, G-04, G-05 |
| 5 | 11:35–12:15 | 40 min | Container & IaC Security | G-06, I-01, I-04 |
| — | 12:15–13:15 | 60 min | Lunch | — |
| 6 | 13:15–14:00 | 45 min | Copilot Autofix & Remediation | G-07, R-01 (subset) |
| 7 | 14:00–14:45 | 45 min | Security, Reviewer & IaC Agents | A-01, A-02, A-03 |
| — | 14:45–15:00 | 15 min | Break | — |
| 8 | 15:00–15:40 | 40 min | Pipeline & Supply Chain Agents + MDC | A-04, A-05, M-01, M-02 |
| 9 | 15:40–16:40 | 60 min | End-to-End Flow (capstone) | Composite |
| 10 | 16:40–17:00 | 20 min | Day 1 wrap-up & Day 2 preview | — |

**Day 1 Modification from 1-Day standalone:** Session 10 wrap-up shifts from "next steps / pitch PoC" to "preview Day 2 topics: deep remediation, DAST, pipeline hardening."

**End of Day 1 State:** Participants have discovered findings across all scan categories (SAST, SCA, secrets, container, IaC) and used all 5 operational agents. No remediation beyond 1-2 Autofix demos.

---

## Day 2 — Hardening: IaC Remediation, Pipeline Security, DAST & Deployment

### Day 2 Theme

Day 1 was about **finding** vulnerabilities. Day 2 is about **fixing infrastructure and pipelines** — the deployment foundation must be secure before we fix application code on Day 3.

### Day 2 Detailed Timeline

#### 09:00–09:30 — Day 1 Recap & Findings Inventory (30 min)

| Time | Activity | Detail |
|------|----------|--------|
| 09:00–09:10 | **Findings review** | Pull up the Security tab on the facilitator's fork. Count findings by category: CodeQL (X), Secret scanning (Y), Dependabot (Z), tfsec (W). Highlight: "Yesterday we found all of these in 6 hours. Today we fix the foundation." |
| 09:10–09:20 | **Day 2 objectives** | Explain the remediation order: Infrastructure first (Terraform, pipelines) → Deploy the app → Test it dynamically (DAST). This mirrors real-world remediation priority: fix the platform before fixing the app. |
| 09:20–09:30 | **Context: IaC Security standards** | Brief verbal context (no slides): CIS Azure Benchmark v2.0 structure, how tfsec/KICS findings map to CIS controls, what "pass" looks like. Show the tfsec finding list from Day 1 — explain we'll fix the top 10 today. |

---

#### 09:30–10:45 — Terraform Remediation Lab (75 min)

**Lab Reference:** R-02, I-01 (deep), I-02

**Objective:** Remediate the top 10 Terraform misconfigurations, re-scan to verify fixes, and compare tfsec vs KICS coverage.

| Time | Activity | Detail |
|------|----------|--------|
| 09:30–09:45 | **Demo: tfsec findings deep dive** | Open the tfsec workflow results. Walk through the top 10 findings in order of severity. For each, show: tfsec rule ID, CIS benchmark control, severity, the Terraform file + line, and what the fix looks like. Create a prioritized list on screen. |
| 09:45–10:15 | **Hands-on: Fix 5 Terraform misconfigurations** | Participants work through fixes on their fork. Suggested fix order: |

**Fix 1 — `sql.tf`: Remove hardcoded password**
```hcl
# BEFORE (vulnerable)
administrator_login_password = "Aa12345678"

# AFTER (remediated)
administrator_login_password = var.sql_admin_password  # Passed via variable, stored in Key Vault
```

**Fix 2 — `networking.tf`: Restrict SSH/RDP access**
```hcl
# BEFORE (vulnerable)
source_address_prefix = "*"

# AFTER (remediated)
source_address_prefix = var.admin_ip_range  # e.g., "10.0.0.0/24" or specific IP
```

**Fix 3 — `storage.tf`: Enable disk encryption**
```hcl
# BEFORE (vulnerable)
encryption_settings { enabled = false }

# AFTER (remediated)
encryption_settings { enabled = true }
```

**Fix 4 — `aks.tf`: Enable RBAC + disable dashboard**
```hcl
# BEFORE (vulnerable)
role_based_access_control { enabled = false }
kube_dashboard { enabled = true }

# AFTER (remediated)
role_based_access_control { enabled = true }
kube_dashboard { enabled = false }
```

**Fix 5 — `security_center.tf`: Upgrade tier + enable alerts**
```hcl
# BEFORE (vulnerable)
tier = "Free"
alert_notifications = false

# AFTER (remediated)
tier = "Standard"
alert_notifications = true
alerts_to_admins    = true
```

| Time | Activity | Detail |
|------|----------|--------|
| 10:15–10:25 | **Hands-on: Re-scan with tfsec** | Commit fixes. Push. Trigger the tfsec workflow. While waiting, run tfsec locally if installed: `tfsec terraform/azure/`. Compare before/after finding counts. |
| 10:25–10:45 | **Demo + Try: KICS comparison (I-02)** | Trigger the KICS workflow against the same Terraform directory. Compare: which findings does KICS report that tfsec doesn't? Which has better CIS mapping? Discuss tool complementarity — using multiple scanners catches more issues. Review the KICS JSON output format vs tfsec SARIF. |

**Success Criteria:**
- [ ] 5 Terraform misconfigurations remediated and committed
- [ ] tfsec re-scan shows reduced finding count
- [ ] Can articulate the difference between tfsec and KICS coverage

---

#### 10:45–11:00 — Break (15 min)

---

#### 11:00–11:45 — Kubernetes Remediation + MSDO (45 min)

**Lab References:** I-04 (deep), I-03

| Time | Activity | Detail |
|------|----------|--------|
| 11:00–11:20 | **Hands-on: Fix the insecure Kubernetes manifest (I-04)** | Open `manifests/critical-double.yaml`. Remediate to match the security level of `score-5-pod-serviceaccount.yaml`. Required changes: set `privileged: false`, set `allowPrivilegeEscalation: false`, add `runAsNonRoot: true`, add `readOnlyRootFilesystem: true`, add `automountServiceAccountToken: false`, add resource limits. Commit changes. Trigger Kubesec workflow. Verify the score improves from ~0 to ≥5. |
| 11:20–11:45 | **Demo + Try: MSDO Multi-Tool Scan (I-03)** | Trigger the `MSDO-Microsoft-Security-DevOps` workflow. This runs: Bandit (Python), Checkov (IaC), TemplateAnalyzer (ARM/Bicep), Terrascan (Terraform), Trivy (containers). Review the consolidated SARIF output. Show how MSDO aggregates findings from 5 different tools into a single Security tab view. Discuss: MSDO as the "umbrella scanner" vs running individual tools. |

**Success Criteria:**
- [ ] `critical-double.yaml` remediated with Kubesec score ≥ 5
- [ ] MSDO workflow results reviewed with findings from multiple tools

---

#### 11:45–12:15 — Pipeline Hardening Lab (30 min)

**Lab Reference:** R-03

**Objective:** Apply pipeline security agent recommendations to harden 2-3 GitHub Actions workflows.

| Time | Activity | Detail |
|------|----------|--------|
| 11:45–11:55 | **Review Pipeline Agent findings from Day 1** | Open the pipeline agent report from Day 1 (A-04). Identify the top 3 issues: (1) actions referenced by tag instead of SHA, (2) overly broad permissions, (3) potential script injection points. |
| 11:55–12:15 | **Hands-on: Harden 2 workflows** | **Workflow 1 — Pin actions to SHA:** Pick a workflow (e.g., `CIS-Trivy-AquaSecurity.yml`). Replace `aquasecurity/trivy-action@0.32.0` with the full SHA pin. Use `gh api` or GitHub UI to find the commit SHA for the tag. **Workflow 2 — Scope permissions:** Pick a workflow with broad permissions. Apply principle of least privilege: replace `contents: write` with `contents: read` where writes aren't needed, remove `id-token: write` if OIDC isn't used. Add `permissions` block at job level if only workflow level exists. Commit changes. Verify workflows still pass. |

**Success Criteria:**
- [ ] At least 2 actions pinned to SHA
- [ ] At least 1 workflow's permissions scoped down
- [ ] Workflows still pass after hardening

---

#### 12:15–13:15 — Lunch (60 min)

---

#### 13:15–14:15 — Deploy Application & DAST (60 min)

**Lab References:** D-01, D-02

**Objective:** Deploy the vulnerable `webapp01` application to Azure and run a dynamic security scan against the running application.

| Time | Activity | Detail |
|------|----------|--------|
| 13:15–13:40 | **Hands-on: Deploy webapp01 to Azure (D-01)** | Option A (Bicep blueprint): Deploy using `blueprints/gh-aspnet-webapp/`. Commands: `az deployment sub create --location eastus --template-file blueprints/gh-aspnet-webapp/main.bicep --parameters blueprints/gh-aspnet-webapp/main.parameters.json`. Wait for ACR + App Service + Managed Identity creation. Build and push Docker image to ACR. Deploy to App Service. Verify app is accessible via HTTPS. Option B (Facilitator pre-deployed): If time is tight or Azure provisioning is slow, use the facilitator's pre-deployed instance. Participants observe the deployment steps on screen. |
| 13:40–13:55 | **Demo: Exercise vulnerable endpoints** | Navigate to the deployed app. Visit the DevSecOps page. Demonstrate the log forging vulnerability via URL parameter: `?user=anonymous%0d%0a[CRITICAL]%20Admin%20access%20granted`. Show how the injected text appears in application logs. Visit the index page with the drive parameter to demonstrate the input validation issue. Participants access the deployed app and try the same attacks. |
| 13:55–14:15 | **Hands-on: Run ZAP scan (D-02)** | Modify the ZAP workflow to target the deployed app URL (replace the Juice Shop target). Trigger the workflow. While ZAP runs (10-15 min), explain: ZAP spider crawls the app, active scanner probes for vulnerabilities (XSS, SQLi, path traversal, etc.), results exported as SARIF. When complete, review findings in the Security tab. Compare DAST findings vs SAST findings — what does ZAP catch that CodeQL doesn't? (Runtime configuration issues, missing security headers, session management problems.) |

**Success Criteria:**
- [ ] Application deployed and accessible via HTTPS (or facilitator demo observed)
- [ ] Log forging attack successfully demonstrated
- [ ] ZAP scan completed with SARIF results uploaded to Security tab

---

#### 14:15–14:30 — Break (15 min)

---

#### 14:30–15:30 — MDC Hands-On Integration (60 min)

**Lab References:** M-01 (hands-on), M-02 (hands-on)

**Objective:** Each participant connects their GitHub organization to MDC (or observes the facilitator doing so) and navigates the DevOps Security dashboard with real findings data.

| Time | Activity | Detail |
|------|----------|--------|
| 14:30–14:50 | **Hands-on: Connect GitHub to MDC (M-01)** | Navigate to Azure portal → Defender for Cloud → Environment settings → Add environment → GitHub. Authenticate with GitHub OAuth. Select the organization and repositories to connect. Enable DevOps Security posture management. Wait for initial sync to begin (full sync takes hours — use facilitator's pre-synced environment for the dashboard walkthrough). If participants don't have MDC access, they observe the facilitator. |
| 14:50–15:15 | **Hands-on: DevOps Security Dashboard (M-02)** | Using the facilitator's pre-synced MDC environment: Navigate to Defender for Cloud → DevOps Security. Walk through each view: **Repository overview** — list of connected repos with finding counts. **Code-to-cloud mapping** — trace a CodeQL finding to the Azure resource it deploys to. **Recommendations** — MDC recommendations derived from GHAS findings. **Severity breakdown** — Critical/High/Medium/Low distribution. **Trends** — how finding counts change over time. Show the "Secure Score" contribution from DevOps findings. |
| 15:15–15:30 | **Discussion: Code-to-Cloud vision** | Verbal discussion: How does the MDC unified view change the security conversation? What workflows change when security teams can see code vulnerabilities alongside cloud misconfigurations? How does this compare to your current tooling? |

**Success Criteria:**
- [ ] GitHub connector configured in MDC (or facilitator demo observed)
- [ ] DevOps Security dashboard navigated with finding data visible
- [ ] Can explain the code-to-cloud security posture concept

---

#### 15:30–16:30 — Agent Orchestration + Security Plan Creation (60 min)

**Lab References:** A-08, A-06

| Time | Activity | Detail |
|------|----------|--------|
| 15:30–15:50 | **Hands-on: Agent orchestration via CI/CD (A-08)** | Open `.github/workflows/security-agent-workflow.yml`. Read through the workflow: manual trigger, install Copilot CLI, load agent prompt, run with `--allow-all-tools`, output report, upload artifact, critical vulnerability check gate. Trigger the workflow via Actions → Run workflow. While running, discuss: "This is how you automate agent-based security reviews in CI. Every push, every PR, every release could trigger an agent assessment." When complete, download the artifact: `security-reports/security-assessment-report.md`. Review the report. Note the critical vulnerability gate: if "THIS ASSESSMENT CONTAINS A CRITICAL VULNERABILITY" appears, the workflow fails. |
| 15:50–16:30 | **Hands-on: Security Plan Creator Agent (A-06)** | Select `@security-plan-creator` in VS Code Copilot Chat. Enter: "Create a comprehensive security plan for the gh-aspnet-webapp blueprint in this repository." Walk through the 5 agent phases as they execute: **Phase 1:** Blueprint selection and planning — agent identifies `blueprints/gh-aspnet-webapp/`. **Phase 2:** Architecture analysis — agent reads Bicep files, maps components (ACR, App Service, Managed Identity). **Phase 3:** Threat assessment — agent identifies threats using the DS/NS/PA/IM/DP/PV/ES/GS category framework. **Phase 4:** Plan generation — agent writes section-by-section security plan with mitigations. **Phase 5:** Validation — agent verifies plan completeness. Compare the generated plan against the existing `security-plan-outputs/security-plan-gh-aspnet-webapp.md`. Discuss: did the agent find the same threats? Different mitigations? How does the plan quality compare? |

**Success Criteria:**
- [ ] Security agent workflow triggered and report artifact downloaded
- [ ] Understand the critical vulnerability gate mechanism
- [ ] Security Plan Creator Agent generated a threat assessment
- [ ] Can compare agent-generated plan vs pre-existing plan

---

#### 16:30–17:00 — Day 2 Wrap-Up & Day 3 Preview (30 min)

| Time | Activity | Detail |
|------|----------|--------|
| 16:30–16:40 | **Progress inventory** | Pull up the fork. Count: findings discovered (Day 1), findings remediated (Day 2: Terraform, K8s, pipelines). Calculate remediation percentage. Show the before/after: "We started with X findings and have remediated Y so far." |
| 16:40–16:50 | **What remains** | List what Day 3 will cover: application code remediation (fix all 7+ vulnerabilities in webapp01), CSPM and attack path analysis, Security Campaigns demo, compliance mapping exercise, PoC readout preparation. |
| 16:50–17:00 | **Homework (optional)** | Participants can continue exploring agents on their own tonight. Suggest: run `@supply-chain-security-agent` and review its governance recommendations. |

**End of Day 2 State:** Infrastructure (Terraform) partially remediated, Kubernetes manifests fixed, 2+ pipelines hardened, app deployed, DAST scan complete, MDC connected with findings visible, agent orchestration and security planning demonstrated. Application code vulnerabilities remain for Day 3.

---

## Day 3 — Remediation: Application Code, Compliance & PoC Readout

### Day 3 Theme

Day 1 found the problems. Day 2 fixed the foundation. Day 3 fixes the application code, maps everything to compliance frameworks, and produces the PoC deliverable.

### Day 3 Detailed Timeline

#### 09:00–09:30 — Day 2 Recap & Remediation Strategy (30 min)

| Time | Activity | Detail |
|------|----------|--------|
| 09:00–09:15 | **Findings dashboard review** | Pull up the Security tab. Show the current state: remaining CodeQL findings, remaining Dependabot alerts, resolved IaC findings. Categorize remaining work: code vulnerabilities in `webapp01`, dependency issues, container image issues. |
| 09:15–09:30 | **Remediation priority discussion** | Discuss with participants: how would you prioritize these remaining findings in a real project? Factors: severity, exploitability, business impact, fix complexity. Agree on the remediation order for the code sprint. |

---

#### 09:30–10:45 — Code Vulnerability Remediation Sprint (75 min)

**Lab Reference:** R-01 (full)

**Objective:** Fix all 7+ intentional vulnerabilities in `webapp01` using a combination of Copilot Autofix, agent assistance, and manual coding. Validate each fix with re-scan.

| Time | Activity | Detail |
|------|----------|--------|
| 09:30–09:40 | **Setup** | Create a remediation branch: `git checkout -b fix/remediate-all-vulnerabilities`. Open the CodeQL findings list. Open VS Code with the Security Reviewer Agent ready. |
| 09:40–10:30 | **Hands-on: Fix all vulnerabilities** | Work through each vulnerability: |

**Vulnerability 1 — Hardcoded DB Connection String (`DevSecOps.cshtml.cs` line 15)**
- Remove `const string CONNECTION_STRING = "Server=localhost;Database=TestDB;..."`
- Replace with `IConfiguration` injection: `_configuration.GetConnectionString("DefaultConnection")`
- Move connection string to Azure Key Vault (conceptual — update `appsettings.json` to reference Key Vault)

**Vulnerability 2 — Log Forging (`DevSecOps.cshtml.cs` lines 28-29)**
- Current: `_logger.LogInformation($"User accessed DevSecOps page: {userInput}")`
- Fix: Sanitize input before logging — strip newlines, CRLF, control characters
- Use structured logging: `_logger.LogInformation("User accessed DevSecOps page: {User}", sanitizedInput)`

**Vulnerability 3 — ReDoS Regex (`DevSecOps.cshtml.cs` line 18)**
- Current: `new Regex(@"^(a+)+$", RegexOptions.Compiled)`
- Fix: Rewrite to non-catastrophic pattern: `new Regex(@"^a+$", RegexOptions.Compiled)`
- Add timeout: `new Regex(@"^a+$", RegexOptions.Compiled, TimeSpan.FromSeconds(1))`

**Vulnerability 4 — Insecure Deserialization (`DevSecOps.cshtml.cs`)**
- Remove `Newtonsoft.Json` usage with unrestricted type handling
- Replace with `System.Text.Json` with strict `JsonSerializerOptions`

**Vulnerability 5 — Hardcoded Password (`Index.cshtml.cs` line 11)**
- Remove `Pass@word1` default
- Replace with configuration-driven authentication

**Vulnerability 6 — Command Injection (`Index.cshtml.cs` lines 22-23)**
- Remove direct user input in process command: `"/C fsutil volume diskfree {drive}:"`
- Add input validation: whitelist allowed drive letters
- Use parameterized execution

**Vulnerability 7 — Secret Exposure (`appsettings.json`)**
- Remove `STORAGE_TEST` and `CUSTOM_TEST` values
- Replace with Key Vault references or user-secrets for development
- Add `appsettings.json` to `.github/secret_scanning.yml` include list

| Time | Activity | Detail |
|------|----------|--------|
| 10:30–10:45 | **Verify fixes** | Run `dotnet build src/webapp01/` — verify compilation succeeds. Commit all changes to the remediation branch. Push and create a PR. Wait for CodeQL re-scan (or trigger manually). Verify alert count decreases. Show: before (X alerts) → after (Y alerts, target 0). |

**Success Criteria:**
- [ ] All 7 vulnerabilities addressed
- [ ] `dotnet build` succeeds
- [ ] PR created with reduced CodeQL alert count

---

#### 10:45–11:00 — Break (15 min)

---

#### 11:00–11:45 — Security Campaigns + Advanced Autofix (45 min)

**Lab References:** G-08, G-07 (advanced)

| Time | Activity | Detail |
|------|----------|--------|
| 11:00–11:20 | **Demo: Security Campaigns (G-08)** | This requires GHAS Enterprise at the org level. **If available:** Navigate to Security → Campaigns → Create campaign. Select up to 1000 alerts to target. Name the campaign (e.g., "Q1 Vulnerability Reduction"). Assign to a team or individual. Show how Autofix generates fix suggestions in bulk for the campaign. Track campaign progress: alerts fixed, alerts remaining, auto-generated PRs. **If not available:** Facilitator walks through pre-captured screenshots showing the campaign workflow. Discuss: how this changes the "one alert at a time" remediation model to batch remediation at scale. |
| 11:20–11:45 | **Hands-on: Multi-language Autofix (G-07 advanced)** | Participants explore Autofix across the multi-language samples: Open `samples/insecure.py` — trigger CodeQL → Autofix for the weak hash finding (MD5 → SHA-256). Open `samples/insecure.js` — trigger CodeQL → Autofix for the `eval()` finding. Open `samples/mongodb.go` — examine the `InsecureSkipVerify: true` finding (Autofix may not cover Go — discuss agent fallback). For each fix: review the suggestion, evaluate correctness, apply if good. |

**Success Criteria:**
- [ ] Security Campaigns concept understood (live or via demo)
- [ ] At least 2 Autofix suggestions reviewed across different languages
- [ ] Can articulate when to use Autofix vs manual fix vs agent assistance

---

#### 11:45–12:15 — CSPM & Attack Path Analysis (30 min)

**Lab Reference:** M-03

| Time | Activity | Detail |
|------|----------|--------|
| 11:45–12:00 | **Demo + Try: Defender CSPM (M-03)** | Navigate to MDC → Secure Score. Walk through score categories: Identity, Networking, Data, Compute, IoT/OT, DevOps. Show recommendations by priority. Click into a recommendation — trace it from MDC to the Azure resource to the code that provisioned it. Show the "Remediate" button and how MDC can auto-fix some cloud misconfigurations. |
| 12:00–12:15 | **Demo: Attack Path Analysis** | Navigate to MDC → Attack path analysis (requires Defender CSPM plan). Show an attack path: e.g., "Internet-exposed web app with SQL injection vulnerability connected to database with public endpoint." Walk through each node in the path. Explain: this is why we fix code AND infrastructure — attack paths cross boundaries. Discuss: how would fixing the Terraform NSG rules (Day 2) break this attack path? How would fixing the SQL injection (Day 3 morning) break it? |

**Success Criteria:**
- [ ] Secure Score reviewed with category breakdown
- [ ] At least one attack path traced from internet to data
- [ ] Can explain how code fixes + IaC fixes together eliminate attack paths

---

#### 12:15–13:15 — Lunch (60 min)

---

#### 13:15–14:15 — Compliance Mapping Exercise (60 min)

**Lab Reference:** R-04

**Objective:** Map the accumulated findings from all scanning tools to a target compliance framework.

| Time | Activity | Detail |
|------|----------|--------|
| 13:15–13:30 | **Context: Compliance frameworks** | Brief verbal explanation of the mapping exercise. CIS Azure Benchmark v2.0 — cloud configuration. NIST 800-53 Rev 5 — federal security controls. SOC 2 Type II — operational controls. Explain the mapping concept: a single scan finding may map to multiple compliance controls. |
| 13:30–14:00 | **Hands-on: Map findings to CIS Azure Benchmark** | Provide participants with a mapping worksheet. Using the tfsec/KICS findings from Day 2, map each finding to its CIS Azure Benchmark control. Example mappings: Open SSH NSG → CIS 6.2 "Ensure that SSH access is restricted from the internet". Disabled encryption → CIS 7.2 "Ensure that Unattached disks are encrypted". RBAC disabled on AKS → CIS 8.5 "Ensure RBAC is enabled on AKS clusters". Weak SQL password → CIS 4.1 "Ensure that 'Auditing' is set to 'On' for SQL servers" (related). Participants work in pairs to complete the mapping for all 10 Terraform findings remediated on Day 2. |
| 14:00–14:15 | **Discussion: Compliance gaps** | Review the completed mapping. Identify: which CIS controls are covered by the current scanning tools? Which controls are NOT covered and need manual assessment? Discuss: "Automated scanning covers ~60-70% of CIS controls. The remaining 30-40% require process and governance controls." |

**Success Criteria:**
- [ ] At least 10 findings mapped to CIS Azure Benchmark controls
- [ ] Compliance coverage gaps identified
- [ ] Understand the difference between tool-auditable vs process-auditable controls

---

#### 14:15–14:30 — Break (15 min)

---

#### 14:30–15:30 — PoC Results Compilation (60 min)

**Objective:** Compile the findings, remediations, and metrics from all 3 days into a structured PoC readout document.

| Time | Activity | Detail |
|------|----------|--------|
| 14:30–14:45 | **Metrics collection** | Gather data from the fork: Total findings discovered (by tool, by severity). Findings remediated (by category). Remediation rate (% fixed). Scan coverage matrix: which tools covered which file types. Agent assessment quality: compare agent findings vs tool findings. Time-to-remediate metrics: Autofix (seconds) vs manual (minutes). |
| 14:45–15:15 | **Hands-on: Write PoC summary** | Using the PoC results template structure, participants (working in pairs or as a group) draft: **Executive Summary** — 3 sentences on what was done and key findings. **Findings by Category** — table of SAST, SCA, Secrets, Container, IaC, DAST, Agent findings with counts and severities. **Remediation Results** — what was fixed, before/after counts, remaining items. **Tool Effectiveness** — which tools found what, overlap analysis, unique findings per tool. **Agent Value Assessment** — what the agents found that tools didn't, agent response quality, usability rating. **Compliance Posture** — CIS mapping coverage, gaps. **Recommendation** — adopt/don't adopt, which capabilities to prioritize. |
| 15:15–15:30 | **Readout review** | Facilitator reviews the draft with the group. Discuss: is this document convincing enough for a leadership presentation? What additional data would strengthen the case? |

**Success Criteria:**
- [ ] PoC summary document drafted with all sections
- [ ] Metrics quantified (finding counts, remediation rates)
- [ ] Clear recommendation statement formulated

---

#### 15:30–16:30 — PoC Readout Presentation (60 min)

| Time | Activity | Detail |
|------|----------|--------|
| 15:30–16:00 | **Group presentation** | One participant (or facilitator) presents the PoC results to the group as if presenting to leadership. Cover: what was tested, what was found, what was fixed, what it would cost, what the recommendation is. Other participants role-play as leadership: ask hard questions. "What's the ROI?" "How long to roll out to all teams?" "What happens to our existing security tools?" |
| 16:00–16:15 | **Discussion: Adoption path** | If the PoC were approved, what would the next steps be? Discuss: pilot team selection, GHAS licensing, MDC plan selection, agent customization for the customer's domain, training plan, governance changes. |
| 16:15–16:30 | **Closeout** | Thank participants. Share: fork URL (participants keep their fork), PoC summary document, facilitator contact for follow-up. Collect feedback (NPS survey or feedback form). Pitch the 5-day PoC if appropriate: "In a 5-day engagement, we'd do all of this against YOUR code, YOUR infrastructure, YOUR pipelines." |

---

## Day 2+3 Lab Coverage Matrix (Incremental from Day 1)

| Lab ID | Title | Day 2 | Day 3 | Depth |
|--------|-------|-------|-------|-------|
| I-01 | Terraform / tfsec | ✅ | — | Deep (remediation + re-scan) |
| I-02 | Terraform / KICS | ✅ | — | Comparison |
| I-03 | MSDO Multi-Tool | ✅ | — | Full |
| I-04 | Kubernetes / Kubesec | ✅ | — | Full (remediation + re-scan) |
| R-03 | Pipeline Hardening | ✅ | — | Full (2 workflows) |
| D-01 | Deploy App | ✅ | — | Full |
| D-02 | ZAP Scan | ✅ | — | Full |
| M-01 | GitHub ↔ MDC Connection | ✅ | — | Hands-on |
| M-02 | MDC Dashboard | ✅ | — | Hands-on |
| A-08 | Agent Orchestration | ✅ | — | Full |
| A-06 | Security Plan Creator | ✅ | — | Full |
| G-07 | Copilot Autofix (advanced) | — | ✅ | Multi-language |
| G-08 | Security Campaigns | — | ✅ | Demo or hands-on (licensing dependent) |
| M-03 | CSPM / Attack Paths | — | ✅ | Full |
| R-01 | Fix Code Vulnerabilities | — | ✅ | Full (all 7+ vulns) |
| R-04 | Compliance Mapping | — | ✅ | Guided exercise |

**3-Day Total Coverage:** 33 of 35 labs touched (94%). Only A-07 (Build Custom Agent) and M-04 (Runtime Threat Detection) are excluded — both reserved for 5-Day.

---

## 3-Day Cumulative Outcomes

| Metric | Target |
|--------|--------|
| Total findings discovered | 50+ across all categories |
| Findings remediated | 25+ (50%+ remediation rate) |
| Vulnerability categories covered | SAST, SCA, Secrets, Container, IaC, DAST, Pipeline, Governance |
| Tools exercised | 10+ (CodeQL, tfsec, KICS, MSDO, Trivy, Grype, ZAP, Kubesec, Syft, Scorecard) |
| Agents used | All 6 |
| Compliance controls mapped | 10+ CIS Azure Benchmark controls |
| Deliverable | Written PoC summary document with metrics and recommendation |
