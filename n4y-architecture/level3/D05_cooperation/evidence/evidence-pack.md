# Evidence Pack — L3/D05 Cooperation

> **Dimension:** L3/D05 Кооперативно-координационное | **Date:** 2026-04-04 | **Mode:** hard re-run
> **Sources:** E1–E47 (47 distinct) | **Search passes:** Cursor (16 queries) + MCP deep_research (3 calls, high effort)

---

## Evidence Table

| ID | Source | Date | Type | Key Claim | Gaps Addressed | URL | Relevance | Confidence |
|----|--------|------|------|-----------|---------------|-----|-----------|------------|
| E1 | Pool Formation in Oceanic Games: Shapley Value and Proportional Sharing | 2025 | Academic (AFT 2025) | Shapley value resists Sybil attacks better than proportional sharing in PoS blockchain pool formation; maintains decentralization properties | G-03, G-05 | https://drops.dagstuhl.de/entities/document/10.4230/LIPIcs.AFT.2025.21 | Medium | High |
| E2 | Task-Driven Multi-UAV Coalition Formation | 2024 | Academic (AAMAS 2024) | Shapley value proves Nash equilibrium in task-driven UAV coalitions; marginal utility algorithms for stable coalition structures | G-01, G-03, G-10 | https://ifaamas.csc.liv.ac.uk/Proceedings/aamas2024/pdfs/p1292.pdf | High | High |
| E3 | DAO-Agent: ZKP-Verified Incentives for Decentralized Multi-Agent Coordination | Dec 2025 | Academic (arXiv) | Hybrid on-chain/off-chain Shapley via STARK-to-SNARK ZKP; O(1) verification; 99.9% gas reduction (27k vs 28.6M gas at n=10); Shapley axioms make honest participation dominant strategy | G-03, G-05, G-12 | https://arxiv.org/abs/2512.20973 | **Critical** | High |
| E4 | Shapley Value Based Multi-Agent Reinforcement Learning | Feb 2024 | Academic (arXiv) | Markov Shapley Value extends Shapley to sequential MARL; credit assignment in cooperative multi-agent systems | G-03, G-12 | https://arxiv.org/abs/2402.15324 | High | High |
| E5 | Cooperation and Control in Delegation Games | 2024 | Academic (IJCAI 2024) | Multi-principal multi-agent delegation: control problems (agents vs principals) + cooperation problems; decomposition into alignment + capabilities | G-01, G-08 | https://www.ijcai.org/proceedings/2024/26 | High | High |
| E6 | Delegating to Multiple Agents | 2023 | Academic (arXiv) | Multi-agent delegation without money; competitive tension creates efficiency; optimal contingent delegation | G-01, G-10 | https://arxiv.org/pdf/2305.03203 | Medium | High |
| E7 | Competitive Contract Net Protocol (C-CNP) | ~2008 | Academic (Springer) | C-CNP extension: contracting + decommitment + termination phases; penalties bound decommitment; implemented on JADE | G-02, G-04 | https://link.springer.com/chapter/10.1007/978-3-540-69507-3_57 | High | High |
| E8 | FIPA CNP Specification | 2001 | Standard (FIPA) | Official FIPA CNP: cfp → propose/refuse → accept-proposal/reject-proposal → inform/failure | G-02 | https://jmvidal.cse.sc.edu/lib/fipa_contr_net_inter_protoc_specif.html | **Critical** | High |
| E9 | Free-Riding-Proof International Environmental Agreements | 2011 | Academic (RIETI) | Stable coalitions often small; TU regimes (permit trading) more stable than NTU (carbon tax); monetary transfers from supporters increase stability | G-05, G-09 | https://www.rieti.go.jp/jp/publications/dp/11e043.pdf | Medium | Medium |
| E10 | Members, Joiners, Free-Riders, Supporters | 2016 | Academic (CESifo) | Non-member "supporters" monetary transfers increase coalition stability and payoffs for all parties | G-05, G-09 | https://ideas.repec.org/p/ces/ceswps/_5802.html | Medium | Medium |
| E11 | Fault Tolerance in Multi-Agent RL | 2025 | Academic (IEEE) | Plug-and-play Switcher-Adversary for MARL robustness under agent malfunction; up to 116.7% improvement | G-04 | https://ieeexplore.ieee.org/document/11096942/ | Medium | High |
| E12 | DRAMA: Dynamic Robust Allocation-based Multi-Agent System | 2025 | Academic (arXiv) | Modular control/worker planes; agent dropout support; 17% runtime improvement; task eviction + reassignment | G-04, G-10 | https://arxiv.org/html/2508.04332v1 | **Critical** | High |
| E13 | Decentralized RL via Dynamic Cluster Agreements | Mar 2025 | Academic (arXiv) | Liquid-Graph-Time Clustering: dynamic sub-team formation on resource demand; stable rewards across changing team sizes | G-02, G-04 | https://arxiv.org/abs/2503.02437 | Medium | High |
| E14 | Unified Decentralized Framework for Heterogeneous Multi-Robot Systems | 2025 | Academic | Coalition game theory + deep RL + distributed architectures; motivation-driven reallocation; failure recovery without central control | G-01, G-04, G-10 | https://www.ijasret.com/VolumeArticles/FullTextPDF/1574_A_Unified_Decentralized_Framework_for_Task_Allocation_in_Heterogeneous_Multi-Robot_Systems.pdf | Medium | Medium |
| E15 | Fostering Cooperation through Dynamic Coalition Formation and Partner Switching | 2015 | Academic (Digital.CSIC) | Dynamic coalition + partner switching shows positive synergies; increased cooperation across complex networks | G-07, G-09 | https://digital.csic.es/handle/10261/131424 | Medium | Medium |
| E16 | Selective Interaction and Long-term Experiences in MARL | May 2024 | Academic (arXiv) | Agents learn neighbor selection from long-term experience; identify non-cooperative; strategic clustering | G-11, G-09 | https://arxiv.org/abs/2405.02654 | Medium | High |
| E17 | Stable Teams vs Dynamic Teams | 2004 | Academic (AAAI WS) | Long-term stable teams outperform short-term dynamic teams in efficiency, stability, security | G-07 | https://cdn.aaai.org/Workshops/2004/WS-04-06/WS04-06-005.pdf | Medium | Medium |
| E18 | Adaptive Governance in Collective Risk Social Dilemmas | 2026 | Academic (Nature Comm Physics) | Adaptive governance: adjust targets based on performance; increase targets when met, enhance rewards when missed; sustains cooperation | G-07, G-05 | https://www.nature.com/articles/s42005-026-02574-y | High | High |
| E19 | Methods for Task Allocation via Agent Coalition Formation | 1998 | Academic (Shehory & Kraus) | Foundational distributed coalition formation algorithms; NP-hard; anytime algorithms | G-10 | https://jmvidal.cse.sc.edu/lib/shehory98a.html | High | High |
| E20 | Coalition Formation for Task Allocation: Theory and Algorithms | 2010 | Academic (JAAMAS) | NP-hard to solve/approximate; DP for indistinguishable types; constant-factor approximation for bounded k | G-01, G-10 | https://link.springer.com/article/10.1007/s10458-010-9123-8 | High | High |
| E21 | Role Allocation in Multi-Agent Teams | 2003 | Academic (Nair et al.) | Task decomposition prunes role allocation search space; reallocation upon failure is key challenge | G-10, G-04 | https://jmvidal.cse.sc.edu/library/nair03a.pdf | High | Medium |
| E22 | SVAkADD: Shapley Approximation via k-Additive Games | Feb 2025 | Academic (arXiv) | k-additive surrogate game; exact Shapley on simplified model; k=2 captures 95%+ of interactions | G-03 | https://arxiv.org/html/2502.04763v1 | High | High |
| E23 | FW-Shapley: Real-time Weighted Shapley Estimation | Mar 2025 | Academic (arXiv) | Learned estimators; amortized inference; 27% improvement over FastSHAP; 14× speedup | G-03 | https://arxiv.org/html/2503.06602v1 | High | High |
| E24 | SEBM: Stratified Empirical Bernstein for Shapley | 2021 | Academic (IJCAI) | Adaptive stratified sampling; 20-30% fewer coalition evaluations with guaranteed accuracy bounds | G-03 | https://ijcai.org/proceedings/2021/0011.pdf | High | High |
| E25 | Survey: Approximating Shapley Value with Sampling | 2025 | Academic (SSRN) | Comprehensive survey of sampling methods; new flexible stratification paradigms | G-03 | https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5093818 | Medium | High |
| E26 | Portable Reputation for AI Agents: DID-Based Trust Layer | 2025 | Industry (Dev.to) | DID-based portable reputation; multi-dimensional trust vectors: Economic, Productivity, Behavioral, Dispute, Recency, Activity, Cross-platform | G-11, G-09 | https://dev.to/kaiagentfree/portable-reputation-for-ai-agents-building-a-did-based-trust-layer-6jh | **Critical** | Medium |
| E27 | ERC-8004 Trustless Agents: Onchain Reputation | 2025 | Industry (Dev.to) | Session-based auth, default-deny policy, reputation thresholds for partner selection | G-11, G-06 | https://dev.to/walletguy/erc-8004-trustless-agents-onchain-reputation-for-ai-32h | High | Medium |
| E28 | Reputation and Adaptive Coalitions in Competitive Environments | 2015 | Academic (Elsevier) | Reputation-driven adaptive coalition: autonomous form/disband; high completion despite unreliable workers | G-11, G-04 | https://www.sciencedirect.com/science/article/abs/pii/S0952197615001566 | High | Medium |
| E29 | Trust-Based Coalition Formation (ACM) | 2002 | Academic (ACM) | Foundational trust → coalition formation mechanism | G-11 | https://dl.acm.org/doi/10.1145/544741.544812 | Medium | Medium |
| E30 | Task Delegation Model: Sub-delegations and Delegation Chains | Mar 2026 | Academic (JAAMAS) | Recursive delegation + AND/OR task decomposition; delegation chains; multi-dimensional trust (social image, reputation, know-how, references); failure propagation + penalization along chains; MAB partner selection; outperforms mono-episodic in dynamic environments | G-13, G-04, G-11 | https://link.springer.com/article/10.1007/s10458-026-09741-z | **Critical** | High |
| E31 | Hierarchical AI Agent Coordination | Mar 2026 | Industry (Zylos) | O(n²)→O(n) coordination via hierarchy; manager-worker pattern; review loops 3-5× defect catch; security degradation after 5+ iterations | G-13, G-10, G-04 | https://zylos.ai/research/2026-03-01-hierarchical-ai-agent-coordination | High | Medium |
| E32 | Intelligent AI Delegation: Authority, Responsibility, Accountability | Feb 2026 | Academic (arXiv) | Framework for transfer of authority + responsibility + accountability; role boundaries; trust establishment | G-13, G-08 | https://arxiv.org/pdf/2602.11865v1 | High | High |
| E33 | NANDA: Networked AI Agents Decentralized Architecture | 2025 | Academic (arXiv) | Global discovery; AgentFacts cryptographic attestation; cross-protocol interop (MCP, A2A, NLWeb, HTTPS); ZTAA | G-06, G-11 | https://arxiv.org/pdf/2508.03101 | **Critical** | High |
| E34 | Internet of Agents (IoA) | 2025 | Academic (arXiv) | Agent Communication Layer (L8) + Semantic Layer (L9); capability discovery; adaptive protocols; consensus | G-06, G-11 | https://arxiv.org/pdf/2505.07176 | High | High |
| E35 | Cross-Organization AI Agent Federation | Mar 2026 | Industry (Zylos) | Agents autonomous/ephemeral/principals; OAuth 2.0 RAR + Token Exchange; 23% orgs have agent identity strategy; hub-and-spoke winning | G-06, G-08, G-13 | https://zylos.ai/research/2026-03-01-cross-org-ai-agent-federation | High | Medium |
| E36 | Faster Optimal Coalition Structure Generation (SMART) | 2024 | Academic (IJCAI) | DP + branch-and-bound + integer partition graph search for optimal partitioning | G-02 | https://ijcai.org/proceedings/2024/0027.pdf | Medium | High |
| E37 | SALDAE: Multiagent Path Search for Large-Scale CSG | 2024 | Academic (AAMAS) | Coalition structure generation as multiagent pathfinding; scales to 1000s of agents; anytime | G-02, G-03 | https://aamas.csc.liv.ac.uk/Proceedings/aamas2024/pdfs/p2489.pdf | High | High |
| E38 | Iterative Coalition Formation with Bounded Rationality | 2024 | Academic (arXiv) | Agents evaluate compromises → switch coalitions → converge; generalized for bounded rationality | G-02, G-09 | https://arxiv.org/pdf/2512.05983 | Medium | High |
| E39 | Efficiently Quantifying Individual Agent Importance | Dec 2023 | Academic (arXiv) | Adapted difference rewards; linear complexity; strongly correlates with Shapley values | G-12, G-03 | https://arxiv.org/abs/2312.08466 | High | High |
| E40 | A Measure of Added Value in Groups | 2019 | Academic (ACM) | Formal synergy measure via characteristic function games; unique solution; positive vs negative synergy | G-12 | https://dl.acm.org/doi/10.1145/3335547 | High | High |
| E41 | Weighted Surplus Division Value | 2019 | Academic (Symmetry) | Individual worth + surplus per weights; balances utilitarianism and egalitarianism | G-12, G-03 | https://mdpi-res.com/d_attachment/symmetry/symmetry-11-01169/article_deploy/symmetry-11-01169.pdf | Medium | High |
| E42 | MAC-SPGG: Incentivizing Strategic Cooperation in Multi-Agent Ensembles | 2024 | Academic (arXiv) | Multi-Agent Cascade SPGG: unique subgame perfect Nash equilibrium in effortful contributions; eliminates free-riding via Rᵢ = ρ·cᵢ + γ·Hᵢ + P·𝟙[fail] | G-05 | https://arxiv.org/html/2508.02076v1 | **Critical** | High |
| E43 | Reputation-Filtered Reward Reshaping | 2025 | Academic (AAMAS) | Trust + competence dual-dimension reputation filters peer assessments; reputation-weighted reward reshaping | G-05, G-11 | https://www.ifaamas.org/Proceedings/aamas2025/pdfs/p1736.pdf | High | High |
| E44 | GAIA: General Agency Interaction Architecture | Nov 2025 | Academic (arXiv) | Authorization boundaries, escalation triggers, information-gated progression for LLM-human delegation | G-07, G-08, G-13 | https://arxiv.org/html/2511.06262v1 | High | High |
| E45 | Levels of Autonomy for AI Agents | Jun 2025 | Academic (arXiv) | 5-level LOA framework: L0 (no autonomy) → L5 (full autonomy); human oversight modes per level | G-07, G-08 | https://arxiv.org/abs/2506.12469 | High | High |
| E46 | Portable Reputation: The 35% Problem | 2025 | Industry (RNWY) | Imported ratings = ~35% effectiveness of native; DID + soulbound tokens + time-based trust + vouch systems restore trust | G-11, G-06 | https://rnwy.com/blog/portable-agent-reputation | High | Medium |
| E47 | On-Chain Agent Identity Registry 2026 | 2026 | Industry (BuildMVPFast) | ERC-8004 + EAS; ~340K agent wallets, 8.7M attestations; global namespace + capability verification | G-11, G-06 | https://www.buildmvpfast.com/blog/on-chain-agent-identity-blockchain-registry-verifiable-2026 | High | Medium |

