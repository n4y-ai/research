# Evidence Pack: Step 5 — L1/D03 Материальное

> **Измерение:** L1/D03  
> **Версия:** 3.0 (v3 pipeline — G13, G15–G18 closure + G8/G9/G11 enrichment)  
> **Дата:** 03.04.2026

---

## Existing Evidence (E1–E20)

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | Relevance | Confidence |
|----|----------|------|-----|----------------|------|-----------|------------|
| E1 | Akash Network Supercloud + 2026 roadmap | 2025-2026 | doc | Mainnet 14 (Cosmos SDK v0.53); AkashML managed AI inference; до 85% дешевле GPU vs централизованные; BME модель (Q1 2026); Hardware Verification через TEE (Q2 2026); HomeNode edge | G1, G3, G5, G6 | H | High |
| E2 | Render Network | 2025 | doc | Эволюция от rendering к AI workloads; enterprise-grade GPU (NVIDIA); burn-and-mint equilibrium; premium tier | G1, G3 | M | High |
| E3 | io.net GPU aggregator | 2025 | doc | Aggregator GPU-ресурсов из множества источников; быстрый clustering для ML задач | G1, G3 | M | High |
| E4 | Fetch.ai Resource Discovery (Almanac) | 2025 | doc | Агенты автономно обнаруживают и арендуют compute через on-chain marketplace | G1, G3 | M | High |
| E5 | IPFS + Filecoin | актуальная | norm | Content-addressed storage; Filecoin proof-of-storage incentives; подходит для RAG-баз и публичных данных | G4 | H | High |
| E6 | Arweave permanent storage | актуальная | doc | Permanent storage для immutable records; аудиторские логи; one-time fee | G4 | H | High |
| E7 | Intel SGX / AMD SEV / ARM TrustZone TEE | 2025-2026 | norm | Trusted Execution Environments для конфиденциальных вычислений; GPU-TEE в разработке; защита данных в анклаве | G2, G6 | H | High |
| E8 | Akash Hardware Verification TEE roadmap Q2 2026 | 2026 | doc | Расширение пула доверенных провайдеров через hardware attestation | G6 | H | Med |
| E9 | Akash BME launch + WASM + ACT token + pricing (Perplexity deep research) | Mar 2026 | doc | BME live 23.03.2026 "Project Twilight". Two-token: AKT (burn) + ACT (USD-pegged compute token). H100 $1.46/hr, A100 $0.78/hr, H200 $2.90/hr vs AWS H100 $4.50-6.88/hr (65-75% savings). No egress fees. Circuit-breaker at CR<0.95. WASM smart contracts. Governance Proposal 318. Sources: akash.network/roadmap/aep-76, akash.network/blog/what-burn-mint-equilibrium-means-for-akash, akash.network/pricing/gpus | G8, G13 | H | High |
| E10 | Akash TEE attestation pipeline (AEP-29) | 2026 | doc | ETA 05/30/2026. CPU/GPU/firmware measurement → Intel Trust Authority + AMD Attestation + NVIDIA NVTrust → policy enforcement. Source: akash.network/roadmap/aep-29/ | G8 | H | High |
| E11 | io.net GPU network metrics + pricing | 2025-2026 | doc | 327K verified GPUs (Mar 2025, 5.4x YoY), 130+ countries, 5,350 cluster-ready. H100 PCIe $0.89/hr vs AWS $12.29/hr. H100 SXM5 $1.19/hr. H200 $2.39/hr. 100×H100: $30K/mo vs $300K/mo AWS. Source: ionetwork.cloud, research.nansen.ai | G9 | H | High |
| E12 | io.net zkTFLOPs Proof-of-Contribution | 2025 | doc | Hardware verification via zero-knowledge proof of compute contribution. Source: io.net docs | G14 | M | High |
| E13 | Render Dispersed.com + AI pivot | Dec 2025 | doc | 600+ models at $1.75/hr (RTX 4090/5090, H200/H100, AMD MI300). AI inference = 40% compute capacity. 65M frames, 5,600 GPU nodes. Source: blockeden.xyz, ownyourmind.ai | G10 | H | High |
| E14 | Render BME tokenomics | 2025 | doc | Burn-and-mint: tokens spent on jobs permanently burned. 530K tokens burned 2025 (279% vs 2024). Source: blockeden.xyz | G10 | M | High |
| E15 | VeriLLM: Verifiable Decentralized LLM Inference | 2025 | academic | Verifiers validate at ~1% of inference cost. Exploits prefill/autoregressive separation. Minimal on-chain checks. Source: arXiv 2509.24257 | G11, G14 | H | High |
| E16 | Cortensor PoUW + PoI dual validation | 2025-2026 | doc | Proof of Inference (embedding vector distance) + Proof of Useful Work (LLM-evaluated quality). State machine: request→create→prepare→active→precommit→commit→end. NodeReputation system. On-chain billing COR/ETH. Source: docs.cortensor.network | G11, G14 | H | High |
| E17 | FluxEdge compute marketplace | 2026 | doc | K8s integration, elastic provisioning, enterprise-grade GPUs (RTX 4090/H100/AMD). Below traditional cloud pricing. Source: runonflux.com/fluxedge | G12 | M | Med |
| E18 | DeRouter AI access layer | Mar 2026 | doc | Idle capacity monetization. Frontier models at up to 90% discount. On-chain USDC settlement every 60s via Solana. Source: derouter.network/whitepaper | G12 | M | Med |
| E19 | AIProx agent discovery registry | 2026 | doc | Sub-1s settlement. Lightning/Solana USDC/Base payment rails. Capability+price+reputation queries for orchestrators. Source: aiprox.dev | G12 | M | Med |
| E20 | ReGraph marketplace | Jan 2026 | doc | OpenAI-compatible API. Up to 80% savings. Supports GPUs/TPUs/NPUs/CPUs/mobile. Source: github.com/ildu00/ReGraph | G12 | M | Med |

