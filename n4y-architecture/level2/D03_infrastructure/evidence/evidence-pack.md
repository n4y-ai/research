# Evidence Pack: L2/D03 — Инфраструктурно-материальное

> **Измерение:** L2/D03 | **Версия:** 2.0 | **Дата:** 04.04.2026  
> **Источники:** E1–E45 | **Новые (v2.0):** E14–E45

---

## Previous Evidence (v1.0, migrated)

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | Confidence |
|----|----------|------|-----|----------------|------|------------|
| E1 | Akash Network mainnet 14 + 2026 roadmap | 2025-2026 | doc | GPU H100 $0.14-3.35/hr; Burn-Mint Equilibrium Q1 2026; TEE Hardware Verification Q2 2026; HomeNode edge 2026 | G1 | High |
| E2 | IO.net (GPU aggregator) | 2025 | doc | H100 SXM5 $1.19-1.99/hr; vs AWS $6.88/hr (71% savings); IDE incentive model | G1 | High |
| E3 | IntuitionLabs LLM API Pricing 2025 | Feb 2026 | doc | GPT-5 $1.25/$10/M tokens; DeepSeek R1 $0.028/$0.42; -10x/year cost trend | G1 | High |
| E4 | vLLM (Red Hat, Oct 2025) | Oct 2025 | doc | PagedAttention 24x throughput; Continuous Batching >90% GPU utilization; llm-d 30-50% cost reduction | G2 | High |
| E5 | NVIDIA Speculative Decoding (Sep 2025) | Sep 2025 | doc | EAGLE-3: up to 3.6x inference throughput | G2 | High |
| E6 | Phala Network TEE comparison (Nov 2025) | Nov 2025 | doc | AMD SEV-SNP: 3-9% overhead; Intel TDX: 1-2%; NVIDIA H100 GPU TEE: 5-15%, $3-5/hr | G3 | High |
| E7 | Open Source For You IPFS/Arweave/Filecoin | Jun 2025 | doc | IPFS: mutable content-addressed; Filecoin: marketplace; Arweave: immutable permanent; 0G: AI-native structured | G4 | High |
| E8 | Filecoin Onchain Cloud (Nov 2025) | Nov 2025 | doc | Verifiable storage + ERC-8004 integration for agent identity metadata | G4 | High |
| E9 | research_L2_infra.md | Mar 2026 | doc | Comprehensive Phase 8 resolutions G1-G9; smart contract stack; oracle stack | G5-G9 | High |
| E10 | Chainlink/Pyth/Uniswap TWAP oracle stack | 2025-2026 | doc | Chainlink price feeds + Pyth real-time + TWAP circuit breaker backup | G6 | High |
| E11 | L2 State Channels + x402 protocol | 2025-2026 | doc | <$0.001/tx; x402 $600M+ volume | G7 | High |
| E12 | ERC-4337 Account Abstraction / Paymaster | 2025-2026 | doc | Gasless UX; Paymaster sponsors transactions | G8 | High |
| E13 | Base L2 Stage 1 (fault proofs) | Feb 2026 | doc | Fault proofs live; multi-provider; 3+ IPFS pinning; TEE failover | G9 | High |

---

## New Evidence (v2.0, 04.04.2026)

