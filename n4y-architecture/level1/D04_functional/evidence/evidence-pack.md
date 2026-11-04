# Evidence Pack: Step 5 — L1/D04 Функционально-инструментальное

> **Измерение:** L1/D04 | **Версия:** 3.0 (v3 hard re-run, G16-G20 + refresh G2/G11/G13) | **Дата:** 03.04.2026

## Previous evidence (v2): E1–E15

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | Relevance | Confidence |
|----|----------|------|-----|----------------|------|-----------|------------|
| E1 | MCP (Model Context Protocol) — Anthropic spec | Nov 2024, spec 2025-11-25 | norm | Open-source стандарт для подключения AI к внешним системам; клиент-серверная архитектура; примитивы: Tools, Resources, Prompts, Sampling; 78K+ GitHub stars | G2 | H | High |
| E2 | LangGraph Supervisor pattern | 2025 | doc | Production-ready Orchestrator: строгая типизация состояния; checkpointing из коробки; interrupt()/resume для HITL; active production support | G1, G3 | H | High |
| E3 | ReAct + Plan-and-Execute + Adaptive Planning | 2023-2025 | paper | ReAct: reason+act; Plan-and-Execute: все шаги заранее; Adaptive: план корректируется после каждого шага | G1, G6 | H | High |
| E4 | Saga Pattern для distributed transactions | 2025 | doc | Compensating transactions при ошибке: rollback N-1, N-2... шагов; Atomix transactional semantics (MPI-DSG Feb 2026) | G4 | H | High |
| E5 | AgentOrchestra + Osprey (2025) | 2025 | doc | Production-ready оркестрация: checkpointing перед каждым шагом; rollback восстановление; audit trail | G3, G4 | H | High |
| E6 | A2A Protocol — input-required + HITL | 2025 | norm | A2A task lifecycle: submitted→working→input-required→completed/failed; LangGraph interrupt() для HITL; risk-oriented policy | G5 | H | High |
| E7 | OpenTelemetry GenAI SemConv | 2025 | norm | Стандарт трассировки LLM и tool calls; Langfuse (MIT) для UI observability; Phoenix как альтернатива | G8 | H | High |
| E8 | A2A AgentCard + MCP Virtual Server | 2025 | doc | Composability: AgentCard публикует микроагентов как skills; MCP Virtual Server aggregation для прямого вызова; CLEAR benchmark | G10 | M | High |
| E9 | CLEAR Benchmark | 2025 | doc | Comprehensive LLM Evaluation and Assessment Registry; двухуровневая система: CLEAR + Benchmark Oracle с cryptographic confirmation | G8, G10 | M | High |
| E10 | MCP Security: OAuth 2.1 + MCP Guard + vulnerabilities | 2025-2026 | doc+norm | MCP lacks built-in caller identity/authorization. OAuth 2.1 for HTTP transports. MCP Guard: trust-level access control (0-4), audit logging. Known vulns: prompt injection, lookalike tools, data exfiltration | G11 | H | High |
| E11 | LangGraph persistence: PostgresSaver, AsyncPostgresSaver, StateGraph | 2025-2026 | doc | Auto-checkpoint after every node. Backends: MemorySaver/Sqlite/Postgres/AsyncPostgres (multi-pod). Pydantic state models. Survives server restarts. Token budget enforcement | G12, G14 | H | High |
| E12 | VMAO: Verified Multi-Agent Orchestration framework | 2026 | paper | Plan-Execute-Verify-Replan. DAG decomposition + verification-driven replanning. Completeness 3.1→4.2, quality 2.6→4.1 vs single-agent | G15 | H | High |
| E13 | GNNVerifier: Graph Neural Network plan verification | 2026 | paper | GNN detects plan flaws (type mismatches, missing intermediates, broken deps). Better than LLM-based verifiers for structural failures | G15 | H | High |
| E14 | MASEval + MultiAgentBench + ProtocolBench | 2025-2026 | paper | System-level evaluation frameworks. MASEval: framework-agnostic. MultiAgentBench: milestone KPIs, topology comparison (graph best). ProtocolBench: 4 axes (success, latency, overhead, robustness) | G13 | H | High |
| E15 | Deep Agents (LangChain, Mar 2026) | 2026 | doc | write_todos planning tool, auto-summarization, subagent spawning, context isolation for long-running tasks | G12, G15 | M | High |

---

