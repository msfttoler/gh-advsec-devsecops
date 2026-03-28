# 5-Day Full Proof of Concept Delivery Guide

> **Format:** 40% hands-on labs / 30% customer code integration / 30% presentation + architecture + planning  
> **Duration:** 5 days × 8 hours (09:00–17:00) = 40 hours total  
> **Audience:** Full development team + security team + DevOps/platform team + executive sponsor (Day 5 PM)  
> **Delivery Model:** Instructor-led workshop with pair programming, customer-specific configuration, and executive readout  
> **Objective:** Complete validation of GHAS + GHCP + MDC against the customer's actual codebase, infrastructure, and CI/CD pipelines. End state: documented security posture improvement, custom agents tailored to the customer's domain, and a go/no-go recommendation with adoption roadmap.

---

## How This Builds on the 3-Day

Days 1–3 of the 5-Day PoC are the **complete 3-Day Light PoC** with targeted modifications to integrate customer code from Day 1 onward. Days 4 and 5 add entirely new content:

| Capability | 3-Day (Days 1–3) | Added in Day 4 | Added in Day 5 |
|-----------|-------------------|-----------------|-----------------|
| All GHAS capabilities | Full labs | Applied to customer code | Org-level configuration |
| All 6 agents | Full labs against reference repo | Applied to customer code | Custom agent creation |
| IaC remediation | Reference repo Terraform | Customer IaC scanned + remediated | — |
| Pipeline hardening | Reference repo workflows | Customer pipelines hardened | Integrated into customer CI/CD |
| MDC | Connection + dashboard + CSPM | Runtime threat detection (M-04) | — |
| DAST | ZAP against reference app | ZAP against customer app (if deployed) | — |
| Customer code | Not used | Full integration: scan, triage, remediate | — |
| Custom agents | Not covered | — | Build custom agent for customer domain |
| Org-level GHAS | Not covered | — | Full org configuration |
| Adoption roadmap | Not covered | — | Collaborative planning workshop |
| Executive readout | PoC summary document (written) | — | Live presentation to leadership |

---

## Pre-Engagement Requirements (Extended)

Everything from the 3-Day prerequisite list, plus:

### Customer-Specific Prerequisites (Required 2 Weeks Before)

| # | Prerequisite | Purpose | Owner |
|---|-------------|---------|-------|
| 1 | Customer identifies 1-3 representative repositories for scanning | Day 1-4 customer code integration | Customer |
| 2 | Customer provides (or grants access to) IaC templates (Terraform, Bicep, ARM, Helm) | IaC scanning against real infrastructure | Customer |
| 3 | Customer provides (or grants access to) CI/CD workflow files | Pipeline analysis and hardening | Customer |
| 4 | Customer identifies target compliance framework(s) (CIS, NIST, SOC 2, PCI-DSS, HIPAA) | Compliance mapping exercise | Customer |
| 5 | Customer's GitHub organization configured with GHAS trial or Enterprise license | Enables all GHAS features on customer repos | Customer + GitHub |
| 6 | Customer's Azure subscription accessible with Security Admin + Contributor roles | MDC integration, Defender plan enablement | Customer |
| 7 | Executive sponsor identified and available for Day 5 PM (15:00–16:30) | Executive readout presentation | Customer |
| 8 | Development team lead identified as primary technical contact | Day-to-day decision making during PoC | Customer |

### Pre-Engagement Call (1 Week Before)

| Agenda Item | Purpose |
|-------------|---------|
| Confirm repository access and GHAS licensing | Prevent Day 1 blockers |
| Review customer's current security tooling landscape | Understand what's being replaced/complemented |
| Identify customer's top security concerns and pain points | Tailor lab emphasis |
| Confirm compliance framework requirements | Prepare mapping worksheets |
| Confirm Azure subscription access and MDC availability | Prevent Day 4 blockers |
| Set executive readout expectations | Align on Day 5 deliverable format |

---

## Day 1 — Detection: Full GHAS Sweep + Initial Customer Code Onboarding

Day 1 is the 1-Day delivery (refer to `planning/1-day-demo-delivery.md`) with one modification: **Session 10 (16:40–17:00)** is replaced with customer code onboarding kickoff.

### Modified Session 10: Customer Code Onboarding Kickoff (16:40–17:00)

| Time | Activity | Detail |
|------|----------|--------|
| 16:40–16:50 | **Customer repo access verification** | Confirm facilitator and participants can access customer repositories. Verify GHAS is enabled (Code scanning, Secret scanning, Dependabot). If GHAS was just enabled today, initial scans may take overnight. |
| 16:50–17:00 | **Trigger initial customer code scans** | Enable CodeQL on the customer's primary repository. Configure the language matrix based on the customer's tech stack. Trigger the first scan. "These scans will run overnight. Tomorrow morning we'll review real findings from your code." |