---

## New Evidence (E21–E39)

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | Relevance | Confidence |
|----|----------|------|-----|----------------|------|-----------|------------|
| E21 | GPUPerHour Data Egress Reference (44 providers) | Feb 2026 | benchmark | 6 of 12 GPU clouds charge zero egress (RunPod, Lambda, Vast.ai, Salad, Voltage Park, VERDA). AWS $90/TB, GCP $120/TB, Azure $87/TB. Decentralized/neocloud: Storj $7/TB, Cloudflare R2 $0. 127× cost spread across 44 providers. Source: gpuperhour.com/reference/data-egress | G13 | H | High |
| E22 | CoreWeave Zero Egress Migration (0EM) | 2026 | doc | Fully covers egress fees for AI workload migrations from third-party clouds. End-to-end data validation, high-speed transfer. Addresses egress as adoption barrier for multi-cloud AI. Source: coreweave.com/news/coreweave-announces-zero-egress-migration | G13 | M | High |
| E23 | Filebase IPFS Bandwidth Pricing + CDN Performance | 2026 | doc+benchmark | Tiered plans: $20/mo (1TB BW), $100/mo (5TB), $500/mo (25TB + unlimited BW). Dedicated gateways + CDN TTFB: Europe 61ms, NA 135ms, APAC 222ms (vs public IPFS 1.1-1.9s). Source: filebase.com/blog/never-pay-for-ipfs-bandwidth-again, filebase.com/blog/optimizing-global-ipfs-performance-with-custom-gateways-cdn | G13, G17 | H | High |
| E24 | Vector DB Benchmarks 2026 (Qdrant, Pinecone, Weaviate, Milvus) | 2026 | benchmark | Qdrant: 3.54ms median / 4.95ms p95 / 25ms p99 @ 1M×1536d, 1,238 QPS, 0.99 recall. Pinecone: 8ms p50 / 45ms p99. Weaviate: 4.99ms / 7.16ms p95. Milvus: <10ms @ 1M. pgvectorscale: 471 QPS @ 50M, 11.4× faster than Qdrant at 99% recall. At 10M: Qdrant 22ms p95 (2× faster than Pinecone 45ms). Network latency dominates remotely (~110ms p50). Source: qdrant.tech/benchmarks, pooya.blog, salttechno.ai, leaper.dev, firecrawl.dev/blog/best-vector-databases | G17 | H | High |
| E25 | IPFS Pinning Propagation Latency | archived+2026 | doc | Pin to global DHT availability: up to ~1 hour via public gateways. Own gateway eliminates propagation lag (local availability + async DHT). Architecture: IPFS for warm/cold tier; local VDB for hot tier mandatory. Source: discuss.ipfs.tech (propagation lag, retrieval time threads) | G17 | M | Med |
| E26 | JFrog MCP Registry | Mar 2026 | doc | Centralized system of record for MCP servers + AI assets. Supply chain governance: pre-execution validation blocking malicious/non-compliant servers. Covers MCP servers, agent skills, models, agentic binary assets. Source: ecommercenews.uk/story/jfrog-unveils-mcp-registry | G15 | H | High |
| E27 | Kong MCP Registry (Konnect) | 2026 | doc | Dynamic discovery, enterprise governance, full observability for MCP servers. Tech preview in Kong Konnect. Register/discover/govern MCP servers with auth controls. Source: konghq.com/products/mcp-registry | G15 | H | High |
| E28 | agentic-community/mcp-gateway-registry | 2026 | open-source | Enterprise-ready MCP Gateway + Registry with OAuth2 (Keycloak/Entra). Dynamic tool discovery, semantic search via metadata, credentials-provider components. Unified access for autonomous agents + coding assistants. Source: github.com/agentic-community/mcp-gateway-registry | G15 | H | High |
| E29 | MCP Tool Monetization Ecosystem (SettleGrid, xpay, Stripe) | 2026 | doc | SettleGrid: per-call billing, 6 pricing models (per-call/token/byte/second/tiered/outcome), Stripe payouts, 2-line integration. xpay: x402-based payment proxy, USDC on Base. Stripe: native MCP checkout (redirect + instant). 20K+ MCP servers, <5% monetized, 97M monthly SDK downloads. Source: dev.to/lexwhiting, docs.xpay.sh, docs.stripe.com/agentic-commerce/apps | G15 | H | Med |
| E30 | X402 HTTP Payment Protocol — adoption + controls | 2025-2026 | doc+norm | HTTP 402 revived for AI agent payments. USDC on L2 (Base). Oct 2025: ~46K tx/week → Nov 2025: 1M+ tx/week. Total: >$600M volume, >100M flows since May 2025. PolicyLayer: per-endpoint limits, daily caps, frequency limits, duplicate detection, circuit breaker (10 violations). Developed by Coinbase Development Platform team. Security risks: infinite loops, prompt injection. Source: policylayer.com, agentpmt.com, dev.to/walletguy, solana.com/x402 | G15, G16 | H | High |
| E31 | Economic Firewalls & Token-Based Rate Limiting | 2026 | doc | Economic firewalls: gateway-layer spend tracking (sub-ms overhead), per-tool cost attribution, per-agent caps, delegation hierarchies, 3 modes (Charge/Control/Observe). SatGate: macaroon-based capability tokens. Token-based rate limiting (Zuplo): counts actual resource consumption not requests. Gartner: >30% API demand increase from AI/LLM by 2026. Source: dev.to/mattdeangit, zuplo.com | G15, G16 | H | Med |
| E32 | LEP100-3: Budget & Cost Accounting (Lithic/Lithosphere) | Mar 2026 | standard | Escrow-based cost enforcement: locks funds before computation. Per-user quotas, deterministic settlement, budget reset, cost ceiling (`max_cost` per request, LithoVM enforces). LITHO-denominated. Complements LEP100-1 (Core) + LEP100-2 (AI Service Provider). Source: docs.lithic.at/lep100-3, newsfilecorp.com/release/288955 | G16 | H | Med |
| E33 | BudgetGuard: FinOps Control Plane | Jul 2025-2026 | open-source | Drop-in API gateway for hard budget enforcement. Multi-tenant quotas, immutable UsageLedger audit trail, OPA/Rego policies, per-minute rate limiting. ~10-15ms overhead. Docker-first, Apache 2.0, TypeScript. Supports OpenAI/Anthropic/Google. Context: 96% enterprises exceed AI cost projections, single agents generated $47K bills. Source: github.com/budgetguard-ai/budgetguard-core | G16 | H | Med |
| E34 | Ophir Protocol — Agent SLA Enforcement | Mar 2026 | doc | Open protocol for AI agent-to-agent commerce on Solana. 8 SLA metrics (p99 latency, uptime %). 5-round RFQ negotiation. Ed25519 dual-signed + SHA-256 hash. Escrow vaults USDC. 12-state lifecycle (discovery→dispute). did:key identity. JSON-RPC 2.0. SDK: @ophirai/sdk, @ophirai/protocol. Source: github.com/Ophir-Protocol/ophir | G18 | H | High |
| E35 | FLUX Marketplace — SLA Slashing | 2026 | doc | Automatic SLA slashing on Solana. Escrow-based payments + staking-based SLA guarantees. Providers stake collateral for performance. Source: github.com/nutcas3/flux | G18 | M | Med |
| E36 | TessPay — Verify-then-Pay | 2026 | academic | Escrow during task execution. Proof of Task Execution (cryptographic evidence). Funds release on verification predicates. Tamper-evident audit trails for post-settlement disputes. Source: arxiv.org/pdf/2602.00213 | G18 | M | Med |
| E37 | Internet Court — AI Jury Dispute Resolution | 2026 | doc | Dispute resolution for agent economy. AI jury evaluates evidence → TRUE/FALSE/UNDETERMINED verdict. Statement → Guidelines → Evidence Submission → AI Validator Consensus. Supports SLA disputes (task completion, uptime, benchmarks, deadlines). Base Sepolia testnet. Source: internetcourt.org | G18 | M | Med |
| E38 | OpenTelemetry AI SLA Monitoring + gen_ai.* conventions | 2026 | doc+norm | Automated SLA compliance from trace data: throughput, error budget, P50/P95/P99 latency, availability. spanmetrics → Prometheus → SLA dashboards. gen_ai.* attribute set: standardized LLM tracing (token counts, latency, model ID, cost). vLLM monitoring with end-to-end metrics. Multi-cloud dependency maps with SLA annotations. Source: oneuptime.com (Feb 2026), parseable.com, webcoderspeed.com, megastorage.cloud | G18 | H | High |
| E39 | io.net Agent Compute + MCP + zkGPU-ID (NovaNet) | Mar 2026 | doc | Agent Compute: autonomous GPU provisioning via MCP (no human). 10K+ GPUs, 138 regions. Continuous auction matching. Developer controls: spending limits, resource caps. zkGPU-ID (NovaNet): zkVM-backed GPU verification, ZK proofs attest GPU integrity, tampered firmware → invalid proofs. Source: io.net/docs/guides/clouds/agent-cloud, greensheet.com (Mar 25, 2026), binance.com — io.net × NovaNet | G9, G15, G16 | H | High |

