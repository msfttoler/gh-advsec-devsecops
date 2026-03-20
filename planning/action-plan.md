# Action Plan: Agentic DevSecOps VBD — Ship Checklist

> **Purpose:** Prioritized checklist of everything needed to deliver the 1-Day, 3-Day, and 5-Day engagements. Items are ordered by dependency (what blocks other work) and then by difficulty within each priority tier.
>
> **How to read this:** Work top-to-bottom within each priority. Items marked with a tier badge indicate the **minimum tier that requires them**: 🟢 1-Day, 🟡 3-Day, 🔴 5-Day. Items without a badge are required for all tiers.

---

## Priority 0 — Decisions (Unblock Everything Else)

These are the open questions from the expansion plan. Nothing else can be finalized until these are answered. Each has a recommended default if no answer is forthcoming.

| # | Decision | Blocks | Recommended Default | Owner | Status |
|---|----------|--------|---------------------|-------|--------|
| - [ ] | **Lab guide format** — Markdown in repo, PDFs, or hosted docs site? | All lab authoring (P1, P2) | Markdown in repo under `labs/` | | ⬜ |
| - [ ] | **GHAS licensing for labs** — Participant's own, shared trial org, or Microsoft-provided? | F-01 setup guide, all G-* labs | Shared trial org provisioned by facilitator | | ⬜ |
| - [ ] | **Azure subscription model** — BYOS, Azure Pass, or shared subscription with RBAC? | F-03, all D-* and M-* labs, cost estimates | Azure Pass per participant | | ⬜ |
| - [ ] | **Copilot licensing** — Enterprise required for all participants, or Individual for some labs? | All A-* labs | Enterprise required (agents need it) | | ⬜ |
| - [ ] | **MDC tier** — Free, Defender CSPM, or full Defender plans? | M-01 through M-04, cost | Defender CSPM + Defender for App Service (trial) | | ⬜ |
| - [ ] | **Security Campaigns availability** — Available in lab org? | G-08 lab | Demo-only with screenshots if unavailable | | ⬜ |
| - [ ] | **Slide deck approach** — Modify the 95-slide deck or create tier-specific derivatives? | Slide deck updates (P3) | Single deck with "DEMO" transition slides; facilitator skips sections per tier | | ⬜ |
| - [ ] | **Multi-cloud scope** — Azure only or include AWS/GCP samples? | I-* lab scope | Azure only; mention GKE/EKS samples exist for reference | | ⬜ |
| - [ ] | **Second PPTX (85-slide variant)** — Maintain both or retire one? | Deck maintenance | Retire the 85-slide variant; keep 95-slide as canonical | | ⬜ |
| - [ ] | **TT343.docx password** — Can it be provided? | Possible speaker notes content | Ignore if password unavailable | | ⬜ |
| - [ ] | **Reusable workflow org access** — Will participants have access to `githubabcs-devops`? | cicd.yml lab | Ensure org is public or fork the reusable workflow | | ⬜ |
| - [ ] | **Target audience model** — Same customer progressing through tiers, or different profiles? | Content framing and pitch language | Different profiles; 1-day = executives, 3-day = technical leads, 5-day = full team | | ⬜ |

**Action:** Schedule a 30-minute decision meeting. Walk through each item. Document answers. Then proceed to P1.

---

## Priority 1 — Foundation (Blocks Everything; Start Here)

These items must exist before any lab can be delivered. They are the scaffolding.

### Infrastructure & Tooling

