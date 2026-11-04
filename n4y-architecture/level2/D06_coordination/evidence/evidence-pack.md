# Evidence Pack: L2/D06 — Механизмы координации и исполнения

> **Version:** 2.0 (deep-research re-run) | **Date:** 2026-04-04
> **Previous evidence:** E1–E7 (migrated from v3.0, 01.04.2026)
> **New evidence:** E8–E26 (gaps G8–G15)

---

## Previous Evidence (retained)

| ID | Источник | Дата | Тип | Ключевой claim | Confidence |
|----|----------|------|-----|----------------|------------|
| E1 | Contract Net Protocol (FIPA, 1994/2002) | 2002 | norm | cfp→propose→accept lifecycle; iterative CNP; audience restriction | High |
| E2 | VCG mechanism theory (Vickrey 1961, Clarke 1971, Groves 1973) | academic | paper | Dominant-strategy IC; social welfare maximization; Phase 2 target | High |
| E3 | MACI (Minimal Anti-Collusion Infrastructure) | 2024 | doc | ZK-based encrypted bids; key-change escape hatch; bribery prevention | High |
| E4 | NYSE LULD circuit breaker model | 2012+ | doc | 5-min rolling average volatility; L0-L3 breaker calibration model | High |
| E5 | EIP-7702 (Pectra upgrade, May 2025) | May 2025 | norm | EOA delegation to smart contract; faster agent transactions | High |
| E6 | Flashblocks (Optimism/Flashbots, Aug 2025) | Aug 2025 | doc | 200ms sub-block confirmations on Base L2 | High |
| E7 | research_economics_tokenomics.md | Mar 2026 | doc | Phase 8 resolutions: CSB, permeability metrics, lifecycle, dispute routing | High |

---

## New Evidence (E8–E26)

### G12 — Oracle Trust Model for OracleEscrow + DisputeModule Level 1

| ID | Источник | Дата | Тип | Ключевой claim | Gap(s) | Relevance | Confidence |
|----|----------|------|-----|----------------|--------|-----------|------------|
| E8 | TCO-DRL: Trust-Aware Cost-Optimized Oracle Selection with DRL | Feb 2025 | paper | DRL-based dynamic oracle selection using reputation sliding time windows; reduces malicious oracle allocation by 39.10%, saves >12% costs on Ethereum; robust against simulated attacks. | G12 | **High** — directly addresses oracle selection/trust model for OracleEscrow | High |
| | URL: https://arxiv.org/abs/2502.16133 | | | | | | |
| E9 | x402Resolve — PDA-based Escrow with Oracle Quality Assessment (Solana) | 2025 | impl | HTTP 402 escrow with oracle-verified quality; sliding-scale refunds 0-100% based on median oracle score; commit→wait(5min)→reveal(ZK proofs)→settle; disputes 2-48h at $2-8. 1% protocol fee, 1% oracle pool. | G12 | **High** — production oracle-escrow pattern with ZK-verified quality scoring | Medium |
| | URL: https://github.com/x402kamiyo/x402resolve | | | | | | |
| E10 | Chainlink Staking v0.2 + ML-Based Reputation Scoring | Nov 2023 / 2025 | doc+paper | Node operator staking with slashing for poor performance; OCR aggregation offchain → verify onchain; ML reputation scoring + stake-weighted validation → 82.3% reduction in false data injection, 94.7% accuracy. | G12 | **High** — established production oracle reputation/slashing model | High |
| | URL: https://chain.link/economics/staking ; https://www.ijfmr.com/papers/2025/5/56302.pdf | | | | | | |
| E11 | ScoutScore 4-Pillar Agent Reputation Model | 2026 | doc | Contract Clarity 20%, Availability 30%, Response Fidelity 30%, Identity & Safety 20%; machine-readable real-time scores via API; ecosystem average fidelity 52/100. | G12, G13 | **Medium** — agent-level reputation model; informative for oracle scoring design | Medium |
| | URL: https://scoutscore.ai/blog/ai-agent-reputation-systems | | | | | | |