## New evidence (v3): E16–E33

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | Relevance | Confidence |
|----|----------|------|-----|----------------|------|-----------|------------|
| E16 | SkillOrchestra: Learning to Route Agents via Skill Transfer | Feb 2026 | paper | Skill-aware orchestration: learns fine-grained skills from execution experience, models agent-specific competence/cost. Outperforms SOTA RL-based orchestrators by 22.5% with 700x lower learning cost vs Router-R1. Explicit performance-cost tradeoff at deployment time. Code open-source. | G16 | H | High |
| | | | | Source: [arXiv:2602.19672](https://arxiv.org/abs/2602.19672v1) | | | |
| E17 | Multi-tier task classification + cascade routing (industry synthesis) | 2025-2026 | doc | 4-tier task classification: Simple (<50 tokens) → Moderate (summarization) → Complex (multi-file) → Reasoning (2+ reasoning keywords). Cascade routing: heuristics → BERT classifier → LLM — achieves 27-55% cost reduction, 5x SLO improvement, 31.6% latency reduction. Dynamic routing saves 40-85% cost at 90-95% quality. | G16, G19 | H | High |
| | | | | Sources: [Zylos Research](https://zylos.ai/research/2026-03-02-ai-agent-model-routing), [Maxim AI](https://www.getmaxim.ai/articles/top-5-llm-routing-techniques/), [Agent Patterns](https://agentpatterns.ai/agent-design/cost-aware-agent-design/) | | | |
| E18 | TCAndon-Router: Adaptive Reasoning Router for Multi-Agent Collaboration | Jan 2026 | paper | Uses natural-language reasoning chains to generate candidate agents before execution. Supports dynamic agent onboarding (not static single-label). Adaptive routing handles novel query patterns. | G16 | M | High |
| | | | | Source: [arXiv:2601.04544](https://arxiv.org/abs/2601.04544) | | | |
| E19 | xMemory: Hierarchical memory for LLM agents (King's College London + Alan Turing Institute) | Feb 2026 | paper | 4-level hierarchy: Messages → Episodes → Semantics → Themes. Two-stage retrieval: (I) greedy submodular selection on theme/semantic graph, (II) adaptive candidate inclusion via predictive entropy gating. Reduces tokens from ~9K to ~4.7K per query (29% reduction). Prevents retrieval collapse from near-duplicate flooding. | G17 | H | High |
| | | | | Sources: [arXiv:2602.02007](https://arxiv.org/abs/2602.02007), [GitHub](https://github.com/HU-xiaobai/xMemory), [Emergent Mind](https://www.emergentmind.com/papers/2602.02007) | | | |
| E20 | LangMem auto-summarization: summarize_messages + RunningSummary + SummarizationNode | 2025-2026 | doc | Production-grade context management for LangGraph. `summarize_messages` triggers at token threshold; `RunningSummary` persists across turns preventing re-summarization. Params: `max_tokens`, `max_tokens_before_summary`, `max_summary_tokens` (default 256). Enables unlimited multi-turn within fixed context window. Two-tier memory: Checkpointer (per-thread) vs Store (cross-thread LTM). | G17 | H | High |
| | | | | Sources: [LangMem API](https://langchain-ai.github.io/langmem/reference/short_term/), [LangMem Guide](https://langchain-ai.github.io/langmem/guides/summarization/), [Dev.to](https://dev.to/sreeni5018/five-agent-memory-types-in-langgraph-a-deep-code-walkthrough-part-2-17kb) | | | |
| E21 | ACON: Failure-Driven Context Compression for Long-Horizon Agents | Oct 2025 | paper | Gradient-free compression guideline optimization. Learns what to preserve from observed compression failures. 26-54% memory reduction while preserving 95%+ accuracy. On Multi-objective QA: surpasses uncompressed baselines (54.5% token reduction). Counterintuitive: selective compression improves quality by reducing context distraction. | G17 | H | High |
| | | | | Source: [arXiv:2510.00615](https://arxiv.org/abs/2510.00615) | | | |
| E22 | Contextual Engineering: ISOLATE/COMPRESS/SELECT/WRITE patterns | 2026 | doc | 4-pillar context management framework for enterprise agents. ISOLATE prevents context poisoning across domains. COMPRESS uses hierarchical summarization. SELECT uses RAG/memory queries. WRITE persists to scratchpads/LTM. Reportedly 90% performance improvement. Claude Code uses 6 mechanisms achieving 5.5x fewer tokens. 65% of enterprise AI failures in 2025 attributed to context drift, not raw context exhaustion. | G17 | H | Medium |
| | | | | Sources: [Medium / enterprise agent](https://ssahuupgrad-93226.medium.com/building-an-enterprise-grade-ai-code-review-agent-with-langgraph-a-deep-dive-into-contextual-514756b803da), [Zylos Research](https://zylos.ai/research/2026-02-28-ai-agent-context-compression-strategies), [Morph](https://www.morphllm.com/llm-context-management) | | | |
| E23 | NabaOS: Tool Receipts for Practical Hallucination Detection | Mar 2026 | paper | HMAC-signed tool execution receipts that models cannot forge. Epistemic source classification (pratyakṣa/anumāna/śabda/abhāva/ungrounded). Detects 94.2% fabricated tool references, 87.6% count misstatements, 91.3% false absence claims. <15ms verification overhead. 78.4% URL fabrication detection for deep delegation. Best cost-latency-coverage tradeoff vs zkLLM (180s/query). NyayaVerifyBench: 1,800 scenarios across 4 languages. | G18 | H | High |
| | | | | Sources: [arXiv:2603.10060](https://arxiv.org/html/2603.10060v1), [Dev.to](https://dev.to/spudbe/if-your-agent-cant-prove-it-it-didnt-happen-3bmj) | | | |
| E24 | AEGIS: Pre-Execution Firewall and Audit Layer for AI Agents | Mar 2026 | paper | 3-stage pipeline: deep string extraction → content-first risk scanning → composable policy validation. Blocked all 48 attack instances before execution. 1.2% false positive on 500 benign calls. 8.3ms median latency (1K consecutive). Ed25519 + SHA-256 hash chain for tamper-evident audit. Supports 14 frameworks (Python/JS/Go). High-risk calls held for human approval. Open-source with zero-code integration. | G18 | H | High |
| | | | | Sources: [arXiv:2603.12621](https://arxiv.org/abs/2603.12621), [GitHub](https://github.com/Justin0504/Aegis) | | | |
| E25 | HaluGate: Token-Level Real-Time Hallucination Detection for Production LLMs | Dec 2025 | doc | Token-level detection pipeline operating at 76-162ms latency (CPU-only). Two-stage conditional detection: compares LLM responses against tool-provided ground truth. Not LLM-as-judge (avoids position/verbosity bias). Designed for production vLLM integration. | G18 | H | High |
| | | | | Source: [vLLM Blog](https://blog.vllm.ai/2025/12/14/halugate.html) | | | |
| E26 | mcp-hallucination-suite: Structural Anti-Hallucination Middleware | 2026 | code | 4 suppressors for MCP: JSON Suppressor (schema validation), Prompt Suppressor (clean structure enforcement), Tool Response Suppressor (verifies claimed vs actual returns), Grounding Enforcer (cross-checks vs retrieved sources). Also: multi-agent handoff validation — 36.9% of failures originate at first handoffs and cascade. Thousands of validations/sec via callbacks. | G18 | M | High |
| | | | | Sources: [GitHub](https://github.com/steveswain14/mcp-hallucination-suite), [Dev.to handoff validation](https://dev.to/agenson_horrowitz/add-output-verification-to-any-langchaincrewai-agent-chain-in-5-lines-1gg7) | | | |
| E27 | Agent Token Economics and FinOps (2025-2026 landscape) | 2025-2026 | doc | Single agent request → 5x token budget of direct chat. Unconstrained agent: $5-8/task. 10-cycle loop: ~50x tokens. 128K context: 64x cost of 8K. Output tokens 3-8x more expensive. Token estimates: simple Q&A 500-1500, tool-calling 2K-5K, multi-step 5K-10K+. Caching: 90% cost reduction (Anthropic $3M→$0.30M). Enterprise: 1.7-2.0x budget multiplier; 80-90% cost reduction achievable. Monthly formula: requests/day × tokens × cost × 30. | G19 | H | High |
| | | | | Sources: [Zylos Research](https://zylos.ai/research/2026-02-19-ai-agent-cost-optimization-token-economics), [TechPlusTrends](https://techplustrends.com/agentic-ai-token-economics-cost-engineering/), [Reactive Agents](https://docs.reactiveagents.dev/guides/cost-optimization/), [Iternal](https://iternal.ai/token-usage-guide) | | | |
| E28 | TokenCost: Client-side token counting and cost estimation library | 2025-2026 | code | 400+ LLMs across major providers (OpenAI, Anthropic, Google, Mistral, Meta, DeepSeek, xAI). Official Tiktoken + Anthropic token counters. Free JSON API, no auth, CORS-enabled. 970K downloads/month. v0.1.26 (Aug 2025). Daily automated price updates. | G19 | M | High |
| | | | | Sources: [GitHub](https://github.com/agentops-ai/tokencost), [PyPI](https://pypi.org/project/tokencost/), [API docs](https://tokencost.ankitaglawe.com/api-docs) | | | |
| E29 | MCP Dynamic Tool Discovery: Registries (Kong Konnect, JFrog, MCP Gateway) | 2026 | doc | Shift from static hardcoded MCP → dynamic registry queries ("What tools help with this?"). Kong Konnect: native registry in Catalog, REST API compliant with Agentic AI Foundation spec. JFrog: system of record for MCP servers as binary assets, proactive malicious blocking. MCP Gateway: unified OAuth + A2A + external registry integration. Enables scalable agentic systems without redeployment. | G20 | H | High |
| | | | | Sources: [Kong](http://konghq.com/blog/engineering/mcp-registry-dynamic-tool-discovery), [JFrog](https://www.gurufocus.com/news/8722945/jfrog-unveils-universal-mcp-registry-delivering-a-secure-system-of-record-for-the-aidriven-software-supply-chain), [GitHub](https://github.com/cxhello/mcp-gateway-registry) | | | |
| E30 | MCP Tool Annotations: Risk Vocabulary and Lifecycle Hints | 2025-2026 | norm | Introduced spec 2025-03-26. 4 hints: readOnlyHint, destructiveHint, idempotentHint, openWorldHint. Defaults: pessimistic (assumed destructive if unannotated). Hints, not guarantees — for UX optimization, not security. Coverage uneven: server-filesystem (14/14 annotated) vs server-everything (0/13). Gold standard for lifecycle metadata. | G20 | H | High |
| | | | | Sources: [MCP Blog](https://blog.modelcontextprotocol.io/posts/2026-03-16-tool-annotations/), [Stanza](https://www.stanza.dev/courses/mcp-fundamentals/tools/mcp-fundamentals-tool-annotations) | | | |
| E31 | MCP 2026 Roadmap + Spec Evolution (Streamable HTTP, Server Cards, Tasks) | 2025-2026 | norm | Spec 2025-03-26: OAuth 2.1 framework + Streamable HTTP (replacing SSE) + JSON-RPC batching + tool annotations + audio data. Spec 2025-11-25: polling SSE, session resumption, 403 for invalid Origin. 2026 roadmap: 4 priorities — (1) Transport Evolution (stateless horizontal scaling, .well-known metadata), (2) Agent Communication (Tasks primitive lifecycle, retry, expiry), (3) Governance Maturation, (4) Enterprise Readiness. MCP Server Cards (.well-known/mcp.json) SEP-2127 — pre-connection discovery. On horizon: DPoP SEP-1932, WIF SEP-1933, triggers/events. | G2↻, G20 | H | High |
| | | | | Sources: [MCP Blog](https://blog.modelcontextprotocol.io/posts/2026-mcp-roadmap/), [modelcontextprotocol.io changelog 2025-03-26](https://modelcontextprotocol.io/specification/2025-03-26/changelog), [AgentSource](https://agentsource.co/articles/mcp-2026-roadmap-what-is-changing), [Ekamoira](https://www.ekamoira.com/blog/mcp-server-discovery-implement-well-known-mcp-json-2026-guide) | | | |
| E32 | MCP OAuth 2.1 Updates: Protected Resource Metadata, Resource Indicators, Horizon DPoP | 2025-2026 | norm | MCP servers = OAuth Resource Servers (spec 2025-06-18+). Protected Resource Metadata at /.well-known/oauth-protected-resource. Resource Indicators (RFC 8707) mandatory — prevents token misuse across servers. PKCE mandatory, implicit grant removed, HTTPS required. On horizon: DPoP (SEP-1932) for sender-constrained tokens, Workload Identity Federation (SEP-1933). | G11↻ | H | High |
| | | | | Sources: [Auth0](https://auth0.com/blog/mcp-specs-update-all-about-auth), [modelcontextprotocol.io auth tutorial](https://modelcontextprotocol.io/docs/tutorials/security/authorization), [MCP Playground](https://mcpplaygroundonline.com/blog/mcp-server-oauth-authentication-guide), [Microsoft ISE](https://devblogs.microsoft.com/ise/aca-secure-mcp-server-oauth21-azure-ad/) | | | |
| E33 | MASEval v0.4.0 + ProtocolBench + ProtocolRouter (latest 2026) | 2026 | paper+code | MASEval v0.4.0 (28 Mar 2026): agent-agnostic via adapters, lifecycle orchestration, per-agent tracing, Python 3.10+. ProtocolBench: protocol choice impacts task completion by up to 36.5%, latency varies by 3.48s, hybrid protocols outperform homogeneous by ~6.6%. ProtocolRouter: learnable routing, reduces fail-storm recovery by 18.1%. | G13↻ | H | High |
| | | | | Sources: [GitHub](https://github.com/parameterlab/MASEval), [PyPI](https://pypi.org/project/maseval/), [OpenReview](https://openreview.net/forum?id=lqNqKUG2dn) | | | |

---

## Source Conflict Notes

1. **xMemory token reduction:** Cursor WebSearch reports "~9K to ~4.7K" (~48% reduction), Perplexity deep_research reports "up to 29% reduction under bounded budgets." Both figures likely correct for different measurement conditions (per-query vs bounded budget optimization). Using both with context.

2. **Contextual engineering "90% improvement":** Claimed by enterprise blog post [E22]; specific conditions unclear. Marked confidence = Medium.

3. **ACON "surpasses uncompressed baselines":** Counterintuitive result specific to Multi-objective QA benchmark [E21]. May not generalize to all task types. Marked High confidence for the paper's own claims, but application scope should be validated.

4. **Cascade routing cost savings:** Ranges vary — Zylos reports 40-85%, Agent Patterns reports ~86%, MindStudio reports 27-55% for RAG-specific setups. Differences reflect measurement methodology and task mix. All are consistent in direction.

5. **MCP spec refresh (G2/G11):** E31/E32 confirm and extend E1/E10 rather than contradicting. Key additions: Streamable HTTP replacing SSE, Protected Resource Metadata, Resource Indicators RFC 8707, 2026 roadmap priorities.

---

## Gap Coverage Summary

| Gap | Status | Evidence |
|-----|--------|----------|
| G16 (Critical) — Task taxonomy A–D | **covered** | E16, E17, E18 — formal tiers with success criteria, decision tree algorithms, cascade/skill-aware routing |
| G17 (High) — Context Manager design | **covered** | E19, E20, E21, E22 — xMemory hierarchy, LangMem summarization, ACON compression, 4-pillar engineering |
| G18 (High) — Tool hallucination at MCP execution | **covered** | E23, E24, E25, E26 — NabaOS receipts, AEGIS firewall, HaluGate token-level, mcp-hallucination-suite |
| G19 (Medium) — Resource estimation/scheduling | **covered** | E17, E27, E28 — token economics, cost formulas, TokenCost library, cascade routing optimization |
| G20 (Medium) — Dynamic MCP tool discovery | **covered** | E29, E30, E31 — registries (Kong/JFrog/Gateway), tool annotations, Server Cards, 2026 roadmap |
| G2↻ — MCP spec latest | **refreshed** | E31 — Streamable HTTP, Tasks primitive, 2026 roadmap confirms MCP as canonical |
| G11↻ — MCP OAuth 2.1 | **refreshed** | E32 — PRM, Resource Indicators, DPoP on horizon |
| G13↻ — MASEval/benchmarks | **refreshed** | E33 — v0.4.0 (Mar 2026), ProtocolBench latest, ProtocolRouter |

### Remaining Holes

- **G16 (partial):** No single authoritative "A/B/C/D" task taxonomy paper exists; evidence synthesizes from multiple production patterns. Decision tree for Logos must be authored at draft time using E16-E18 as basis.
- **G19 (partial):** No dedicated ESc-integration framework found. TokenCost [E28] covers pricing; token estimation formulas [E27] are industry practice but not a formal spec. Logos-specific ESc/D03 integration must be designed.
- **G17:** Well-covered across multiple angles; no significant gaps remain.
- **G18:** Strong coverage from 4 independent systems; deployment integration pattern for Logos Orchestrator must be specified at draft time.
- **G20:** Registry ecosystem maturing rapidly; specific Logos Virtual Server lifecycle design must be authored.
