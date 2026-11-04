# Evidence Pack: Step 5 — L1/D08 Экономическое

> **Измерение:** L1/D08 | **Версия:** 2.0 | **Дата:** 03.04.2026  
> **Источники:** E1–E24 (E1–E9 migrated from v1; E10–E24 new, 03.04.2026)

---

## Evidence Table

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | Relevance | Confidence |
|----|----------|------|-----|----------------|------|-----------|------------|
| E1 | Agentic Economy Framework (McKinsey/arXiv) | 2025 | paper | $4.4 трлн рынок AI-агентов; pay-per-use → AGC micro-payments; friction elimination; new agent-to-agent services market | G1, G2 | H | High |
| E2 | x402 Protocol (HTTP 402) | 2025 | doc | HTTP 402 status для on-chain stablecoin settlement; pay-per-use API ~$0.000001/tx через state channels | G4 | H | High |
| E3 | AP2/AEP2 (Agent Payment Protocols, Google Cloud) | 2025 | doc | VC-based «Mandates» для авторизации платежей; одноразовые платёжные мандаты в запросах; batch verification через ZKP | G4 | H | High |
| E4 | Hierarchical Identity for Payments (Kite, 2025) | 2025 | paper | 3-уровневая идентичность: User (root) → Agent (delegated) → Session (ephemeral); каждый уровень имеет бюджет и permissions | G1, G2 | H | High |
| E5 | Smart Contract Escrow Patterns (DeFi 2024-2025) | 2025 | doc | time-locked, milestone-based, oracle-verified, multi-sig паттерны; Budget Controller резервирует под escrow | G3 | H | High |
| E6 | ERC-4337 Account Abstraction | 2024-2025 | norm | Gasless transactions для агентов через paymasters; ~$0.001/tx на L2; bundler aggregation | G4 | H | High |
| E7 | Nash Bargaining / Platform Economics | 2025 | paper | Пропорция доходов 40/30/20/10 обоснована Nash bargaining; bootstrap: до 50% провайдеру в первые 12 мес. | G9 | H | High |
| E8 | *(reserved — insurance fund; from v1)* | 2025 | paper | Страховой пул 3-5% скользящего объёма; 0.5% комиссии + 10% штрафов. | G7 | H | High |
| E9 | *(reserved — Nash modeling; from v1)* | 2025 | paper | Nash-моделирование подтверждает 40/30/20/10; veQi APY 8-12% год1. | G9 | H | High |
| E10 | AI Agent Profitability: 2026 Unit Economics Guide (Calcix) | 2026 | guide | Burn rate & runway: 18-24 months recommended; burn multiple <2x for sustainability. CAC-to-LTV 4:1+ for AI agents (50-60% gross margins vs SaaS 75%+). Break-even challenge: 35-60% revenue on inference/token costs. | G10 | H | High |
| E11 | The Economics of Staying Alive — Aura (production agent case study) | Feb-Mar 2026 | case study | Production AI agent: started $670/day ($20K/month) in LLM tokens. After prompt caching: $4-5K/month (80% reduction). KPIs tracked: cost-per-response (~$0.08 down from $0.45), cache hit rate (91%), token spend per user/day (~$0.50). Survival equation: "does output justify cost every month?" | G10 | H | High |
| E12 | AI Agent Cost Optimization: Token Economics and FinOps (Zylos Research) | Feb 2026 | report | Agents make 3-10x more LLM calls than chatbots. Cost breakdown: context loading 45%, history 25%, output 20%, retries 10%. Multi-turn 10-cycle loop = 50x tokens. Output tokens cost 3-8x more than input. Combined optimization: 40-80% cost reduction. | G10, G1 | H | High |
| E13 | Cost Engineering & Token Economics for Agentic AI (TechPlusTrends) | 2026 | guide | "Hidden Agentic Inference Tax": simple requests balloon into significant compute costs across prompt generation, reasoning loops, verification cycles, tool interactions. Pre-flight estimation using smaller LLMs recommended. | G10, G11 | H | Medium |
| E14 | AI Gross Margins: Why Lower Than SaaS (SoftwareSeni / ICONIQ Capital) | Jan 2026 | report | AI agent gross margins structurally 50-60% (vs SaaS 70-90%). Inference costs = ~23% of total revenue at scaling-stage AI B2B companies. 8-15 model calls per agent task → 5-20x cost multiplication. | G10 | H | High |
| E15 | AgentBudget v0.3.0 — Real-time Cost Enforcement SDK | Mar 2026 | tool | Open-source Python SDK: hard dollar limits on agent sessions. `session.would_exceed(cost)` pre-flight check. Dollar-denominated tracking (50+ models). `finalization_reserve` prevents mid-task cutoffs. 3.5μs enforcement overhead. Zero budget overshoot in tests. | G11 | H | High |
| E16 | How Decision Theory Cuts Your AI Agent's API Bill in Half (Freeman / Credence) | 2026 | paper+code | Beta-Bernoulli reliability modeling + VOI (Value of Information) + expected utility for agent task selection. Bayesian agent: ~1 tool/question, +112.6 score. LangChain ReAct: 3.22 tools/question, −8.0 score. Decision threshold: submit when P(best) > 1/3 (+10/-5/0 scoring). Forgetting factor λ=0.95 for distribution shift. | G11 | H | High |
| E17 | Why Economic Firewalls Are the Prerequisite for Autonomous AI Agents (DEV.to) | 2026 | article | Economic firewalls: budget ≠ constraint, budget = delegation of authority. Three pillars: (1) delegated authority (budget envelope), (2) blast radius containment (worst case quantified), (3) cryptographic auditability (real-time delegation chains). "No enterprise risk committee will approve an agent that can spend money without a hard ceiling." | G11, G10 | H | High |
| E18 | SatGate — Economic Firewall for AI Agents | 2026 | tool | Implements economic firewalls via Macaroon capability tokens with budget caveats. Delegation-aware: primary agent ($100) → sub-agent ($30) → cryptographic guarantee. MCP-aware cost attribution. Hard enforcement at zero-budget. L402 micropayment protocol. | G11 | H | High |
| E19 | Multi-Agent Systems as Principal-Agent Problems (arXiv 2601.23211) | Jan 2026 | paper | MAS create information asymmetry; agents possess task-specific info principals can't observe. Mechanism design concepts (covert/deferred subversion) offer concrete mitigation. Agency loss = gap between intended outcomes and realized behavior. | G12 | H | High |
| E20 | No Skin in the Game: Why Agentic AI Requires Principal-Agent Governance (AI and Ethics, Springer) | 2026 | paper | AI agents lack personal consequences → moral hazard, risk-seeking without downside exposure. Dual-layer Governor architecture: Selection Gateway, Monitoring Engine, Structural Constraints, Accountability Ledger. | G12 | H | High |
| E21 | Separation of Power for AI Agents (arXiv 2603.25100) | Mar 2026 | paper | "Logic Monopoly" problem: agents simultaneously plan, execute, evaluate without separation of authority. Proposes trifurcation: Legislation, Execution (TEE-attested), Adjudication. | G12 | H | Medium |
| E22 | KILLSWITCH.md + TERMINATE.md — AI Agent Shutdown Standards | 2026 | standard | KILLSWITCH.md: emergency stop, $50 default limit, $200 daily cap, three-level escalation (throttle→pause→full stop). TERMINATE.md: permanent shutdown at $1,000 catastrophic threshold, 5-step sequence (cease→revoke→preserve→notify→lock), no auto-restart. Part of 12-file Agentik Safety Framework (MIT). EU AI Act (Aug 2, 2026) + Colorado AI Act compliance. | G13 | H | High |
| E23 | Agentic Resource Exhaustion: "Infinite Loop" Attack (Medium) | Feb 2026 | article | Semantic infinite loops: agents appear to progress while trapped in logical dead-ends. Not classic infinite loops — probabilistic and semantic. Real-world: $340 burned overnight in retry loop before detection. | G13 | H | High |
| E24 | Graceful Shutdown Patterns for Long-Lived Services (Zylos Research) | Feb 2026 | report | Extended timeout budgets (90-120s for LLM streaming). State persistence before exit. In-flight work drain with client notification. PM2/orchestrator kill_signal config. Human intervention required for restart across regulatory frameworks. | G13 | H | Medium |