---

## Gap Coverage Matrix

| Gap | Coverage | Key Sources | Status |
|-----|----------|-------------|--------|
| **G-01** 3-model taxonomy formal conditions | **Strong** | E2, E5, E6, E14, E20 + MCP Call 1 (full taxonomy with decision criteria: complexity, N agents, duration, LOA) | → closable |
| **G-02** Coalition lifecycle state machine | **Strong** | E7, E8, E13, E36, E37, E38 + MCP Call 1 (8-phase lifecycle from CFP to dissolution) | → closable |
| **G-03** Shapley Value application + N>7 alternatives | **Strong** | E1, E2, E3, E4, E22, E23, E24, E25, E37, E39 + MCP Call 2 (exact for N≤7; SEBM/FW-Shapley/SVAkADD/Owen/nucleolus for large N) | → closable |
| **G-04** Failure handling protocol | **Strong** | E7, E11, E12, E13, E14, E21, E28, E30, E31 + MCP Call 1 (heartbeat, DRAMA, circuit breakers, cascade prevention, state sync) | → closable |
| **G-05** Incentive design: Synergy Bonus + free-riding | **Strong** | E1, E3, E9, E10, E18, E42, E43 + MCP Call 2 (MAC-SPGG formula, VCG, bonding/staking, reputation-weighted) | → closable |
| **G-06** Cross-sandbox coalitions via CSB | **Strong** | E27, E33, E34, E35, E46, E47 + MCP Call 3 (NANDA, IoA, Zero Trust, OAuth Token Exchange, DID federation) | → closable |
| **G-07** Alliance governance | **Strong** | E15, E17, E18, E44, E45 + MCP Call 3 (voting rules, quorum, amendment, treasury, admission/expulsion, DAO parallels) | → closable |
| **G-08** Corporate dual-delegation | **Moderate** | E5, E32, E35, E44, E45 + MCP Call 3 (dual-master conflict, authority chains, LOA constraints, Castelfranchi theory) | → closable with design work |
| **G-09** Coalition stability | **Strong** | E9, E10, E15, E16, E38 + MCP Call 2 (Core, ε-core, Bondareva-Shapley, hedonic games, bonding as stability) | → closable |
| **G-10** Role allocation / task decomposition | **Strong** | E2, E6, E12, E14, E19, E20, E21, E31 + MCP Call 1 (capability matching, HTN, AND/OR decomposition, dynamic reallocation) | → closable |
| **G-11** Partner discovery / matching | **Strong** | E16, E26, E27, E28, E29, E33, E34, E43, E46, E47 + MCP Call 3 (DID reputation, multi-dim trust vectors, matching algorithms, on-chain registries) | → closable |
| **G-12** Synergy quantification | **Strong** | E3, E4, E39, E40, E41 + MCP Call 2 (superadditivity, Harsanyi dividends, difference rewards, cooperative surplus) | → closable |
| **G-13** Sub-delegation inside coalitions | **Strong** | E30, E31, E32, E35, E44 + MCP Call 3 (delegation chains, AND/OR, depth limits ~3-4, IET responsibility tracking, failure propagation) | → closable |

