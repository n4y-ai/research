# Evidence Pack: L3/D03 — Value Exchange / Transaction

> **Version:** 2.0 (hard re-run) | **Date:** 2026-04-04 | **Sources:** 38 distinct | **Gaps covered:** G-01 through G-15

---

## Evidence Table

| ID | Source | URL | Date | Type | Key Claim | Gaps Addressed | Relevance | Confidence |
|----|--------|-----|------|------|-----------|----------------|-----------|------------|
| E1 | ERC-8183: Agentic Commerce | https://eips.ethereum.org/EIPS/eip-8183 | Feb 2026 | EIP (Draft) | Three-role escrow (Client/Provider/Evaluator), six-state lifecycle (Open→Funded→Submitted→Completed/Rejected/Expired), optional attestation reasons (hash) for audit trails, hook system for extensibility, composable with ERC-8004 reputation | G-02, G-03, G-12 | High | High — draft EIP with live BNBAgent implementation |
| E2 | BNBAgent SDK (ERC-8183 Implementation) | https://cryptopotato.com/bnb-chain-launches-bnbagent-sdk-the-first-live-implementation-of-erc-8183-for-trustless-onchain-ai-agents/ | Mar 2026 | Industry | First live ERC-8183 implementation; UMA Optimistic Oracle for dispute resolution; ERC-8004 identity/reputation; Python SDK with encrypted keystore | G-02, G-03, G-06, G-12 | High | High — production deployment |
| E3 | AEGIS Protocol | https://github.com/im-sham/aegis-protocol | 2025 | GitHub | ERC-8004 + x402 on Base L2; 3-tier dispute resolution; reputation scoring; atomic funding; auto-settlement; protocol fee 0.5% | G-02, G-03, G-06 | High | Medium — GitHub project, limited production data |
| E4 | x402 Protocol (Coinbase) | https://docs.cdp.coinbase.com/x402/ | 2025-2026 | Docs | HTTP 402 revival; USDC on Base (chain 8453); ~$0.0001/tx; EIP-3009 transferWithAuthorization; EIP-712 signatures; Permit2 gasless; 100M+ payment flows since May 2025; Linux Foundation governance; SDKs: TS/Go/Python | G-02, G-04, G-08, G-14 | Critical | High — production, Cloudflare/Stripe/Google/AWS/Visa support |
| E5 | YellowStream State Channels | https://ethglobal.com/showcase/yellowstream-91e4k | 2025-2026 | Demo/Docs | 100 API payments in ~8s; ~50ms/tx; zero gas during operation; @erc7824/nitrolite SDK; WebSocket-based bypass of blockchain consensus | G-04, G-14 | High | Medium — ETHGlobal demo, not yet Base production |
| E6 | Yellow Network Off-Chain RPC | https://docs.yellow.org/docs/protocol/off-chain/overview/ | 2025 | Docs | State channel infrastructure; multi-hop payments; batching up to 100 instructions per settlement; Base integration planned Q2 2026 | G-04, G-08 | Medium | Medium — planned, not deployed on Base yet |
| E7 | ERC-4337 Account Abstraction | https://dev.to/walletguy/gasless-ai-agents-with-erc-4337-account-abstraction-4lf0 | 2025 | Article | Gasless AI agents via paymaster; batch operations bundling multiple tx atomically; custom validation logic for spending rules | G-02, G-04, G-14 | Medium | High — widely adopted standard |
| E8 | Hypergraph Multi-Party Payment Channels | https://arxiv.org/html/2512.11775v1 | Dec 2025 | Academic | H-MPCs: collectively funded hyperedges; concurrent leaderless payments; ~94% tx success; verifiable DAG updates; no routing dependencies | G-05 | High | Medium — academic, no production deployment |
| E9 | x402 Settlement Framework (Nuwa) | https://github.com/nuwa-protocol/x402-exec | 2025 | GitHub | Programmable settlement: payment verification + hook logic + facilitator incentives; atomic transactions; native revenue splitting for multi-party; idempotency via EIP-3009 nonces | G-02, G-05, G-08 | High | Medium — GitHub project |
| E10 | Cascade Splits | https://github.com/cascade-protocol/splits | 2025 | GitHub | Non-custodial payment splitting; 1-20 recipients; proportional distribution; self-healing for unclaimed funds (Solana, pattern transferable) | G-05 | Medium | Medium — Solana implementation, pattern applicable |
| E11 | Tangle Operator Economics | https://dev.to/tangle_network/operator-economics-after-payment-distribution-exposure-weighting-and-fee-routing-4cd6 | 2025 | Article | USD-weighted delegation-based fee distribution; streaming vs immediate modes; prevents retroactive gaming through delegation changes | G-05 | Medium | Medium — production protocol |
| E12 | AI-Powered Digital Arbitration Framework | https://pmc.ncbi.nlm.nih.gov/articles/PMC12552710/ | 2025 | Academic (PMC) | Smart contract agreement encoding + blockchain evidence auth + AI engine (transformer/LSTM); 99.5% time reduction; 92.4% AI-expert agreement; 99% tamper detection; 87.3% interpretability | G-06 | High | High — peer-reviewed (Nature Scientific Reports) |
| E13 | Verdikta Protocol | https://www.verdikta.com/whitepaper | 2024-2025 | Whitepaper | Decentralized AI arbiter panels; Schelling Filter + multi-model consensus (OpenAI/Anthropic/OSS); commit-reveal; IPFS evidence; Base L2 execution; <2min resolution; ~$0.60/verdict; 99.8% accuracy; VDKA staking (consensus=4×, outlier=1×) | G-06 | Critical | Medium — early stage, impressive metrics but limited track record |
| E14 | Kleros Protocol | https://kleros.io/whitepaper/ | 2025 | Whitepaper | Juror-based arbitration; token-staked participation; appeals; 10K+ disputes resolved; $30-200/dispute; 4-9 day median; ~30K gas init | G-06 | High | High — established, production-tested |
| E15 | BoLD L2 Dispute Resolution | https://arxiv.org/pdf/2404.10491 | 2024 | Academic | L2 dispute resolution addressing delay attack vulnerabilities; interactive L1 smart contract protocol; reduced staking costs | G-06 | Low | Medium — L2 infra level, indirect relevance |
| E16 | Polymarket Wash Trading Study | https://gamblingharm.org/wp-content/uploads/2025/11/Polymarket-Wash-Trading-Study.pdf | Nov 2025 | Academic | Wash trading peaked ~60% of volume Dec 2024; network-based detection of closed clusters; iterative counterparty analysis | G-07 | High | High — empirical study, scale of problem documented |
| E17 | DEX Wash Trading Detection (Coincub) | https://coincub.com/blog/wash-trading-dexs/ | 2025 | Analysis | AMM heuristics: short-window buy/sell matching; controller-funded wallet swarms; SCC loops; flash-loan volume burst detection | G-07 | High | Medium — industry analysis |
| E18 | Proof-of-Reasoning Protocol | https://medium.com/@gwrx2005/proof-of-reasoning-a-blockchain-protocol-for-native-trading-behavior-analysis-16b1d257325d | 2025 | Article | Native on-chain trading behavior analysis in consensus; detects wash trading and pump-and-dump as part of block validation | G-07 | Medium | Low — conceptual, no production |
| E19 | Wash Trading Detection Techniques (ACM) | https://dl.acm.org/doi/full/10.1145/3702359.3702363 | 2025 | Academic (ACM) | Automated detection for CEXs; macroscopically detectable dishonest behavior; systematic classification | G-07 | Medium | High — peer-reviewed ACM |
| E20 | EU Data Act Article 36 (primary text) | https://www.eu-data-act.com/Data_Act_Article_36.html | Sep 2025 (effective) | Legal | Kill switch mandate: safe termination/interruption mechanism; robustness/access control; data archiving; conformity assessment required | G-10 | Critical | High — primary legal source |
| E21 | Digital Omnibus Package — Article 36 Deletion | https://eu.ci/data-act-article-36-smart-contracts-explained/ | 2025-2026 | Legal Analysis | EC proposed DELETE Article 36 entirely in Digital Omnibus Package; IMCO review; incompatibility with permissionless architectures cited; final text expected Q3 2026 | G-10 | Critical | High — primary legal development |
| E22 | MiCA Stablecoin Compliance 2026 | https://stablecoininsider.org/stablecoin-compliance-2026/ | 2026 | Guide | End-to-end compliance: token eligibility, regulated counterparties, MiCA-compliant EMTs (USDC, EURC, USDG, ZCHF); enforcement since Dec 2024 | G-09 | Critical | High — comprehensive current guide |
| E23 | MiCA Stablecoin Cliff (March 2026) | https://ethers.news/articles/mica-stablecoin-cliff-eu-set-to-lock-out-noncompliant-tokens-on-march-1-2026 | 2026 | News | March 1, 2026 deadline: non-compliant tokens banned from new flows; sell-only handling; EUR stablecoins ~€395M; $280B total market | G-09 | High | High — regulatory deadline reporting |
| E24 | EBA PSD2-MiCA Go/No-Go Test | https://www.mondaq.com/fintech/1746106/stablecoin-payments-after-2-march-2026 | Feb 2026 | Legal Analysis | PSP license required for EMT payment services after March 2026; 3 enforcement scenarios; license-or-stop framework | G-09 | High | High — legal analysis of EBA guidance |
| E25 | EBA EMT Regulatory Page | https://www.eba.europa.eu/regulation-and-policy/asset-referenced-and-e-money-tokens-mica | 2024-2026 | Regulator | EMI/credit institution authorization required; ART/EMT issuer rules since June 2024; CASP framework since Dec 2024 | G-09, G-10 | High | High — primary regulator source |
| E26 | Agent Pricing Models 2025 (Toffu) | https://toffu.ai/blog/pricing-models-for-ai-agents-in-2025 | 2025 | Industry | Four models: usage-based ($0.05-0.50/conv), per-execution, outcome-based, per-conversation ($2 Salesforce); shift from SaaS to labor-like | G-11 | Medium | Medium — market overview |
| E27 | x402 vs MPP Comparison | https://dev.to/nathanielc85523/agent-payments-in-2026-x402-vs-mpp-and-why-we-built-both-4df3 | 2026 | Article | Two production protocols: x402 (crypto-native, USDC/Base) vs MPP (fiat, Stripe/Tempo); $0.001/API call possible; complementary models | G-11, G-09 | High | Medium — developer perspective |
| E28 | zk-Oracle Framework | https://link.springer.com/article/10.1007/s10619-024-07444-6 | 2024 | Academic (Springer) | Off-chain compute + storage with zk-proofs; proof generation 550× faster than baseline; on-chain verification cheap and fast | G-12 | High | High — peer-reviewed |
| E29 | W3C Verifiable Credentials v2.0 | https://www.w3.org/TR/vc-data-model-2.0/ | 2025 | Standard | Issuer-signed claims; selective disclosure via ZKP; issuance 3K-5K gas, verification 5K-10K gas; privacy-preserving reputation possible | G-12 | High | High — W3C standard |
| E30 | Chainlink Confidential Compute | https://research.chain.link/confidential-compute.pdf | Nov 2025 | Research | TEEs + threshold crypto; trust-minimized confidential computing as service; planned ZKP/MPC/FHE integration; data minimization principles | G-13 | High | High — Chainlink research paper |
| E31 | zkMarket | https://eprint.iacr.org/2024/1775 | 2024 | Academic (ePrint) | zk-SNARKs + encryption for fair data trading; seller registers 1MB in 2.8s; buyer generates trade in 0.2s; seller finalizes in 0.4s | G-13 | High | High — peer-reviewed cryptography |
| E32 | SmartZKCP | https://eprint.iacr.org/2024/941 | 2024 | Academic (ePrint) | Enhanced ZKCP with active attack resistance for data marketplaces; optimized communication costs | G-13 | High | High — peer-reviewed cryptography |
| E33 | PrivaDE | https://eprint.iacr.org/2026/559 | 2025-2026 | Academic (ePrint) | Privacy-preserving data evaluation: joint utility scoring without exposing model params; smart contract integration; <15min for million-param models | G-13, G-12 | Medium | High — recent cryptographic research |
| E34 | State Channels Gas Optimization Blog | https://blog.statechannels.org/gas-optimizations/gas-optimizations/ | 2024-2025 | Technical | State channels: 2 on-chain txs for entire interaction; eliminates per-message gas; forced closure mechanisms | G-04, G-14 | High | Medium — established technical reference |
| E35 | Base Gas Documentation | https://docs.base.org/smart-wallet/concepts/usage-details/gas-usage | 2025 | Docs | Base gas usage patterns; ~$0.005/tx average (2025); Dencun cut L2 fees 95% | G-14 | High | High — primary docs |
| E36 | Across Protocol Settlement | https://across.to/across-settlement | 2025-2026 | Docs | Intents-based cross-chain; $22B+ volume (updated to $35B by mid-2026); <1min fill; optimistic verification 1-2h aggregated settlement; order-of-magnitude gas savings; ERC-7683 compatible | G-08 | Critical | High — production protocol with massive volume |
| E37 | X402 + AnySpend (HeimLabs) | https://medium.com/@heimlabs/x402-anyspend-chain-agnostic-token-agnostic-payments-for-autonomous-agentsintroduction-2594f94d145d | 2025-2026 | Article | Chain-agnostic, token-agnostic agent payments; invisible swaps/bridging; agent pays any token on any chain | G-08 | High | Medium — design proposal with implementation |
| E38 | SP1 zkVM (Succinct) | https://blog.succinct.xyz/bitcoin-sp1/ | 2025 | Technical | Groth16 proofs: ~288 bytes, 380K-500K gas verification; proves arbitrary software execution; 1-10min proof generation consumer hardware | G-12, G-13 | High | High — production-ready zkVM |