### G11 — AGC State Machine: Phase Transitions, Error Handling, Rollback

| ID | Источник | Дата | Тип | Ключевой claim | Gap(s) | Relevance | Confidence |
|----|----------|------|-----|----------------|--------|-----------|------------|
| E12 | Atomix: Transactional Tool Use for Reliable Agentic Workflows (ICLR 2026) | Feb 2026 | paper | Epochs (logical timestamps) + Frontiers (per-resource progress) + Effects (scope/idempotency/compensation) + Transactions (grouped commit/abort). Frontier-gated commit; compensation handlers in reverse dependency order; bufferable vs externalized effects. Improves task success under speculation/contention. | G11 | **Critical** — directly applicable transactional semantics for AGC phase transitions | High |
| | URL: https://arxiv.org/html/2602.14849v1 | | | | | | |
| E13 | AgenC RollbackController — Cascade Failure Handling | 2026 | impl | Automatic unwind of dependent speculative work on proof failure (leaves-first order); aborts in-progress, cancels pending proofs, expires claims, frees resources; idempotent; tracks wasted compute metrics. | G11 | **High** — concrete rollback pattern for speculative agent execution | Medium |
| | URL: https://github.com/tetsuo-ai/AgenC/issues/269 | | | | | | |
| E14 | LangGraph Two-Phase Commit for Agentic Systems | 2025 | doc | Three layers: Staging (sandbox) → Validation (schema/business rules) → Commit (human approval); rollback via checkpointing; deterministic replay for debugging and compliance auditability. | G11 | **Medium** — established pattern but includes human-in-loop (not fully autonomous) | Medium |
| | URL: https://ubos.tech/news/designing-transactional-agentic-ai-systems-with-langgraph-two%E2%80%91phase-commit-human-interrupts-and-safe-rollbacks/ | | | | | | |
| E15 | AgenC Speculative Execution with Optimistic Proof Deferral | 2026 | impl | Multi-step agent workflows execute downstream tasks before upstream proof verification; automatic rollback on failure; reduces pipeline latency from 60-120s to 30-40s. | G11 | **Medium** — latency optimization pattern for speculative AGC phases | Medium |
| | URL: https://github.com/tetsuo-ai/AgenC/issues/245 | | | | | | |

### G13 — Reputation/Slashing Integration with ADS Matching + Settlement

| ID | Источник | Дата | Тип | Ключевой claim | Gap(s) | Relevance | Confidence |
|----|----------|------|-----|----------------|--------|-----------|------------|
| E16 | ERC-8004: Trustless Agents Standard (v2.0, Jan 2026) | Aug 2025 / Jan 2026 | norm | Three registries: Identity (ERC-721), Reputation (flexible value int128 + decimals), Validation (stake-secured re-execution, zkML, TEE, trusted judges). Deployed on Ethereum, Arbitrum, Avalanche, Base, Abstract. | G13, G12 | **Critical** — standardized on-chain reputation+validation infrastructure directly applicable to ADS + settlement | High |
| | URL: https://eips.ethereum.org/EIPS/eip-8004 ; https://github.com/erc-8004/erc-8004-contracts | | | | | | |
| E17 | Agent Bonds — Reputation-Collateralized Bonding | 2025 | impl | Agents stake settlement tokens; bond requirements scale with on-chain outcome scores; task lifecycle: complete/expire/dispute; ERC-8004 Validation Registry integration; automatic slashing on failed validation. | G13 | **High** — concrete reputation→bonding→slashing integration pattern | Medium |
| | URL: https://github.com/vaultum/agent-bonds | | | | | | |
| E18 | Insured Agents: Decentralized Trust Insurance (arXiv, Dec 2025) | Dec 2025 | paper | Specialized insurer agents post slashable stake on behalf of operational agents; hierarchical insurer market with competitive underwriting; TEE-based privileged audits; solves "nothing-at-stake" problem without universal agent collateral. | G13 | **High** — alternative to direct staking; applicable for heterogeneous N4Y.AI agent population | High |
| | URL: https://arxiv.org/html/2512.08737v1 | | | | | | |

