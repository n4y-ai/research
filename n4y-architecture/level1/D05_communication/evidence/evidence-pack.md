# Evidence Pack: Step 5 — L1/D05 Коммуникационное

> **Измерение:** L1/D05 | **Версия:** 2.0 (hard re-run) | **Дата:** 03.04.2026  
> **Sources:** E1–E7 (migrated) + E11–E28 (new, this run)  
> **Search passes:** Cursor WebSearch (15 queries, ~25 sources), Perplexity deep_research (3 queries, ~60 cited sources)

---

## Evidence Table

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | Relevance | Confidence |
|----|----------|------|-----|----------------|------|-----------|------------|
| E1 | A2A Protocol (Google) v0.2.1 / RC v1.0 | Apr 2025 | norm | 50+ enterprise partners; task lifecycle submitted→working→input-required→completed; HTTP + JSON-RPC 2.0 + SSE; multimodal Parts; Agent Card | G1, G3, G5 | H | High |
| E2 | DIDComm Messaging (DIF) v2.1 | 2025 | norm | E2E encryption (X25519 + XChaCha20-Poly1305); transport-agnostic; DID-rooted; nonce+timestamp; async relay | G1, G6, G7 | H | High |
| E3 | AG-UI Protocol | 2025 | doc | Стандартизация событий Логос↔Человек: message, tool_call, state_snapshot, run_complete; мультиканальность | G4 | H | High |
| E4 | Linux Foundation Agentgateway | Jun 2025 | doc | Centralized rate limiting + per-tenant namespacing для A2A-потоков; HTTP 429 + Retry-After | G10 | H | High |
| E5 | FIPA ACL (IEEE, 2002) | 2002 | norm | Speech act theory; перформативы: inform, request, propose, cfp, accept, reject и др. (подробнее → L3_D02 §3-4) | G1 | M | High |
| E6 | Fetch.ai uAgents framework | 2025 | doc | Typed messages (Pydantic); Almanac on-chain discovery; async messaging через endpoints | G3 | M | High |
| E7 | ASI Alliance (Fetch.ai + SingularityNET) | Jul 2025 | doc | Unified ASI token; cross-platform protocol alignment; A2A adapter для Fetch.ai | G5 | M | High |
| E11 | A2A Protocol v0.3.0 Specification (a2a-protocol.org) | Apr 2026 | norm | gRPC support (a2a.proto); signed Agent Cards (JWS); TCK for multi-transport validation (JSON-RPC, gRPC, REST); 150+ org supporters; Linux Foundation governance | G1 | H | High |
| E12 | Apigene Blog — "MCP vs A2A: When to Use Each Protocol (2026)" | Mar 2026 | guide | MCP: 97M+ monthly SDK downloads, 5000+ servers. A2A: ~200 agents. Both under Linux Foundation. Complementary: MCP=vertical (tools), A2A=horizontal (agents). Production 2026 uses both. | G1, G11 | H | High |
| E13 | AG-UI on Amazon Bedrock AgentCore Runtime (AWS) | Mar 2026 | doc | Native AG-UI support across 14 AWS regions. Protocol stack: HTTP(8080), MCP(8000), A2A(9000), AG-UI(8080). Framework-agnostic: Strands, LangGraph, CrewAI. Auth, session isolation, auto-scaling. | G4, G11 | H | High |
| E14 | Microsoft A2A Implementation (Azure AI Foundry, Copilot Studio) | 2026 | doc | Microsoft implementing A2A in Azure AI Foundry and Copilot Studio. Cross-platform agent interop. 50+ major contributors (SAP, Oracle, Accenture, Deloitte). AI agent market €7.84B→€52B+ (2025→2030). | G1 | H | High |
| E15 | Linux Foundation AAIF (Agentic AI Foundation) | Dec 2025 | norm | AAIF formed Dec 9, 2025. Inaugural projects: MCP (Anthropic), goose (Block), AGENTS.md (OpenAI). Platinum: AWS, Anthropic, Bloomberg, Cloudflare, Google, Microsoft, OpenAI. | G1 | H | High |
| E16 | A2A Trust Layer Proposal — Agent-Mesh (GitHub Issue #1472) | Feb-Mar 2026 | proposal | TrustedAgentCard, CMVKIdentity (DIDs for agents), TrustHandshake (pre-task verification), DelegationChain (verifiable capability delegation). Pragmatic alt: OAuth + DNS + API keys + trust scoring. | G6, G15 | M | Medium |
| E17 | Agentgateway v1.0.1 (Linux Foundation) | Mar 2026 | doc | Built in Rust. 79 releases, 120 contributors (AWS, Cisco, Microsoft, IBM). Kubernetes Gateway API + custom resources. Protocol-aware "Layer 8" proxy: MCP Gateway (tool federation) + A2A Gateway (capability discovery). LLM provider translation. PreRouting policies via CEL. | G10, G11 | H | High |
| E18 | StackA2A — "Error Handling Patterns for A2A Agents" | Mar 2026 | guide | JSON-RPC 2.0 errors: standard codes + A2A custom (-32001...-32008). retryable/retryAfter metadata. Exponential backoff. Circuit breaker (3 failures → 30s cooldown). Error propagation across multi-agent chains. Fan-out-resilient pattern. Never retry client errors. | G12 | H | High |
| E19 | AXME Lifecycle Layer (github.com/AxmeAI/axme) | Mar 2026 | doc | Wraps A2A in "intents". At-least-once delivery (3 retries default). Auto-redelivery on crash. Durable state. Configurable deadlines. Alpha status. Open-source + managed cloud. | G12 | H | Medium |
| E20 | A2A Python SDK — RetryTransport (Issue #871) | 2026 | proposal | Proposed decorator: transparent retries with exponential backoff. Transient error set: 429, 502-504, timeouts. Handles Retry-After headers. Not yet merged; devs implement at call site. | G12 | M | Medium |
| E21 | AgentLock Open Standard (agentlock.dev) | 2025-2026 | norm | Framework-agnostic authorization gate. RBAC, scope constraints, rate limiting, PII redaction, audit logging, single-use tokens (SHA-256 parameter hashing). v1.2.0, 745+ tests, Apache 2.0. LangChain/CrewAI/AutoGen integrations. Reduced injections 73→12 in 181 adversarial tests. | G15 | H | High |
| E22 | Zylos Research — "Capability-Based Security (OCap) for AI Agent Tool Invocation" | Mar 2026 | research | OCap model: unforgeable tokens, transferable, attenuatable. Eliminates ambient authority, confused deputy, non-compositional security. OCapN for networked capabilities. Hybrid: OCap + deterministic governance kernel (OPA + Erlang/OTP actor model). | G15 | H | High |
| E23 | Open Agent Passport (OAP) — arXiv preprint | Mar 2026 | research | Deterministic pre-action auth: ALLOW/CLAMP/DENY. Cryptographic audit (ML-DSA-87/FIPS 204 post-quantum). Median latency 53ms. Social engineering: 0% success under restrictive policy (vs 74.6% permissive). Apache 2.0. Compatible: OpenClaw, Claude Code, LangChain, CrewAI. | G15 | H | High |
| E24 | Microsoft Agent Governance Toolkit | Apr 2026 | doc | 7-package system (Python, TS, Rust, Go, .NET). Agent OS: stateless policy engine, p99<0.1ms, YAML/OPA Rego/Cedar. Agent Mesh: Ed25519 zero-trust, Inter-Agent Trust Protocol, 0-1000 trust scoring. Agent Runtime: execution rings, saga orchestration, kill switch. Agent Compliance: EU AI Act, HIPAA, SOC2. 9500+ tests, MIT. | G15 | H | High |
| E25 | docs.ag-ui.com — "Agentic Protocols" comparison | 2025-2026 | doc | Three protocols complementary, not competing. Single agent can use all three simultaneously. AG-UI + A2A + MCP as unified stack. State mgmt: snapshots + deltas (JSON Patch RFC 6902). | G4, G11 | H | High |
| E26 | dev.to — "A2A Tells Agents How to Talk. It Doesn't Tell Them What Happens When Things Break." | 2026 | analysis | A2A core spec lacks: DLQ, crash recovery, delivery guarantees, state reconciliation. Requires operational lifecycle layer (e.g., AXME). Production gap between communication protocol and operational reliability. | G12 | H | High |
| E27 | LavinMQ — "Build Scalable AI Agents with A2A and LavinMQ" | 2026 | guide | A2A HTTP endpoint → AMQP backing queue → round-robin workers → SSE progress. Durable message persistence. Exactly-once via message ID dedup. Reference DLQ architecture for A2A. | G12 | M | High |
| E28 | Adversa.ai — "Cascading Failures in Agentic AI: OWASP ASI08" | Jan 2026 | research | Cascading failure taxonomy: fan-out, oscillation, deadlock, resource exhaustion cascades. Cross-protocol propagation: MCP→A2A→business logic. Defensive layers: circuit breakers, rate limiting, bulkheads, graduated response. | G12 | H | High |

---

## Gap Coverage Summary

| Gap | Status | Evidence | Notes |
|-----|--------|----------|-------|
| **G1** (refresh) | **Refreshed** — v0.3 confirmed | E1, E11, E12, E14, E15 | A2A v0.3 (gRPC, signed Agent Cards, TCK); 150+ orgs; Linux Foundation AAIF; Microsoft Azure AI Foundry implementation. Significant evolution since v0.2.1. |
| **G4** (refresh) | **Refreshed** — production-ready | E3, E13, E25 | AG-UI on AWS Bedrock (14 regions, Mar 2026). CopilotKit SDKs. State management via snapshots + JSON Patch deltas. |
| **G6** (refresh) | **Refreshed** — partial | E2, E16 | DIDComm v2.1 stable (minor: alternative serviceEndpoint, optional body). A2A trust layer (TrustedAgentCard, TrustHandshake) proposed but pragmatic OAuth approach dominant. **No mTLS-specific standard found.** |
| **G10** (refresh) | **Refreshed** — v1.0.1 | E4, E17 | Agentgateway v1.0.1 (Mar 2026). Rust. Kubernetes-native. Protocol-aware: MCP+A2A gateway. Local+remote rate limiting. CEL policies. 120 contributors. |
| **G11** (open→closable) | **Strong coverage** | E12, E13, E17, E25 | Three-protocol model clear: MCP=tools, A2A=agents, AG-UI=UI. Decision tree: directionality → capability → latency. Agentgateway as routing infrastructure. Bedrock port conventions. No formal routing spec but industry consensus strong. |
| **G12** (partial→closable) | **Strong coverage** | E18, E19, E20, E26, E27, E28 | A2A error codes fully documented. Retry patterns (exponential backoff, circuit breaker). DLQ via AMQP/Kafka backing. AXME lifecycle layer. OWASP ASI08 cascading failure taxonomy. Cross-protocol propagation model clear. |
| **G15** (open→closable) | **Strong coverage** | E16, E21, E22, E23, E24 | Four complementary frameworks: AgentLock (RBAC gate), OCap/OCapN (unforgeable tokens), OAP (deterministic auth + post-quantum audit), Microsoft Governance Toolkit (policy engine + trust scoring + execution rings). A2A signed cards = identity only, not action-level. |

---

## Source Conflicts and Uncertainties

1. **A2A v0.3 vs v0.2.1 naming:** Some sources refer to latest as "v0.3", others as "v0.3.0". Official spec site uses v0.3.0. Agent Card path varies: `/.well-known/agent.json` vs `/.well-known/agent-card.json` across sources. **Resolution:** Use v0.3.0 and `/.well-known/agent.json` per official a2a-protocol.org.

2. **A2A error codes numbering:** StackA2A guide uses -32001 to -32008 range. AWS Bedrock uses -32501 to -32503 for platform-specific errors. **Resolution:** Both are valid — spec reserves -32000 to -32099 for server-defined; AWS adds platform-specific codes in different range.

3. **DIDComm + mTLS:** No concrete standard or implementation found for mTLS integration with DIDComm v2.1. Trust layer proposals (Issue #1472) use OAuth+DNS pragmatically. **Uncertainty flag: mTLS integration remains unvalidated.**

4. **AXME maturity:** Alpha status (March 2026). No production adoption data. Claims of at-least-once delivery not independently verified. **Low confidence for production readiness; architecture pattern is sound.**

5. **G15 framework overlap:** AgentLock, OAP, and Microsoft Governance Toolkit all address similar action-level authorization problem with different approaches. No interoperability standard between them yet. **Open question: which becomes dominant standard?**

---

## Quality Checklist

- [x] Both search-draft files exist and are non-empty
- [x] evidence-pack.md uses E1…E28 and maps gaps from §6
- [x] No evidence row without a URL/source identification
- [x] Conflicts between sources called out (§ Source Conflicts)
- [x] Existing E1-E7 preserved; new evidence starts from E11