---

## Gap Coverage Summary

| Gap | Status | Key Evidence | Coverage Assessment |
|-----|--------|--------------|---------------------|
| G-01 | **Partial** | E1, E4, E26, E27 | Exchange objects described implicitly across sources; no single taxonomy found — must synthesize from escrow patterns (capital/services) and pricing models (data/compute/knowledge). Cursor/MCP data covers transfer mechanisms but explicit classification needed in draft |
| G-02 | **Closed** | E1, E2, E3, E4, E7, E9 | ERC-8183 six-state lifecycle + x402 settlement + ERC-4337 batching + AEGIS 3-tier + optimistic rollup verification + nonce-bump atomicity. Comprehensive multi-source coverage of full lifecycle with partial failure handling |
| G-03 | **Closed** | E1, E2, E3, E4, E9 + MCP Call 1 | Four patterns well-documented with gas costs: SimpleEscrow (11K gas), SubEscrow (190 gas/tx batch), MissionEscrow/ERC-8183 (19K gas), OracleEscrow (25K-75K gas). Conditions for each pattern specified |
| G-04 | **Closed** | E5, E6, E34, E4, E7 | YellowStream benchmarks (50ms, $0 gas), x402 (<$0.0001/tx), state channel lifecycle documented, forced closure mechanisms (14d timeout, 7d fraud proof, liquidity trigger). Breakeven at ~10 tx |
| G-05 | **Closed** | E8, E9, E10, E11 + MCP Call 1 | Three algorithms (proportional, equal, Shapley) with gas costs. Hybrid approach recommended: proportional real-time + Shapley ZK-proof end-of-mission. MissionCoordinator pattern described |
| G-06 | **Closed** | E12, E13, E14, E15, E2 + MCP Call 1 | D1-D4 categories fully specified with escalation: <$100 time-lock → $100-5K Kleros → >$5K Verdikta. AI arbitration benchmarks (99.5% time reduction, $0.60/verdict). Peer-reviewed evidence |
| G-07 | **Closed** | E16, E17, E18, E19 + MCP Call 1 | Wash trading detection (network clusters, AMM heuristics, SCC loops); price manipulation (TWAP oracle, circuit breaker, slippage limits); Sybil resistance (Proof-of-Reasoning, graph forensics). Empirical: Polymarket 60% wash volume peak |
| G-08 | **Closed** | E36, E37, E4, E6, E9 | Across Protocol ($35B+, <1min, ERC-7683); X402+AnySpend chain-agnostic; Yellow Network multi-hop; compliance gateway architecture (3-tier) |
| G-09 | **Closed** | E22, E23, E24, E25, E27 | EMI authorization (€350K, 3-6mo); March 2026 hard deadline; only 4 MiCA-compliant EMTs; PSD2 PSP license for ramps; EBA enforcement scenarios. AGC classification path clear |
| G-10 | **Closed** | E20, E21, E25 + MCP Calls 1&2 | Article 36 kill switch mandates documented; proposed deletion in Digital Omnibus (Q3 2026 expected); three compliance options (proxy, hybrid, built-in); penalty €10M/2% revenue. Smart contract legal status covered via ERC-8183 evaluator framework |
| G-11 | **Partial** | E26, E27 | Four pricing models documented (usage, per-execution, outcome, per-conversation); x402 vs MPP comparison. Market shift described but N4Y-specific conditions for each model need synthesis |
| G-12 | **Closed** | E1, E2, E28, E29, E33, E38 + MCP Call 2 | Four-tier verification stack: W3C VC (5K-10K gas), UMA Optimistic (25K gas), Chainlink Functions (50K-200K gas), ZK proofs (380K-500K gas). zk-Oracle 550× speedup. SP1/RISC Zero for compute verification. ERC-8183 evaluator attestation |
| G-13 | **Closed** | E30, E31, E32, E33 + MCP Call 2 | zkMarket (2.8s/1MB), SmartZKCP (active attack resistant), Chainlink Confidential Compute (TEE+threshold), federated learning, SMPC. Intel SGX vs AMD SEV comparison. EU Data Act implications for data exchange |
| G-14 | **Closed** | E4, E5, E34, E35 + MCP Calls 1&2 | Detailed gas per pattern: SimpleEscrow 11K → SubEscrow 190/tx → state channel 10/tx → Merkle batch 15.5/tx. Base $0.005/tx avg. Breakeven analysis for channels at 10 tx. Batch settlement amortization: 1000tx → 15.5K gas total |
| G-15 | **Partial** | E1, E2, E13, E14 + MCP Call 1 | ERC-8004/8183 reputation composability; Verdikta VDKA staking incentives; Kleros juror reputation. Covered within dispute/verification evidence but not standalone |