### G10 — Resource Router Scoring Algorithm & On-Chain Enforcement

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | URL | Confidence |
|----|----------|------|-----|----------------|------|-----|------------|
| E14 | x402 RouteNet — composite scoring for payable services on Base | 2026 | code/doc | Composite scoring: success_rate (0.35) + schema_compliance (0.15) + quality_average (0.20) + trust_metrics (0.15) + domain_specific (0.15). Multi-provider fallback; latency-based selection. 251+ payable services. Alternative weighting from Cursor: uptime 40% + speed 30% + ERC-8004 trust 30%. | G10 | https://github.com/rplryan/x402-routenet / https://mintlify.com/Aypp23/x402-hackathon/api/admin/procurement | High |
| E15 | dRPC — multi-provider routing layer (2026 review) | 2026 | doc | 115 chains, 203 networks, 60+ RPC providers. Provider selection by: location, capacity, recent performance, provider status, CU load. Dproxy → independent Dshackle instances. | G10 | https://cryptoadventure.com/drpc-review-2026-multi-provider-routing-unified-endpoints-and-incident-response-tradeoffs/ | Medium |
| E16 | DeRouter — AI subscription marketplace (whitepaper) | 2025-2026 | doc | Reputation-weighted, load-aware matching with session stickiness. On-chain Solana settlement every 60s. Phase 3 roadmap: SLA bonds (on-chain latency/uptime guarantees). | G10 | https://derouter.network/whitepaper | Medium |
| E17 | Blockmachine (Bittensor subnet) — Merkle proof verification | 2025-2026 | doc | Gateway verifies responses via Merkle proof against trusted block state roots in real time. Validators post-hoc re-execute sampled requests. Permanent bans for confirmed mismatches. USD/CU pricing. | G10 | https://blockmachine.io/whitepaper | Medium |
| E18 | Verifiable SLA Framework (arXiv 2510.13370) | Oct 2025 | academic | Trustless SLA monitoring via TEE monitors + ZK proofs. Machine-readable SLA → verifiable predicates → Merkle trees with signed attestations. Properties: integrity, authenticity, validity. | G10 | https://arxiv.org/abs/2510.13370 | High |
| E19 | Acurast Network — proportional slashing | 2025-2026 | doc | Proportional slashing vs committed compute across 4 benchmark metrics (CPU Single/Multi, RAM, Storage). 50% shortfall → proportional token slash. Slasher reward 10%, 90% burned. Any participant can trigger. | G10 | https://docs.acurast.com/staked-compute/slashing/ | High |
| E20 | Aethir Network — tiered slashing mechanism | 2025-2026 | doc | Tiered monthly slashing: 10% fee fine (95-99% uptime) → 100% fee + full stake forfeiture (<70% uptime). Daily offline penalties >30-60 min. | G10 | https://docs.aethir.com/aethir-cloud/aethir-cloud-host/operational-requirements-for-cloud-hosts/slashing-mechanism | High |
| E21 | Enterprise AI Gateways 2026 (Bifrost, Gatewyse, Valgo, Infron) | 2026 | doc | Bifrost: 11µs routing overhead, 5K RPS, 4-tier budget hierarchy. Gatewyse: 10 routing strategies, 28 providers, PII detection. Valgo: 99.9% SLA, <50ms routing, 60% cost reduction. Infron: ZDR enforcement. | G10 | https://dev.to/pranay_batta/top-5-enterprise-ai-gateways-for-dynamic-routing-in-2026-514b | Medium |
| E22 | io.net + NovaNet — zkGPU-ID (ZK proof GPU verification) | 2025-2026 | doc | Zero-knowledge GPU identification via zkVM-backed verification (NovaNet zkEngine). Cryptographic proofs of GPU authenticity/performance. Tampering → invalid proof. Cheap on-chain verification. | G10 | https://www.binance.com/en/square/post/16144069507137 | Medium |

