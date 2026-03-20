# 1-Day Delivery: Agentic DevSecOps — Labs & Demos Only

> **Format:** 100% live demos and hands-on labs — no slide presentations  
> **Duration:** 8 hours (09:00–17:00) with breaks and lunch  
> **Audience:** Security leaders, engineering managers, senior developers, DevOps leads  
> **Delivery Model:** Facilitator-driven live demos with guided participant follow-along  
> **Objective:** Experience every major GHAS + GHCP + MDC capability through direct interaction in a single intensive day

---

## Delivery Philosophy

This is not a presentation day. Every minute is spent inside GitHub, VS Code, the Azure portal, or a terminal. The facilitator demonstrates each capability live while participants follow along on their own forked repositories. Context is provided verbally during transitions — there are no slides.

**Pacing:** Each session follows a Demo → Try → Verify cadence:
1. **Demo** (facilitator shows the capability live)
2. **Try** (participants replicate on their own fork)
3. **Verify** (participants confirm expected results, facilitator troubleshoots)

---

## Pre-Day Requirements

All setup must be completed **before** the day begins. Send participants a setup checklist at minimum 3 business days prior.

### Participant Checklist (Completed Before Day 1)

| # | Prerequisite | Verification |
|---|-------------|--------------|
| 1 | GitHub account with access to a GHAS-enabled org (Enterprise trial acceptable) | Can see "Security" tab with Code scanning, Secret scanning, Dependabot sections |
| 2 | Fork of `devopsabcs-engineering/gh-advsec-devsecops` to personal account or lab org | Fork exists, default branch is `main` |
| 3 | GHAS features enabled on fork: Code scanning (CodeQL), Secret scanning + push protection, Dependabot alerts | Settings → Code security and analysis → all toggles green |
| 4 | VS Code installed with extensions: GitHub Copilot, GitHub Copilot Chat, Azure Tools, Docker | Extensions panel shows all four installed and authenticated |
| 5 | CLI tools installed: `gh` (authenticated), `az` (logged in), `docker`, `dotnet` SDK 9.0 | `gh auth status`, `az account show`, `docker --version`, `dotnet --version` all succeed |
| 6 | Azure subscription with Contributor role (Azure Pass or BYOS) | `az account list` shows active subscription |
| 7 | Repository cloned locally | `cd gh-advsec-devsecops && git status` succeeds |

### Facilitator Checklist (Completed Before Day 1)

| # | Prerequisite | Purpose |
|---|-------------|---------|
| 1 | Pre-baked fork with all workflows already run at least once | Ensures Security tab has results to show even if live runs are slow |
| 2 | Azure subscription with deployed `webapp01` via `gh-aspnet-webapp` blueprint | For DAST and MDC demos |
| 3 | MDC workspace configured with GitHub connector active | For M-01/M-02 demos |
| 4 | Defender CSPM + Defender for App Service enabled | For security posture demos |
| 5 | Backup screenshots for every demo step | Offline fallback if network/service issues arise |
| 6 | Pre-created branch `demo/vulnerable-code` with intentional secret + vulnerable code ready to PR | For G-01 and G-07 demos |
| 7 | Pre-created PR with Autofix suggestion waiting | For G-07 if live Autofix generation is slow |

---

## Detailed Timeline

### 09:00–09:20 — Session 1: Environment Verification & Orientation (20 min)

**Lab Reference:** F-01 (Environment Setup) — verification subset only

**Objective:** Confirm every participant has a working environment and can navigate the repository.

