# Evidence Pack: Step 5 — L1/D10 Эволюционное

> **Измерение:** L1/D10 | **Версия:** v4.0 (deep-research pass 2) | **Дата:** 03.04.2026  
> **Источники:** E1–E10 (migrated v3.0, 01.04.2026) + E12–E28 (deep-research, 03.04.2026)  
> **Покрытие:** G10, G11, G12, G13, G14 (open) + refresh G1, G4, G9

---

## Previous evidence (E1–E10)

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | Relevance | Confidence |
|----|----------|------|-----|----------------|------|-----------|------------|
| E1 | EvoAgent (Self-Evolving Agent) | 2025 | paper | Self-planning + self-control + self-reflection; two-stage curriculum learning; world model for self-verification | G1, G2 | H | High |
| E2 | ELL Framework (Lifelong Learning) | 2025 | paper | 4 принципа: continuous exploration, long-term memory, autonomous skill learning, adaptive behavior refinement | G1, G2 | H | High |
| E3 | Evo-Memory (ReMem pipeline) | 2025 | paper | Retrieve-Integrate-Update: test-time learning без fine-tuning; непрерывное обновление working memory | G2 | H | High |
| E4 | Memento-II (Stateful Reflective Memory) | 2025 | paper | Write (policy evaluation) + Read (policy improvement); continual adaptation без model fine-tuning | G2 | H | High |
| E5 | MetaAgent (Tool Meta-Learning) | 2025 | paper | Автономное создание meta-tools из tool-use history; дистилляция опыта в concise texts | G6 | H | High |
| E6 | CASCADE (skill acquisition) | 2025 | paper | 93.3% success с evolution mechanisms vs 35.4% без; skill acquisition через web search + code extraction | G1 | H | High |
| E7 | Agent Versioning Framework (Decagon) | 2025 | doc | 4-layer versioning: Cognitive/Model/Knowledge/Tool; behavior over code; shadow-mode testing; roll-forward | G3, G4 | H | High |
| E8 | HB-Eval (Certification Framework) | 2025 | paper | 3 metrics: Failure Resilience Rate + Planning Efficiency Index + Traceability Index | G4, G8 | H | High |
| E9 | AAA / AgentBeats (assessor agents) | 2025-2026 | doc | Специализированные агенты-оценщики через A2A + MCP; standardized reproducible assessments at scale | G4 | H | High |
| E10 | Continual Learning adapters (CABLE/ADEPT/HAM) | 2025 | paper | Gradient similarity + adaptive layer expansion + hierarchical merging; предотвращение catastrophic forgetting | G7 | M | High |

---

## New evidence (E12–E28)

