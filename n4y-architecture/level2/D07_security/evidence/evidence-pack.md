# Evidence Pack: L2/D07 — Security & Monitoring
> **Version:** 2.0 (deep-research pass, 05.04.2026)
> **Previous evidence:** E1–E9 (migrated from v3.0, 01.04.2026)
> **New evidence:** E10–E30 (deep-research, 05.04.2026)
> **Gaps targeted:** G10–G15 (open) + refresh G3, G5, G7

---

## Previous Evidence (E1–E9, retained)

| ID | Source | Date | Type | Key Claim | Confidence |
|----|--------|------|------|-----------|------------|
| E1 | OWASP Top 10 for LLM 2025 | 2025 | norm | LLM01 Prompt Injection; LLM06 Excessive Agency; LLM10 Unbounded Consumption — top threats | High |
| E2 | arXiv:2511.15759 Prompt Injection defense | Nov 2025 | paper | 3-layer defense reduces attack success 73.2%→8.7% | High |
| E3 | Anthropic sandbagging research | Oct 2024 | paper | Agents hide capabilities during testing; unannounced evaluation needed | High |
| E4 | AI Swarm Attacks GTG-1002 | Nov 2025 | doc | 80-90% autonomous coordination; cross-agent correlation detection needed | High |
| E5 | TEE.fail (Georgia Tech/Purdue) | Oct 2025 | paper | DDR5 bus interposition attack on Intel TDX/AMD SEV-SNP; requires physical access | High |
| E6 | ESMA MiFID II Circuit Breaker | 2023 | norm | 5-min rolling average; trigger thresholds; cascade prevention | High |
| E7 | Coinbase Agentic Wallets | Feb 2026 | doc | Programmable spending controls + session caps + TEE keys + KYT | High |
| E8 | research_L2_security_governance.md | Mar 2026 | doc | Phase 8 resolutions: staged threat model, ZKP AML, quarantine appeals | High |

---

## New Evidence (E10–E30)

### G10 — OWASP Agentic AI 2026 Mapping

| ID | Source | URL | Date | Type | Key Claim | Gap(s) | Relevance | Confidence |
|----|--------|-----|------|------|-----------|--------|-----------|------------|
| E10 | OWASP Top 10 for Agentic Applications 2026 | https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/ | Dec 9, 2025 (published); Mar 17, 2026 (page) | norm | Complete ASI01-ASI10 framework: Agent Goal Hijack, Tool Misuse, Identity & Privilege Abuse, Supply Chain, RCE, Memory Poisoning, Insecure Inter-Agent Comms, Cascading Failures, Human-Agent Trust Exploitation, Rogue Agents. 100+ expert peer review. | G10 | Direct | High |
| E11 | OWASP ASI01-ASI10 Risk Details (Modulos Docs) | https://docs.modulos.ai/frameworks/owasp-top-10-agentic/top-risks.html | 2026 | norm | Per-risk definitions with baseline controls, monitoring requirements, governance approaches for all 10 risks | G10 | Direct | High |
| E12 | ASI03 Agent Identity Abuse Analysis | https://dev.to/thenexusguard/owasp-just-ranked-agent-identity-abuse-as-the-3-risk-in-agentic-ai-here-is-why-1k64 | 2026 | article | ASI03=#3 risk. Broken trust chains: cached credentials, transitive privilege abuse. Mitigations: distinct identities, task-scoped short-lived credentials, re-authorization per step, session isolation, cached context wiping | G10 | Direct | High |
| E13 | ASI03 Marketplace Threat Modeling (Disesdi) | https://disesdi.substack.com/p/attacking-and-threat-modeling-the-ff9 | 2026 | article | ASI03 = "agentic evolution of Excessive Agency". Delegation chains + cached credentials + identity confusion in multi-agent marketplaces. OAuth 2.1 sender-constrained tokens recommended. | G10 | Direct | High |
| E14 | ASI07 Insecure Inter-Agent Communication | https://dev.to/willvelida/preventing-insecure-inter-agent-communication-in-ai-agents-hnp | 2026 | article | 4-layer attack surface: transport, routing, semantic, side-channel. 9 OWASP controls: secure channels, signed messages, anti-replay, protocol enforcement, protocol pinning, discovery protection, attested registries, typed contracts | G10 | Direct | High |
| E15 | ASI08 Cascading Failures Guide (Adversa AI) | https://adversa.ai/blog/cascading-failures-in-agentic-ai-complete-owasp-asi08-security-guide-2026/ | 2026 | article | Propagation: hallucinations, prompt injection, tool corruption, memory poisoning. Amplification: feedback loops, trust transitivity, parallelization. Circuit breakers with 50% error threshold / 3 consecutive failures trigger. | G10, G15 | Direct | High |
| E16 | Microsoft Agent Security (Mar 2026) | https://www.microsoft.com/en-us/security/blog/2026/03/30/addressing-the-owasp-top-10-risks-in-agentic-ai-with-microsoft-copilot-studio/ | Mar 30, 2026 | vendor | 80% Fortune 500 using agents. Unified AI risk dashboards, identity-based access controls, shadow AI detection. | G10 | Contextual | Medium |