### G13 — Scalability: Elastic Provisioning & Demand Forecasting

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | URL | Confidence |
|----|----------|------|-----|----------------|------|-----|------------|
| E23 | Karpenter 2026 — K8s autoscaling standard | Feb 2026 | doc | Replaced CAS. Direct EC2 Fleet API; 30-60s cold start (vs CAS 3-5 min). Sub-second reaction. 65%+ Global 2000 migrated by Dec 2025. 20-40% compute cost reduction first quarter. | G13 | https://bytexel.org/kubernetes-autoscaling-2026-the-new-era-of-karpenter-and-ai-inference/ / https://tasrieit.com/blog/karpenter-vs-cluster-autoscaler-eks-comparison-2026 | High |
| E24 | NVIDIA Grove + DRA Driver (KubeCon 2026) | Apr 2026 | doc | Hierarchical gang scheduling, topology-aware placement, multi-level autoscaling for distributed inference (tens of thousands of GPUs). DRA Driver donated to CNCF: GPU MPS/MIG sharing, multi-node NVLink. | G13 | https://developer.nvidia.com/blog/streamline-complex-ai-inference-on-kubernetes-with-nvidia-grove/ / https://blogs.nvidia.com/blog/nvidia-at-kubecon-2026/ | High |
| E25 | Azure KAITO v0.8.0 + KEDA autoscaling | Feb 2026 | doc | KEDA-based event-driven autoscaling for AI inference on AKS. Kubernetes AI Toolchain Operator. | G13 | https://blog.aks.azure.com/2026/02/03/autoscale-inference-workloads-with-kaito | High |
| E26 | Kedify — predictive + multi-cluster autoscaling | 2025-2026 | doc | Extends KEDA with predictive autoscaling (ML 15 min ahead), vertical resource optimization, multi-cluster coordination. Consistent policies across providers/regions. | G13 | https://kedify.io/resources/blog/kedify-extends-keda-predictive-vertical-multicluster | Medium |
| E27 | OpenGPU Network — decentralized GPU marketplace | 2026 | doc | 272+ active GPU providers, 40+ countries, 41M verified tasks. 97.8% reliability. Task-based billing: 60-70% idle cost reduction. Elastic scaling for inference. | G13 | https://www.opengpu.network/ | Medium |
| E28 | Aethir — 440K+ GPU containers | 2026 | doc | 440,000+ GPU containers, 94 countries, 200+ locations. AI agents autonomously book enterprise-grade GPU inference in real-time via smart contract SLAs. Geographic distribution hedges export controls. | G13 | https://ecosystem.aethir.com/blog-posts/a-2026-vision-agentic-ai-booking-real-time-gpu-inference-on-aethir | Medium |
| E29 | io.net Agent Compute + MCP (March 2026) | Mar 2026 | doc | AI agents autonomously provision GPUs via Model Context Protocol (MCP). B200/H200/MI300X support. Leonardo.AI: 14K→19M users, >50% GPU cost reduction. Vistara Labs: 5,600 apps/2 months, 3x cost reduction, zero failures. zkTFLOPs verification. | G13, G1-refresh | https://saudishopper.com.sa/en/io-net-agent-compute-ai-gpu-platform/ / https://io.net/blog/compute-for-agents-by-agents-introducing-io-nets-agent-cloud | High |

### G11 — Observability, Monitoring & Cross-Provider Failover

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | URL | Confidence |
|----|----------|------|-----|----------------|------|-----|------------|
| E30 | OpenTelemetry Cross-Cloud Pipelines (AWS+GCP+Azure) | Feb 2026 | doc | Regional OTel Collectors per cloud → central OTLP backend (Jaeger, Grafana Tempo). W3C Trace Context (traceparent/tracestate) for cross-provider propagation. Async: inject into message attributes. Standardized instrumentation with cloud-specific metadata. | G11 | https://oneuptime.com/blog/post/2026-02-06-cross-cloud-opentelemetry-pipelines-aws-gcp-azure/view | High |
| E31 | OTel Failover Connector — priority-based routing | Feb 2026 | doc | Priority queue: primary (load-balanced) → secondary → tertiary → local storage fallback. Continuous health monitoring. Auto-switch on failure. Maintains observability continuity during outages. | G11 | https://oneuptime.com/blog/post/2026-02-06-failover-connector-opentelemetry-collector/view / https://oneuptime.com/blog/post/2026-02-06-multi-region-failover-opentelemetry-collection/view | High |
| E32 | Prometheus Pipeline Health Monitoring | Feb 2026 | doc | Prometheus-compatible metrics from OTel Collector. Alert thresholds: export failure >5%, queue saturation >85%, receiver backpressure ≠ 0, memory >80%. Auto failover triggers at >5% failure or >85% queue. health_check extension for liveness/readiness probes. | G11 | https://oneuptime.com/blog/post/2026-02-06-monitor-opentelemetry-pipeline-health-automated-failover/view | High |
| E33 | check_d — decentralized endpoint monitoring | Mar 2026 | doc | Real protocol-level queries (JSON-RPC, gRPC, REST). Per-region latency, historical uptime (7 days), sync status. Multi-geo: US, Central Europe, Canada. Evaluates real operational state, not just connectivity. | G11 | https://medium.com/cumulo-pro/check-d-a-decentralized-endpoint-monitoring-tool-d459ca229c2d | Medium |
| E34 | Vigil — ML-powered predictive failure detection (Solana) | 2025-2026 | doc | Failure Probability Score via ML on historical patterns. Client-aware anomaly detection (Agave/Firedancer). Proactive rerouting before failure. Memory pressure, scheduling stalls, RPC latency variance as inputs. | G11 | https://www.getvigil.io/ | Medium |
| E35 | Forta Network — SLA scoring for scan nodes | 2025-2026 | doc | SLA Score = weighted(resource_score, data_quality_score, uptime_score). Resource requirement failure → entire SLA = 0 (hard constraint). Prometheus + Grafana dashboards. Reward calculations tied to SLA. | G11 | https://docs.forta.network/en/latest/sla/ | Medium |