**End of Day 1 State:** Same as 1-Day standalone (all GHAS capabilities + agents exercised against reference repo), plus customer code scans triggered and running overnight.

---

## Day 2 — Hardening: Infrastructure + Pipelines + DAST (with Customer Overlay)

Day 2 follows the 3-Day Day 2 structure (refer to `planning/3-day-light-poc-delivery.md`) with customer code integration woven in.

### Day 2 Modifications from 3-Day

| Time Block | 3-Day Content | 5-Day Modification |
|------------|---------------|-------------------|
| 09:00–09:30 | Day 1 recap + findings inventory | **Add:** Review overnight customer CodeQL results. Initial triage of customer findings by severity. |
| 13:15–14:15 | Deploy reference app + DAST | **Replace with:** Configure tfsec/KICS against customer's actual Terraform/Bicep/ARM. Run scans. Review and triage customer IaC findings. |
| 15:30–16:30 | Agent orchestration + security plan | **Replace with:** Pipeline Security Agent against customer's actual CI/CD workflows. Supply Chain Agent governance audit of customer repos. |

### Day 2 Modified Timeline (Detailed Deltas Only)

#### 09:00–09:30 — Day 1 Recap + Customer Scan Results (30 min)

| Time | Activity | Detail |
|------|----------|--------|
| 09:00–09:10 | Reference repo findings review | Quick count of findings from Day 1 (same as 3-Day). |
| 09:10–09:30 | **Customer CodeQL results review** | Navigate to customer's repository → Security → Code scanning. This is the first time the customer sees real findings from their own code. Walk through: total finding count, severity distribution, top CWE categories. Do NOT attempt to fix yet — just inventory. Create a shared findings inventory (spreadsheet or markdown) with columns: finding ID, severity, file, CWE, category. This becomes the "baseline" for measuring PoC progress. |

#### 13:15–14:15 — Customer IaC Scanning (60 min)

| Time | Activity | Detail |
|------|----------|--------|
| 13:15–13:35 | **Configure IaC scanning for customer** | Add tfsec and/or KICS workflow to the customer's repository (or a fork of it). Configure the path to scan based on where the customer's IaC lives. Trigger the scan. |
| 13:35–14:00 | **Customer IaC findings triage** | Review tfsec/KICS results against customer's real infrastructure code. Categorize: misconfigurations that are intentional (accepted risk) vs unintentional (real vulnerabilities). Discuss with customer team: "Is this SSH rule open intentionally for a bastion host, or is this a mistake?" This context is critical — automated scanners can't distinguish intentional from accidental. |
| 14:00–14:15 | **Prioritize customer IaC remediation** | Rank findings by: severity × exploitability × business impact. Select the top 5 for remediation on Day 3. |

#### 15:30–16:30 — Customer Pipeline & Supply Chain Analysis (60 min)

| Time | Activity | Detail |
|------|----------|--------|
| 15:30–16:00 | **Pipeline Security Agent on customer workflows** | Run `@pipeline-security-agent` against the customer's actual CI/CD workflow files. Review findings: unpinned actions, overly broad permissions, missing environment protections, potential script injection. Compare against the reference repo findings from Day 1 — customer workflows likely have similar patterns. |
| 16:00–16:30 | **Supply Chain Agent on customer repos** | Run `@supply-chain-security-agent` against the customer's repository. Review: dependency vulnerability inventory, secrets detection results, branch protection assessment, CODEOWNERS coverage, Dependabot configuration status. Create a governance gap list. |

**End of Day 2 State:** Same infrastructure hardening as 3-Day (Terraform, K8s, pipelines on reference repo), plus customer code scan baseline established, customer IaC findings triaged, customer pipeline/supply chain analysis complete.

---

## Day 3 — Remediation: Application Code + Customer Code Sprint

Day 3 follows the 3-Day Day 3 structure with customer code remediation replacing some reference repo work.

### Day 3 Modifications from 3-Day

| Time Block | 3-Day Content | 5-Day Modification |
|------------|---------------|-------------------|
| 09:30–10:45 | Fix all reference repo code vulns | **Split:** 09:30–10:00 fix reference repo (quick, 3 vulns), 10:00–10:45 begin customer code remediation |
| 13:15–14:15 | Compliance mapping (reference findings) | **Replace with:** Customer code remediation sprint (continued) |
| 14:30–15:30 | PoC results compilation | **Replace with:** Customer IaC remediation sprint |
| 15:30–16:30 | PoC readout (draft) | **Replace with:** Day 3 retrospective + Days 4-5 planning |

### Day 3 Modified Timeline (Detailed Deltas Only)

#### 09:30–10:45 — Code Remediation (75 min)