### G12 — Multi-Agent Anomaly Detection Techniques & Metrics

| ID | Source | URL | Date | Type | Key Claim | Gap(s) | Relevance | Confidence |
|----|--------|-----|------|------|-----------|--------|-----------|------------|
| E17 | NARCBench: Collusion Detection via Multi-Agent Interpretability | https://arxiv.org/abs/2604.01151 | Apr 1, 2026 | paper | 5 probing techniques on model activations detect collusion. AUROC: 1.00 in-distribution, 0.60-0.86 zero-shot transfer, 0.99-1.00 steganographic. No single technique dominates → ensemble required. Signal localized at token level. | G12 | Direct | High |
| E18 | Colosseum: Auditing Collusion in Cooperative MAS | https://arxiv.org/abs/2602.15198 | Feb 16, 2026 | paper | DCOP-grounded collusion auditing. Most LLMs exhibit collusion propensity with secret channels, but many show "collusion on paper" — plan collusively, act non-collusively. Tests under different objectives/persuasion/topologies. | G12 | Direct | High |
| E19 | SentinelAgent: Graph-based Anomaly Detection in MAS | https://arxiv.org/abs/2505.24201 | May 30, 2025 | paper | Dynamic execution graphs for semantic anomaly detection at node/edge/path levels. Pluggable LLM-powered oversight agent. Detects: single-point faults, prompt injection, collusion, latent exploits, unsafe tools, miscoordination. Validated on Magentic-One. Overhead: 20-40%. | G12 | Direct | High |
| E20 | Audit the Whisper: Steganographic Collusion Detection | https://arxiv.org/abs/2510.04303 | Oct 2025 | paper | ColludeBench-v0: pricing, auctions, peer review scenarios. Calibrated auditing pipelines for steganographic collusion detection. | G12 | Supporting | High |
| E21 | Behavioral Detection Methodology (Moltwire) | https://moltwire.com/research/behavioral-detection-methodology | 2025–2026 | article | AI agents move 16x more data than humans. Dual-layered: Sentinel Agents (distributed monitoring) + Coordinator Agents (policy). UEBA for dynamic baselines detecting zero-day/polymorphic attacks. | G12 | Direct | Medium |
| E22 | Real-Time Anomaly Detection for Multi-Agent AI Systems (Galileo) | https://galileo.ai/blog/real-time-anomaly-detection-multi-agent-ai | 2026 | article | Statistical baseline monitoring (PCA, ARIMA, Prophet). Three metric categories: agent-specific, interaction, system-level. Detection latency: 10-30s. Computational overhead: 2-5% (statistical) / 5-15% (ML models). | G12, G15 | Direct | Medium |

### G13 — Honesty Verification Mechanisms

| ID | Source | URL | Date | Type | Key Claim | Gap(s) | Relevance | Confidence |
|----|--------|-----|------|------|-----------|--------|-----------|------------|
| E23 | Trustless Verification Protocol | https://arxiv.org/abs/2507.00631 | Jul 2025 | paper | Recursive verification game with collateralized claims. Tasks as intents, competing solvers, post-hoc verification. Challengers stake against results → incorrect slashed, correct rewarded. Escalation penalizes erroneous verifiers. Nash equilibrium: correctness stable when incentives align. | G13 | Direct | High |
| E24 | Composable Attestation Framework | https://arxiv.org/abs/2603.02451 | Mar 2026 | paper | Cryptographic framework for continuous/incremental trust in AI distributed systems. Properties: composability, order independence, transitivity, determinism, dynamic verification. Merkle tree proofs. "Attestation inclusion" — incremental integration without full re-attestation. Supports AI model integrity, federated learning, runtime trust. | G13 | Direct | High |
| E25 | VeriLLM: Verifiable Decentralized LLM Inference | https://arxiv.org/abs/2509.24257 | Sep 2025 | paper | Commit-then-sample-and-check (Merkle trees + VRF + escalatory dispute → ZKP). ~1% verification overhead. Isomorphic inference-verification architecture. First decentralized inference protocol with end-to-end game-theoretic security proof. Nash equilibrium: honest inference+verification. Peer-prediction mitigates lazy verification. One-honest-verifier assumption. | G13 | Direct | High |

