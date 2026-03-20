# VBD Expansion Plan: Agentic DevSecOps with GHAS + GHCP + MDC

> **Document Purpose:** Identify where to add demos and hands-on lab scenarios for the `gh-advsec-devsecops` repository and GHAS+MDC presentation materials. Define content required for a 1-Day Demo, 3-Day Light Proof of Concept, and 5-Day Full Proof of Concept.
>
> **Repository:** `devopsabcs-engineering/gh-advsec-devsecops`  
> **Presentation Deck:** TT343 — Agentic AI for DevSecOps: Transforming Security with GHAS and GHCP (95 slides)

---

## Table of Contents

1. [Current State Assessment](#1-current-state-assessment)
2. [Gap Analysis](#2-gap-analysis)
3. [Proposed Lab & Demo Catalog](#3-proposed-lab--demo-catalog)
4. [Tier 1 — 1-Day Executive Demo](#4-tier-1--1-day-executive-demo)
5. [Tier 2 — 3-Day Light Proof of Concept](#5-tier-2--3-day-light-proof-of-concept)
6. [Tier 3 — 5-Day Full Proof of Concept](#6-tier-3--5-day-full-proof-of-concept)
7. [Content Creation Backlog](#7-content-creation-backlog)
8. [Prerequisites & Environment Requirements](#8-prerequisites--environment-requirements)
9. [Mapping: PPTX Slides to Labs](#9-mapping-pptx-slides-to-labs)
10. [Open Questions & Decisions Required](#10-open-questions--decisions-required)

---

## 1. Current State Assessment

### 1.1 Repository Assets Inventory

| Category | Asset | Status | Lab-Ready? |
|----------|-------|--------|------------|
| **Application** | ASP.NET 9.0 Razor Pages app (`src/webapp01`) | ✅ Complete | Partially — contains 7+ intentional vulnerabilities (hardcoded creds, log forging, ReDoS, insecure deserialization, command injection, secret exposure) |
| **IaC — Terraform** | 15 Terraform files (`terraform/azure/`) | ✅ Complete | Yes — 20+ intentional misconfigurations (open NSGs, disabled encryption, weak passwords, RBAC disabled on AKS, Security Center on Free tier) |
| **IaC — Bicep** | 2 blueprint sets (`blueprints/`) | ✅ Complete | Partially — `gh-aspnet-webapp` is secure pattern, `sample-web-app` is 3-tier reference. No intentionally vulnerable Bicep exists |
| **Kubernetes** | 2 manifests (`manifests/`) | ✅ Complete | Yes — `critical-double.yaml` (privileged pod) vs `score-5-pod-serviceaccount.yaml` (secure pod) |
| **Multi-language Samples** | Python, JS, Go, Dockerfile, ARM, Terraform samples (`samples/`) | ✅ Complete | Yes — intentionally vulnerable code for Bandit, ESLint, gosec, Checkov scanning |
| **CI/CD Workflows** | 17 GitHub Actions workflows (`.github/workflows/`) | ✅ Complete | Yes — covers SAST (CodeQL), SCA (Dependency Review, Syft, Microsoft SBOM, Scorecard), Container (Trivy, Grype), IaC (tfsec, KICS, MSDO), DAST (ZAP) |
| **Custom Copilot Agents** | 6 agents (`.github/agents/`) | ✅ Complete | Partially — agents exist but no step-by-step lab guides for using them |
| **Security Plans** | 2 generated plans (`security-plan-outputs/`) | ✅ Complete | Yes — demonstrate agent output for `gh-aspnet-webapp` and `sample-web-app` blueprints |
| **Spec** | 1 demo spec (`specs/devsecops-new-feature-demo.md`) | ✅ Complete | Partially — instructions only, no facilitator guide or success criteria |
| **Templates** | Security plan template (`docs/templates/`) | ✅ Complete | Yes — defines standard structure for security plan generation |

### 1.2 PPTX Presentation Structure (95 Slides)

The main deck (TT343) follows this flow, which maps to potential lab insertion points:

| Slide Range | Section | Content | Lab Opportunity |
|-------------|---------|---------|-----------------|
| 1–6 | Introduction | Title, speakers, audience polling | None (context setting) |
| 7–21 | Why DevSecOps & GHAS Matter | Industry standards (NIST SSDF, SLSA, CRA), threat landscape, shift-left economics, DevSecOps barriers, shared responsibility, SDLC overview | **Whiteboard/discussion exercise** |
| 22–53 | Deep Dive: GHAS & Secure Across the Stack | Secret scanning, code scanning, dependency review, SBOM, Scorecard, container scanning, IaC scanning, pipeline security, 3rd-party integration | **Primary lab insertion zone** — each GHAS capability maps to a hands-on exercise |
| 54–61 | Eradicating Security Debt | Security Campaigns, Copilot Autofix, time-saved metrics | **Autofix demo** |
| 62–70 | Secure Across the Stack (Azure + GitHub) | MDC overview, MDC DevOps Security, Defender + GitHub unified view | **MDC integration demo** |
| 71–84 | Agentic AI for DevSecOps | Agentic workflows, custom agents, agent catalog, DevSecOps guidelines | **Agent hands-on labs** |
| 85–88 | Demo | Live demo slides, screenshots of GitHub + MDC + Agents | **Expand to structured lab** |
| 89–95 | Wrap-up | Takeaways, resources, feedback, appendix | None (closing) |

### 1.3 Second PPTX (TT343.pptx — 85 Slides)

This is the MCAPS Tech Connect 2026 variant of the same deck. It shares the same core content with 10 fewer slides. The expansion plan should use the 95-slide version as the canonical reference and note where the 85-slide version diverges.

### 1.4 Supporting Documents

| File | Content | Usability |
|------|---------|-----------|
| `Agentic DevSecOps with GitHub Advanced Security.docx` | 2-page overview (340 words) by Todd Toler | Could serve as executive handout or session abstract |
| `TT343.docx` | Password-protected; contents unknown | Needs password to evaluate; likely speaker notes |

---

## 2. Gap Analysis

### 2.1 What Exists vs. What's Needed

| Need | Current State | Gap |
|------|---------------|-----|
| **Lab Guides** — step-by-step facilitator + participant instructions | 1 spec file (`devsecops-new-feature-demo.md`) with 11 bullet points | No structured lab guides with prerequisites, steps, expected outputs, success criteria, troubleshooting |
| **Participant Workbooks** — handouts for attendees | None | No printable/shareable participant materials |
| **Facilitator Guide** — timing, talking points, transitions | None | No delivery script mapping slides to labs |
| **Environment Setup Automation** — scripts to provision lab environments | Terraform + Bicep exist but are designed as scan targets, not lab provisioning | Need setup/teardown scripts, ARM/Bicep for lab environment (GHAS-enabled repo, Azure resources, MDC workspace) |
| **Before/After Remediation Examples** — show fixing vulnerabilities | Vulnerable code exists; no fixed versions alongside | Need paired examples: vulnerable → remediated for each vulnerability category |
| **MDC Integration Lab** — connecting GHAS findings to Defender for Cloud | No MDC configuration or integration code | Need walkthrough for connecting GitHub connector to MDC, viewing unified security posture |
| **Copilot Autofix Lab** — demonstrating AI-powered remediation | Slide content only (slides 54–61) | Need lab with specific PR triggering Autofix suggestions |
| **Security Campaigns Lab** — bulk remediation workflow | Slide content only (slide 56) | Need lab demonstrating campaign creation, alert triage, bulk fix flow |
| **Agent Lab Guides** — step-by-step for each of the 6 agents | Agent definitions exist; no usage guides | Need per-agent lab guide with sample prompts, expected outputs, evaluation criteria |
| **Assessment/Scoring Rubric** — measure participant outcomes | None | Need rubric for PoC success criteria and customer readiness scoring |
| **DAST Lab with Local App** — dynamic testing against the actual webapp | ZAP workflow targets external Juice Shop app, not the repo's own webapp | Need lab deploying `webapp01` and running ZAP against it |
| **Compliance Mapping Exercise** — mapping findings to frameworks | Security plan template exists; no structured exercise | Need guided exercise mapping scan results to CIS/NIST/SOC2 controls |
| **Multi-Repo / Org-Level Demo** — GHAS at scale | Single-repo focus only | Need guidance on demonstrating org-level security overview, code scanning defaults, secret scanning at org level |

### 2.2 Presentation Gaps

| Slide Range | Presentation Gap | Recommended Addition |
|-------------|-----------------|----------------------|
| 85–88 | Demo section has 4 slides with screenshots but no structure | Replace with structured demo runbook referencing specific labs |
| 62–70 | MDC section is conceptual; no live walkthrough | Add MDC portal walkthrough lab guide |
| 71–84 | Agent section describes agents but doesn't show creation workflow | Add "Build Your Own Agent" lab |
| 22–53 | GHAS deep dive has slides per capability but no pause points for labs | Mark specific slides as "Lab Break" insertion points |
| 94–95 | Appendix and resources are minimal | Expand with lab links, repo fork instructions, further reading |

---

## 3. Proposed Lab & Demo Catalog

Each lab is assigned an ID, difficulty level, and estimated duration. Labs are composable across the three delivery tiers.

### 3.1 Foundation Labs (Setup & Orientation)

| Lab ID | Title | Duration | Difficulty | Description |
|--------|-------|----------|------------|-------------|
| **F-01** | Environment Setup & Repo Fork | 30 min | Beginner | Fork the repo, enable GHAS features (Code Scanning, Secret Scanning, Dependabot), verify workflows run. Confirm VS Code + Copilot Chat extension installed. |
| **F-02** | Repository Walkthrough | 20 min | Beginner | Guided tour of repo structure: application code, IaC, manifests, samples, workflows, agents. Identify intentional vulnerabilities without fixing them. |
| **F-03** | Azure Environment Provisioning | 45 min | Intermediate | Deploy lab Azure resources using provided Bicep/ARM templates: Resource Group, ACR, App Service, SQL Database, Key Vault, AKS cluster. |

### 3.2 GHAS Capability Labs

| Lab ID | Title | Duration | Difficulty | Maps to Slides | Description |
|--------|-------|----------|------------|----------------|-------------|
| **G-01** | Secret Scanning & Push Protection | 30 min | Beginner | 31–37 | Enable secret scanning + push protection. Attempt to commit a secret (simulated token). Observe block. Review alerts dashboard. Configure custom patterns. |
| **G-02** | Code Scanning with CodeQL | 45 min | Intermediate | 38–39, 43 | Trigger CodeQL workflow. Review findings in Security tab. Understand severity levels. Examine `DevSecOps.cshtml.cs` findings (log forging, ReDoS, hardcoded creds). |
| **G-03** | Dependency Review & Dependabot | 30 min | Beginner | 40 | Create a PR adding a vulnerable dependency (`Newtonsoft.Json 12.0.2`). Observe Dependency Review action block the PR. Review Dependabot alerts and create a fix PR. |
| **G-04** | SBOM Generation | 20 min | Beginner | 41 | Run both SBOM workflows (Microsoft SBOM Tool + Anchore Syft). Compare outputs. Discuss SPDX vs CycloneDX formats. View dependency graph. |
| **G-05** | OpenSSF Scorecard | 20 min | Beginner | 42 | Run Scorecard workflow. Review score dimensions (branch protection, CI tests, vulnerabilities, pinned dependencies). Identify improvement actions. |
| **G-06** | Container Image Scanning | 30 min | Intermediate | 44–45 | Build Docker image for `webapp01`. Run Trivy and Grype scans. Compare findings. Discuss base image selection and rootless containers. |
| **G-07** | Copilot Autofix for Code Scanning | 30 min | Intermediate | 57–61 | Create a PR introducing a known vulnerability. Wait for CodeQL + Autofix suggestion. Review, modify, and merge the AI-generated fix. |
| **G-08** | Security Campaigns | 30 min | Intermediate | 54–56 | (Requires GHAS Enterprise) Create a Security Campaign targeting multiple alerts. Assign to developer. Demonstrate bulk Autofix generation. Track campaign progress. |

### 3.3 IaC Security Labs

| Lab ID | Title | Duration | Difficulty | Maps to Slides | Description |
|--------|-------|----------|------------|----------------|-------------|
| **I-01** | Terraform Scanning with tfsec | 30 min | Intermediate | 48 | Run tfsec against `terraform/azure/`. Review findings: open NSGs, hardcoded passwords, disabled encryption. Remediate 3 findings and re-scan. |
| **I-02** | IaC Scanning with KICS | 20 min | Intermediate | 48 | Run KICS workflow. Compare findings with tfsec. Discuss tool complementarity. |
| **I-03** | Microsoft Security DevOps (MSDO) for IaC | 30 min | Intermediate | 48 | Run MSDO workflow (Checkov + TemplateAnalyzer). Review multi-tool consolidated results. Map findings to CIS Azure Benchmark controls. |
| **I-04** | Kubernetes Manifest Security | 20 min | Beginner | 45, 51 | Run Kubesec against both manifests. Compare scores. Fix `critical-double.yaml` to achieve score ≥5. Re-scan to verify. |
| **I-05** | Bicep Security Review | 30 min | Intermediate | 48 | Use IaC Security Agent to review `blueprints/` Bicep files. Compare agent findings against MSDO results. Discuss managed identity as secure pattern. |

### 3.4 DAST & Runtime Labs

| Lab ID | Title | Duration | Difficulty | Maps to Slides | Description |
|--------|-------|----------|------------|----------------|-------------|
| **D-01** | Deploy Vulnerable App to Azure | 45 min | Intermediate | 85–86 | Deploy `webapp01` using the `gh-aspnet-webapp` blueprint. Verify app is running. Access the app and exercise vulnerable endpoints. |
| **D-02** | OWASP ZAP Scan Against Deployed App | 30 min | Intermediate | 43 | Configure and run ZAP workflow against the deployed `webapp01` (replacing the Juice Shop target). Review SARIF output in Security tab. |
| **D-03** | Log Forging Attack Demonstration | 20 min | Intermediate | N/A (new) | Exercise the log forging vulnerability in `DevSecOps.cshtml.cs` via URL parameter manipulation. Demonstrate CRLF injection in application logs. Discuss detection and remediation. |

### 3.5 MDC Integration Labs

| Lab ID | Title | Duration | Difficulty | Maps to Slides | Description |
|--------|-------|----------|------------|----------------|-------------|
| **M-01** | Connect GitHub to Microsoft Defender for Cloud | 30 min | Intermediate | 67–70 | Configure GitHub connector in MDC. Enable DevOps Security posture. View unified security findings from GHAS + Azure resources. |
| **M-02** | MDC DevOps Security Dashboard | 20 min | Beginner | 69 | Navigate MDC portal: DevOps Security blade, code-to-cloud mapping, resource health, recommendations prioritization. |
| **M-03** | Defender for Cloud Security Posture (CSPM) | 45 min | Intermediate | 67–68 | Enable Defender CSPM. Review Secure Score. Explore attack path analysis. Map cloud misconfigurations to code-level fixes. |
| **M-04** | Runtime Threat Detection with MDC | 30 min | Advanced | 52, 70 | Enable Defender for App Service and Defender for Containers. Deploy vulnerable app. Simulate suspicious activity. Review MDC alerts. |

### 3.6 Agentic AI Labs

| Lab ID | Title | Duration | Difficulty | Maps to Slides | Description |
|--------|-------|----------|------------|----------------|-------------|
| **A-01** | Security Agent — Full Repository Assessment | 30 min | Beginner | 80–82 | Open VS Code Copilot Chat. Select Security Agent. Run: "Perform a security review of this repository." Review generated report. Compare against CodeQL findings. |
| **A-02** | Security Reviewer Agent — Code-Level Vulnerabilities | 30 min | Intermediate | 80 | Select Security Reviewer Agent. Target specific files (`DevSecOps.cshtml.cs`, `Index.cshtml.cs`). Review OWASP-mapped findings. Discuss severity accuracy. |
| **A-03** | IaC Security Agent — Terraform Deep Scan | 30 min | Intermediate | 80 | Select IaC Security Agent. Scan `terraform/azure/`. Compare agent findings with tfsec/KICS results. Evaluate compliance mapping (CIS, NIST). |
| **A-04** | Pipeline Security Agent — Workflow Hardening | 30 min | Intermediate | 80 | Select Pipeline Security Agent. Analyze `.github/workflows/`. Get specific hardening recommendations (SHA pinning, permission scoping, secret handling). Apply 3 fixes. |
| **A-05** | Supply Chain Security Agent — Governance Audit | 30 min | Intermediate | 80 | Select Supply Chain Agent. Run governance audit. Review: secrets exposure, dependency vulnerabilities, branch protection, CODEOWNERS coverage. Implement 3 recommendations. |
| **A-06** | Security Plan Creator Agent — Threat Modeling | 45 min | Advanced | 80 | Select Security Plan Creator Agent. Choose a blueprint. Walk through all 5 phases: blueprint selection → architecture analysis → threat assessment → plan generation → validation. Compare output against existing security plans. |
| **A-07** | Build Your Own Custom Agent | 60 min | Advanced | 73, 80 | Create a new custom Copilot agent (e.g., "Compliance Mapping Agent" or "API Security Agent"). Define system prompt, capabilities, and output format. Test against the repo. |
| **A-08** | Agent Orchestration via Workflow | 20 min | Intermediate | 80 | Review and run the `security-agent-workflow.yml`. Understand how agents can be automated in CI/CD. Examine the critical vulnerability check gate. |

### 3.7 Remediation & Best Practices Labs

| Lab ID | Title | Duration | Difficulty | Description |
|--------|-------|----------|------------|-------------|
| **R-01** | Fix the App — Remediate All Code Vulnerabilities | 60 min | Intermediate | Working from CodeQL + agent findings, fix all 7+ vulnerabilities in `webapp01`. Validate fixes with re-scan. Topics: parameterized queries, safe regex, secure deserialization, log sanitization, secret management. |
| **R-02** | Harden the Infrastructure — Fix Terraform Misconfigurations | 45 min | Intermediate | Remediate the top 10 Terraform findings: enable encryption, restrict NSGs, fix passwords, enable RBAC on AKS, upgrade Security Center tier. Re-scan with tfsec. |
| **R-03** | Secure the Pipeline — Workflow Hardening | 30 min | Intermediate | Pin all actions to SHA, scope permissions with least privilege, add environment protection rules, enable required status checks. |
| **R-04** | Compliance Mapping Exercise | 45 min | Advanced | Take consolidated scan findings and map them to: CIS Azure Benchmark v2.0, NIST 800-53 Rev 5, SOC 2 Type II controls. Use the security plan template. |

---

## 4. Tier 1 — 1-Day Executive Demo

**Audience:** Security leaders, engineering managers, CISOs, architecture decision makers  
**Objective:** Demonstrate the full breadth of GHAS + GHCP + MDC capabilities with a focus on business value, AI-powered automation, and developer experience  
**Format:** Presentation-led with embedded live demos (no participant hands-on)  
**Delivery Model:** Presenter drives all demos; attendees observe and ask questions

### 4.1 Agenda

| Time Block | Duration | Activity | Content Source | Labs/Demos Used |
|------------|----------|----------|----------------|-----------------|
| 09:00–09:30 | 30 min | **Welcome & Setting the Stage** | Slides 1–21 | Discussion: audience security maturity polling (slide 6) |
| 09:30–10:15 | 45 min | **GHAS Deep Dive — Prevent** | Slides 22–37 | **Demo G-01** (Secret scanning push protection — live) |
| 10:15–10:30 | 15 min | Break | — | — |
| 10:30–11:15 | 45 min | **GHAS Deep Dive — Detect & Analyze** | Slides 38–53 | **Demo G-02** (CodeQL scan results walkthrough), **Demo G-06** (Container scan results), **Demo I-01** (tfsec results — show only) |
| 11:15–12:00 | 45 min | **Eradicating Security Debt + Autofix** | Slides 54–61 | **Demo G-07** (Copilot Autofix on a real PR — live) |
| 12:00–13:00 | 60 min | Lunch | — | — |
| 13:00–13:45 | 45 min | **Secure Across the Stack — Azure + GitHub + MDC** | Slides 62–70 | **Demo M-01/M-02** (MDC portal walkthrough, GitHub connector, unified view) |
| 13:45–14:30 | 45 min | **Agentic AI for DevSecOps** | Slides 71–84 | **Demo A-01** (Security Agent live assessment), **Demo A-03** (IaC Agent — show specific finding) |
| 14:30–14:45 | 15 min | Break | — | — |
| 14:45–15:30 | 45 min | **Live End-to-End Demo** | Slides 85–88 | Full flow: commit vulnerable code → PR triggers scans → review findings → Autofix suggests fix → agent validates → MDC shows posture change |
| 15:30–16:00 | 30 min | **Key Takeaways, Q&A, Next Steps** | Slides 89–95 | Discuss PoC options (3-day, 5-day). Provide repo fork instructions for self-exploration. |

### 4.2 Content to Create for Tier 1

| Deliverable | Description | Priority |
|-------------|-------------|----------|
| **Demo Runbook** | Step-by-step script for each demo with exact commands, expected outputs, fallback screenshots if live demo fails | HIGH |
| **Pre-baked Environment** | Azure subscription with deployed app + MDC configured. GitHub org with GHAS enabled, pre-run scan results visible | HIGH |
| **Executive Summary Handout** | 2-page PDF: value proposition, capability matrix, ROI metrics (use existing `Agentic DevSecOps with GitHub Advanced Security.docx` as base) | MEDIUM |
| **Slide Deck Modifications** | Add "DEMO" transition slides at each demo insertion point. Add speaker notes with demo talking points | MEDIUM |

### 4.3 Presentation Slide Mapping

Update the existing 95-slide deck with these additions:

| Insert After Slide | New Content | Type |
|--------------------|-------------|------|
| 37 | "DEMO: Secret Scanning Push Protection" transition slide + demo runbook reference | Transition slide |
| 43 | "DEMO: CodeQL Findings Walkthrough" transition slide | Transition slide |
| 61 | "DEMO: Copilot Autofix in Action" transition slide | Transition slide |
| 70 | "DEMO: Defender for Cloud Unified Security View" transition slide | Transition slide |
| 84 | "DEMO: Agentic Security Assessment" transition slide | Transition slide |
| 88 | Replace existing demo screenshots with structured end-to-end demo section (3–4 slides) | Replace |

---

## 5. Tier 2 — 3-Day Light Proof of Concept

**Audience:** Security engineers, senior developers, DevOps leads, platform team members  
**Objective:** Hands-on validation of GHAS + GHCP + MDC capabilities against representative scenarios. Participants configure, scan, and remediate using the provided repository.  
**Format:** 60% hands-on labs / 40% presentation + discussion  
**Delivery Model:** Instructor-led with each participant working on their own forked repo

### 5.1 Agenda

#### Day 1 — Foundation: GHAS & Shift-Left Security

| Time Block | Duration | Activity | Format | Labs |
|------------|----------|----------|--------|------|
| 09:00–09:30 | 30 min | Welcome, objectives, environment check | Presentation | — |
| 09:30–10:15 | 45 min | Setting the Stage (slides 7–21) | Presentation + Discussion | Whiteboard: map participants' current SDLC security practices |
| 10:15–10:30 | 15 min | Break | — | — |
| 10:30–11:15 | 45 min | Environment Setup | Hands-on | **F-01**: Fork repo, enable GHAS |
| 11:15–12:00 | 45 min | Repository Walkthrough & Vulnerability Discovery | Hands-on | **F-02**: Identify vulnerabilities manually |
| 12:00–13:00 | 60 min | Lunch | — | — |
| 13:00–13:30 | 30 min | Secret Scanning Deep Dive (slides 31–37) | Presentation | — |
| 13:30–14:15 | 45 min | Secret Scanning Lab | Hands-on | **G-01**: Push protection + custom patterns |
| 14:15–14:30 | 15 min | Break | — | — |
| 14:30–15:00 | 30 min | Code Scanning & CodeQL (slides 38–39, 43, 46) | Presentation | — |
| 15:00–16:00 | 60 min | Code Scanning Lab | Hands-on | **G-02**: CodeQL scan + findings analysis |
| 16:00–16:30 | 30 min | Day 1 Wrap-up & Preview | Discussion | Review findings discovered. Preview Day 2. |

#### Day 2 — Supply Chain, IaC & Containers

| Time Block | Duration | Activity | Format | Labs |
|------------|----------|----------|--------|------|
| 09:00–09:30 | 30 min | Day 1 Recap + Supply Chain Context (slides 40–42) | Presentation | — |
| 09:30–10:30 | 60 min | Dependency Review + SBOM Labs | Hands-on | **G-03**: Dependency Review, **G-04**: SBOM generation |
| 10:30–10:45 | 15 min | Break | — | — |
| 10:45–11:30 | 45 min | Container Security (slides 44–45) | Presentation | — |
| 11:30–12:00 | 30 min | Container Scanning Lab | Hands-on | **G-06**: Trivy + Grype scanning |
| 12:00–13:00 | 60 min | Lunch | — | — |
| 13:00–13:30 | 30 min | IaC Security Context (slide 48) | Presentation | — |
| 13:30–14:30 | 60 min | IaC Scanning Labs | Hands-on | **I-01**: tfsec scanning + remediation, **I-04**: Kubernetes manifest fix |
| 14:30–14:45 | 15 min | Break | — | — |
| 14:45–15:15 | 30 min | Pipeline Security (slides 49–50) | Presentation | — |
| 15:15–16:00 | 45 min | Pipeline Hardening Lab | Hands-on | **R-03**: Workflow hardening (SHA pinning, permissions) |
| 16:00–16:30 | 30 min | Day 2 Wrap-up | Discussion | Consolidated findings review across all scan types |

#### Day 3 — Agentic AI, MDC Integration & Remediation

| Time Block | Duration | Activity | Format | Labs |
|------------|----------|----------|--------|------|
| 09:00–09:30 | 30 min | Day 2 Recap + Autofix & Security Campaigns (slides 54–61) | Presentation | — |
| 09:30–10:15 | 45 min | Copilot Autofix Lab | Hands-on | **G-07**: Trigger and review Autofix suggestions |
| 10:15–10:30 | 15 min | Break | — | — |
| 10:30–11:00 | 30 min | Agentic AI for DevSecOps (slides 71–84) | Presentation | — |
| 11:00–12:00 | 60 min | Agent Labs | Hands-on | **A-01**: Security Agent assessment, **A-03**: IaC Agent scan |
| 12:00–13:00 | 60 min | Lunch | — | — |
| 13:00–13:30 | 30 min | MDC Integration (slides 62–70) | Presentation | — |
| 13:30–14:15 | 45 min | MDC Lab | Hands-on | **M-01**: Connect GitHub to MDC, **M-02**: Dashboard walkthrough |
| 14:15–14:30 | 15 min | Break | — | — |
| 14:30–15:30 | 60 min | Remediation Sprint | Hands-on | **R-01** (subset): Fix top 5 code vulnerabilities with agent assistance |
| 15:30–16:00 | 30 min | **PoC Readout & Next Steps** | Discussion | Present findings, demonstrate before/after, discuss 5-day PoC for full remediation + customer code integration |

### 5.2 Content to Create for Tier 2

| Deliverable | Description | Priority |
|-------------|-------------|----------|
| **Lab Guide Document** | Per-lab guide with: objectives, prerequisites, step-by-step instructions, expected outputs, screenshots, success criteria, troubleshooting tips | HIGH |
| **Participant Workbook** | Printable/PDF companion with exercises, note-taking space, cheat sheets for each tool | HIGH |
| **Environment Setup Script** | Automated script (`setup-lab-env.sh` / `setup-lab-env.ps1`) that provisions: Azure resources via Bicep, enables GHAS features via `gh` CLI, verifies prerequisites | HIGH |
| **Facilitator Timing Guide** | Slide-by-slide timing with lab transition cues, discussion prompts, common questions + answers | HIGH |
| **Before/After Code Pairs** | For each vulnerability category: vulnerable code + remediated code + explanation. Stored in `labs/remediation-examples/` | MEDIUM |
| **Scoring/Assessment Rubric** | Per-lab completion checklist + overall PoC success metrics | MEDIUM |

---

## 6. Tier 3 — 5-Day Full Proof of Concept

**Audience:** Full development team + security team + DevOps/platform team  
**Objective:** Complete validation of GHAS + GHCP + MDC against the customer's actual codebase, infrastructure, and CI/CD pipelines. End state: documented security posture improvement with a go/no-go recommendation for adoption.  
**Format:** 40% hands-on labs / 30% customer code integration / 30% presentation + architecture + planning  
**Delivery Model:** Instructor-led workshop with pair programming, customer-specific configuration, and executive readout

### 6.1 Agenda

#### Day 1 — Foundation, Assessment & GHAS Core

| Time Block | Duration | Activity | Format | Labs |
|------------|----------|----------|--------|------|
| 09:00–09:45 | 45 min | Kickoff: objectives, success criteria, team introductions | Presentation | — |
| 09:45–10:30 | 45 min | Current State Assessment | Workshop | Whiteboard: map customer's current SDLC, security tools, pain points, compliance requirements |
| 10:30–10:45 | 15 min | Break | — | — |
| 10:45–11:30 | 45 min | Setting the Stage (slides 7–21) + Industry Context | Presentation + Discussion | Map customer's maturity against DevSecOps framework |
| 11:30–12:00 | 30 min | Environment Setup (Reference Repo) | Hands-on | **F-01**, **F-02**: Fork + walkthrough |
| 12:00–13:00 | 60 min | Lunch | — | — |
| 13:00–14:00 | 60 min | Secret Scanning Deep Dive + Lab | Mixed | Slides 31–37 → **G-01** |
| 14:00–14:15 | 15 min | Break | — | — |
| 14:15–15:15 | 60 min | Code Scanning Deep Dive + Lab | Mixed | Slides 38–39 → **G-02** |
| 15:15–16:00 | 45 min | **Customer Code Onboarding (Part 1)** | Hands-on | Enable GHAS on customer's actual repository. Configure CodeQL language matrix. Run initial scan. |
| 16:00–16:30 | 30 min | Day 1 Retrospective | Discussion | Initial findings from customer code scan |

#### Day 2 — Supply Chain, Containers & IaC

| Time Block | Duration | Activity | Format | Labs |
|------------|----------|----------|--------|------|
| 09:00–09:30 | 30 min | Day 1 Recap + Customer Scan Results Review | Discussion | Triage initial CodeQL findings from customer code |
| 09:30–10:30 | 60 min | Supply Chain Security (slides 40–42) + Labs | Mixed | **G-03** (reference repo), **G-04**, **G-05** |
| 10:30–10:45 | 15 min | Break | — | — |
| 10:45–12:00 | 75 min | Container + IaC Security (slides 44–45, 48) + Labs | Mixed | **G-06**, **I-01**, **I-02**, **I-04** |
| 12:00–13:00 | 60 min | Lunch | — | — |
| 13:00–14:00 | 60 min | **Customer IaC Onboarding** | Hands-on | Configure tfsec/KICS/MSDO against customer's Terraform/Bicep/ARM templates. Run scans. Review findings. |
| 14:00–14:15 | 15 min | Break | — | — |
| 14:15–15:15 | 60 min | Pipeline Security (slides 49–50) + Lab | Mixed | **R-03** (reference repo), then analyze customer's workflows |
| 15:15–16:00 | 45 min | **Customer Pipeline Hardening** | Hands-on | Apply pipeline security agent recommendations to customer's actual CI/CD workflows |
| 16:00–16:30 | 30 min | Day 2 Retrospective | Discussion | Consolidated scan results across customer code + IaC + pipelines |

#### Day 3 — Agentic AI, Autofix & Deep Remediation

| Time Block | Duration | Activity | Format | Labs |
|------------|----------|----------|--------|------|
| 09:00–09:30 | 30 min | Day 2 Recap + Remediation Strategy | Discussion | Prioritize customer findings by severity and business impact |
| 09:30–10:30 | 60 min | Copilot Autofix + Security Campaigns (slides 54–61) + Lab | Mixed | **G-07**, **G-08** |
| 10:30–10:45 | 15 min | Break | — | — |
| 10:45–12:00 | 75 min | Agentic AI Deep Dive + Agent Labs | Mixed | Slides 71–84 → **A-01**, **A-02**, **A-03**, **A-04**, **A-05** |
| 12:00–13:00 | 60 min | Lunch | — | — |
| 13:00–14:00 | 60 min | **Customer Code Remediation Sprint (Part 1)** | Hands-on | Use Copilot Autofix + agents to remediate top 10 customer findings. Create fix PRs. |
| 14:00–14:15 | 15 min | Break | — | — |
| 14:15–15:15 | 60 min | **Customer Code Remediation Sprint (Part 2)** | Hands-on | Continue remediation. Validate fixes with re-scan. Measure alert reduction. |
| 15:15–16:00 | 45 min | Security Plan Creation | Hands-on | **A-06**: Use Security Plan Creator Agent against customer's architecture blueprint |
| 16:00–16:30 | 30 min | Day 3 Retrospective | Discussion | Remediation progress, agent effectiveness assessment |

#### Day 4 — MDC Integration, Deployment & Runtime Security

| Time Block | Duration | Activity | Format | Labs |
|------------|----------|----------|--------|------|
| 09:00–09:30 | 30 min | Day 3 Recap + Azure Security Context | Discussion | — |
| 09:30–10:30 | 60 min | MDC Deep Dive (slides 62–70) + Lab | Mixed | **M-01**: Connect GitHub to MDC |
| 10:30–10:45 | 15 min | Break | — | — |
| 10:45–11:30 | 45 min | MDC Dashboard + CSPM | Hands-on | **M-02**, **M-03**: DevOps Security blade, Secure Score, attack path analysis |
| 11:30–12:00 | 30 min | **Customer Azure Environment Integration** | Hands-on | Connect customer's Azure subscription to MDC. Enable Defender plans. |
| 12:00–13:00 | 60 min | Lunch | — | — |
| 13:00–14:00 | 60 min | Deploy App + DAST | Hands-on | **D-01**: Deploy webapp to Azure, **D-02**: Run ZAP scan against deployed app |
| 14:00–14:15 | 15 min | Break | — | — |
| 14:15–15:15 | 60 min | Runtime Protection + Threat Detection | Hands-on | **M-04**: Defender for App Service/Containers alerts |
| 15:15–16:00 | 45 min | Compliance Mapping Exercise | Hands-on | **R-04**: Map findings to customer's required compliance framework (CIS/NIST/SOC2/PCI-DSS) |
| 16:00–16:30 | 30 min | Day 4 Retrospective | Discussion | End-to-end visibility: code → build → deploy → runtime |

#### Day 5 — Advanced Scenarios, Custom Agents & Executive Readout

| Time Block | Duration | Activity | Format | Labs |
|------------|----------|----------|--------|------|
| 09:00–09:30 | 30 min | Day 4 Recap + Advanced Topics Setup | Discussion | — |
| 09:30–10:30 | 60 min | Build Your Own Custom Agent | Hands-on | **A-07**: Design and build agent tailored to customer's domain |
| 10:30–10:45 | 15 min | Break | — | — |
| 10:45–11:30 | 45 min | Org-Level GHAS Configuration | Hands-on | Configure org-level: code scanning defaults, secret scanning, security overview dashboard, required workflows |
| 11:30–12:00 | 30 min | CI/CD Agent Orchestration | Hands-on | **A-08**: Integrate security agents into customer's CI/CD pipeline |
| 12:00–13:00 | 60 min | Lunch | — | — |
| 13:00–14:00 | 60 min | **PoC Results Compilation** | Workshop | Compile: total findings discovered, findings remediated, security posture delta, tool coverage matrix, compliance gaps |
| 14:00–14:15 | 15 min | Break | — | — |
| 14:15–15:15 | 60 min | **Adoption Roadmap Planning** | Workshop | Define: rollout phases, team training plan, tool procurement, governance policies, success metrics |
| 15:15–16:00 | 45 min | **Executive Readout** | Presentation | Present PoC results to leadership: before/after security posture, ROI projection, recommended next steps, adoption timeline |
| 16:00–16:30 | 30 min | Closeout & Feedback | Discussion | NPS/feedback collection, follow-up action items, resource handoffs |

### 6.2 Content to Create for Tier 3

All Tier 2 deliverables PLUS:

| Deliverable | Description | Priority |
|-------------|-------------|----------|
| **Customer Onboarding Playbook** | Guide for integrating GHAS/MDC with customer's existing repos, Azure subscriptions, and compliance requirements | HIGH |
| **PoC Results Template** | Standardized report template: executive summary, findings by category, remediation metrics, ROI analysis, recommendation | HIGH |
| **Adoption Roadmap Template** | Phased rollout plan: pilot → team → org → enterprise. Includes training, governance, and metric definitions | HIGH |
| **Custom Agent Workshop Guide** | Step-by-step guide for designing, building, testing, and deploying custom Copilot agents | MEDIUM |
| **Compliance Mapping Worksheets** | Pre-built mapping tables for CIS Azure v2.0, NIST 800-53 Rev 5, SOC 2, PCI-DSS | MEDIUM |
| **Org-Level Configuration Guide** | How to configure GHAS at org level: security policies, default CodeQL configs, required workflows | MEDIUM |
| **Executive Readout Deck Template** | Slide template for final presentation: before/after metrics, findings summary, recommendation | MEDIUM |

---

## 7. Content Creation Backlog

Prioritized list of all new content to create, ordered by dependency and impact.

### 7.1 Priority 1 — Blocking (Required for any tier)

| ID | Deliverable | Description | Estimated Effort |
|----|-------------|-------------|------------------|
| **C-01** | Lab Guide Template | Standardized template for all lab guides (objectives, prereqs, steps, expected output, troubleshooting) | Small |
| **C-02** | Environment Setup Script | `setup-lab-env.sh` / `.ps1`: fork repo via `gh`, enable GHAS features, provision Azure resources via Bicep, verify prerequisites | Medium |
| **C-03** | Demo Runbook (Tier 1) | Step-by-step demo script with commands, expected outputs, and fallback screenshots | Medium |
| **C-04** | Pre-baked Demo Environment | Azure subscription with deployed app, MDC configured, pre-run scan results | Medium |

### 7.2 Priority 2 — Core Labs (Required for Tier 2+)

| ID | Deliverable | Description | Estimated Effort |
|----|-------------|-------------|------------------|
| **C-05** | Lab Guides: F-01, F-02 | Foundation labs (setup + walkthrough) | Small |
| **C-06** | Lab Guides: G-01 through G-08 | All 8 GHAS capability labs | Large |
| **C-07** | Lab Guides: I-01 through I-05 | All 5 IaC security labs | Medium |
| **C-08** | Lab Guides: A-01 through A-06 | All 6 agent labs | Large |
| **C-09** | Lab Guides: M-01, M-02 | MDC connection + dashboard labs | Medium |
| **C-10** | Before/After Remediation Examples | Paired code for all vulnerability categories (code, IaC, K8s, pipeline) | Medium |
| **C-11** | Participant Workbook | PDF companion with exercises and cheat sheets | Medium |
| **C-12** | Facilitator Timing Guide | Slide-to-lab mapping with timing and transition cues | Small |

### 7.3 Priority 3 — Advanced Content (Required for Tier 3)

| ID | Deliverable | Description | Estimated Effort |
|----|-------------|-------------|------------------|
| **C-13** | Lab Guides: D-01, D-02, D-03 | DAST + deployment labs | Medium |
| **C-14** | Lab Guides: M-03, M-04 | Advanced MDC labs (CSPM, runtime detection) | Medium |
| **C-15** | Lab Guide: A-07 | Build Your Own Agent workshop | Medium |
| **C-16** | Lab Guide: R-01 through R-04 | All remediation labs | Medium |
| **C-17** | Customer Onboarding Playbook | Customer code integration guide | Medium |
| **C-18** | PoC Results Template | Standardized results report | Small |
| **C-19** | Adoption Roadmap Template | Phased rollout plan template | Small |
| **C-20** | Compliance Mapping Worksheets | CIS, NIST, SOC 2, PCI-DSS mapping tables | Medium |
| **C-21** | Executive Readout Deck Template | Final presentation slide template | Small |
| **C-22** | Org-Level Configuration Guide | Org-wide GHAS setup instructions | Small |

### 7.4 Priority 4 — Polish & Enhancement

| ID | Deliverable | Description | Estimated Effort |
|----|-------------|-------------|------------------|
| **C-23** | Slide Deck Updates | Add demo transition slides, speaker notes, lab references | Small |
| **C-24** | Wiki Updates | Update wiki per existing wiki.instructions.md with lab links | Medium |
| **C-25** | Video Walkthroughs | Record short videos for each lab (optional, for async delivery) | Large |
| **C-26** | Lab Environment Teardown Script | Automated cleanup to avoid Azure cost overruns | Small |

---

## 8. Prerequisites & Environment Requirements

### 8.1 Per-Participant Requirements (Tier 2 & 3)

| Requirement | Details |
|-------------|---------|
| **GitHub Account** | With access to a GHAS-enabled organization (Enterprise or trial) |
| **Azure Subscription** | With Contributor role. Needs: App Service, ACR, AKS, SQL, Key Vault, MDC, Defender plans |
| **VS Code** | Latest stable version |
| **VS Code Extensions** | GitHub Copilot, GitHub Copilot Chat, Azure Tools, Docker |
| **CLI Tools** | `gh` CLI, `az` CLI, `docker`, `dotnet` SDK 9.0, `terraform` (optional for local scans) |
| **Network** | Internet access to GitHub.com, Azure portal, VS Code marketplace |

### 8.2 Facilitator Requirements

| Requirement | Details |
|-------------|---------|
| **GitHub Org** | With GHAS + Copilot Enterprise enabled. Pre-configured org-level policies for Day 5 demos. |
| **Azure Subscription** | Pre-provisioned with: deployed webapp, MDC workspace, Defender plans enabled, GitHub connector configured |
| **Backup Screenshots** | For every live demo step in case of network/service issues |
| **Participant Azure Credentials** | Pre-provisioned service principals or Azure AD accounts with scoped permissions |

### 8.3 Azure Resource Estimates

| Resource | Purpose | Estimated Cost/Day |
|----------|---------|-------------------|
| App Service Plan (S1) | Host webapp01 | ~$2.40 |
| Azure Container Registry (Basic) | Store container images | ~$0.50 |
| Azure SQL Database (Basic) | Demo database for IaC scanning | ~$0.16 |
| Key Vault (Standard) | Secrets management demo | ~$0.03 |
| AKS (1 node, Standard_B2s) | Kubernetes demos (Tier 3 only) | ~$3.00 |
| MDC (Defender CSPM) | Security posture | ~$0 (trial) |
| Log Analytics Workspace | MDC backend | ~$0.50 |
| **Total per participant** | | **~$6.50/day** |

---

## 9. Mapping: PPTX Slides to Labs

This is the canonical reference for inserting labs into the presentation flow.

| Slide(s) | Topic | Recommended Lab(s) | Tier |
|----------|-------|---------------------|------|
| 6 | Audience polling | Discussion exercise | All |
| 7–21 | Why DevSecOps matters | Whiteboard: current state assessment | 2, 3 |
| 31–37 | Secret Scanning | **G-01** | All |
| 38–39, 43, 46 | Code Scanning / CodeQL | **G-02** | All |
| 40 | Dependency Review | **G-03** | 2, 3 |
| 41 | SBOM | **G-04** | 2, 3 |
| 42 | OpenSSF Scorecard | **G-05** | 2, 3 |
| 44–45, 51 | Container Security | **G-06**, **I-04** | 2, 3 |
| 48 | IaC Security | **I-01**, **I-02**, **I-03**, **I-05** | 2, 3 |
| 49–50 | Pipeline Security | **R-03**, **A-04** | 2, 3 |
| 54–56 | Security Campaigns | **G-08** | 3 |
| 57–61 | Copilot Autofix | **G-07** | All |
| 62–70 | MDC Integration | **M-01**, **M-02**, **M-03**, **M-04** | All (demo) / 2, 3 (hands-on) |
| 71–84 | Agentic AI | **A-01** through **A-08** | All (demo) / 2, 3 (hands-on) |
| 85–88 | Demo section | End-to-end flow using multiple labs | All |

---

## 10. Open Questions & Decisions Required

These items need resolution before content creation begins. No assumptions have been made.

| # | Question | Impact | Options |
|---|----------|--------|---------|
| 1 | **GHAS licensing model for labs** — Will participants use their own GHAS licenses, a shared trial org, or will Microsoft provide lab-specific licenses? | Affects F-01 setup, all G-* labs | Own license / Shared trial / Microsoft-provided |
| 2 | **Azure subscription provisioning** — Will participants bring their own Azure subscriptions, or will lab subscriptions be provided (e.g., Azure Passes)? | Affects F-03, all D-* and M-* labs, cost model | BYOS / Azure Pass / Shared subscription with RBAC |
| 3 | **Customer code integration (Tier 3)** — How will customer source code be handled? Will scans run on customer repos directly, or will code be imported into the lab environment? | Affects Day 1 (Tier 3), security/compliance of lab | Direct scan / Import to lab org / Air-gapped |
| 4 | **MDC tier for labs** — Which Defender for Cloud plans should be enabled? Free tier limits security posture demos. Defender CSPM + Defender for App Service are needed for full capability. | Affects M-01 through M-04, cost | Free / Defender CSPM / Full Defender plans |
| 5 | **Copilot licensing** — Do all participants need Copilot Enterprise with agent access, or can some labs use Copilot Individual? | Affects all A-* labs | Enterprise required / Individual sufficient for some |
| 6 | **Security Campaigns availability** — Security Campaigns require GHAS Enterprise at org level. Is this available for the lab environment? | Affects G-08 | Available / Skip lab / Demo-only |
| 7 | **Target audience for each tier** — Are the three tiers for the same customer at different stages, or different customer profiles? | Affects content depth and customization level | Same customer progression / Different profiles |
| 8 | **Slide deck ownership** — Should the 95-slide deck be modified directly, or should a derivative deck be created for each tier? | Affects C-23 | Modify original / Create tier-specific decks |
| 9 | **Lab guide format** — Markdown in the repo, standalone PDFs, or a hosted docs site (e.g., GitHub Pages, Learn-style modules)? | Affects all C-05 through C-22 | Markdown in repo / PDF / Hosted site |
| 10 | **TT343.docx password** — The encrypted docx may contain speaker notes or additional content. Can the password be provided? | May affect content completeness | Provide password / Ignore file |
| 11 | **Reusable workflow org** — The cicd.yml references `githubabcs-devops/devsecops-reusable-workflows`. Will participants have access to this org? | Affects cicd workflow labs | Public access / Fork / Reconfigure |
| 12 | **Multi-cloud scope** — Samples include GKE (`gke.tf`) and EKS (`eks.tf`) Terraform. Should labs cover AWS/GCP or focus exclusively on Azure? | Affects I-* labs scope | Azure only / Multi-cloud |
| 13 | **Existing presentation variant** — The 85-slide MCAPS Tech Connect deck (TT343.pptx) is similar but not identical. Should both be maintained or should one be retired? | Affects deck maintenance burden | Maintain both / Retire one / Merge differences |

---

## Appendix A: Repository File Map (Reference)

```
gh-advsec-devsecops/
├── .github/
│   ├── agents/                          # 6 custom Copilot agents
│   │   ├── security-agent.md
│   │   ├── security-reviewer-agent.md
│   │   ├── security-plan-creator.agent.md
│   │   ├── pipeline-security-agent.md
│   │   ├── iac-security-agent.md
│   │   └── supply-chain-security-agent.md
│   ├── workflows/                       # 17 CI/CD workflows
│   │   ├── ci.yml
│   │   ├── cicd.yml
│   │   ├── security-agent-workflow.yml
│   │   ├── SAST-GitHubAdvancedSecurity-CodeQL.yml
│   │   ├── SAST-Kubesec.yml
│   │   ├── SCA-GitHubAdvancedSecurity-DependencyReview.yml
│   │   ├── SCA-Anchore-Syft-SBOM.yml
│   │   ├── SCA-Microsoft-SBOM.yml
│   │   ├── SCA-OpenSSF-Scorecard.yml
│   │   ├── CIS-Trivy-AquaSecurity.yml
│   │   ├── CIS-Anchore-Grype.yml
│   │   ├── IACS-AquaSecurity-tfsec.yml
│   │   ├── IACS-Checkmarx-kics.yml
│   │   ├── IACS-Microsoft-Security-DevOps.yml
│   │   ├── MSDO-Microsoft-Security-DevOps.yml
│   │   └── DAST-ZAP-Zed-Attach-Proxy-Checkmarx.yml
│   ├── instructions/wiki.instructions.md
│   ├── copilot-instructions.md
│   ├── dependabot.yml
│   └── secret_scanning.yml
├── GHAS+MDC/                            # Presentation materials
│   ├── TT343 - Agentic AI for DevSecOps...pptx  (95 slides, 85 MB)
│   ├── TT343.pptx                                (85 slides, 82 MB)
│   ├── Agentic DevSecOps with GitHub Advanced Security.docx
│   └── TT343.docx                                (encrypted)
├── src/webapp01/                        # ASP.NET 9.0 vulnerable app
├── terraform/azure/                     # 15 Terraform files (intentionally vulnerable)
├── manifests/                           # 2 Kubernetes manifests (secure + insecure)
├── blueprints/                          # 2 Bicep blueprint sets
├── samples/                             # Multi-language vulnerable code samples
├── specs/                               # 1 demo spec
├── docs/templates/                      # Security plan template
├── security-plan-outputs/               # 2 generated security plans
├── README.md
├── SECURITY.md
├── CODEOWNERS
├── azure.yaml
└── gh-aspnet-webapp-01.sln
```

## Appendix B: Proposed Repository Structure for Lab Content

```
gh-advsec-devsecops/
├── labs/                                # NEW — All lab content
│   ├── README.md                        # Lab catalog with links
│   ├── facilitator-guide.md             # Timing, transitions, tips
│   ├── participant-setup.md             # Prerequisites and setup instructions
│   ├── foundation/
│   │   ├── F-01-environment-setup.md
│   │   └── F-02-repo-walkthrough.md
│   ├── ghas/
│   │   ├── G-01-secret-scanning.md
│   │   ├── G-02-code-scanning-codeql.md
│   │   ├── G-03-dependency-review.md
│   │   ├── G-04-sbom-generation.md
│   │   ├── G-05-openssf-scorecard.md
│   │   ├── G-06-container-scanning.md
│   │   ├── G-07-copilot-autofix.md
│   │   └── G-08-security-campaigns.md
│   ├── iac/
│   │   ├── I-01-terraform-tfsec.md
│   │   ├── I-02-terraform-kics.md
│   │   ├── I-03-msdo-iac.md
│   │   ├── I-04-kubernetes-kubesec.md
│   │   └── I-05-bicep-review.md
│   ├── dast/
│   │   ├── D-01-deploy-vulnerable-app.md
│   │   ├── D-02-zap-scan.md
│   │   └── D-03-log-forging-demo.md
│   ├── mdc/
│   │   ├── M-01-github-mdc-connection.md
│   │   ├── M-02-devops-security-dashboard.md
│   │   ├── M-03-cspm-attack-paths.md
│   │   └── M-04-runtime-threat-detection.md
│   ├── agents/
│   │   ├── A-01-security-agent.md
│   │   ├── A-02-security-reviewer-agent.md
│   │   ├── A-03-iac-security-agent.md
│   │   ├── A-04-pipeline-security-agent.md
│   │   ├── A-05-supply-chain-agent.md
│   │   ├── A-06-security-plan-creator.md
│   │   ├── A-07-build-custom-agent.md
│   │   └── A-08-agent-orchestration.md
│   ├── remediation/
│   │   ├── R-01-fix-code-vulnerabilities.md
│   │   ├── R-02-harden-terraform.md
│   │   ├── R-03-harden-pipelines.md
│   │   └── R-04-compliance-mapping.md
│   └── remediation-examples/           # Before/after code pairs
│       ├── code/
│       ├── iac/
│       ├── kubernetes/
│       └── pipelines/
├── scripts/                             # NEW — Lab automation
│   ├── setup-lab-env.sh
│   ├── setup-lab-env.ps1
│   ├── teardown-lab-env.sh
│   └── teardown-lab-env.ps1
├── templates/                           # NEW — Deliverable templates
│   ├── poc-results-report.md
│   ├── adoption-roadmap.md
│   └── executive-readout-deck/          # Slide template
└── ... (existing content unchanged)
```

## Appendix C: Lab Guide Template

Each lab guide should follow this structure:

```markdown
# Lab [ID]: [Title]

## Overview
Brief description of what this lab covers and why it matters.

## Learning Objectives
- Objective 1
- Objective 2
- Objective 3

## Prerequisites
- [ ] Prerequisite 1
- [ ] Prerequisite 2

## Estimated Duration
XX minutes

## Difficulty Level
Beginner | Intermediate | Advanced

## Instructions

### Step 1: [Step Title]
Detailed instructions with commands and expected output.

### Step 2: [Step Title]
...

## Expected Results
What participants should see when the lab is complete.

## Success Criteria
- [ ] Criterion 1
- [ ] Criterion 2

## Troubleshooting
| Problem | Solution |
|---------|----------|
| Issue 1 | Fix 1 |

## Key Takeaways
- Takeaway 1
- Takeaway 2

## Next Steps
Link to next lab or related content.
```