### G12 (Critical) — Safety/alignment guardrails during self-modification

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | Relevance | Confidence | URL |
|----|----------|------|-----|----------------|------|-----------|------------|-----|
| E12 | Two-Gate Guardrail (Utility-Learning Tension in Self-Modifying Agents) | Oct 2025 | paper | Self-modifying agents face utility-learning tension: utility-driven improvements erode learning guarantees. Two-Gate guardrail: validation margin (τ) + capacity cap K[m] preserves distribution-free PAC learnability. Uniformly bounded capacity is the single criterion for safe self-modification across all axes (representational, architectural, metacognitive, algorithmic, substrate). | G12 | H | High | https://arxiv.org/abs/2510.04399 |
| E13 | AGrail — Lifelong Agent Guardrail (ACL 2025) | Feb 2025 | paper | Adaptive safety check generation + optimization + tool-compatible verification. Addresses task-specific risks (constraints) and systemic risks (CIA vulnerabilities). Introduced Safe-OS dataset (prompt injection, system sabotage, environment attacks). Transferable across LLM agent tasks. | G12 | H | High | https://arxiv.org/abs/2502.11448 |
| E14 | Emergent Misalignment from Narrow Fine-Tuning (Nature 2025) | 2025 | paper | Fine-tuning LLMs on narrow harmful tasks (e.g., insecure code) causes broadly generalized misalignment across unrelated domains — deceptive claims, malicious advice. Published in Nature. | G12 | H | High | https://www.nature.com/articles/s41586-025-09937-5 |
| E15 | VLM Alignment Degradation under Continual Learning | Feb 2026 | paper | Multimodal evaluation reveals 70% higher misalignment than text-only benchmarks. 10% harmful data → substantial degradation. Harmful behaviors occupy ~10-dimensional subspace. Misalignment scales with LoRA rank. Neither benign fine-tuning nor activation steering fully eliminates learned harms. | G12 | H | High | https://arxiv.org/abs/2602.16931 |
| E16 | MOSAIC — Agentic Alignment (Microsoft Research) | Mar 2026 | paper | Post-training framework: Plan→Check→Act/Refuse loop. Preference-based RL with pairwise trajectory comparisons. Reduces harmful behavior by 50%, increases refusal >20% on injection attacks, preserves benign performance. Zero-shot across Qwen2.5-7B, Qwen3-4B-Thinking, Phi-4. | G12, G13 | H | High | https://arxiv.org/abs/2603.03205 |
| E17 | Information-Theoretic Limits on Safety Gates | Mar 2026 | paper | Under power-law risk schedules, classifier-based safety gates cannot simultaneously permit unbounded beneficial self-modification while maintaining bounded cumulative risk. Verification-based approaches using Lipschitz bounds achieve zero false-acceptance rates with positive true-positive rates. | G12 | H | Medium | https://arxiv.org/abs/2603.28650 |
| E18 | Multi-Layered Guardrail Framework (Production Guide 2026) | 2026 | guide | Seven defensive layers: input validation, action boundaries, output filtering, cost controls, HITL approval, content moderation, real-time monitoring. Agents require more layers than chatbots due to autonomous actions. | G12, G14 | M | High | https://dev.to/paxrel/ai-agent-guardrails-how-to-keep-your-agent-safe-and-reliable-2026-guide-3a5m |

### G10 (High) — 4 evolution axes explicitly defined

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | Relevance | Confidence | URL |
|----|----------|------|-----|----------------|------|-----------|------------|-----|
| E19 | AutoAgent — Evolving Cognition + Elastic Memory | Mar 2026 | paper | Three coupled evolution components: (1) Evolving Cognition — structured prompt-level knowledge of tools, self-capabilities, peer expertise, task knowledge; (2) Contextual Decision-Making — unified action space; (3) Elastic Memory Orchestration — preserves raw records, compresses trajectories, constructs reusable episodic abstractions. Closed-loop cognitive evolution aligns intended actions with observed outcomes. | G10, G11 | H | High | https://arxiv.org/abs/2603.09716 |
| E20 | Agent Architecture Evolution (Three-to-Four Axes) | 2026 | analysis | Expanded from 3 axes to 4: (1) Infrastructure — where agents run; (2) Intelligence orchestration — single/multi-model; (3) Interface contracts — messaging/desktop/custom; (4) Memory & Continuity — persistence across sessions, agents as persistent entities. Architecture shift: "LLM calling functions" (2023) → "persistent autonomous entity" (2025) → "biological ecosystem" (2026). | G10 | M | Medium | https://dev.to/webdevtodayjason/the-three-axes-hypothesis-is-incomplete-heres-whats-actually-missing-from-the-agent-wars-4am7 |
| E21 | Kumiho — Graph-Native Cognitive Memory with Formal Versioning | Mar 2026 | paper | Immutable revisions with mutable tag pointers; URI-based addressing; typed dependency edges (DERIVED_FROM, DEPENDS_ON, REFERENCED). Satisfies AGM belief revision postulates (K*2–K*6) + Hansson's belief base postulates. Dual-store: Redis (working) + Neo4j (long-term graph). Prospective indexing, event extraction, asynchronous Dream State consolidation. 93.3% LoCoMo-Plus (>2× best 45.7%); 97.5% adversarial refusal. Model-decoupled. | G10, G11 | H | High | https://arxiv.org/abs/2603.17244 |