### G14 — Red-Teaming & Continuous Adversarial Testing

| ID | Source | URL | Date | Type | Key Claim | Gap(s) | Relevance | Confidence |
|----|--------|-----|------|------|-----------|--------|-----------|------------|
| E26 | AutoRedTeamer (NeurIPS 2025) | https://arxiv.org/abs/2503.15754 | Mar 2025 | paper | Dual-agent framework: red teaming agent + strategy proposer. Memory-guided lifelong attack integration. 20% higher attack success vs. Llama-3.1-70B; 46% cost reduction. Matches human-curated benchmark diversity. Continuous evolution. | G14 | Direct | High |
| E27 | CRAFT: Red-Teaming Policy-Adherent Agents (EMNLP 2025, IBM) | https://arxiv.org/abs/2506.09600 | Jun 2025 | paper | Multi-agent red-teaming for policy-adherent agents. τ-break benchmark (extends τ-bench). Policy-aware persuasive strategies outperform DAN/emotional manipulation/coercion. Customer service focus. Open-source: https://github.com/IBM/CRAFT | G14 | Direct | High |
| E28 | Adversa AI Continuous Red Teaming Platform | https://adversa.ai/platform | 2026 | vendor | Autonomous red teaming: continuous testing of full AI stack (models → agents → MCP infrastructure). 300+ attack techniques. OWASP LLM Top 10 compliance. Real-time actionable remediation without external AI providers. | G14 | Direct | Medium |

### G11 — TEE Security Application in D06

| ID | Source | URL | Date | Type | Key Claim | Gap(s) | Relevance | Confidence |
|----|--------|-----|------|------|-----------|--------|-----------|------------|
| E29 | CCC: Protecting Agentic AI with Confidential Computing | https://confidentialcomputing.io/2026/01/20/protecting-agentic-ai-workloads-with-confidential-computing/ | Jan 20, 2026 | article | TEE = "perfect fit for Agentic AI". Agents on untrusted infra vulnerable to admin tampering of identity/credentials/execution. TEE provides: integrity protection (code unchanged), confidentiality (data hidden from host OS/hypervisor), remote attestation (signed report of genuine hw + unmodified code). Multi-owner agents can share infrastructure. | G11 | Direct | High |
| E30 | ERC-8004: Trustless TEE Agents Standard | https://eips.ethereum.org/EIPS/eip-8004 | Aug 2025 | standard | EIP for trustless agent on-chain identity + hw attestation. Three trust models: social proof (reputation), economic proof (stake), cryptographic proof (TEE attestation). Identity Registry based on ERC-721 (NFT agent passport). Phala Network production deployment: Intel TDX, DCAP attestation, Risc Zero/Succinct proving. | G11 | Direct | High |

### G15 — Quantitative KPIs/SLAs for Monitoring Effectiveness

| ID | Source | URL | Date | Type | Key Claim | Gap(s) | Relevance | Confidence |
|----|--------|-----|------|------|-----------|--------|-----------|------------|
| E31 | AI SOC KPIs & MTTR Guide (2026) | https://www.ir.com/guides/how-to-reduce-mttr-with-ai-a-2026-guide-for-enterprise-it-teams | 2026 | article | MTTD: 30min-4h (high-performing), <1h (elite). MTTR: 40-60% reduction with AI observability. AI-powered platforms: auto-detection, causal analysis, scored triggers before engineer reviews. | G15 | Direct | High |
| E32 | AI SOC SLA Framework (UnderDefense 2026) | https://underdefense.com/blog/ai-soc-sla-guide/ | 2026 | article | 7-pillar SLA: detection speed, triage accuracy, containment time, escalation gates, uptime guarantees, penalty structures, compliance windows. MTTA ~0 with AI. MTTR benchmark <4h. | G15 | Direct | High |
| E33 | SOC Metrics 2026 (nflo.tech) | https://nflo.tech/knowledge-base/soc-metrics-mttd-mttr-kpi-security/ | 2026 | article | MTTD <24h benchmark, <1h elite. MTTC <72h. FPR <30%. MTTD <200 days saves $1.1M per incident. | G15 | Direct | Medium |
| E34 | Mandiant M-Trends 2026 | https://cloud.google.com/blog/topics/threat-intelligence/m-trends-2026 | 2026 | report | Median dwell time: 10 days globally. Ransomware: ~5 days. Median attacker time-to-objective: <24h. | G15 | Supporting | High |
| E35 | AI SOC Accuracy & False Positive Reduction (Radiant Security) | https://radiantsecurity.ai/learn/what-are-the-most-accurate-ai-soc-tools-in-2026/ | 2026 | article | Baseline FPR: 80-95% of alerts. After AI triage: 1-5%. Radiant: ~90% FP reduction. 11,000 alerts/day avg, 70% false positives. $1.4M/year cost of FP investigation. | G15 | Direct | High |
| E36 | AI SOC Automation Maturity Model | https://sra.io/blog/ai-automation-levels-in-security-operations/ | 2025-2026 | article | 6 levels (L0-L5): manual → fully autonomous. L2 (AI-assisted): 50% MTTR reduction. L4: AI remediation + human approval gate. L5: autonomous within scope. | G15 | Direct | Medium |