| # | Task | Tier | Difficulty | Description | Depends On |
|---|------|------|------------|-------------|------------|
| - [ ] | **Create `labs/` directory structure** | 🟢 | Easy | Create the directory tree from Appendix B of the expansion plan: `labs/{foundation,ghas,iac,dast,mdc,agents,remediation}/` + `labs/remediation-examples/{code,iac,kubernetes,pipelines}/` | P0 decisions (format) |
| - [ ] | **Create `scripts/` directory** | 🟢 | Easy | Create `scripts/` for setup/teardown automation | — |
| - [ ] | **Create `templates/` directory** | 🟡 | Easy | Create `templates/` for PoC results, adoption roadmap, executive readout | — |
| - [ ] | **Write lab guide template** (`labs/lab-template.md`) | 🟢 | Easy | Standardized structure: Overview, Learning Objectives, Prerequisites, Duration, Difficulty, Steps, Expected Results, Success Criteria, Troubleshooting, Key Takeaways, Next Steps. Copy from Appendix C of expansion plan. | — |
| - [ ] | **Write participant setup guide** (`labs/participant-setup.md`) | 🟢 | Easy | Comprehensive prerequisite checklist with verification commands for every tool (gh, az, docker, dotnet, terraform, VS Code extensions). Include "run this script to verify" section. | P0 (licensing model) |
| - [ ] | **Write environment setup script** (`scripts/setup-lab-env.sh` + `.ps1`) | 🟢 | Medium | Automate: fork repo via `gh repo fork`, enable GHAS features via `gh api`, verify CLI tools installed, optionally provision Azure resources via `az deployment`. Both bash and PowerShell versions. | P0 (licensing, Azure model) |
| - [ ] | **Write environment teardown script** (`scripts/teardown-lab-env.sh` + `.ps1`) | 🟡 | Easy | Delete Azure resource group, remove fork (optional), clean up local clone. Prevents cost overruns. | Setup script |
| - [ ] | **Create pre-baked facilitator environment** | 🟢 | Medium | Azure subscription with: deployed webapp01, MDC workspace with GitHub connector synced, Defender plans enabled, pre-run scan results in Security tab. Document the setup steps so it's reproducible. | P0 (MDC tier, Azure model) |

### Core Documentation

| # | Task | Tier | Difficulty | Description | Depends On |
|---|------|------|------------|-------------|------------|
| - [ ] | **Write demo runbook** (`labs/facilitator-demo-runbook.md`) | 🟢 | Medium | Step-by-step script for every live demo in the 1-Day delivery: exact commands, expected output, talking points, fallback screenshots. Covers: G-01 demo, G-02 demo, G-06 demo, G-07 demo, M-01/M-02 demo, A-01 demo, A-03 demo, E2E capstone. | Pre-baked environment |
| - [ ] | **Write facilitator timing guide** (`labs/facilitator-guide.md`) | 🟢 | Easy | Slide-to-lab mapping, session transition cues, pacing notes, common participant questions + answers. Pull from the delivery timeline docs. | 1-day delivery doc |
| - [ ] | **Capture fallback screenshots** | 🟢 | Medium | For every demo step: screenshot of expected GitHub Security tab state, CodeQL results, secret scanning alert, Autofix suggestion, MDC dashboard, agent output. Store in `labs/screenshots/` or upload as GitHub assets. | Pre-baked environment |

---

## Priority 2 — Core Labs (Required for 1-Day and 3-Day)

Write the actual lab guides. Each follows the lab template. Ordered by the 1-Day session sequence (the order participants encounter them).

### Foundation Labs

| # | Task | Tier | Difficulty | Description | Depends On |
|---|------|------|------------|-------------|------------|
| - [ ] | **Write F-01: Environment Setup** | 🟢 | Easy | Fork repo, enable GHAS, verify tools. Mostly references the participant setup guide with verification steps. | Participant setup guide |
| - [ ] | **Write F-02: Repository Walkthrough** | 🟢 | Easy | Guided tour: open key files, identify intentional vulnerabilities, understand repo structure. Include a "vulnerability scavenger hunt" checklist. | — |

### GHAS Labs (8 labs — the bulk of the work)