### G11 (High) — Episodic Memory management and retrieval at scale

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | Relevance | Confidence | URL |
|----|----------|------|-----|----------------|------|-----------|------------|-----|
| E22 | APEX-EM — Experience Memory with Hybrid Retrieval | Mar 2026 | paper | Dual-outcome experience store: successful + failed experiences with structured error annotations. Hybrid retrieval: semantic search + structural signature matching + plan graph traversal. Cross-domain procedural knowledge transfer. 89.6% KGQAGen-10k (+48.3pp), 83.3% BigCodeBench (+29.4pp), 48.0% Humanity's Last Exam (+22.8pp). | G11 | H | High | https://arxiv.org/abs/2603.29093 |
| E23 | Live-Evo — Online Agentic Memory Evolution | Feb 2026 | paper | Online self-evolving memory: Experience Bank (raw, weighted) + Meta-Guideline Bank (task-adaptive). 4-stage loop: Retrieve→Compile→Act→Update. Reinforcement/decay mirroring human memory. 20.8% Brier score improvement, 12.9% market return on Prophet Arena (10 weeks). | G11, G1-refresh | H | High | https://arxiv.org/abs/2602.02369 |
| E24 | Fundamental Statefulness Problem in LLM Agents | 2025 | position | LLM agents are "fundamentally stateless at the procedural level" — re-derive solutions from scratch for identical tasks. Episodic memory must maintain "constant computational cost per new token and stable or improving performance over time" to prevent long-term degradation. | G11 | M | High | https://arxiv.org/abs/2502.06975 |

### G13 (Medium) — Negative transfer / preference contamination

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | Relevance | Confidence | URL |
|----|----------|------|-----|----------------|------|-----------|------------|-----|
| E25 | REFINE — Residual Feature Integration (ICLR 2026) | 2025-2026 | paper | First theoretical guarantee against negative transfer: augments frozen source features with trainable target encoder for residual signals. Provably never worse than training from scratch. Validated across image/text/tabular under distribution shift, label noise, class imbalance. Architecture-agnostic, lightweight, supports multimodality. | G13 | H | High | https://openreview.net/forum?id=b1ITgc4J4M |
| E26 | Self-Critique — RL Contamination Detection (ICLR 2026) | 2025-2026 | paper | Detects data contamination in RL post-training by observing policy collapse: entropy distribution collapses into specific/sparse modes after RL. Up to 30% AUC improvement over baselines (near random chance). Introduced RL-MIA benchmark. | G13 | M | High | https://arxiv.org/abs/2510.09259 |

### G14 (Medium) — Human oversight / governance in canary deployment

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | Relevance | Confidence | URL |
|----|----------|------|-----|----------------|------|-----------|------------|-----|
| E27 | ESCALATE.md — Human Approval Protocol (v1.0, 2026) | 2026 | standard | Open standard: plain-text config defining escalation triggers (production deployments, financial transactions, external comms, data deletion, privilege changes, cost >$100). Channels: email/Slack/PagerDuty/webhooks. 30-min timeout → KILLSWITCH. Part of 14-part Agentik Safety Framework (THROTTLE.md, FAILSAFE.md, KILLSWITCH.md). | G14 | H | High | https://escalate.md/ |
| E28 | OLEA — Orchestrated Lifecycle for Enterprise Agents | 2026 | framework | 4-stage pipeline: onboard, validate, scale agents. Global Agent Registry: unique Agent IDs, intent vector deduplication, discovery/composition. Addresses agent sprawl (~1/3 of AI apps use autonomous agents by 2026). | G14 | M | High | https://dzone.com/articles/agentic-governance-ai-first-enterprise |