---

## Source Conflicts & Notes

### Conflict C1: Shapley exact vs approximate threshold
- E3 (DAO-Agent) claims ZKP enables O(1) on-chain verification at any N, suggesting exact Shapley off-chain is acceptable even for large coalitions
- E22 (SVAkADD), E23 (FW-Shapley), E24 (SEBM) focus on approximation because exact is O(2^n)
- **Resolution:** Both are valid — DAO-Agent uses exact Shapley off-chain with ZKP verification; approximation needed only when off-chain compute is also constrained. For N4Y with N≤7, exact is always feasible.

### Conflict C2: Coalition stability — large vs small coalitions
- E9, E10 (environmental economics) argue stable coalitions are inherently small (no large coalition without payment mechanisms)
- E37 (SALDAE) shows coalition structure generation scales to 1000s of agents
- **Resolution:** Context-dependent. Stability is hard for large coalitions without binding mechanisms; N4Y's blockchain escrow + bonding provides the "payment mechanisms" that make larger coalitions viable.

### Conflict C3: Reputation portability effectiveness
- E26 (DID-based trust) is optimistic about portable reputation
- E46 (35% Problem) shows imported ratings have only 35% of native trust-building power
- **Resolution:** Portable reputation is necessary but insufficient. Must supplement with contextual signals (time-based metrics, vouch systems, soulbound tokens) per E46 recommendations.