| # | Task | Tier | Difficulty | Description | Depends On |
|---|------|------|------------|-------------|------------|
| - [ ] | **Write G-01: Secret Scanning & Push Protection** | 🟢 | Medium | Steps: view existing alerts, attempt push with simulated secret, observe block, create custom pattern. Include exact secret patterns to use that trigger detection without being real credentials. | F-01 |
| - [ ] | **Write G-02: Code Scanning with CodeQL** | 🟢 | Medium | Steps: review pre-existing findings, trigger manual scan, create vulnerable PR (provide exact code to add), trace finding from alert to line. Include CWE reference table. | F-01 |
| - [ ] | **Write G-03: Dependency Review** | 🟢 | Medium | Steps: modify csproj to downgrade Newtonsoft.Json, create PR, observe Dependency Review block. Include the exact XML diff to apply. | F-01 |
| - [ ] | **Write G-04: SBOM Generation** | 🟢 | Easy | Steps: trigger both SBOM workflows, download artifacts, compare SPDX vs CycloneDX. Mostly observation + artifact inspection. | F-01 |
| - [ ] | **Write G-05: OpenSSF Scorecard** | 🟢 | Easy | Steps: trigger Scorecard workflow, review score dimensions, identify improvement actions. Mostly observation. | F-01 |
| - [ ] | **Write G-06: Container Scanning** | 🟢 | Medium | Steps: build Docker image, trigger Trivy + Grype, compare results. Include the exact `docker build` command and expected finding categories. | F-01, Docker installed |
| - [ ] | **Write G-07: Copilot Autofix** | 🟢 | Medium | Steps: find a CodeQL finding with Autofix available, generate fix, review suggestion, apply via PR. Include guidance on evaluating fix correctness. If Autofix isn't available for a finding, fall back to Copilot Chat manual fix. | G-02 (needs CodeQL results) |
| - [ ] | **Write G-08: Security Campaigns** | 🟡 | Medium | Steps: create campaign, target alerts, generate bulk fixes, track progress. Include fallback: demo-only path with screenshots if Enterprise features unavailable. | P0 (Campaigns availability) |

### IaC Labs

| # | Task | Tier | Difficulty | Description | Depends On |
|---|------|------|------------|-------------|------------|
| - [ ] | **Write I-01: Terraform Scanning with tfsec** | 🟢 | Medium | Steps: trigger tfsec workflow, review findings (top 10 with CIS mappings), for 3-Day: remediate 5 findings with exact code diffs provided. | F-01 |
| - [ ] | **Write I-02: Terraform Scanning with KICS** | 🟡 | Easy | Steps: trigger KICS workflow, compare with tfsec results, discuss tool complementarity. Shorter lab. | I-01 |
| - [ ] | **Write I-03: MSDO Multi-Tool Scan** | 🟡 | Easy | Steps: trigger MSDO workflow, review consolidated results from Bandit + Checkov + TemplateAnalyzer + Terrascan + Trivy. | F-01 |
| - [ ] | **Write I-04: Kubernetes Manifest Security** | 🟢 | Easy | Steps: run Kubesec against both manifests, compare scores, for 3-Day: fix `critical-double.yaml` (provide exact YAML diff). | F-01 |
| - [ ] | **Write I-05: Bicep Security Review** | 🟡 | Easy | Steps: use IaC Security Agent to review blueprints, compare with MSDO results. Agent-driven lab. | A-03 |

### Agent Labs

| # | Task | Tier | Difficulty | Description | Depends On |
|---|------|------|------------|-------------|------------|
| - [ ] | **Write A-01: Security Agent** | 🟢 | Medium | Steps: select agent in VS Code, run full assessment prompt, review report, compare with CodeQL results. Include 3-4 sample prompts with expected output excerpts. | F-01, VS Code + Copilot |
| - [ ] | **Write A-02: Security Reviewer Agent** | 🟢 | Medium | Steps: target specific files, review OWASP-mapped findings, evaluate severity accuracy. Include file-specific prompts. | A-01 |
| - [ ] | **Write A-03: IaC Security Agent** | 🟢 | Medium | Steps: scan Terraform directory, review CIS-mapped findings, compare with tfsec. Include compliance-focused prompts. | A-01 |
| - [ ] | **Write A-04: Pipeline Security Agent** | 🟢 | Medium | Steps: analyze workflows, get hardening recommendations, apply 3 fixes. Include before/after diffs. | A-01 |
| - [ ] | **Write A-05: Supply Chain Agent** | 🟢 | Medium | Steps: run governance audit, review secrets/dependencies/branch protection. Include governance checklist. | A-01 |
| - [ ] | **Write A-06: Security Plan Creator** | 🟡 | Medium | Steps: walk through all 5 phases, compare with existing security plan outputs. Longer lab. | A-01 |
| - [ ] | **Write A-08: Agent Orchestration** | 🟡 | Easy | Steps: review and trigger `security-agent-workflow.yml`, download report artifact, understand critical vuln gate. | A-01 |