### Refresh G5 — OWASP LLM 2025 Prompt Injection Updates

| ID | Source | URL | Date | Type | Key Claim | Gap(s) | Relevance | Confidence |
|----|--------|-----|------|------|-----------|--------|-----------|------------|
| E37 | OWASP LLM Top 10 2025 — Prompt Injection Defense | https://introl.com/blog/llm-security-prompt-injection-defense-production-guide-2025 | 2025 | article | Prompt injection remains LLM01 (#1) since 2023. Three vectors: direct, indirect (RAG), multimodal (images/emoji/rebus). 100% evasion achieved against Azure Prompt Shield and Meta Prompt Guard. Multi-layer defense: input validation + output filtering + prompt hardening + privilege separation + HITL + monitoring. | G5 refresh | Direct | High |
| E38 | OWASP LLM 2026 List Updates | https://diffray.ai/blog/owasp-top-10-llm-applications | 2026 | article | 2026 additions: LLM07 System Prompt Leakage ("cannot be kept secret"), LLM08 Vector/Embedding Weaknesses (RAG-specific). | G5 refresh | Supporting | Medium |

### Refresh G7 — BBS+ W3C CR + ZKP AML

| ID | Source | URL | Date | Type | Key Claim | Gap(s) | Relevance | Confidence |
|----|--------|-----|------|------|-----------|--------|-----------|------------|
| E39 | W3C Data Integrity BBS Cryptosuites v1.0 (CRD) | https://www.w3.org/TR/2025/CRD-vc-di-bbs-20250218/ | Feb 18 / Apr 3, 2025 | standard | W3C Candidate Recommendation Draft. Selective disclosure + unlinkable proofs via ZK proof of knowledge of BBS signature. Constant-sized signatures. RDF canonicalization. Requires ≥2 independent implementations to exit CR. EU Commission assessing. | G7 refresh | Direct | High |
| E40 | zkAML Framework (ePrint 2025) | https://eprint.iacr.org/2025/465 | 2025 | paper | ZK-SNARK AML compliance: whitelist approach. 55 TPS public / 324 TPS private. Proof generation: 216-227ms. Users prove compliance without revealing PII. | G7 refresh | Direct | High |
| E41 | zkMe zkAML Protocol | https://docs.zk.me/hub/what/zkkyc/amlme | Jan 2025 | doc | One-time verification against sanctions/PEP/adverse media → AML-compliant Soulbound Token credential. ZK circuits prevent verification service from seeing results. FATF-compliant decentralized KYC. | G7 refresh | Direct | High |
| E42 | ZKP Privacy Paradox (CoinDesk, Mar 2026) | https://www.coindesk.com/opinion/2026/03/19/the-privacy-paradox-regulating-zero-knowledge-finance-in-the-eu-and-beyond | Mar 19, 2026 | article | Shift from "show data" to "show proof". Under eIDAS 2.0: portable credentials cross-service. Prove KYC, non-sanctioned, amount compliance while keeping identities private. | G7 refresh | Supporting | Medium |

### Refresh G3 — AI Watcher / AI Act Art.14