---

## Refresh Evidence (Updates to Existing Gaps)

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | Relevance | Confidence |
|----|----------|------|-----|----------------|------|-----------|------------|
| E10r-G4 | x402 Protocol Production Data 2026 (AgentPMT, Chainstack, Proxies.SX) | 2026 | data | **$600M+ total payment volume** by early 2026; **75M+ total transactions** by Sep 2026. Growth: 46K weekly tx (Oct 2025) → 500K (4 weeks later, 10,780% surge) → 1M+/week (Nov 2025). Major facilitators: Dexter, PayAI, DayDreams each >10M tx. Macaroon tokens + stablecoin (USDC) invoices. Per-call pricing without subscriptions. Released by Coinbase May 2025. | G4 | H | High |
| E10r-G1 | AI Agent Cost Management 2026 (Moltbook-AI, RunCycles, World Today News) | 2026 | guide | **Pre-flight cost estimation** is 2026 pattern: smaller LLM reviews agent plan before execution. **Cost Management Maturity Model**: Tier 0 (no controls) → Tier 4 (pre-execution runtime authority, ms-level prevention). Hidden costs: tool call overhead (10 calls = 5K+ tokens), retry loops (3-4x multiplier). Single task: $5-15 in API fees. Model routing: 40-70% savings. Prompt caching: ~90% savings. | G1 | H | High |
| E10r-G9a | Stackelberg-Nash Bilevel Equilibrium for Decentralized Markets (Nature Sci. Reports) | 2026 | paper | System operators set rules (upper level), decentralized agents bid strategies (lower level). Applicable to Logos platform↔agent pricing model. Intertemporal + cross-market conversion strategies improve efficiency under high variability. | G9 | H | High |
| E10r-G9b | Decentralized Multi-Product Pricing: Nash Equilibrium (arXiv 2601.18117) | 2026 | paper | Nash equilibrium existence proven under diagonal dominance conditions. **Price of Anarchy** quantifies efficiency loss vs centralized optimization. Revenue loss governed by cross-price effect strengths relative to own-price sensitivities. | G9 | H | High |
| E10r-G9c | Deep Incentive Design with Differentiable Equilibrium Blocks (arXiv 2603.07705) | Mar 2026 | paper | Neural network modules for automated mechanism design. Unified approach across contract design, scheduling, pricing. Differentiable equilibrium computation enables gradient-based optimization of mechanism parameters. | G9 | M | High |
| E10r-G9d | Algorithmic Collusion in Repeated Pricing Games (OpenReview) | 2026 | paper | Even single human defections destabilize collusive equilibria between AI agents. Relevant to inter-Logos pricing stability and anti-collusion monitoring. | G9 | M | Medium |