### MDC Labs

| # | Task | Tier | Difficulty | Description | Depends On |
|---|------|------|------------|-------------|------------|
| - [ ] | **Write M-01: Connect GitHub to MDC** | 🟢 | Medium | Steps: configure GitHub connector in MDC portal, enable DevOps Security. Include screenshots for each portal step since MDC UI changes frequently. For 1-Day this is facilitator-demo only; for 3-Day it's hands-on. | Pre-baked environment (facilitator) or P0 (Azure/MDC model for participants) |
| - [ ] | **Write M-02: DevOps Security Dashboard** | 🟢 | Easy | Steps: navigate DevOps Security blade, code-to-cloud mapping, recommendations. Screenshot-heavy guide. | M-01 |

---

## Priority 3 — Extended Labs (Required for 3-Day and 5-Day)

### DAST & Deployment

| # | Task | Tier | Difficulty | Description | Depends On |
|---|------|------|------------|-------------|------------|
| - [ ] | **Write D-01: Deploy Vulnerable App** | 🟡 | Medium | Steps: deploy via Bicep blueprint, push Docker image to ACR, verify HTTPS access. Include exact `az` CLI commands and the Bicep parameter overrides. | Setup script, Azure subscription |
| - [ ] | **Write D-02: ZAP Scan** | 🟡 | Medium | Steps: modify ZAP workflow target URL, trigger scan, review SARIF. Include the workflow YAML edit needed. | D-01 |
| - [ ] | **Write D-03: Log Forging Demo** | 🟡 | Easy | Steps: craft URL with CRLF payload, observe log injection, discuss detection. Provide the exact URL to use. | D-01 |

### Remediation Labs

| # | Task | Tier | Difficulty | Description | Depends On |
|---|------|------|------------|-------------|------------|
| - [ ] | **Write R-01: Fix Code Vulnerabilities** | 🟡 | Hard | For each of the 7+ vulnerabilities: describe the issue, show the vulnerable code, provide the fix with explanation, show the verification step. This is the most content-heavy lab. | G-02 |
| - [ ] | **Write R-02: Harden Terraform** | 🟡 | Medium | Top 10 Terraform fixes with exact HCL diffs, tfsec re-scan verification. | I-01 |
| - [ ] | **Write R-03: Harden Pipelines** | 🟡 | Medium | SHA pinning steps (how to find the SHA for a tag), permission scoping examples, environment protection rules. | A-04 |
| - [ ] | **Write R-04: Compliance Mapping** | 🟡 | Hard | Provide pre-built mapping worksheets for CIS Azure Benchmark v2.0. Include 10+ finding-to-control mappings as worked examples. Participants complete the rest. | I-01, G-02 |

### Before/After Remediation Examples

| # | Task | Tier | Difficulty | Description | Depends On |
|---|------|------|------------|-------------|------------|
| - [ ] | **Create code remediation examples** (`labs/remediation-examples/code/`) | 🟡 | Medium | Paired files for each vulnerability: `hardcoded-creds-before.cs` / `hardcoded-creds-after.cs`, `log-forging-before.cs` / `log-forging-after.cs`, etc. 7 pairs total. | — |
| - [ ] | **Create IaC remediation examples** (`labs/remediation-examples/iac/`) | 🟡 | Easy | Paired Terraform snippets: `open-nsg-before.tf` / `open-nsg-after.tf`, `hardcoded-password-before.tf` / `hardcoded-password-after.tf`, etc. 5 pairs. | — |
| - [ ] | **Create K8s remediation examples** (`labs/remediation-examples/kubernetes/`) | 🟡 | Easy | Paired manifests: `privileged-pod-before.yaml` / `privileged-pod-after.yaml`. 1 pair (the existing manifests can almost serve as-is). | — |
| - [ ] | **Create pipeline remediation examples** (`labs/remediation-examples/pipelines/`) | 🟡 | Easy | Paired workflow snippets: `unpinned-actions-before.yml` / `pinned-actions-after.yml`, `broad-permissions-before.yml` / `scoped-permissions-after.yml`. 2-3 pairs. | — |