| Time | Activity | Facilitator Action | Participant Action |
|------|----------|-------------------|-------------------|
| 09:00–09:05 | Welcome & ground rules | State objectives: "By 5pm you will have used every major GHAS capability, run 6 custom AI security agents, and seen code-to-cloud security in MDC." Explain Demo → Try → Verify cadence. | Listen, open laptop |
| 09:05–09:10 | Environment check | Run verification commands on screen | Run same commands, report issues in chat/raise hand |
| 09:10–09:15 | Quick repo tour | Terminal: `tree -L 2` or VS Code explorer. Point out: `src/webapp01` (vulnerable app), `terraform/azure/` (vulnerable IaC), `manifests/` (K8s), `.github/agents/` (6 agents), `.github/workflows/` (17 workflows), `samples/` (multi-language). | Follow along in VS Code, open key files |
| 09:15–09:20 | Identify target vulnerabilities | Open `src/webapp01/Pages/DevSecOps.cshtml.cs` — point out hardcoded DB connection string (line 15), log forging (line 28-29), ReDoS regex (line 18). Open `terraform/azure/sql.tf` — point out hardcoded password. "These are what the tools will find today." | Read through the vulnerable code, note the patterns |

**Verification:** Every participant can run `gh repo view --json name` and see their fork name.

---

### 09:20–09:55 — Session 2: Secret Scanning & Push Protection (35 min)

**Lab Reference:** G-01

**Objective:** Experience GitHub's secret scanning in action — attempt to push a secret, get blocked, review the alert, and configure a custom pattern.

| Time | Activity | Detail |
|------|----------|--------|
| 09:20–09:25 | **Demo: View existing secret alerts** | Navigate to fork → Security → Secret scanning. Show existing alerts from `appsettings.json` (the `STORAGE_TEST` and `CUSTOM_TEST` values). Explain alert states: open, resolved, revoked. Show the "Locations" tab showing exact file + line. |
| 09:25–09:35 | **Demo + Try: Push protection in action** | Create a new branch: `git checkout -b demo/secret-test`. Create a test file with a simulated AWS key or GitHub PAT format. Attempt `git push`. Show the push protection block message. Explain bypass options (with justification). Participants replicate on their forks. |
| 09:35–09:45 | **Demo + Try: Custom secret patterns** | Navigate to Settings → Code security → Secret scanning → Custom patterns. Create a pattern matching internal credential format (e.g., `INTERNAL_KEY_[A-Z0-9]{32}`). Add a test string to a file, push, verify detection. Participants create their own custom pattern. |
| 09:45–09:50 | **Demo: Validity checks** | Show how GitHub checks if detected secrets are still active (for supported providers). Show the "Active" vs "Inactive" badge on alerts. Explain the remediation workflow: revoke → rotate → resolve. |
| 09:50–09:55 | **Verify & discuss** | Every participant should have: (1) a blocked push attempt in their git history, (2) at least one secret scanning alert visible, (3) one custom pattern configured. Quick Q&A. |

**Success Criteria:**
- [ ] Push protection blocked a commit containing a secret
- [ ] Secret scanning alert visible in Security tab
- [ ] Custom pattern created and functional

---

### 09:55–10:45 — Session 3: Code Scanning with CodeQL (50 min)

**Lab Reference:** G-02

**Objective:** Trigger a CodeQL scan, analyze findings across multiple languages, understand severity and CWE mapping, and trace a finding from alert to vulnerable line.