---

## Enrichment Updates to Existing Evidence

### E9 (Akash BME) — enriched with AEP-76 formula
- BME formula: ΔSupply(AKT) = 1/P_mint − 1/P_settle (P = 30-min TWAP AKT/USD)
- Initial spread: 25 basis points
- Collateral ratio: CR = (VaultAKT × P) / OutstandingACT; circuit-breaker at CR < 0.95
- 300K AKT volatility buffer seeded from community pool
- Governance Proposal 318 passed, hard fork at block 26,063,777
- Source: akash.network/roadmap/aep-76, ourcryptotalk.com, coinmarketcal.com

### E10 (Akash TEE AEP-29) — enriched with hardware requirements
- CPU: Intel Xeon 5th gen (Sapphire Rapids) + TDX, or AMD EPYC Milan/Genoa + SEV-SNP
- GPU: NVIDIA H100/H200 Hopper with NVTrust + CC-on mode (only current production option)
- Enables AEP-65 (Confidential Computing)
- Source: akash.network/roadmap/aep-29

### E11/E12 (io.net) — enriched with Agent Compute + zkGPU-ID
- See E39 for full Agent Compute + NovaNet data
- Updated network scale: 10K+ active GPUs (marketplace), target 1M+ aggregated
- 200 exaFLOPs estimated additional capacity
- Source: io.net, greensheet.com, binance.com