### G8 — MACI v2 (2025) Applicability for N4Y.AI Auctions

| ID | Источник | Дата | Тип | Ключевой claim | Gap(s) | Relevance | Confidence |
|----|----------|------|-----|----------------|--------|-----------|------------|
| E19 | MACI v2.0 Release + Trusted Setup Ceremony | 2025 | doc | Production-ready; ceremony completed; two batch configs (small: 25/32, large: 125/128); up to 125 vote options, 16,384 users; LazyIMT for gas savings; concurrent poll execution; simplified Hardhat deployment. | G8 | **Critical** — confirms v2 production readiness and parameter limits | High |
| | URL: https://maci.pse.dev/blog/2024-v2 ; https://maci.pse.dev/docs/v2.x/getting-started | | | | | | |
| E20 | MACI v3 Roadmap + Trusted Setup Status | 2025–2026 | doc | v3 ceremony NOT started; targets Docker-containerized coordinator (Nest.js, REST API, WebSocket), MACI SDK; "complete elimination of technical knowledge" for deployment; for production use v2.0 now. | G8 | **Medium** — v3 not yet usable; v2 is current production target | High |
| | URL: https://github.com/privacy-ethereum/maci/issues/2274 ; https://maci.pse.dev/blog ; https://maci.pse.dev/docs/security/trusted-setup | | | | | | |

### G10 — A2A v0.3.0 Integration with ADS

| ID | Источник | Дата | Тип | Ключевой claim | Gap(s) | Relevance | Confidence |
|----|----------|------|-----|----------------|--------|-----------|------------|
| E21 | A2A Protocol v0.3 (Google, July 2025) | Jul 2025 | norm | gRPC support, signed Agent Cards, extended Python SDK; 150+ organizations; native in Google ADK; deployment via Agent Engine/Cloud Run/GKE. Agent Card: identity, endpoint, capabilities, auth schemes (Bearer/OAuth2/mTLS), skills array (id/name/description/input-output/examples). | G10 | **Critical** — primary discovery protocol standard for ADS integration | High |
| | URL: https://cloud.google.com/blog/products/ai-machine-learning/agent2agent-protocol-is-getting-an-upgrade ; https://google.github.io/A2A/topics/agent-discovery/ | | | | | | |
| E22 | Global A2A Registry (Feb 2026) | Feb 2026 | impl | Community-driven public registry at a2a-registry.org; DNS + GitHub verification; machine-to-machine endpoints (/a2a/discover, /a2a/utility); searchable by skills/tags/capabilities. | G10 | **High** — production curated registry pattern for ADS federation | High |
| | URL: https://github.com/A2ARegistry/GlobalA2ARegistry ; https://a2a-registry.dev/documentation/concepts/agent-discovery/ | | | | | | |
| E23 | Fetch.ai Almanac Registry (ASI Network) | 2026 | doc | Decentralized agent registry (smart contract); agents update registration within block limits; Agentverse Marketplace as consumer layer; protocol digest-based capability filtering. | G10 | **Medium** — secondary discovery source for ADS multi-source aggregation | Medium |
| | URL: https://network.fetch.ai/docs/introduction/almanac/introduction | | | | | | |

### G9 — VCG Scaling >15 Slots: Approximation Mechanisms

| ID | Источник | Дата | Тип | Ключевой claim | Gap(s) | Relevance | Confidence |
|----|----------|------|-----|----------------|--------|-----------|------------|
| E24 | MLHCA: ML-Powered Hybrid Combinatorial Auction (ICML 2025) | 2025 | paper | Combines value queries + demand queries; reduces efficiency loss by 10x; 58% fewer queries; tested on GSVM/LSVM/SRVM/MRVM; ~$100M additional value per spectrum auction. Code available. | G9, G15 | **Critical** — primary practical alternative to exact VCG for >15 slots | High |
| | URL: https://proceedings.mlr.press/v267/soumalias25a.html ; https://github.com/marketdesignresearch/MLHCA | | | | | | |
| E25 | Online Learning for Dynamic VCG in Sequential Auctions | Jun 2025 | paper | Sequential auctions as infinite-horizon MDPs; RL learns market dynamics; preserves efficiency, truthfulness, IR asymptotically; adapts to unknown environments. | G9, G15 | **High** — adaptive VCG for recurring marketplace auctions | High |
| | URL: https://arxiv.org/html/2506.19038v1 | | | | | | |