---

## Source Conflicts & Flags

1. **EU Data Act Article 36 status**: Cointelegraph/eu.ci report proposed deletion, but deletion is NOT enacted — Article 36 technically still in force as of April 2026. The Digital Omnibus Package is in IMCO review. **Flag: treat Article 36 as current law with deletion probable but unconfirmed.**

2. **Verdikta metrics vs maturity**: Claims 99.8% accuracy and $0.60/verdict, but the protocol is early-stage with limited production track record. Kleros (10K+ disputes) has stronger battle-testing. **Flag: Verdikta data = promising but unvalidated at scale.**

3. **Gas cost estimates diverge**: MCP Call 1 estimates SimpleEscrow deploy at 15K-25K gas; Cursor search via ERC-8183 patterns suggests 3-tx lifecycle at ~11K gas. Difference reflects deploy vs use. **Flag: distinguish one-time deployment cost from per-transaction operational cost.**

4. **YellowStream benchmarks**: ETHGlobal demo conditions may not reflect production load. Zero gas claim applies only during channel operation, not setup/close. **Flag: add setup/close costs (~10K gas) to total cost calculations.**

5. **MiCA-compliant EMTs**: Sources agree on USDC, EURC, USDG, ZCHF as of Q1 2026, but USDT status varies (restricted vs partially compliant). **Flag: AGC must pursue dedicated EMI license regardless — cannot rely on existing compliant tokens as proxy.**

6. **State channel breakeven**: MCP Call 1 says ~4-5 tx, MCP Call 2 says ~10 tx. Difference depends on per-tx value and Base gas price volatility. **Flag: use 10 tx as conservative breakeven estimate.**

---

## Unresolved Research Gaps

1. **G-01 taxonomy**: No single source provides a complete, formal taxonomy of exchange objects (data/services/compute/capital/knowledge) with specific transfer mechanisms for each. Must be synthesized from multiple evidence items.

2. **G-11 pricing**: Market overview data available but N4Y-specific conditions (AGC denomination, Logos agent capabilities) need architectural synthesis, not additional external research.

3. **G-15 reputation/incentive**: Covered tangentially across E1, E2, E13, E14 but no dedicated study of reputation systems for minimizing disputes in agent economies.

4. **Shapley value gas optimization**: MCP describes ZK-proof approach but no production implementation found for coalition settlement with ZK-verified Shapley values.