### E15 (VeriLLM) — enriched with v4 cryptographic details
- Paper version v4 (Jan 22, 2026)
- Commit-then-sample-check: Merkle root → VRF challenge → on-chain verify
- Isomorphic inference-verification: same GPU workers for both roles
- Task indistinguishability prevents selective behavior
- Two-tier: off-chain compute + Merkle proofs, on-chain: compact digests + VRF + sampled openings
- Collateral staking with auto reward/penalty
- Source: arxiv.org/abs/2509.24257v3 (v4)

### E16 (Cortensor) — enriched with 2026 roadmap + Corgent
- Corgent Virtual Trust Oracle: delegation/validation/arbitration-as-a-service
- MCP + x402 support in Router Agent v2.0
- PoUW 6-phase state machine detailed (create→prepare→active→precommit→commit→end)
- Timeline: Q3 Mainnet-Lite (Arbitrum L2), Q4 Mainnet-Full (COR L3 sovereign rollup)
- Source: docs.cortensor.network

---

## Gap Coverage Summary

| Gap | Status | Primary Evidence |
|-----|--------|-----------------|
| G13 (Data egress + RAG sync) | **closed-by-evidence** | E21 (44-provider egress benchmarks), E22 (CoreWeave 0EM), E23 (Filebase IPFS BW + CDN TTFB), E9 (Akash no egress) |
| G15 (Tooling & API access) | **closed-by-evidence** | E26 (JFrog MCP Registry), E27 (Kong MCP Registry), E28 (OSS mcp-gateway-registry), E29 (SettleGrid/xpay/Stripe monetization), E30 (x402 protocol + controls), E31 (economic firewalls) |
| G16 (Quota enforcement) | **closed-by-evidence** | E30 (x402 spending controls), E31 (economic firewalls, delegation hierarchies), E32 (LEP100-3 escrow + per-user quotas), E33 (BudgetGuard FinOps), E39 (io.net developer spending limits) |
| G17 (Vector DB sync latency) | **closed-by-evidence** | E24 (VDB benchmarks: Qdrant 3.54ms p50 → 25ms p99), E23 (IPFS CDN 61-222ms TTFB), E25 (IPFS propagation ~1hr → own gateway eliminates). Architecture: hot=local VDB, warm=IPFS+CDN, cold=Arweave |
| G18 (SLA observability + disputes) | **closed-by-evidence** | E34 (Ophir: 8 SLA metrics, escrow, disputes on Solana), E35 (FLUX: auto-slashing), E36 (TessPay: verify-then-pay), E37 (Internet Court: AI jury), E38 (OTel gen_ai.* + SLA dashboards) |
| G8 (Akash enrichment) | **enriched** | E9 (BME formula + CR), E10 (AEP-29 hardware reqs), plus new pricing data in E21 |
| G9 (io.net enrichment) | **enriched** | E39 (Agent Compute + zkGPU-ID via NovaNet) |
| G11 (VeriLLM/Cortensor enrichment) | **enriched** | E15 (VeriLLM v4 crypto details), E16 (Cortensor 2026 roadmap + Corgent) |