### G12 — Inter-Tier Networking: Bandwidth, Latency, Data Transfer Costs

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | URL | Confidence |
|----|----------|------|-----|----------------|------|-----|------------|
| E36 | Cloud Egress Pricing 2026 (multi-source) | 2026 | doc | AWS: $0.09/GB (→$0.07 at 50+TB). Azure: $0.087/GB (→$0.07). GCP: $0.12/GB Premium, $0.085/GB Standard (→$0.04 at 50+TB). Cross-AZ: $0.01/GB. Cross-region: $0.02-$0.09/GB. Cross-cloud: $0.08-$0.09/GB. Egress = 6-12% typical bill; 20-40% in multi-cloud. | G12 | https://www.spendark.com/blog/cloud-egress-costs-guide / https://gpuperhour.com/reference/data-egress / https://firstpasslab.com/blog/2026-03-08-cloud-networking-hidden-costs-egress-ipv4-nat-gateway/ | High |
| E37 | Multi-Cloud Data Transfer Optimization (40-70% reduction) | Jan-Feb 2026 | doc | Topology-aware K8s routing: 25% cross-zone reduction (60-80% with full implementation). Architecture locality: 35% reduction (case study). CDN offloading for model weights: 60-80% origin bandwidth reduction. Combined: 40-70% total reduction ($20K-$35K/month on $50K spend). VPC Endpoints/PrivateLink, response compression. | G12 | https://zeonedge.com/blog/multi-cloud-networking-costs-transit-gateway-vpc-peering-cross-cloud-egress / https://oneuptime.com/blog/post/2026-01-30-data-transfer-optimization/view | High |

### G1 Refresh — Akash BME + io.net 2026

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | URL | Confidence |
|----|----------|------|-----|----------------|------|-----|------------|
| E38 | Akash BME Mainnet 17 (v2.0.0, March 23, 2026) | Mar 2026 | doc | BME live: burn AKT → mint ACT (USD-pegged, non-transferable compute credit). Oracle-based pricing (PythNet AKT/USD). Deflationary when AKT rises; circuit breaker at collateral ratio <0.93. Also: CosmWasm smart contracts (governance-controlled); CometBFT Tachyon. Block 26,063,777. | G1-refresh | https://akash.network/blog/what-burn-mint-equilibrium-means-for-akash/ / https://akash.network/roadmap/aep-76/ / https://polkachu.com/gov_proposals/6985 | High |
| E29 | *(see G13 above — io.net Agent Compute also covers G1-refresh)* | | | | | | |