| Time | Activity | Detail |
|------|----------|--------|
| 09:30–10:00 | **Reference repo quick fixes** | Quickly fix the top 3 reference repo vulnerabilities (hardcoded creds, log forging, ReDoS) — participants have already identified these on Day 1. This serves as a warm-up before tackling customer code. |
| 10:00–10:45 | **Customer code remediation — Round 1** | Using the findings inventory from Day 2 morning, work on the top 5 customer CodeQL findings. For each: open the finding in the Security tab, trace to the file, use Copilot Autofix (if available), or use `@security-reviewer-agent` for remediation guidance. Create fix PRs. This is pair programming: facilitator + customer developer working together. The customer developer owns the fix; the facilitator guides the process. |

#### 13:15–14:15 — Customer Code Remediation Sprint (60 min)

| Time | Activity | Detail |
|------|----------|--------|
| 13:15–14:15 | **Customer code remediation — Round 2** | Continue working through customer findings. Target: remediate 10+ findings by end of Day 3. For complex findings, use the Security Reviewer Agent for analysis and Copilot Chat for fix suggestions. Track: finding ID, before state, after state, PR number. Re-run CodeQL on the customer repo to measure alert reduction. |

#### 14:30–15:30 — Customer IaC Remediation Sprint (60 min)

| Time | Activity | Detail |
|------|----------|--------|
| 14:30–14:45 | **Recall Day 2 IaC priorities** | Review the top 5 customer IaC findings selected for remediation on Day 2 afternoon. |
| 14:45–15:30 | **Fix customer IaC** | Work through the 5 prioritized IaC findings. For each: apply the fix in the Terraform/Bicep/ARM template, run tfsec/KICS locally to verify, commit and push. Use `@iac-security-agent` for guidance on complex fixes. |

#### 15:30–16:30 — Day 3 Retrospective + Days 4-5 Planning (60 min)

| Time | Activity | Detail |
|------|----------|--------|
| 15:30–15:50 | **Progress dashboard** | Compile metrics: reference repo findings (discovered vs remediated), customer code findings (discovered vs remediated), customer IaC findings (discovered vs remediated), pipeline findings (discovered vs remediated). Calculate overall remediation rate. |
| 15:50–16:10 | **Days 4-5 planning** | Review what remains: MDC integration with customer Azure environment, DAST against customer app (if deployed), remaining customer code findings, compliance mapping against customer's target framework, custom agent design, org-level GHAS configuration, adoption roadmap, executive readout. Prioritize with the customer team: which of these matters most? |
| 16:10–16:30 | **Homework assignment** | Customer team continues remediation overnight if motivated. Facilitator prepares: MDC connector configuration (if not done), compliance mapping worksheets for customer's target framework, custom agent draft based on customer's domain. |

**End of Day 3 State:** Reference repo fully remediated. Customer code partially remediated (10+ findings). Customer IaC partially remediated (5 findings). Customer pipelines analyzed. All scanning tools validated against real customer code.

---

## Day 4 — Integration: MDC, DAST, Runtime & Deep Customer Work

### Day 4 Theme

Days 1-3 operated primarily in the "code and build" phase. Day 4 extends into "deploy and run" — connecting everything to Azure runtime and MDC for the code-to-cloud security story. This is also the deepest customer integration day.

### Day 4 Detailed Timeline

#### 09:00–09:30 — Day 3 Recap + Runtime Context (30 min)

| Time | Activity | Detail |
|------|----------|--------|
| 09:00–09:15 | **Cumulative progress review** | Show the metrics dashboard: total findings across 3 days, remediation rate, category coverage. Highlight: "We've addressed the code and infrastructure layers. Today we connect to the runtime layer." |
| 09:15–09:30 | **Runtime security context** | Verbal context: why runtime monitoring matters even with shift-left practices. Not all vulnerabilities are detectable statically. Configuration drift, zero-days, behavioral anomalies require runtime detection. MDC provides this layer. |

---

#### 09:30–10:30 — MDC Full Integration (60 min)

**Lab References:** M-01 (customer environment), M-02 (customer data)

| Time | Activity | Detail |
|------|----------|--------|
| 09:30–09:50 | **Connect customer GitHub org to MDC (M-01)** | If not already done: navigate to Azure portal → Defender for Cloud → Environment settings → Add environment → GitHub. Authenticate with the customer's GitHub org admin credentials. Select repositories to connect (the same repos scanned on Days 1-3). Enable DevOps Security posture management. If already connected (facilitator pre-work), verify the sync is complete and findings are populated. |
| 09:50–10:10 | **Customer DevOps Security Dashboard (M-02)** | Navigate to MDC → DevOps Security. Review customer-specific data: repository finding counts from GHAS (these should match what participants discovered on Days 1-3), code-to-cloud mapping (if the customer has Azure resources connected), recommendations derived from GHAS findings, severity distribution. Compare MDC's view with the GitHub Security tab view — discuss: "Same data, different audience. GitHub is for developers. MDC is for security operations." |
| 10:10–10:30 | **CSPM + Secure Score (M-03)** | Navigate to Secure Score. Review the customer's current score (this uses the customer's actual Azure subscription data). Walk through recommendations by category. Show how DevOps findings contribute to the score. If Defender CSPM is enabled: show attack path analysis with customer's actual resources. Trace: code vulnerability → deployed resource → potential attack path. |