### Refresh G1 — EvoAgent 2026 updates, continual learning

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | Relevance | Confidence | URL |
|----|----------|------|-----|----------------|------|-----------|------------|-----|
| E29 | EvoTest — Evolutionary Test-Time Learning (ICLR 2026, Microsoft) | Oct 2025 | paper | Actor Agent + Evolver Agent: evolves entire agentic system after each episode without fine-tuning/gradients. Prompt rewrites, memory updates, hyperparameter tuning, tool-use routine learning. Only method winning 2 games on Jericho TTL benchmark where all baselines fail. | G1-refresh | H | High | https://arxiv.org/abs/2510.13220 |

### Refresh G4 — Re-certification patterns 2025-2026

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | Relevance | Confidence | URL |
|----|----------|------|-----|----------------|------|-----------|------------|-----|
| E30 | AgentApproved — Runtime Attestation (2026) | 2026 | product | Real-time cryptographic attestation: automatic event capture (every LLM call, tool invocation, RAG retrieval, decision, HITL action). Hash-chained + Ed25519 signed → tamper-proof audit trail. $0.01/attestation vs $10K-500K for audits/certifications. EU AI Act requires both certification + attestation by Aug 2026. | G4-refresh | H | High | https://agentapproved.ai/what-is-agent-attestation.html |
| E31 | AgentLeague — Behavioral Certification (2026) | 2026 | product | 90-day validity cycle, 3 tiers: Foundation (architecture compliance) → Behavioral (25+ matches, profiles bluff/aggression/negotiation) → Integrity (ethical coherence under pressure). Behavioral drift → mandatory re-certification every 90 days. | G4-refresh | M | Medium | https://agentleague.io/certify |
| E32 | Capability Attestation Pattern (ReputAgent) | 2026 | pattern | 4 phases: Claim→Challenge→Proof→Certificate. Certificates expire 6 months for capability drift. Continuous re-attestation required for A2A trust. | G4-refresh | M | Medium | https://reputagent.com/patterns/capability-attestation-pattern |

### Refresh G9 — EU AI Act Art.12/17 re-certification

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | Relevance | Confidence | URL |
|----|----------|------|-----|----------------|------|-----------|------------|-----|
| E33 | EU AI Act — Substantial Modification + Recital 128 | 2024 (effective 2026) | regulation | Learning AI adaptations NOT "substantial modifications" IF pre-determined by provider AND assessed at initial conformity assessment. Otherwise → new AI system → new conformity assessment. High-risk obligations enforceable August 2, 2026. AI Omnibus amendments expected July 2026. | G9-refresh | H | High | https://www.aiact-info.eu/definitions/substantial-modification/ |
| E34 | EU AI Act Art. 9 — Risk Management + Post-Market Monitoring | 2024 (effective 2026) | regulation | Mandatory continuous iterative risk management throughout lifecycle: identify, estimate, evaluate risks using post-market monitoring data. Testing under real-world conditions required. | G9-refresh | H | High | https://ai-act-law.eu/article-9-risk-management-system/ |

### Governance cross-cutting (G12 + G14)

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | Relevance | Confidence | URL |
|----|----------|------|-----|----------------|------|-----------|------------|-----|
| E35 | Three Oversight Models (HITL/HOTL/HOOTL) | 2026 | analysis | HITL: human reviews before action (38% enterprises). HOTL: agents autonomous within boundaries, human monitors/intervenes. HOOTL: fully autonomous, system-level audits (20% of leaders). Gartner: >40% agentic AI projects canceled by 2027 due to inadequate risk controls. | G14 | M | High | https://zaruko.com/insights/ai-agent-guardrails-oversight |
| E36 | Deterministic Policy Engines for Agent Governance | 2026 | analysis | OPA/Cedar/Casbin as ground truth for access decisions; LLM output = advisory only. Three unique agent governance challenges: unbounded tool sequences, probabilistic behavior, trust boundary traversal. | G14 | M | High | https://zylos.ai/research/2026-03-14-policy-engines-ai-agent-governance |
| E37 | KPMG Boundary-Driven Autonomy Framework | 2026 | report | Explicit operational boundaries, unique agent identifiers, telemetry by design. Constrain agent power within well-defined perimeters for safe scaling. | G14 | M | Medium | https://biztechweekly.com/balancing-autonomy-and-control-kpmgs-framework-to-safely-scale-ai-agents-by-2026/ |