### Advanced MDC Labs

| # | Task | Tier | Difficulty | Description | Depends On |
|---|------|------|------------|-------------|------------|
| - [ ] | **Write M-03: CSPM & Attack Paths** | 🟡 | Medium | Steps: review Secure Score, explore attack path analysis, trace code→cloud path. Screenshot-heavy. | M-01, Defender CSPM enabled |
| - [ ] | **Write M-04: Runtime Threat Detection** | 🔴 | Hard | Steps: enable Defender plans, simulate suspicious activity, review alerts. Complex because alert generation is non-deterministic and timing-dependent. Include pre-staged alert screenshots as fallback. | M-01, deployed app, Defender plans |

---

## Priority 4 — 5-Day Exclusive Content

### Custom Agent Workshop

| # | Task | Tier | Difficulty | Description | Depends On |
|---|------|------|------------|-------------|------------|
| - [ ] | **Write A-07: Build Your Own Custom Agent** | 🔴 | Hard | Design workshop: agent purpose selection, system prompt authoring, scope definition, output format, testing methodology. Include 3 example agent concepts with starter prompts. This is the most creative lab — needs worked examples. | A-01 through A-05 (familiarity) |

### Customer Integration Guides

| # | Task | Tier | Difficulty | Description | Depends On |
|---|------|------|------------|-------------|------------|
| - [ ] | **Write customer onboarding playbook** (`templates/customer-onboarding-playbook.md`) | 🔴 | Hard | Guide for integrating GHAS/MDC with customer's existing repos: how to enable GHAS on an existing org, how to configure CodeQL for various tech stacks (Java, Python, JS, Go, C#), how to handle existing Dependabot alerts, how to connect customer Azure to MDC. Needs to be flexible enough for any customer stack. | P0 decisions |
| - [ ] | **Write org-level configuration guide** (`labs/org-level-ghas-config.md`) | 🔴 | Medium | Steps: configure org security policies, code scanning defaults, secret scanning org-wide, required workflows, Security Overview dashboard. | P0 (licensing) |

### Deliverable Templates

| # | Task | Tier | Difficulty | Description | Depends On |
|---|------|------|------------|-------------|------------|
| - [ ] | **Write PoC results template** (`templates/poc-results-report.md`) | 🔴 | Medium | Structured report: executive summary, scope, findings by category (with placeholder tables), remediation metrics, tool effectiveness comparison, agent value assessment, compliance posture, risk assessment, recommendation, adoption roadmap summary. Include guidance text in each section telling the author what to write. | — |
| - [ ] | **Write adoption roadmap template** (`templates/adoption-roadmap.md`) | 🔴 | Medium | 4-phase template: Pilot → Team → Org → Maturity. Each phase: scope, tools, agents, success metrics, team, dependencies. Include fill-in-the-blank sections. | — |
| - [ ] | **Create executive readout deck template** (`templates/executive-readout/`) | 🔴 | Medium | Slide template (PPTX or markdown-based): title slide, scope, key findings (with chart placeholder), before/after posture, agent demo screenshot, recommendation, roadmap summary, next steps. Keep it to 10-12 slides. | — |
| - [ ] | **Write compliance mapping worksheets** (`templates/compliance-worksheets/`) | 🔴 | Hard | Pre-built tables for CIS Azure v2.0, NIST 800-53 Rev 5, SOC 2 Type II. Each table: control ID, control description, automated evidence source (which scanning tool), status (met/not met/partial), remediation action. Partially pre-filled with common scan-to-control mappings. | R-04 |

---

## Priority 5 — Polish & Enhancement