---

#### 10:30–10:45 — Break (15 min)

---

#### 10:45–12:00 — Deploy & DAST Customer Application (75 min)

**Lab References:** D-01, D-02, D-03 (adapted for customer)

| Time | Activity | Detail |
|------|----------|--------|
| 10:45–11:15 | **Deploy reference app for DAST baseline (D-01)** | Deploy `webapp01` to Azure using the Bicep blueprint (if not done on Day 2). This provides the DAST baseline against a known-vulnerable app. If already deployed from Day 2, skip to DAST. |
| 11:15–11:45 | **ZAP scan against reference app (D-02)** | Run the ZAP workflow against the deployed `webapp01`. Review SARIF findings in the Security tab. Walk through: the types of issues ZAP finds (missing security headers, cookie flags, information disclosure, XSS reflected). Compare with CodeQL findings — show the complementary coverage. |
| 11:45–12:00 | **Log forging attack demonstration (D-03)** | Live demonstration of the log forging vulnerability: craft a URL with CRLF injection payload, show the forged log entry, demonstrate how this could be used to cover tracks or inject false alerts. Discuss: this is a finding that CodeQL detected statically AND can be exploited dynamically. Show the before (vulnerable) and after (Day 3 fix) behavior. |

**Note:** If the customer has a deployed application in a non-production environment, the facilitator may run ZAP against it as well (with explicit written customer approval). This would provide DAST results against real customer code.

---

#### 12:00–13:00 — Lunch (60 min)

---

#### 13:00–14:00 — Runtime Threat Detection (60 min)

**Lab Reference:** M-04