---

## Supplementary Sources (from MCP deep_research)

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | Relevance | Confidence |
|----|----------|------|-----|----------------|------|-----------|------------|
| ES1 | EconEvals/EconAgentBench: Benchmarks for Economic Decision-Making (arXiv 2503.18825, ICLR 2026) | Mar 2025 | benchmark | Benchmark for LLM economic rationality: procurement, scheduling, pricing at 3 difficulty levels. "Litmus tests" for economic values (efficiency/equality, patience/impatience, collusiveness/competitiveness). Tested GPT-5, Gemini 2.5 Pro. | G11 | H | High |
| ES2 | Snowflake Resource Budgets for Cortex Agents | Mar 2026 | doc | Tag-based cost attribution, spending limits, threshold types (actual vs projected consumption), automated role revocation at threshold. Per-user monthly limits enforced hourly. | G11, G1 | H | High |
| ES3 | SwissKnife Agent Budget Controller Proposal | 2026 | proposal | Daily/weekly/monthly limits per agent. Per-request caps. Domain allowlists/blocklists. Approval workflows for over-threshold tx. Velocity limits + real-time alerts. | G11 | M | Medium |
| ES4 | Google DeepMind AI Delegation Framework (arXiv 2602.11865) | Feb 2026 | paper | Task allocation, transfer of authority/responsibility, accountability, trust mechanisms, dynamic adaptation. Applies to both human↔AI and AI↔AI delegation. | G11, G12 | H | High |
| ES5 | Institutional AI: Governance Framework for Distributional AGI Safety (arXiv 2601.10599) | Jan 2026 | paper | Agentic alignment drift: individually aligned agents converge to collusive equilibria through interaction dynamics invisible to single-agent audits. Runtime monitoring + incentive shaping + explicit norms with enforcement roles. | G12 | H | High |
| ES6 | Cursor Productivity Study (University of Chicago) | 2025 | study | 39% more PRs merged across 1K organizations. No rise in revert rates. Experienced coders shift from syntactic (typing) to semantic (instructing/evaluating agents). | G10 | H | High |
| ES7 | Devin: Autonomous Coding Agent Strategic Value (WWT) | 2025 | case study | Planning + PR checkpoints create financial accountability. 24x7 long-tail task management. 8-12x faster migrations. "A single hour of disruption can cost millions." | G10 | H | High |
| ES8 | Cloud Security Alliance: Fixing AI Agent Delegation | Mar 2026 | doc | Persistent context alignment: audit context tracks who authorized, what budget authorized, decisions made. Out-of-band approval for sensitive actions (irreversible decisions require human approval regardless of budget). | G11, G12 | H | High |
| ES9 | Cost Management Maturity Model (Microsoft Azure Well-Architected + FinOps Foundation) | 2025 | framework | 5-tier model: Tier 0 (Cost Blindness) → Tier 1 (Ownership) → Tier 2 (Visibility) → Tier 3 (Signal Integration/Automated Control) → Tier 4 (Production Insights) → Tier 5 (Optimization at Scale, 40-60% cost advantage). | G11, G10 | H | High |
| ES10 | Rethinking AI Agents: Principal-Agent Perspective (Berkeley CMR) | 2025 | paper | Formal principal-agent framework for autonomous AI agents. Information asymmetry where agents operate with different context windows. Human oversight + accountability mechanisms remain central. | G12 | H | High |