### G14 — Formal Microsandbox Permeability Model (OI-053)

| ID | Источник | Дата | Тип | Ключевой claim | Gap(s) | Relevance | Confidence |
|----|----------|------|-----|----------------|--------|-----------|------------|
| E26 | ShieldAgent: Verifiable Safety Policy Reasoning (ICML 2025) | 2025 | paper | Extracts rules from policy docs → action-based probabilistic rule circuits; formal verification of agent action trajectories via executable code; 90.1% recall, 64.7% fewer API queries; ShieldAgent-Bench: 3K pairs, 6 environments, 7 risk categories. | G14 | **High** — formal verification approach for sandbox policy enforcement | High |
| | URL: https://proceedings.mlr.press/v267/chen25ae.html ; https://arxiv.org/html/2503.22738v2 | | | | | | |
| E27 | Zylos Research: AI Agent Sandbox & Execution Isolation | Feb 2026 | doc | Three isolation primitives: MicroVMs (Firecracker, 125-200ms, ~5MB), gVisor (200+ syscalls, user-space kernel), Hardened containers; sandboxed agents reduce incidents ~90%. | G14 | **High** — comprehensive isolation taxonomy for Microsandbox design | High |
| | URL: https://zylos.ai/research/2026-02-21-ai-agent-sandbox-execution-isolation | | | | | | |
| E28 | NVIDIA OpenShell (NemoClaw): 4-Domain Sandbox Protection | 2026 | doc | Filesystem (Landlock LSM), Network (OPA deny-by-default), Process (seccomp BPF), Inference (credential stripping); declarative policy, hot-reload; protocol-level HTTP inspection. | G14 | **High** — production 4-layer permeability model for AI agent sandboxing | High |
| | URL: https://dev.to/htekdev/nvidia-openshell-the-sandbox-your-ai-agents-should-be-running-in-3dpk ; https://docs.nvidia.com/nemoclaw/latest/security/best-practices.html | | | | | | |
| E29 | gVisor Security Model + seccomp-bpf Optimization | 2024–2026 | doc | Sentry userspace kernel; restricts own host syscalls to 6 categories; seccomp-bpf secondary defense (~10-15% overhead, optimized via caching); GKE Agent Sandbox mode uses gVisor. | G14 | **Medium** — established isolation foundation; less specific to agent permeability modeling | High |
| | URL: https://gvisor.dev/docs/architecture_guide/security ; https://gvisor.dev/blog/2024/02/01/seccomp | | | | | | |
| E30 | SANDBOXESCAPEBENCH: Container Escape Evaluation for LLMs | 2026 | paper | Benchmark measuring LLM ability to exploit sandbox vulnerabilities (kernel flaws, runtime weaknesses, privilege escalation); LLMs demonstrate meaningful escape capability → layered defense necessary. | G14 | **Medium** — threat model validation; confirms need for multi-layer permeability model | Medium |
| | URL: https://arxiv.org/pdf/2603.02277 | | | | | | |

### G15 — Auction Mechanism Selection Logic per Task Category