### G3 Refresh — Intel TDX & Phala GPU TEE 2026

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | URL | Confidence |
|----|----------|------|-----|----------------|------|-----|------------|
| E39 | Intel TDX GA on Azure (Feb 2026) | Feb 2026 | doc | 5th Gen Intel Xeon. VM series: DCesv6, DCedsv6 (general), ECesv6, ECedsv6 (memory-optimized). AMX for confidential AI. NVMe SSD: ~5x throughput, ~27µs latency improvement. OpenHCL paravisor (open-source). Up to 512GB memory for large model inference. | G3-refresh | https://techcommunity.microsoft.com/blog/azureconfidentialcomputingblog/announcing-general-availability-of-azure-intel-tdx-confidential-vms/4495693 | High |
| E40 | Intel TDX GCP + Intel-NVIDIA bounce buffer | Feb 2026 | doc | GCP: GKE nodes + Confidential Space with GPU support, multi-region/zone. Intel+Google joint security review of TDX Module 1.5 (Live Migration). Intel+NVIDIA bounce buffer: encrypted intermediary memory (AES-GCM) for CPU↔GPU TEE data transfer. Supports H100, H200, B200, B300 CC modes. Production-ready reference (Ubuntu Linux). | G3-refresh | https://cloud.google.com/blog/products/identity-security/from-clicks-to-clusters-confidential-computing-expands-with-intel-tdx / https://community.intel.com/t5/Blogs/Tech-Innovation/Artificial-Intelligence-AI/Confidential-AI-with-GPU-Acceleration-Bounce-Buffers-Offer-a/post/1740417 | High |
| E41 | Phala GPU TEE launched (H100/H200) | 2025-2026 | doc | First open, real-time attested confidential AI on NVIDIA H100/H200. GPU TEE Marketplace: choose config (H200/H100, region, VRAM, vCPU), real-time pricing. OpenAI-compatible Confidential AI API (cryptographically attested responses). <2% overhead on Llama-class models. Private ML SDK: confidential Docker → TEE with auto attestation. On-chain anchored TEE identities. | G3-refresh | https://www.phala.network/posts/gpu-tee-is-launched-on-phala-cloud-for-confidential-ai / https://www.phala.network/posts/Phala-GPU-TEE-Deep-Dive | High |

### G4 Refresh — IPFS/Arweave/Filecoin 2026

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | URL | Confidence |
|----|----------|------|-----|----------------|------|-----|------------|
| E42 | Filecoin Onchain Cloud mainnet (Jan 2026) | Jan 2026 | doc | Full-stack decentralized AWS alternative: compute + storage + payments + verifiable retrieval. PDP (Proof of Data Possession) for hot storage. $2.50/TiB/month/copy (2-copy default = $5.00/TiB, ~45% less than AWS S3). 49.41 TiB stored, 478 datasets. Synapse SDK. Strategy pivot: supply → demand scaling. | G4-refresh | https://filecoin.io/blog/posts/the-2026-filecoin-network-strategy/ / https://www.filecoin.io/blog/posts/filecoin-onchain-cloud-is-live-on-mainnet/ | High |
| E43 | Arweave AO compute + permanent storage | 2025-2026 | doc | AO Computer layer (Feb 2025): decentralized compute on permaweb. Agents invoke inference directly on stored models (no egress). Permanent storage: one-time fee, 200+ years. Token +60% late 2025. AI provenance tracking, decentralized publishing. | G4-refresh | https://adipek.com/articles/the-web3-storage-war-is-here-why-decentralized-file-systems-are-suddenly-everywhere-in-2026 / https://cryptoadventure.com/arweave-review-2026-permanent-data-storage-the-permaweb-and-ao-compute-expansion/ | Medium |
| E44 | IPFS Storacha Forge — warm storage with on-chain proofs | 2025-2026 | doc | Verifiable warm storage at $5.99/TB. Built on Filecoin Onchain Cloud. On-chain PDP proofs. IPFS stores >60% of NFT metadata/images. | G4-refresh | https://adipek.com/articles/the-web3-storage-war-is-here-why-decentralized-file-systems-are-suddenly-everywhere-in-2026 | Medium |