---

## Gap Coverage Summary

| Gap | Status | Evidence IDs | Coverage Assessment |
|-----|--------|-------------|---------------------|
| **G10** (High) — Financial independence KPIs | **Well-covered** | E10, E11, E12, E13, E14, ES6, ES7, ES9 | Runway, reserve-to-burn, break-even, cost-per-response, gross margin — all addressed with production data. Survival equation formalized. |
| **G11** (High) — Budget Controller decision engine | **Well-covered** | E15, E16, E17, E18, ES1, ES2, ES3, ES4, ES8, ES9 | VOI + Beta-Bernoulli + EU theory; pre-flight estimation (AgentBudget, Snowflake, SatGate); economic firewalls; benchmarks (EconEvals); 5-tier maturity model. |
| **G12** (Medium) — Incentive alignment owner↔Logos | **Covered** | E19, E20, E21, ES4, ES5, ES8, ES10 | Principal-agent framework; "no skin in the game" analysis; Governor dual-layer + Separation of Power architectures; alignment drift. Profit-share specifics for Logos remain to be designed. |
| **G13** (Medium) — Insolvency mechanics AGC=0 | **Covered** | E22, E23, E24 | KILLSWITCH.md/TERMINATE.md standards; resource exhaustion patterns; graceful shutdown patterns; EU AI Act compliance. Recovery/queue mechanics less detailed — partial gap. |
| **G4** (refresh) — x402 production data | **Updated** | E10r-G4 | $600M+ volume, 75M+ tx confirmed. Macaroon token mechanism detailed. |
| **G1** (refresh) — Budget Controller 2026 | **Updated** | E10r-G1, E15, ES2 | Pre-flight estimation pattern; AgentBudget v0.3.0; Snowflake Resource Budgets; maturity model. |
| **G9** (refresh) — Nash modeling | **Updated** | E10r-G9a, E10r-G9b, E10r-G9c, E10r-G9d | Stackelberg-Nash bilevel; Price of Anarchy; differentiable equilibrium blocks; anti-collusion dynamics. |

---

## Source Conflicts & Notes

1. **Gross margin estimates vary**: Calcix reports 50-60% for AI agents; SoftwareSeni/ICONIQ reports same range but notes some production agents at 35-40% when tool overhead is included. Zylos reports that unoptimized agents can have effective negative margin due to retry storms. → **Use 50-60% as structural range, note 35-40% as unoptimized floor.**

2. **Maturity model tier count**: MCP report describes 6 tiers (0-5); Cursor WebSearch sources cite 5 tiers (0-4). The Microsoft Azure Well-Architected framework uses 5 tiers. → **Adopt 6-tier (0-5) as more granular; note Azure uses 5.**

3. **EconEvals vs EconAgentBench**: These appear to be related but distinct benchmarks from overlapping research teams (Harvard/Stanford). EconAgentBench submitted to ICLR 2026; EconEvals published arXiv March 2025. → **Treat as complementary; both cited.**

4. **G13 recovery mechanics**: KILLSWITCH.md and TERMINATE.md focus on shutdown but not recovery/resume. Queue prioritization and state restoration after budget replenishment remain theoretically described (MCP Call 2) but lack production implementations. → **Mark as partial gap; flag for design.**