| # | Task | Tier | Difficulty | Description | Depends On |
|---|------|------|------------|-------------|------------|
| - [ ] | **Write labs/README.md** | 🟢 | Easy | Lab catalog: table of all 35 labs with ID, title, duration, difficulty, tier requirement, and link to the guide. | All lab guides |
| - [ ] | **Create participant workbook** | 🟡 | Medium | PDF companion: exercise summaries, tool cheat sheets (gh CLI, az CLI, CodeQL queries, tfsec rules), note-taking space per session. Could be markdown rendered to PDF. | All lab guides |
| - [ ] | **Update slide deck with demo transitions** | 🟢 | Medium | Add "DEMO" transition slides after slides 37, 43, 61, 70, 84. Add speaker notes referencing the demo runbook. Update slides 85-88 with structured demo section. | Demo runbook |
| - [ ] | **Update wiki** | 🟢 | Medium | Update the project wiki per `wiki.instructions.md` with links to lab guides, delivery timelines, and the expansion plan. | Lab guides, delivery docs |
| - [ ] | **Write executive summary handout** | 🟢 | Easy | 2-page PDF: value proposition, capability matrix, ROI metrics. Base on existing `Agentic DevSecOps with GitHub Advanced Security.docx`. | — |
| - [ ] | **Record video walkthroughs** (optional) | 🟡 | Hard | Short (5-10 min) video per lab for async/self-paced delivery. Record: facilitator running through each lab with narration. | All lab guides, pre-baked environment |

---

## Execution Summary

### By the numbers

| Priority | Items | Blocking? | Estimated Total Effort |
|----------|-------|-----------|----------------------|
| **P0 — Decisions** | 12 decisions | Yes — blocks everything | 1 meeting (30 min) |
| **P1 — Foundation** | 11 tasks | Yes — blocks all labs | Medium (scaffolding, scripts, runbook, screenshots) |
| **P2 — Core Labs** | 24 tasks | Yes — required for 1-Day | Large (the bulk of content authoring) |
| **P3 — Extended Labs** | 16 tasks | Required for 3-Day | Medium-Large |
| **P4 — 5-Day Content** | 8 tasks | Required for 5-Day | Medium |
| **P5 — Polish** | 6 tasks | Nice-to-have | Medium |
| **Total** | **77 tasks** | | |

### Minimum viable delivery

| Tier | Required Priorities | Tasks | Can Deliver With |
|------|-------------------|-------|-----------------|
| **1-Day Demo** | P0 + P1 + P2 (partial: F-01, F-02, G-01–G-07, I-01, I-04, A-01–A-05, M-01, M-02) | ~35 tasks | Demo runbook + facilitator guide + pre-baked environment + 18 lab guides |
| **3-Day Light PoC** | P0 + P1 + P2 (all) + P3 | ~51 tasks | Everything above + DAST labs + remediation labs + before/after examples + MDC advanced |
| **5-Day Full PoC** | P0 + P1 + P2 + P3 + P4 | ~59 tasks | Everything above + custom agent workshop + customer playbook + templates |

### Recommended execution order

```
Week 1:  P0 decisions → P1 foundation (directory structure, scripts, setup guide)
Week 2:  P1 continued (demo runbook, screenshots, pre-baked env) → P2 start (F-01, F-02, G-01, G-02)
Week 3:  P2 continued (G-03–G-07, I-01, I-04) → P2 agents (A-01–A-05)
Week 4:  P2 finish (M-01, M-02, G-08, I-02, I-03, I-05, A-06, A-08) → 1-Day is deliverable
Week 5:  P3 (D-01–D-03, R-01–R-04, remediation examples, M-03) → 3-Day is deliverable
Week 6:  P4 (A-07, customer playbook, templates, M-04) → 5-Day is deliverable
Week 7:  P5 polish (README, workbook, slide updates, wiki)
```

### Critical path

```
P0 Decisions
  └──→ P1 Participant Setup Guide
         └──→ P1 Setup Script
                └──→ P2 F-01 Lab Guide
                       └──→ P2 G-01, G-02 (first labs participants hit)
                              └──→ P2 G-07 (depends on CodeQL results from G-02)
                                     └──→ P3 R-01 (depends on knowing the findings)

P1 Pre-baked Environment (parallel)
  └──→ P1 Demo Runbook
         └──→ P1 Fallback Screenshots
                └──→ 1-Day is deliverable
```

The pre-baked environment and lab guides can be authored in parallel by different people. The critical bottleneck is getting P0 decisions made — everything else flows from there.