| Time | Activity | Detail |
|------|----------|--------|
| 13:00–13:20 | **Enable Defender plans** | Navigate to MDC → Environment settings → Azure subscription. Enable: Defender for App Service (protects web apps), Defender for Containers (protects AKS/containers), Defender for Key Vault (detects unusual secret access), Defender for SQL (detects injection attempts). Explain each plan's detection capabilities and cost. |
| 13:20–13:40 | **Simulate suspicious activity** | Against the deployed reference app: generate multiple rapid requests to simulate brute force. Access unusual endpoints to trigger anomaly detection. Execute the log forging payload to see if Defender for App Service detects it. Against AKS (if deployed): run a privileged pod to trigger Defender for Containers alert. Attempt to mount a host path to trigger runtime protection. |
| 13:40–14:00 | **Review MDC alerts** | Navigate to MDC → Security alerts. Review any alerts generated by the simulation (alerts may take 15-30 minutes to appear — use facilitator's pre-staged alerts if needed). Walk through the alert: description, affected resource, recommended actions, MITRE ATT&CK mapping. Show the investigation workflow: alert → resource → activity log → remediation. |

**Success Criteria:**
- [ ] At least 2 Defender plans enabled on customer subscription
- [ ] Suspicious activity simulation executed
- [ ] MDC alert reviewed (live or pre-staged)

---

#### 14:00–14:15 — Break (15 min)

---

#### 14:15–15:15 — Customer Remediation Sprint — Final Push (60 min)

| Time | Activity | Detail |
|------|----------|--------|
| 14:15–14:45 | **Customer code: remaining high-severity findings** | Review the findings inventory. Target: close all CRITICAL and HIGH severity findings in customer code. Use Copilot Autofix for quick fixes, agents for complex analysis. Pair programming continues: facilitator guides, customer developer codes. |
| 14:45–15:15 | **Customer pipeline hardening** | Apply the pipeline security improvements identified on Day 2 to the customer's actual CI/CD workflows. Pin actions to SHA, scope permissions, add environment protection rules. Create PRs for each change. Verify workflows still pass after hardening. |

---

#### 15:15–16:30 — Compliance Mapping Workshop (75 min)

**Lab Reference:** R-04 (customer-specific)

| Time | Activity | Detail |
|------|----------|--------|
| 15:15–15:30 | **Framework selection** | Confirm the customer's target compliance framework(s) from the pre-engagement call. Distribute the appropriate mapping worksheet(s). |
| 15:30–16:15 | **Hands-on: Full compliance mapping** | Using ALL accumulated findings (SAST, SCA, secrets, container, IaC, DAST, pipeline, MDC), map each finding category to the target compliance controls. Work through the framework systematically: **CIS Azure Benchmark v2.0** (if selected): Identity & Access Management, Security Center, Storage Accounts, Database Services, Logging & Monitoring, Networking, Virtual Machines, Key Vault. **NIST 800-53 Rev 5** (if selected): Access Control (AC), Audit & Accountability (AU), Configuration Management (CM), Identification & Authentication (IA), Risk Assessment (RA), System & Communications Protection (SC), System & Information Integrity (SI). For each control: document which scanning tool provides evidence, whether the control is met/not met/partially met, what remediation action is needed for unmet controls. |
| 16:15–16:30 | **Compliance gap analysis** | Summarize: total controls in scope, controls with automated evidence, controls requiring manual processes, controls not yet addressed. This becomes a section of the executive readout. |

---

#### 16:30–17:00 — Day 4 Retrospective + Day 5 Prep (30 min)

| Time | Activity | Detail |
|------|----------|--------|
| 16:30–16:45 | **Metrics update** | Update the cumulative dashboard: customer findings discovered, customer findings remediated, IaC findings remediated, pipeline improvements applied, compliance controls mapped. |
| 16:45–17:00 | **Day 5 prep** | Preview Day 5: custom agent creation, org-level configuration, adoption roadmap, executive readout. Confirm executive sponsor availability for 15:00 on Day 5. Facilitator prepares overnight: draft executive readout deck, compile metrics, prepare custom agent template based on customer's domain needs. |

**End of Day 4 State:** MDC fully integrated with customer environment. Runtime detection demonstrated. Customer app deployed and DAST-scanned. CRITICAL/HIGH customer code findings remediated. Pipelines hardened. Compliance mapping complete. Ready for final day: custom agents, org configuration, and executive readout.

---

## Day 5 — Operationalize: Custom Agents, Org Config & Executive Readout

### Day 5 Theme

Days 1–4 proved the technology works. Day 5 answers the question: "How do we operationalize this across the organization?" This day shifts from technical proving to organizational planning.

### Day 5 Detailed Timeline

#### 09:00–09:30 — Day 4 Recap + Final Metrics (30 min)

| Time | Activity | Detail |
|------|----------|--------|
| 09:00–09:15 | **Final metrics compilation** | Update all dashboards one final time. Pull the definitive numbers: findings discovered (by tool, by category, by severity), findings remediated (count and percentage), time-to-remediate (Autofix vs manual vs agent-assisted), compliance control coverage. These numbers go directly into the executive readout. |
| 09:15–09:30 | **Day 5 objectives** | "Today we move from 'does this work?' to 'how do we roll this out?' By 4pm, we'll have: a custom agent tailored to your team, org-level GHAS configuration, an adoption roadmap, and a presentation for your leadership." |

---

#### 09:30–10:45 — Build Your Own Custom Agent (75 min)

**Lab Reference:** A-07

**Objective:** Design and build a custom Copilot agent tailored to the customer's specific security domain (e.g., compliance-specific scanner, API security reviewer, internal coding standards enforcer).

| Time | Activity | Detail |
|------|----------|--------|
| 09:30–09:50 | **Agent design workshop** | Discuss with the customer team: what security review task do you do repeatedly that could be automated? What expert knowledge does your senior security engineer have that you'd encode into an agent? Examples: "HIPAA compliance checker for our healthcare app", "PCI-DSS scope validator for our payment services", "Internal API security standard reviewer", "AWS-to-Azure migration security validator". Choose one agent to build. Define: agent name, description, scope (which files/directories), system prompt (what it should look for), output format (report structure), reference standards. |
| 09:50–10:30 | **Hands-on: Build the agent** | Create a new file in `.github/agents/` (e.g., `custom-compliance-agent.md`). Write the agent definition following the pattern of the existing 6 agents. Key sections: name/description, model selection, system prompt with domain-specific instructions, scope definition (include/exclude paths), output format specification, reference standards and severity classification. Test the agent: open VS Code Copilot Chat, select the new agent, run a representative prompt. Iterate on the system prompt based on the output quality. |
| 10:30–10:45 | **Agent validation** | Run the custom agent against both the reference repo and the customer's repo. Compare output: does it find domain-specific issues the other agents miss? Is the output actionable? Refine the prompt if needed. Commit the agent definition to the customer's repository. |

**Success Criteria:**
- [ ] Custom agent designed with customer-specific domain knowledge
- [ ] Agent definition file created and tested
- [ ] Agent produces actionable findings against customer code

---

#### 10:45–11:00 — Break (15 min)

---

#### 11:00–12:00 — Org-Level GHAS Configuration (60 min)

**Objective:** Configure GHAS at the organization level for enterprise-wide deployment, not just single-repo.

| Time | Activity | Detail |
|------|----------|--------|
| 11:00–11:20 | **Org-level security overview** | Navigate to GitHub org → Settings → Code security and analysis. Walk through the org-level controls: **Code scanning default setup** — enable CodeQL for all repositories, select default languages, choose query suite (security vs security-and-quality). **Secret scanning** — enable for all repositories, configure push protection at org level, enable validity checks. **Dependabot** — enable alerts for all repositories, enable security updates. Show the Security Overview dashboard: org-wide finding counts, risk distribution across repositories, coverage metrics. |
| 11:20–11:40 | **Hands-on: Configure org policies** | Apply org-level configurations to the customer's GitHub org (or demonstrate on a lab org). Enable code scanning defaults for all new repos. Enable secret scanning push protection org-wide. Configure custom secret patterns at org level. Set up required workflows: create a reusable security scanning workflow that all repos must run. Configure org-level security policies via `security-analysis` settings. |
| 11:40–12:00 | **CI/CD agent orchestration for the org** | Adapt the `security-agent-workflow.yml` pattern for the customer's CI/CD. Discuss: which agent(s) should run on every PR? Which should run on every push to main? Which should run on a schedule (weekly/monthly audit)? Create a reusable workflow that other repos can call. Discuss gating: should agent findings block deployment? What severity threshold? |

**Success Criteria:**
- [ ] Org-level code scanning defaults configured
- [ ] Secret scanning push protection enabled org-wide
- [ ] Security Overview dashboard reviewed
- [ ] Agent integration into CI/CD pipeline planned

---

#### 12:00–13:00 — Lunch (60 min)

---

#### 13:00–14:00 — Adoption Roadmap Workshop (60 min)

**Objective:** Collaboratively build a phased adoption roadmap for rolling out GHAS + GHCP + MDC across the customer's organization.

| Time | Activity | Detail |
|------|----------|--------|
| 13:00–13:15 | **Current state summary** | Review the cumulative PoC results one more time. Summarize: what works, what the customer team liked, what concerns remain, what technical gaps exist. |
| 13:15–13:45 | **Hands-on: Build the roadmap** | Using the adoption roadmap template, collaboratively define: |

**Phase 1 — Pilot (Suggested scope)**
- Select 2-3 repositories for initial rollout
- Enable: CodeQL, Secret Scanning + Push Protection, Dependabot
- Deploy: 2-3 custom agents (Security Agent, Security Reviewer, one customer-specific)
- Connect: GitHub to MDC for unified visibility
- Success metric: 80% of CRITICAL/HIGH findings remediated within defined period
- Team: security champion + 2-3 developers

**Phase 2 — Team Expansion (Suggested scope)**
- Expand to all repositories owned by the pilot team
- Add: IaC scanning (tfsec/KICS) to CI/CD for all IaC repos
- Add: Container scanning (Trivy) to CI/CD for all container builds
- Deploy: Pipeline Security Agent on all workflows
- Enable: Security Campaigns for batch remediation
- Success metric: <5 new CRITICAL findings introduced per period

**Phase 3 — Org-Wide Rollout (Suggested scope)**
- Enable org-level GHAS defaults for all repositories
- Mandate: secret scanning push protection for all repos
- Deploy: required security scanning workflow for all repos
- Enable: Defender CSPM + Defender for App Service across all Azure subscriptions
- Implement: compliance reporting dashboard (MDC + GHAS)
- Success metric: Org Secure Score ≥ target

**Phase 4 — Maturity (Suggested scope)**
- Advanced: custom agents per team/domain
- Advanced: Security Campaigns for quarterly security debt reduction
- Advanced: SBOM generation + artifact attestation for all releases
- Integration: GHAS findings → ticketing system (Jira/ADO)
- Integration: MDC → SIEM (Sentinel)
- Success metric: Mean time to remediate < target for all severity levels

| Time | Activity | Detail |
|------|----------|--------|
| 13:45–14:00 | **Roadmap review and commitment** | Review the draft roadmap with the customer team. Identify: blockers (licensing, budget, team capacity), dependencies (Azure subscription changes, GitHub org changes), quick wins (can be done next week). Assign owners for Phase 1 actions. |

---

#### 14:00–14:15 — Break (15 min)

---

#### 14:15–15:00 — Executive Readout Preparation (45 min)

| Time | Activity | Detail |
|------|----------|--------|
| 14:15–14:35 | **Compile the readout document** | Using the PoC results template, finalize: **Executive Summary** (3-4 sentences): what was tested, key finding, recommendation. **Scope** : repositories scanned, tools used, days invested. **Findings Summary Table**: category, tool, findings discovered, findings remediated, remaining. **Key Metrics**: total findings, remediation rate, time saved with Autofix (estimate), compliance coverage percentage. **Before/After Comparison**: security posture delta (CodeQL alert count, Dependabot alert count, IaC finding count, Secure Score change). **Agent Value Assessment**: unique findings from agents, agent response quality rating, most valuable agent. **Compliance Posture**: controls mapped, coverage percentage, gaps. **Risk Assessment**: remaining unaddressed findings by severity. **Recommendation**: adopt/don't adopt, which capabilities to prioritize. **Adoption Roadmap**: summary of the 4-phase plan from the morning workshop. |
| 14:35–14:50 | **Prepare visual aids** | Pull screenshots: Security tab before/after, MDC dashboard, agent output example, Autofix PR example. Create a simple metric visualization (could be a markdown table or a quick chart). |
| 14:50–15:00 | **Dry run** | Quick verbal walkthrough of the readout flow. Identify: who presents which section (facilitator vs customer technical lead). Confirm timing: 30 min presentation + 15 min Q&A. |

---

#### 15:00–16:15 — Executive Readout (75 min)

| Time | Activity | Detail |
|------|----------|--------|
| 15:00–15:05 | **Welcome + context** | Facilitator welcomes executive sponsor. Brief context: "Over the past 5 days, your team has evaluated GitHub Advanced Security, GitHub Copilot agents, and Microsoft Defender for Cloud against your actual codebase and infrastructure." |
| 15:05–15:15 | **What we tested** | Customer technical lead presents: scope (repos, IaC, pipelines), tools used, approach (reference repo first, then customer code). |
| 15:15–15:30 | **Key findings** | Facilitator presents: findings summary table, before/after security posture, most impactful discoveries. Lead with the strongest data point (e.g., "We discovered X CRITICAL findings in your production code that existing tools had not detected"). |
| 15:30–15:40 | **AI agent value** | Customer developer demonstrates: one agent prompt live, show the output, compare with manual review effort. Key metric: "Agent-assisted remediation took X minutes vs Y minutes manually." |
| 15:40–15:50 | **Code-to-cloud story** | Facilitator shows MDC: trace a finding from code → build → deploy → runtime. Show the Secure Score contribution. Show the attack path analysis (if available). |
| 15:50–16:00 | **Recommendation + roadmap** | Facilitator presents: go/no-go recommendation, phased adoption roadmap summary, Phase 1 quick wins, estimated licensing costs, estimated team effort. |
| 16:00–16:15 | **Q&A** | Open floor for executive questions. Common questions to prepare for: "What's the total cost?" → Have licensing estimates ready. "How long to full rollout?" → Reference the 4-phase roadmap. "What happens to our existing security tools?" → Discuss complementary vs replacement positioning. "What's the developer experience impact?" → Reference time-to-remediate metrics and Autofix data. |

---

#### 16:15–16:45 — Closeout & Handoffs (30 min)

| Time | Activity | Detail |
|------|----------|--------|
| 16:15–16:25 | **Deliverable handoffs** | Share with the customer: PoC results document (finalized), adoption roadmap (finalized), custom agent definition file, compliance mapping worksheets (completed), all lab guides and reference materials, fork of the reference repository (participants keep their forks), facilitator contact information for follow-up. |
| 16:25–16:35 | **Action items** | Document and assign: Phase 1 pilot start actions (who, what, by when), licensing procurement next steps, follow-up meeting date (recommend a checkpoint after the first defined period), escalation contacts at GitHub/Microsoft for technical issues. |
| 16:35–16:45 | **Feedback + thank you** | Collect NPS scores or feedback survey. Thank the team. Close. |

---

## 5-Day Cumulative Lab Coverage

| Lab ID | Title | Day 1 | Day 2 | Day 3 | Day 4 | Day 5 | Depth |
|--------|-------|-------|-------|-------|-------|-------|-------|
| F-01 | Environment Setup | ✅ | — | — | — | — | Verification |
| F-02 | Repo Walkthrough | ✅ | — | — | — | — | Embedded |
| F-03 | Azure Environment | — | — | — | ✅ | — | Full (for MDC/DAST) |
| G-01 | Secret Scanning | ✅ | — | — | — | — | Full |
| G-02 | CodeQL | ✅ | — | — | — | — | Full |
| G-03 | Dependency Review | ✅ | — | — | — | — | Full |
| G-04 | SBOM | ✅ | — | — | — | — | Demo + try |
| G-05 | Scorecard | ✅ | — | — | — | — | Demo + review |
| G-06 | Container Scanning | ✅ | — | — | — | — | Full |
| G-07 | Copilot Autofix | ✅ | — | ✅ | — | — | Full + multi-language |
| G-08 | Security Campaigns | — | — | ✅ | — | — | Demo or hands-on |
| I-01 | tfsec | ✅ | ✅ | — | — | — | Findings → remediation |
| I-02 | KICS | — | ✅ | — | — | — | Comparison |
| I-03 | MSDO | — | ✅ | — | — | — | Full |
| I-04 | Kubesec | ✅ | ✅ | — | — | — | Findings → remediation |
| I-05 | Bicep Review | — | — | — | — | — | Covered by A-03 |
| D-01 | Deploy App | — | — | — | ✅ | — | Full |
| D-02 | ZAP Scan | — | — | — | ✅ | — | Full |
| D-03 | Log Forging | — | — | — | ✅ | — | Full |
| M-01 | MDC Connection | ✅ | ✅ | — | ✅ | — | Demo → hands-on → customer |
| M-02 | MDC Dashboard | ✅ | ✅ | — | ✅ | — | Demo → hands-on → customer |
| M-03 | CSPM | — | — | — | ✅ | — | Full |
| M-04 | Runtime Detection | — | — | — | ✅ | — | Full |
| A-01 | Security Agent | ✅ | — | — | — | — | Full |
| A-02 | Security Reviewer | ✅ | — | — | — | — | Full |
| A-03 | IaC Agent | ✅ | — | — | — | — | Full |
| A-04 | Pipeline Agent | ✅ | — | — | — | — | Full |
| A-05 | Supply Chain Agent | ✅ | — | — | — | — | Full |
| A-06 | Security Plan Creator | — | ✅ | — | — | — | Full |
| A-07 | Build Custom Agent | — | — | — | — | ✅ | Full |
| A-08 | Agent Orchestration | — | ✅ | — | — | ✅ | Full → org-level |
| R-01 | Fix Code Vulns | ✅ | — | ✅ | ✅ | — | Subset → full → customer |
| R-02 | Harden Terraform | — | ✅ | ✅ | — | — | Reference → customer |
| R-03 | Harden Pipelines | — | ✅ | — | ✅ | — | Reference → customer |
| R-04 | Compliance Mapping | — | — | — | ✅ | — | Full (customer framework) |

**Coverage:** 35 of 35 labs (100%). Every lab exercised at least once.

---

## 5-Day Cumulative Outcomes

| Metric | Target |
|--------|--------|
| Reference repo findings discovered | 50+ |
| Reference repo findings remediated | 40+ (80%+) |
| Customer code findings discovered | Varies (baseline established) |
| Customer code findings remediated | CRITICAL: 100%, HIGH: 80%+ |
| Customer IaC findings remediated | Top 5+ |
| Customer pipelines hardened | All primary workflows |
| Tools exercised | All 12+ (CodeQL, tfsec, KICS, MSDO, Trivy, Grype, ZAP, Kubesec, Syft, Scorecard, MDC, custom agents) |
| Agents used | All 6 + 1 custom-built |
| Compliance controls mapped | Full framework coverage for customer's target |
| Custom agent delivered | 1 domain-specific agent |
| MDC integration | GitHub connector + Defender plans + CSPM |
| Deliverables | PoC results document, adoption roadmap, custom agent, compliance mapping, executive readout |
| Executive readout | Presented to leadership with recommendation |

---

## Risk Mitigation (5-Day Specific)

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Customer code scan produces overwhelming finding count | High | Paralysis / discouragement | Pre-triage: focus on CRITICAL/HIGH only. Frame: "These are findings your current tools missed — that's the value." |
| Customer developers defensive about findings in their code | Medium | Resistance / disengagement | Frame all findings as "improvement opportunities, not blame." Emphasize: "These are patterns that exist in every codebase." |
| Customer IaC is in a non-supported format | Low | IaC labs less relevant | Adapt: use the reference repo Terraform as primary, discuss how the customer's format would be scanned. |
| Executive sponsor cancels Day 5 attendance | Medium | No readout audience | Prepare a written readout document that can be sent async. Schedule a follow-up virtual presentation. |
| MDC connector sync incomplete by Day 4 | Medium | Limited MDC demo | Configure connector on Day 1 (not Day 4). Use facilitator's pre-configured environment as backup. |
| Autofix not available for customer's language | Low-Medium | Reduced AI value demo | Lean on agent-assisted remediation via Copilot Chat instead. Autofix coverage is expanding — note which languages are covered. |
| Customer's Azure subscription has policy restrictions | Medium | Can't enable Defender plans | Identify restrictions in pre-engagement call. Work with customer's cloud admin to create exemptions or use a lab subscription. |

---

## Facilitator Preparation Timeline

| When | Action |
|------|--------|
| **2 weeks before** | Pre-engagement call. Confirm all prerequisites. Request customer repo access. |
| **1 week before** | Verify GHAS licensing on customer org. Test MDC connector setup. Prepare compliance mapping worksheets for customer's target framework. |
| **3 days before** | Send participant setup checklist. Verify Azure subscriptions. Pre-deploy facilitator demo environment. |
| **1 day before** | Final environment check. Pre-trigger CodeQL on customer repos (if permitted). Configure MDC connector (needs 24+ hours to sync). Prepare backup screenshots. |
| **Each evening (Days 1–4)** | Review day's progress. Adjust next day's emphasis based on what worked/struggled. Prepare any additional materials needed. |