| Time | Activity | Detail |
|------|----------|--------|
| 09:55–10:05 | **Demo: Pre-existing CodeQL results** | Navigate to Security → Code scanning. Show the alerts list. Filter by severity (Critical, High, Medium, Low). Filter by language (C#, Python, JavaScript). Click into a C# finding — show: alert title, CWE reference, affected file, vulnerable line highlighted, data flow trace (if available). Show the `DevSecOps.cshtml.cs` hardcoded credential finding. |
| 10:05–10:15 | **Demo: Trigger a new scan** | Navigate to Actions → CodeQL workflow. Click "Run workflow" (manual dispatch). While it runs, explain: CodeQL compiles the code into a database, runs queries against it, uploads SARIF results. Show the workflow YAML: language matrix (C#, Python, JS/TS, Actions), `security-and-quality` query suite, scheduled + PR + push triggers. |
| 10:15–10:30 | **Try: Create a vulnerable PR** | Participants create a new branch. Add a new file `samples/vuln-demo.py` with a deliberate SQL injection pattern: `cursor.execute("SELECT * FROM users WHERE name = '" + user_input + "'")`. Push and create a PR against `main`. Observe the CodeQL check status on the PR. Wait for results (or view facilitator's pre-run results if scan is still running). |
| 10:30–10:40 | **Demo: Multi-language findings walkthrough** | Walk through findings in each language: C# (hardcoded creds, log forging, ReDoS), Python (bare except, weak hash, insecure imports from `samples/insecure.py`), JavaScript (eval injection from `samples/insecure.js`), GitHub Actions (if any workflow findings). Explain CWE mapping: CWE-798 (hardcoded credentials), CWE-117 (log injection), CWE-1333 (ReDoS). |
| 10:40–10:45 | **Verify** | Every participant should see CodeQL results in their Security tab. At minimum, the pre-existing findings from the forked code should be visible. Discuss: "These are the findings the AI agents will also detect — later we'll compare." |

**Success Criteria:**
- [ ] CodeQL workflow triggered (manually or via PR)
- [ ] At least 5 findings visible in Security → Code scanning
- [ ] Can trace a finding from alert → file → vulnerable line

---

### 10:45–11:00 — Break (15 min)

---

### 11:00–11:35 — Session 4: Supply Chain Security (35 min)

**Lab References:** G-03, G-04, G-05

**Objective:** Block a vulnerable dependency via Dependency Review, generate an SBOM, and run an OpenSSF Scorecard assessment — three supply chain security tools in rapid succession.

| Time | Activity | Detail |
|------|----------|--------|
| 11:00–11:12 | **Demo + Try: Dependency Review (G-03)** | Create a branch. Modify `src/webapp01/webapp01.csproj` to downgrade `Newtonsoft.Json` from `13.0.1` to `12.0.2` (known vulnerable version). Push and create PR. Show the Dependency Review check blocking the PR with the vulnerability details. Show the PR comment with license and vulnerability summary. Participants replicate. |
| 11:12–11:15 | **Demo: Dependabot alerts** | Navigate to Security → Dependabot. Show alert list. Show auto-generated fix PRs. Explain the `dependabot.yml` configuration: NuGet weekly, GitHub Actions daily, 15 PR limit. |
| 11:15–11:25 | **Demo + Try: SBOM Generation (G-04)** | Navigate to Actions. Trigger the `SCA-Anchore-Syft-SBOM` workflow. While running, also trigger `SCA-Microsoft-SBOM`. Explain the difference: Syft (CycloneDX format, Anchore ecosystem) vs Microsoft SBOM Tool (SPDX 2.2 format). Once complete, download artifacts. Open the SBOM JSON — walk through the component inventory. Navigate to Insights → Dependency graph → show the visual representation. |
| 11:25–11:35 | **Demo + Try: OpenSSF Scorecard (G-05)** | Trigger the Scorecard workflow (or show pre-run results). Walk through the score dimensions: Branch Protection, CI Tests, Code Review, Dangerous Workflow, Dependency Update Tool, License, Maintained, Pinned Dependencies, SAST, Security Policy, Signed Releases, Token Permissions, Vulnerabilities. For each low score, explain what action would improve it. Participants review their own fork's Scorecard results. |

**Success Criteria:**
- [ ] Dependency Review blocked a PR with a vulnerable downgrade
- [ ] At least one SBOM artifact downloaded and inspected
- [ ] Scorecard results visible with improvement recommendations noted

---

### 11:35–12:15 — Session 5: Container & IaC Security (40 min)

**Lab References:** G-06, I-01, I-04

**Objective:** Scan a Docker image for CVEs, scan Terraform for misconfigurations, and compare secure vs insecure Kubernetes manifests.

| Time | Activity | Detail |
|------|----------|--------|
| 11:35–11:50 | **Demo + Try: Container Scanning (G-06)** | Build the Docker image locally: `docker build -t webapp01:scan -f src/webapp01/Dockerfile src/webapp01/`. Trigger the Trivy workflow (or show pre-run results). Walk through SARIF results: base image CVEs, OS package vulnerabilities, application dependency vulnerabilities. Also trigger Grype workflow. Compare: which tool found what. Discuss image hardening: pin to digest, use distroless/Alpine, add USER directive, add HEALTHCHECK. Participants build and review their own scan results. |
| 11:50–12:05 | **Demo + Try: Terraform Scanning (I-01)** | Trigger the `IACS-AquaSecurity-tfsec` workflow. Walk through findings: `terraform/azure/sql.tf` — hardcoded password (`Aa12345678`), disabled SSL enforcement. `terraform/azure/networking.tf` — SSH/RDP open to `0.0.0.0/0`. `terraform/azure/storage.tf` — encryption disabled. `terraform/azure/aks.tf` — RBAC disabled, dashboard enabled. `terraform/azure/security_center.tf` — free tier, alerts disabled. For each finding: show the tfsec rule ID, severity, CIS benchmark mapping, and the exact line in the Terraform file. Participants review findings on their fork. |
| 12:05–12:15 | **Demo: Kubernetes Manifest Comparison (I-04)** | Open `manifests/critical-double.yaml` — privileged: true, allowPrivilegeEscalation: true. Open `manifests/score-5-pod-serviceaccount.yaml` — runAsNonRoot: true, readOnlyRootFilesystem: true, automountServiceAccountToken: false. Trigger or show Kubesec scan results. Compare scores side-by-side. Explain: what makes a pod secure (security context, non-root, read-only FS, no privilege escalation, resource limits). |

**Success Criteria:**
- [ ] Container scan results visible with CVE counts
- [ ] At minimum 10 Terraform findings identified
- [ ] Can articulate the security difference between the two K8s manifests

---

### 12:15–13:15 — Lunch (60 min)

---

### 13:15–14:00 — Session 6: Copilot Autofix & Remediation (45 min)

**Lab References:** G-07, R-01 (subset)

**Objective:** Experience AI-powered vulnerability remediation — trigger Copilot Autofix on a real finding, review the suggestion, and apply a fix.

| Time | Activity | Detail |
|------|----------|--------|
| 13:15–13:30 | **Demo: Copilot Autofix on existing findings** | Navigate to Security → Code scanning. Find a finding that has an Autofix suggestion available (Copilot Autofix generates fix suggestions for CodeQL findings). Click "Generate fix" or show a pre-generated fix. Walk through: the original vulnerable code, the AI-generated fix, the explanation of what changed and why. Show the diff view. Demonstrate one-click "Create PR with fix" flow. Show the resulting PR with the Autofix commit. |
| 13:30–13:45 | **Try: Trigger and apply Autofix** | Participants find a CodeQL finding on their fork that supports Autofix. Generate the fix. Review the suggestion critically — is it correct? Does it maintain functionality? Does it fully remediate the vulnerability? Apply the fix by creating the PR. Merge the fix PR. Re-run CodeQL to verify the alert closes. |
| 13:45–14:00 | **Demo + Try: Manual remediation with agent assistance** | Pick a finding that Autofix does not cover (e.g., the ReDoS pattern in `DevSecOps.cshtml.cs`). Open VS Code. In Copilot Chat, ask: "How do I fix the ReDoS vulnerability in this regex pattern `^(a+)+$`?" Review the agent's suggestion (atomic grouping, possessive quantifiers, or rewriting the regex). Apply the fix manually. Rebuild to verify no compilation errors: `dotnet build src/webapp01/`. Participants replicate with a different finding. |

**Success Criteria:**
- [ ] At least one Autofix suggestion reviewed and applied via PR
- [ ] At least one manual fix applied with Copilot Chat assistance
- [ ] `dotnet build` succeeds after fixes

---

### 14:00–14:45 — Session 7: Custom Copilot Security Agents (45 min)

**Lab References:** A-01, A-02, A-03

**Objective:** Run three different custom security agents against the repository and compare their specialized analysis capabilities.

| Time | Activity | Detail |
|------|----------|--------|
| 14:00–14:05 | **Demo: Agent architecture** | Open `.github/agents/` in VS Code. Show the six agent definition files. Explain the structure: name, model, description, system prompt, capabilities, output format. Open VS Code Copilot Chat. Show the agent picker dropdown — all six agents should appear. |
| 14:05–14:20 | **Demo + Try: Security Agent (A-01)** | Select `@security-agent` in Copilot Chat. Enter: "Perform a comprehensive security review of this repository." Watch the agent analyze: `src/webapp01/` (SAST findings), `terraform/azure/` (IaC findings), `.github/workflows/` (CI/CD findings), `manifests/` (container findings). Review the generated report — structured by severity (CRITICAL/HIGH/MEDIUM/LOW). Compare the agent's findings against the CodeQL results from Session 3: overlap? Unique findings? Participants run the same prompt on their fork. |
| 14:20–14:35 | **Demo + Try: Security Reviewer Agent (A-02)** | Switch to `@security-reviewer-agent`. Enter: "Review src/webapp01/Pages/DevSecOps.cshtml.cs for OWASP Top 10 vulnerabilities." Watch the code-level analysis: OWASP mapping, specific line references, severity classification, remediation suggestions. Compare: this agent goes deeper on code than the Security Agent — it's the specialist. Try a second file: "Review src/webapp01/Pages/Index.cshtml.cs for injection vulnerabilities." Participants run both prompts. |
| 14:35–14:45 | **Demo + Try: IaC Security Agent (A-03)** | Switch to `@iac-security-agent`. Enter: "Scan terraform/azure/ for security misconfigurations and map to CIS Azure Benchmarks." Review: structured findings with CIS control IDs, severity, remediation Terraform snippets. Compare against the tfsec results from Session 5. Ask the agent a follow-up: "Which of these findings are CRITICAL and need immediate remediation?" Participants run on their fork. |

**Success Criteria:**
- [ ] Security Agent report generated with findings across all categories
- [ ] Security Reviewer Agent produced OWASP-mapped findings for specific files
- [ ] IaC Agent produced CIS-mapped findings for Terraform
- [ ] Can articulate the difference between each agent's specialty

---

### 14:45–15:00 — Break (15 min)

---

### 15:00–15:40 — Session 8: Pipeline & Supply Chain Agents + MDC (40 min)

**Lab References:** A-04, A-05, M-01, M-02

**Objective:** Run the remaining specialized agents (Pipeline, Supply Chain) and see the code-to-cloud unified view in Microsoft Defender for Cloud.

| Time | Activity | Detail |
|------|----------|--------|
| 15:00–15:12 | **Demo + Try: Pipeline Security Agent (A-04)** | Select `@pipeline-security-agent`. Enter: "Audit all GitHub Actions workflows in this repository for security weaknesses and produce a hardened configuration." Review findings: actions not pinned to SHA (using tags instead), overly broad permissions, potential script injection points, secrets handling issues. The agent should produce a diff showing before/after for at least 2 workflows. Participants run on their fork and pick one workflow to harden based on the agent's recommendation. |
| 15:12–15:24 | **Demo + Try: Supply Chain Security Agent (A-05)** | Select `@supply-chain-security-agent`. Enter: "Perform a full supply chain security audit of this repository including secrets detection, dependency vulnerabilities, and repository governance." Review: secrets exposure findings, dependency manifest analysis, SBOM/provenance recommendations, branch protection gaps, CODEOWNERS coverage assessment. The agent should recommend specific governance improvements. Participants run and note the top 3 recommendations. |
| 15:24–15:40 | **Demo: Microsoft Defender for Cloud Integration (M-01/M-02)** | Switch to browser — Azure portal → Defender for Cloud. Show the GitHub connector (pre-configured by facilitator): DevOps Security blade → GitHub organization connected → repositories listed. Walk through: DevOps security posture overview, code-to-cloud finding mapping (GHAS findings surfaced in MDC), recommendations prioritized by risk, Secure Score contribution from DevOps findings. Show the attack path analysis (if Defender CSPM enabled): how a code vulnerability + infrastructure misconfiguration could chain into an attack path. This is facilitator-driven demo only — participants observe (MDC requires the pre-configured facilitator environment). |

**Success Criteria:**
- [ ] Pipeline Agent findings reviewed with at least 1 workflow hardening applied
- [ ] Supply Chain Agent governance audit completed
- [ ] MDC DevOps Security blade observed with GitHub findings visible

---

### 15:40–16:40 — Session 9: End-to-End Flow — Commit to Cloud (60 min)

**Lab References:** Composite — uses G-01, G-02, G-07, A-01, M-01 concepts in a single flow

**Objective:** Execute the complete DevSecOps lifecycle in one continuous flow: introduce a vulnerability → get blocked/detected → use AI to fix it → verify the fix propagates to cloud posture. This is the capstone demo.

| Time | Activity | Detail |
|------|----------|--------|
| 15:40–15:50 | **Step 1: Introduce vulnerable code** | Create a new feature branch: `git checkout -b feature/new-api-endpoint`. Create a new C# file in `src/webapp01/Pages/` that contains: (a) a hardcoded API key, (b) an unsanitized user input used in a database query, (c) a reference to a vulnerable NuGet package. This simulates a developer's typical "fast feature" commit. Stage and attempt to push. **Expected:** Secret scanning push protection blocks the push due to the API key pattern. |
| 15:50–15:55 | **Step 2: Fix the secret, push, create PR** | Remove the hardcoded API key — replace with `IConfiguration` injection. Push succeeds. Create a PR against `main`. **Expected:** Multiple status checks start running: CodeQL, Dependency Review, Trivy, tfsec, MSDO. |
| 15:55–16:05 | **Step 3: Review automated scan results on PR** | Wait for checks to complete (or show facilitator's pre-run version). Walk through each check: Dependency Review catches the vulnerable package downgrade. CodeQL catches the SQL injection pattern. Show the annotations inline on the PR diff — findings appear exactly where the vulnerable code is. |
| 16:05–16:15 | **Step 4: AI-powered remediation** | Click "Generate fix" on the CodeQL finding (Copilot Autofix). Review the AI suggestion. Also open VS Code Copilot Chat and ask `@security-reviewer-agent`: "Review my PR changes for security issues." Compare the Autofix suggestion with the agent's recommendation. Apply the fix. |
| 16:15–16:25 | **Step 5: Verify and merge** | Push the fix commit. Wait for checks to pass. Show: the CodeQL alert is now resolved, Dependency Review passes with the corrected package version, all green checks. Merge the PR. |
| 16:25–16:35 | **Step 6: Observe cloud posture update** | Switch to MDC portal. Show the DevOps Security blade. After the merge, the security posture should reflect the remediated finding (this may take a few minutes — use the facilitator's pre-staged environment to show the before/after if timing is tight). Show: one fewer active alert, improved secure score. |
| 16:35–16:40 | **Recap the flow** | Verbally walk through what just happened: Developer committed → Secret scanning blocked the secret → Push protection enforced → PR created → CodeQL + Dependency Review + Container scans ran automatically → Copilot Autofix suggested a fix → Agent validated → Merge → MDC posture updated. "This entire loop happened in under 30 minutes. Without these tools, this would take weeks of manual security review." |

**Success Criteria:**
- [ ] Secret scanning blocked the initial push
- [ ] PR had multiple automated security checks
- [ ] Autofix or agent-assisted remediation applied
- [ ] MDC reflected the posture change (or facilitator demonstrated it)

---

### 16:40–17:00 — Session 10: Wrap-Up & Next Steps (20 min)

| Time | Activity | Detail |
|------|----------|--------|
| 16:40–16:50 | **Findings recap** | Facilitator shares screen: pull up the fork's Security tab. Count total findings discovered across all tools today. Categorize: SAST (CodeQL), SCA (Dependency Review, Dependabot), Secrets (Secret Scanning), Container (Trivy/Grype), IaC (tfsec), Agents (unique findings). "In 7 hours of hands-on work, we discovered X findings across Y categories using Z tools — all integrated into the developer workflow." |
| 16:50–16:55 | **What we covered** | Quick verbal recap of all 9 sessions. Hand out (or share link to) a one-page cheat sheet listing every tool used today, what it does, and where to find results. |
| 16:55–17:00 | **Next steps** | If appropriate, pitch the 3-Day Light PoC or 5-Day Full PoC. Explain: "Today you saw everything against our reference repo. In a 3-day PoC, you'd do deeper hands-on with remediation sprints. In a 5-day PoC, we'd run all of this against your actual codebase and infrastructure." Share: repo fork URL, documentation links, GHAS trial signup URL, MDC free trial URL. Collect feedback (NPS or short survey). |

---

## Timing Summary

| Session | Time | Duration | Focus | Labs |
|---------|------|----------|-------|------|
| 1 | 09:00–09:20 | 20 min | Environment verification + repo tour | F-01 (subset) |
| 2 | 09:20–09:55 | 35 min | Secret Scanning & Push Protection | G-01 |
| 3 | 09:55–10:45 | 50 min | Code Scanning with CodeQL | G-02 |
| — | 10:45–11:00 | 15 min | Break | — |
| 4 | 11:00–11:35 | 35 min | Supply Chain (Dependency Review, SBOM, Scorecard) | G-03, G-04, G-05 |
| 5 | 11:35–12:15 | 40 min | Container & IaC Security | G-06, I-01, I-04 |
| — | 12:15–13:15 | 60 min | Lunch | — |
| 6 | 13:15–14:00 | 45 min | Copilot Autofix & Remediation | G-07, R-01 (subset) |
| 7 | 14:00–14:45 | 45 min | Security, Reviewer & IaC Agents | A-01, A-02, A-03 |
| — | 14:45–15:00 | 15 min | Break | — |
| 8 | 15:00–15:40 | 40 min | Pipeline & Supply Chain Agents + MDC | A-04, A-05, M-01, M-02 |
| 9 | 15:40–16:40 | 60 min | End-to-End Flow (capstone) | Composite |
| 10 | 16:40–17:00 | 20 min | Wrap-up & next steps | — |
| | | **390 min active** (6.5 hrs) + **90 min breaks** = **8 hrs total** | | |

## Lab Coverage Matrix

| Lab ID | Title | Included | Depth |
|--------|-------|----------|-------|
| F-01 | Environment Setup | ✅ | Verification only (pre-work) |
| F-02 | Repo Walkthrough | ✅ | Embedded in Session 1 |
| G-01 | Secret Scanning | ✅ | Full |
| G-02 | Code Scanning / CodeQL | ✅ | Full |
| G-03 | Dependency Review | ✅ | Full |
| G-04 | SBOM Generation | ✅ | Demo + quick try |
| G-05 | OpenSSF Scorecard | ✅ | Demo + review |
| G-06 | Container Scanning | ✅ | Full |
| G-07 | Copilot Autofix | ✅ | Full |
| G-08 | Security Campaigns | ❌ | Not included (requires org-level Enterprise) |
| I-01 | Terraform / tfsec | ✅ | Findings review (no remediation) |
| I-02 | Terraform / KICS | ❌ | Covered by I-01 tooling |
| I-03 | MSDO IaC | ❌ | Not enough time |
| I-04 | Kubernetes / Kubesec | ✅ | Comparison demo |
| I-05 | Bicep Review | ❌ | Not enough time |
| D-01 | Deploy App | ❌ | Not enough time (moved to 3-day) |
| D-02 | ZAP Scan | ❌ | Not enough time (moved to 3-day) |
| D-03 | Log Forging Demo | ❌ | Not enough time (moved to 3-day) |
| M-01 | GitHub ↔ MDC Connection | ✅ | Facilitator demo (pre-configured) |
| M-02 | MDC Dashboard | ✅ | Facilitator demo |
| M-03 | CSPM / Attack Paths | ❌ | Not enough time (moved to 5-day) |
| M-04 | Runtime Threat Detection | ❌ | Not enough time (moved to 5-day) |
| A-01 | Security Agent | ✅ | Full |
| A-02 | Security Reviewer Agent | ✅ | Full |
| A-03 | IaC Security Agent | ✅ | Full |
| A-04 | Pipeline Security Agent | ✅ | Full |
| A-05 | Supply Chain Agent | ✅ | Full |
| A-06 | Security Plan Creator | ❌ | Not enough time (moved to 3-day) |
| A-07 | Build Custom Agent | ❌ | Not enough time (moved to 5-day) |
| A-08 | Agent Orchestration | ❌ | Not enough time (moved to 3-day) |
| R-01 | Fix Code Vulnerabilities | ✅ | Subset (1-2 fixes) |
| R-02 | Harden Terraform | ❌ | Not enough time (moved to 3-day) |
| R-03 | Harden Pipelines | ❌ | Not enough time (moved to 3-day) |
| R-04 | Compliance Mapping | ❌ | Not enough time (moved to 5-day) |

**Coverage:** 18 of 35 labs touched (51%), with 13 at full depth. All 5 non-planning agents exercised.

---

## Risk Mitigation

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| CodeQL scan takes too long during live demo | High (scans can take 10-20 min) | Delays Session 3 | Pre-trigger the scan 30 min before Session 3. Have pre-baked results ready. |
| Autofix suggestion not generated in time | Medium | Delays Session 6 | Pre-create a PR with Autofix suggestion already available on facilitator's fork. |
| MDC connector sync delay | High (initial sync takes hours) | No MDC data to show | Configure MDC connector 24+ hours before the event. Use facilitator's pre-configured environment. |
| Participant GitHub org doesn't have GHAS | Low (if pre-work is done) | Participant can't follow along | Provide a shared lab org with GHAS trial. Have backup fork in shared org ready. |
| Network connectivity issues | Low-Medium | All demos fail | Backup screenshots for every step. Consider offline fallback plan for critical demos. |
| Docker build fails on participant machine | Medium | Can't do container scanning | Provide pre-built image via shared ACR. Participants can pull instead of build. |

---

## Facilitator Notes

### Pacing
- Sessions 2-5 (morning) cover **detection** — finding vulnerabilities with automated tools
- Sessions 6-8 (afternoon) cover **remediation** — fixing vulnerabilities with AI assistance
- Session 9 (capstone) ties it all together in one continuous flow
- If running behind schedule, Session 4 (Supply Chain) can be compressed to demo-only (cut participant try time)
- Session 5 (Container + IaC) can drop the Kubernetes comparison if needed

### Transitions
Between each session, verbally bridge with one sentence:
- 2→3: "Secrets are caught at push time. Now let's see what CodeQL catches in the code itself."
- 3→4: "Our code is scanned. But what about the libraries our code depends on?"
- 4→5: "Dependencies are tracked. Now let's look at the container we ship and the infrastructure it runs on."
- 5→6: "We've found dozens of issues. Now let's see how AI can fix them for us."
- 6→7: "Autofix handles individual findings. Now let's see what specialized AI agents can do."
- 7→8: "We've used code and IaC agents. Let's check pipelines and supply chain, then see it all unified in MDC."
- 8→9: "Individual capabilities work. Now let's see the full loop — commit to cloud — in one flow."

### Key Talking Points per Session
| Session | Key Point to Emphasize |
|---------|----------------------|
| 2 (Secrets) | "Push protection shifts secret detection to the earliest possible moment — before the secret ever reaches the remote." |
| 3 (CodeQL) | "CodeQL doesn't just pattern match — it understands data flow. It can trace user input through function calls to a dangerous sink." |
| 4 (Supply Chain) | "You write 20% of your code. The other 80% comes from dependencies. SBOM gives you visibility into that 80%." |
| 5 (Container/IaC) | "Infrastructure misconfigurations are the #1 cause of cloud breaches. Scanning IaC catches them before deployment." |
| 6 (Autofix) | "GitHub data shows Autofix reduces median fix time from 28 minutes to under 2 minutes." |
| 7 (Agents) | "These agents bring security expertise on demand. Every developer gets a security reviewer sitting next to them." |
| 8 (MDC) | "MDC provides the code-to-cloud view — connecting a code vulnerability to the production resource it affects." |
| 9 (E2E) | "This entire loop — commit, detect, fix, verify, deploy — happened in under 30 minutes with zero manual security review." |