| ID | Source | URL | Date | Type | Key Claim | Gap(s) | Relevance | Confidence |
|----|--------|-----|------|------|-----------|--------|-----------|------------|
| E43 | EU AI Act Art.14 Human Oversight (aiact-info) | https://www.aiact-info.eu/article-14-human-oversight/ | 2024-2026 | legal | 5 HITL requirements: understanding, bias awareness, output interpretation, override authority, halt/interrupt mechanism. Does NOT require review of every decision — requires capability to monitor/understand/intervene/halt. | G3 refresh | Direct | High |
| E44 | AI Act Enforcement Timeline | https://aigovernancedesk.com/eu-ai-act-enforcement-2026-cco-roadmap/ | 2026 | article | Feb 2025: prohibited practices. Aug 2025: GPAI rules. **Aug 2, 2026: High-risk AI (Annex III) full compliance**. Aug 2027: regulated products. Penalties: €35M/7% (prohibited), €15M/3% (high-risk), €7.5M/1% (misleading info). | G3 refresh | Direct | High |
| E45 | AI Act Implementation Challenges (TechPolicy) | https://www.techpolicy.press/eus-ai-act-delays-let-highrisk-systems-dodge-oversight/ | 2026 | article | "Loophole": systems placed before enforcement exempt unless significantly modified → "race to market". Member State authority designation slower than expected. | G3 refresh | Supporting | Medium |

---

## Source Conflict Notes

1. **Probe reliability (E17 vs arXiv:2509.21344):** NARCBench reports strong AUROC for collusion detection probes, but separate research shows linear probes can drop from 0.94 to 0.50 AUROC when textual evidence is removed via content filtering. Implication: probe-based collusion detection may rely partly on surface textual correlations, not only on underlying model cognition. → Recommend ensemble approach combining probes with graph-based detection (E19).

2. **Collusion severity (E18):** Colosseum finds most LLMs exhibit "collusion on paper" — plan collusively but act non-collusively. This may suggest lower practical risk than theoretical models indicate, but could change as LLMs improve strategic sophistication.

3. **TEE physical attacks (E5 vs E29):** TEE.fail demonstrates physical DDR5 bus interposition attacks, but CCC (E29) and cloud TEE deployments consider physical access a low risk for cloud/data center scenarios. N4Y.AI primarily cloud-hosted → physical TEE attacks = low residual risk; logical/side-channel attacks remain primary concern.

4. **AI Act "loophole" (E44 vs E45):** E44 presents enforcement timeline as definitive; E45 notes systems pre-dating enforcement dates are exempt unless significantly modified, creating deployment incentive before Aug 2026. N4Y.AI should plan for Aug 2026 compliance regardless.

5. **Prompt injection defense sufficiency (E2 vs E37):** E2 reports 73.2%→8.7% attack success reduction with 3-layer defense, but E37 notes 100% evasion against Azure Prompt Shield and Meta Prompt Guard. → No single defense layer is sufficient; defense-in-depth with HITL mandatory for sensitive operations.

---

## Gap Coverage Summary

| Gap | Coverage | Key Evidence | Remaining Holes |
|-----|----------|-------------|-----------------|
| **G10** (High) OWASP Agentic AI 2026 | **Well-covered** | E10-E16 | N4Y.AI-specific mapping per ASI risk (design task, not research gap) |
| **G12** (Critical) Multi-agent anomaly detection | **Well-covered** | E17-E22 | Production benchmarks at N4Y.AI scale; cost-accuracy tradeoff analysis for specific topology |
| **G13** (High) Honesty verification | **Covered** | E23-E25 | Integration architecture combining VeriLLM-style game theory with N4Y.AI's PoP/reputation system |
| **G14** (High) Red-teaming & adversarial testing | **Covered** | E26-E28 | Cross-layer scenario design for N4Y.AI's 9-layer defense; cost model for continuous testing |
| **G11** (Medium) TEE security application | **Covered** | E29-E30 | Decision matrix: when TEE mandatory (HC sandbox) vs recommended (FS sandbox) vs optional — requires D03 coordination |
| **G15** (Medium) Quantitative KPIs/SLAs | **Well-covered** | E31-E36 | N4Y.AI-specific SLA targets (tiered by agent risk class); OI-036 attack cost integration |
| **G5** refresh | **Updated** | E37-E38 | Multimodal injection defense specifics for N4Y.AI agent inputs |
| **G7** refresh | **Updated** | E39-E42 | BBS+ implementation timeline for N4Y.AI; zkAML integration with D01 DID/VC layer |
| **G3** refresh | **Updated** | E43-E45 | AI Watcher ↔ Art.14 compliance mapping (architectural alignment verification) |