### Context

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | URL | Confidence |
|----|----------|------|-----|----------------|------|-----|------------|
| E45 | DePIN market growth 2024-2026 | 2025-2026 | doc | $5.2B → $19B market cap (270% growth, 2024→Sep 2025). Projections $3.5T by 2028. OpenAI $10B Cerebras deal (inference). GPU market $60B+ globally. | context | https://blockeden.xyz/blog/2026/02/07/decentralized-gpu-networks-2026/ | Medium |

---

## Source Conflict Notes

1. **x402 RouteNet scoring weights:** Cursor WebSearch returned two different weight schemas for x402 RouteNet — (uptime 40% + speed 30% + trust 30%) vs Perplexity's (success_rate 0.35 + schema 0.15 + quality 0.20 + trust 0.15 + domain 0.15). These may represent different versions or endpoints (procurement routing vs service routing). **Both recorded in E14; treat as complementary.**

2. **io.net GPU count discrepancy:** research.md cites "327K+ verified GPUs" but Messari Q4 2024 data shows 7,600 verified GPUs/day average. The 327K figure may represent total registered (not concurrently verified) devices. io.net Agent Compute (March 2026) references "10,000+ GPUs across 138 regions." **Treat 327K as peak registered; 7,600-10,000 as daily verified.**

3. **Karpenter provisioning time:** Cursor draft says "sub-second reaction" (vs CAS 60-120s); Perplexity says "30-60 second cold start" (vs CAS 3-5 min). These are different metrics: reaction time (detecting need) vs cold start (full node ready). **Both are correct for different stages.**

4. **Phala TEE overhead:** E6 (Nov 2025) states "5-15% for GPU TEE" while E41 (2026) states "<2% on Llama-class models." The improvement likely reflects maturation between initial benchmarks and production deployment on larger models. **Both valid for their respective timeframes.**

---

## Gap Coverage Summary

| Gap | Status | Evidence | Coverage Assessment |
|-----|--------|----------|---------------------|
| **G10** (High) | **covered** | E14-E22 | Strong: 3 scoring algorithm implementations (x402, dRPC, DeRouter), 3 slashing mechanisms (Acurast, Aethir, Verifiable SLA), ZK verification (io.net+NovaNet), enterprise gateways. Sufficient for specification-level design. |
| **G13** (High) | **covered** | E23-E29 | Strong: Karpenter benchmarks, NVIDIA Grove/DRA, KAITO/KEDA, predictive autoscaling (Kedify), decentralized scaling (OpenGPU, Aethir, io.net Agent Compute). Both centralized K8s and decentralized paradigms covered. |
| **G11** (Medium) | **covered** | E30-E35 | Strong: Full OTel stack (cross-cloud, failover connector, Prometheus), decentralized monitoring (check_d, Vigil, Forta). Covers both centralized observability and decentralized provider health. |
| **G12** (Medium) | **covered** | E36-E37 | Adequate: Comprehensive 2026 egress pricing with optimization strategies (40-70% reduction). Could benefit from Akash/io.net-specific egress data (not publicly detailed). |
| **G1** (refresh) | **refreshed** | E38, E29 | Confirmed: Akash BME live March 23, 2026; io.net Agent Compute + MCP March 2026. |
| **G3** (refresh) | **refreshed** | E39-E41 | Confirmed: Intel TDX GA Azure Feb 2026; GCP expanded; Intel+NVIDIA bounce buffer; Phala GPU TEE <2% overhead. |
| **G4** (refresh) | **refreshed** | E42-E44 | Confirmed: Filecoin Onchain Cloud Jan 2026; Arweave AO compute; Storacha Forge $5.99/TB. |

### Remaining Holes

- **G12 partial:** Akash/io.net-specific network egress costs and inter-provider transfer pricing not publicly documented. Centralized cloud pricing well-covered but decentralized tier-to-tier costs remain opaque.
- **G10 detail:** N4Y-specific Resource Router weight calibration needs design decisions (not an evidence gap but a specification gap).