---

## Source Conflicts & Notes

1. **Akash H100 pricing discrepancy:** E9 reports $1.46/hr, GPUPerHour reports $1.33/hr (Feb 2026), Spheron benchmark shows $2.01/hr on their platform. Pricing varies by provider within Akash marketplace — $1.33-1.46/hr is the typical range. Spheron is a separate entity using Akash infra.

2. **io.net GPU count:** E11 (Mar 2025) reports 327K verified, E39 (Mar 2026) reports 10K+ active on marketplace. The 327K figure includes all verified GPUs in the network; 10K+ reflects currently active/available marketplace supply. No contradiction — different metrics.

3. **Redis vector latency:** Qdrant benchmark shows Redis 140ms median (cross-index operations), while other sources report sub-ms for simple KV vector lookups. Redis is optimized for caching pre-computed vectors, not general vector search.

4. **IPFS propagation:** Up to ~1 hour for public gateway availability (community reports). Filebase CDN shows 61-222ms TTFB for cached content. These are different measurements: DHT propagation vs retrieval of already-cached content.

5. **LEP100-3 and BudgetGuard:** Both are early-stage (Mar 2026 / Jul 2025 respectively). LEP100-3 is blockchain-native (LithoVM); BudgetGuard is conventional API gateway. Neither has large-scale production deployment data. Confidence: Med.

6. **Internet Court:** Currently on Base Sepolia testnet only. No mainnet deployment or production case data available. Confidence: Med.