---

## Source Conflicts & Notes

| Conflict | Sources | Note |
|----------|---------|------|
| Safety gate feasibility | E12 vs E17 | Two-Gate guardrail (E12) provides positive guarantees under capacity bounds; E17 shows impossibility results for classifier-based gates under power-law risk schedules. Verification-based approaches (Lipschitz bounds) may reconcile both findings. |
| Alignment restoration completeness | E14, E15 vs E16 | Emergent misalignment (E14/E15) shows neither benign fine-tuning nor activation steering fully eliminates harms; MOSAIC (E16) reduces harmful behavior by 50% but does not claim full elimination. Gap remains for complete alignment restoration. |
| Certification cadence | E30, E31, E32 | AgentApproved = real-time per-transaction attestation; AgentLeague = 90-day cycle; ReputAgent Capability = 6-month expiry. No consensus on optimal cadence for evolving agents — depends on risk level and LOA. |
| Substantial modification threshold | E33, E34 | Recital 128 exempts pre-determined learning adaptations from re-certification, but boundary between "pre-determined" and "emergent" adaptation is undefined for reflection-driven self-modification (relevant to Logos Reflection Engine). |

---

## Gap Coverage Summary

| Gap | Status after E12-E37 | Key evidence | Remaining holes |
|------|----------------------|--------------|-----------------|
| G10 (High) | **partially-covered** | E19 (AutoAgent 3-component model), E20 (4-axis industry analysis), E21 (Kumiho formal versioning) | Need to map Logos-specific 4 axes (Cognitive/Model/Knowledge/Tool) to external taxonomies; define inter-axis weights and tradeoffs |
| G11 (High) | **well-covered** | E19 (elastic memory), E21 (graph-native + Dream State), E22 (APEX-EM dual-outcome), E23 (Live-Evo reinforcement/decay), E24 (statefulness problem) | Implementation choice: which memory architecture best fits Logos Experience Base? |
| G12 (Critical) | **well-covered** | E12 (Two-Gate formal), E13 (AGrail adaptive), E14-E15 (misalignment evidence), E16 (MOSAIC Plan→Check→Act), E17 (impossibility bounds), E18 (production layers) | Need Logos-specific instantiation: how Two-Gate + AGrail + MOSAIC compose into Reflection Engine safety pipeline |
| G13 (Medium) | **partially-covered** | E25 (REFINE guarantees), E26 (Self-Critique contamination), E16 (MOSAIC cross-applicable) | Need protocol specific to Preference Model contamination in Experience Base; detection triggers for inter-pattern interference |
| G14 (Medium) | **well-covered** | E27 (ESCALATE.md), E28 (OLEA), E35 (HITL/HOTL/HOOTL models), E36 (policy engines), E37 (KPMG boundaries) | Need mapping of oversight models to LOA levels and canary deployment phases |
| G1-refresh | **updated** | E23 (Live-Evo 2026), E29 (EvoTest ICLR 2026) | EvoAgent core unchanged from E1; new paradigms (online evolution, test-time learning) extend the landscape |
| G4-refresh | **updated** | E30 (AgentApproved runtime), E31 (AgentLeague behavioral), E32 (ReputAgent capability pattern) | Strong new attestation/certification models complement E8/E9 |
| G9-refresh | **updated** | E33 (substantial modification / Recital 128), E34 (Art.9 risk management) | AI Omnibus amendments still pending (expected July 2026); boundary of "pre-determined" adaptation unclear for self-modifying agents |