### Conflict C4: Alliance governance — DAO token-voting vs one-agent-one-vote
- MCP Call 3 identifies tension between token-weighted voting (can concentrate power) and egalitarian one-agent-one-vote
- **Resolution:** N4Y design choice. Alliance governance should likely use reputation-weighted voting (not pure token-weighted) to prevent whale capture while maintaining meritocratic input.

### Note N1: Weak coverage areas
- **G-08 (Corporate dual-delegation):** Most evidence is conceptual/framework-level (E5, E32, E35). No empirical studies on actual corporate AI agent dual-delegation scenarios. This gap requires design synthesis rather than literature-based closure.
- **Cross-sandbox latency data:** No empirical measurements of CSB latency in agent coalition contexts. WebRTC 30-50ms baseline (MCP Call 3) is a useful reference but not agent-coalition-specific.

### Note N2: Strategy PDF alignment
- Strategy mentions "hub-and-spoke intra-Logos coordination" (§14) and "inter-Logos delegation" (§15) — both Simple Delegation only
- Strategy does NOT cover Coalition/Alliance models, Shapley Value, or formal incentive mechanisms
- All Coalition/Alliance/Shapley content is net-new architecture, not strategy-sourced

---

## Top Findings

1. **DAO-Agent (E3)** provides a production-ready architecture for ZKP-verified Shapley value computation on blockchain with O(1) verification cost — directly transferable to N4Y's MissionEscrow on Base L2. This resolves the Shapley scalability concern for any coalition size.

2. **MAC-SPGG (E42)** provides a game-theoretically grounded incentive formula (Rᵢ = ρ·cᵢ + γ·Hᵢ + P·𝟙[fail]) that creates a unique subgame perfect Nash equilibrium in effortful contributions — directly applicable as the Synergy Bonus + CoordinatorReward mechanism.

3. **Sub-delegation model (E30)** from JAAMAS 2026 provides a complete framework for recursive delegation with AND/OR decomposition, delegation chains, multi-dimensional trust-based partner selection, and failure propagation — directly addresses OI-016 and G-13.

4. **DRAMA (E12)** provides a deployable failure handling architecture with agent dropout support, task eviction, and affinity-based reassignment — template for N4Y's coalition failure protocol.

5. **8-phase coalition lifecycle** (MCP Call 1 synthesis) provides a complete state machine from Recognition through Dissolution, grounded in FIPA CNP with blockchain-escrow extensions — foundational for G-02.