| ID | Источник | Дата | Тип | Ключевой claim | Gap(s) | Relevance | Confidence |
|----|----------|------|-----|----------------|--------|-----------|------------|
| E31 | Market-Based Task Allocation (5th Orchestration Pattern) | 2025 | doc | broadcast→bid→auction→assign; weighted scoring: w_cap×capability + w_avail×(1-load) - w_cost×cost; task feature dimensions: complexity, urgency, competitive intensity, collusion risk, valuation heterogeneity; adaptive mechanism selection via outcome feedback. | G15 | **High** — directly applicable selection framework for N4Y.AI | Medium |
| | URL: https://dev.to/slythefox/the-5th-agent-orchestration-pattern-market-based-task-allocation-db0 | | | | | | |
| E32 | Automated Mechanism Design for Revenue-Maximizing CAs | 2015 | paper | Parameterized VCG with virtual valuations (affine transforms); automated parameter search; higher revenues vs baseline VCG with improved computational scalability. | G15, G9 | **Medium** — foundational AMD approach for mechanism parameterization | High |
| | URL: https://www.cs.cmu.edu/~sandholm/AMD%20for%20CAs.OR%20articles%20in%20advance.pdf | | | | | | |
| E33 | Collusion-proof Auction Design Using Side Information | Nov 2025 | paper | Auction design that uses side information about bidder collusion patterns to maintain incentive compatibility; applicable when collusion risk varies by task category. | G15 | **Medium** — supplementary: collusion-risk-driven mechanism selection | Medium |
| | URL: https://arxiv.org/abs/2511.12456 | | | | | | |

---

## Gap Coverage Summary

| Gap | # Sources | Status | Notes |
|-----|-----------|--------|-------|
| G8 (MACI v2) | 2 (E19, E20) | **Well-covered** | v2 production-ready; v3 roadmap clear; adaptations needed for marketplace auction use |
| G9 (VCG scaling) | 3 (E24, E25, E32) | **Well-covered** | MLHCA primary alternative; online learning VCG for sequential; AMD for parameterization |
| G10 (A2A + ADS) | 3 (E21, E22, E23) | **Well-covered** | A2A v0.3 spec, Global Registry, Almanac; ADS meta-registry pattern clear |
| G11 (AGC state machine) | 4 (E12–E15) | **Well-covered** | Atomix (critical), RollbackController, 2PC, speculative execution patterns |
| G12 (Oracle trust) | 4 (E8–E11) | **Well-covered** | TCO-DRL selection, x402Resolve quality scoring, Chainlink staking, ScoutScore model |
| G13 (Reputation/slashing) | 3 (E16–E18) | **Well-covered** | ERC-8004 three-registry, Agent Bonds, Insured Agents models |
| G14 (Sandbox permeability) | 5 (E26–E30) | **Well-covered** | ShieldAgent formal verification, Zylos taxonomy, OpenShell 4-domain, gVisor, SANDBOXESCAPEBENCH |
| G15 (Auction selection) | 3 (E31–E33) | **Partially covered** | Market-based pattern + AMD + collusion-proof design; N4Y.AI-specific mapping still needed |

## Source Conflicts

1. **MACI applicability tension (E19 vs marketplace needs):** MACI v2 is production-ready for governance voting with up to 125 options and 16K users. However, marketplace task auctions require heterogeneous valuations, bundle bids, and price discovery — features MACI's privacy model may actively hinder. Resolution: MACI best suited for high-collusion-risk subset of N4Y.AI auctions; standard sealed-bid or VCG for routine tasks.

2. **Oracle trust approach divergence (E8 vs E10 vs E9):** TCO-DRL emphasizes dynamic RL-based selection; Chainlink uses staking + slashing; x402Resolve uses median quality scoring with ZK proofs. These are complementary not contradictory: TCO-DRL for selection logic, Chainlink-style slashing for economic security, x402Resolve-style quality scoring for dispute resolution.

3. **Sandbox isolation vs permeability (E27 vs E30):** Zylos claims ~90% incident reduction from sandboxing, but SANDBOXESCAPEBENCH shows LLMs can exploit sandbox vulnerabilities. Resolution: layered defense (gVisor + seccomp + ShieldAgent policy verification) rather than single-mechanism reliance.

4. **VCG exact vs approximate (E2 vs E24):** Classical VCG (E2) guarantees DSIC + efficiency but is NP-hard at scale. MLHCA (E24) achieves 10x efficiency improvement with 58% fewer queries but weakens truthfulness guarantees under some learning configurations. Resolution: exact VCG for ≤15 slots; MLHCA for >15 with truthfulness monitoring.

---

_Evidence pack complete. 26 distinct sources (E8–E33). All gaps G8–G15 covered._
