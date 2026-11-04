# Evidence Pack: L2/D01 — Идентификационно-онтологическое

> **Измерение:** L2/D01 | **Версия:** 2.0 (hard re-run) | **Дата:** 04.04.2026

---

## Previous Evidence (E1–E6, migrated from v1)

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | Relevance | Confidence |
|----|----------|------|-----|----------------|------|-----------|------------|
| E1 | Base L2 documentation + Stage 1 (Feb 2026) | Feb 2026 | doc | Optimistic Rollup; Stage 1 decentralization (Security Council + fault proofs); 100-300 TPS burst 1500+; ~$0.0002–$0.01/tx | G1, G7 | H | High |
| E2 | MiCA (Reg. 2023/1114) | Dec 2024 | norm | AGC=EMT→CASP авторизация; Qi=OCA→white paper only. CASP includes management of crypto-assets platforms | G3 | H | High |
| E3 | EU AI Act (2024/1689) | Aug 2024 | norm | N4Y.AI as Provider Art.16-23; Microsandbox частично соответствует AI Act Art.57 regulatory sandbox | G3 | H | High |
| E4 | Fetch.ai / ASI Alliance | 2025 | doc | Decentralized AI agent network on Cosmos; no native stablecoin; no MiCA compliance | comparative | H | High |
| E5 | SingularityNET | 2025 | doc | Decentralized AI marketplace; Trust Registry (первый в индустрии); без managed permeability | comparative | H | High |
| E6 | research_L2_infra.md | Mar 2026 | doc | OP Stack canonical bridge 7д; Across Protocol <5мин; rate limits $5M/24h; LayerZero V2 + IBC v2 Q2-Q3 2026 | G8 | H | High |

---

## New Evidence (E7–E20)

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | Relevance | Confidence |
|----|----------|------|-----|----------------|------|-----------|------------|
| E7 | Base L2 Stage 1 — fault proofs + Security Council confirmation | Feb 2026 | doc | Stage 1 confirmed: permissionless fault proofs (Oct 2024, 3.5-day challenge window), Security Council (Apr 2025, 12 entities, 75% quorum 9/12, quorum-blocking group outside Base). All Vitalik Buterin Stage 1 requirements met. No Stage 2 timeline published. | G1 (refresh) | H | High |
| E8 | AMLR 2024/1624 Art.79 — anonymous accounts prohibition (full text) | Jul 2024 | norm | CASPs prohibited from keeping anonymous crypto-asset accounts or accounts allowing anonymisation/obfuscation including via anonymity-enhancing coins. Existing accounts → CDD before any use. Full AMLR application: **10 July 2027**. No explicit mention of AI agents in text. | G2 | H | High |
| E9 | AMLR 2024/1624 — CASPs as obliged entities | Jul 2024 | norm | CASPs included as obliged entities within 'financial institutions' definition. CDD: Art.20 (measures), Art.22 (identification/verification), Art.25 (purpose/nature), Art.26 (ongoing monitoring). Art.58(1) prohibits anonymous wallets specifically. | G2 | H | High |
| E10 | MCP legal analysis — AMLR Art.79 agent wallet interpretation | Apr 2026 | analysis | Three compliance interpretations: (A) Agent as proxy for controller — account under human beneficial owner, recommended; (B) Agent as legal entity — rare; (C) No direct prohibition — weakest. Regulation does not explicitly address autonomous agent wallets. Conservative approach: treat agent accounts as derivatives of human beneficial owner accounts. | G2 | H | Medium |
| E11 | EU AI Act GPAI obligations — enforcement August 2, 2025 | Aug 2025 | norm | GPAI obligations effective Aug 2, 2025 for new models. Compute threshold: 10²³ FLOP + generative capabilities. Enforcement powers (fines): Aug 2, 2026. Pre-existing models: Aug 2, 2027. Voluntary Code of Practice (effective Aug 2, 2025): adherents get lighter scrutiny. Non-binding guidelines issued Jul 2025. | G5 | H | High |
| E12 | GPAI provider vs deployer classification — N4Y.AI analysis | Aug 2025 | analysis | N4Y.AI = **deployer** (not provider) unless fine-tuning/releasing own foundation model. Deployer obligations: transparency docs, risk assessment, incident reporting (Art.73, 30 days). If agents generate financial/health output → high-risk mitigation required. Fines: up to €30M or 6% global turnover. | G5 | H | Medium |
| E13 | AEPD guidance on agentic AI and GDPR (Spain) | Feb 2026 | guidance | 71-page non-binding guidance (Feb 18, 2026): most detailed EU guidance on AI agents + data protection. Controller/processor roles may shift dynamically in multi-agent architectures. Must identify C/P at each pipeline stage before deployment. Art.6 lawful basis per processing step. Art.30 records. **"Rule of 2"**: prevent simultaneous (1) uncontrolled input + (2) sensitive data access + (3) autonomous actions. | G6 | H | High |
| E14 | GDPR cross-system transfers — agentic AI risks | 2025–2026 | analysis | Agents autonomously select tools, call APIs, route tasks across services at runtime. Controllers may not know data destinations. GDPR Chapter V compliance difficult with non-predetermined transfer paths. Cross-sandbox transitions (HC→FS) create re-purposing risk: legal basis change (Art.9(2)(h) → Art.6(1)(f)) may violate original basis. | G6 | H | Medium |
| E15 | GDPR HC Microsandbox controller analysis | Apr 2026 | analysis | In HC Microsandbox: N4Y.AI = potential joint controller (Art.26) if it sets agent parameters + determines data processing; Healthcare institution = primary controller; AI agent = not controller (lacks legal personhood). DPIA mandatory (Art.35) for agent-driven health data processing. Art.9 special categories: express consent difficult for autonomous agents → research/public health basis preferred. | G6 | H | Medium |
| E16 | EU AI Act Art.57 — regulatory sandboxes | Aug 2024 | norm | Member States must establish ≥1 national sandbox by **Aug 2, 2026**. Sandboxes: controlled testing environment, supervised development, time-limited, real-world testing under supervision. Exit reports = documented compliance history, can accelerate conformity assessment, but cannot replace CE marking or third-party assessment for high-risk AI. | G5 | M | High |
| E17 | ASI Alliance — current status and roadmap | 2024–2026 | doc | Merger: Fetch.ai + SingularityNET + Ocean + CUDOS (Oct 2024). FET token (→ASI rebrand pending). Phase I complete (Jul 2024): AGIX+OCEAN→FET. Phase II (FET→ASI 1:1) TBD. ASI:Chain DevNet Nov 2025; TestNet 2026; mainnet late 2026–early 2027. MeTTa framework. Google Cloud/Gemini partnership Jan 2026. No native stablecoin, no MiCA compliance, no managed permeability. Market cap ~$2B (Mar 2026). | G3 | H | High |
| E18 | Bittensor + Virtuals Protocol + ElizaOS — agent economy landscape | 2024–2026 | comparative | **Bittensor:** Substrate-based; subnet model + TAO emissions; entry ~$300K; no sandbox/regulatory compliance; technical community. **Virtuals Protocol:** Base L2; 18K+ agents; $470M agentic GDP; $8B DEX volume; low entry (100 VIRTUAL); 40% revenue cut; "biggest unverified agent identity concentration in Web3"; developing ERC-8183/8004 identity standards. **ElizaOS (ai16z):** Solana; TypeScript framework; 10K+ agents; autonomous trading; Trust Scoring; migrated to utility architecture Feb 2026; fully permissionless, no KYC/AML. All three = **permissionless**, no managed permeability, no regulatory framework. | G8 | H | High |
| E19 | MI9 + OpenShell + DeepMind — circuit breaker / sandbox enforcement state-of-art | 2025–2026 | technical | **MI9 (Aug 2025):** 6-component runtime governance — Agency-Risk Index, telemetry, continuous authorization, FSM conformance, goal-drift detection (99.81%), graduated containment. Model-agnostic, open-source. **OpenShell (Mar 2026):** NVIDIA; kernel-level agent sandbox; Landlock filesystem, seccomp, per-method/per-endpoint/per-binary policy engine; audit trails; Apache 2.0. **DeepMind Distributional AGI Safety (Dec 2025):** "patchwork AGI" via multi-agent coordination; four-layer defense (market design → agent safety → continuous oversight → governance); "virtual agentic sandbox economies" with semi-permeable boundaries. | G7 | H | High |
| E20 | Progressive decentralization — industry patterns 2025-2026 | 2025–2026 | comparative | Common pattern: managed → stake-weighted → permissionless. **XMTP (3-phase):** Phase 1 trusted operators + Security Council → Phase 2 stake-weighted + slashing → Phase 3 fully permissionless. **Starknet:** staking + operation + governance tracks. **The Graph:** Horizon (Dec 2025) → modular multi-service data layer. **ONINO:** validator sunset Feb 2026. Security Council oversight universal in Phase 1; economic security (staking+slashing) precedes permissionlessness. | G4 | H | High |

---

## Source URL Index

| ID | URL(s) |
|----|--------|
| E7 | https://base.mirror.xyz/tWDMlGp48fF0MeADcLQruUBq1Qxkou4O5x3ax8Rm3jA · https://stage.theblock.co/post/352320/base-stage-1 · https://docs.base.org/chain/security-council · https://blog.base.org/fault-proofs-are-now-live-on-base-mainnet |
| E8 | https://amlr.eu/article-79-anonymous-accounts-and-bearer-shares-and-bearer-share-warrants/ · https://anti-money-laundering.eu/regulation-eu-2024-1624 |
| E9 | https://compliance360.mt/expansion-of-obliged-entities-under-amlr-regulation-eu-2024-1624/ · https://anti-money-laundering.eu/crypto-asset-service-providers/ |
| E10 | MCP deep_research analysis (perplexity/sonar-deep-research); framework refs: https://eur-lex.europa.eu/eli/reg/2024/1624/oj |
| E11 | https://digital-strategy.ec.europa.eu/en/policies/guidelines-gpai-providers · https://www.skadden.com/insights/publications/2025/08/eus-general-purpose-ai-obligations · https://regulations.ai/regulations/RAI-EU-NA-ECGSOXX-2025 |
| E12 | https://www.orrick.com/en/Insights/2025/08/EU-Commission-Guidelines-on-the-scope-of-the-obligation-for-GPAI-models-under-the-AI-Act · https://regulations.ai/regulations/RAI-EU-NA-GACPVXX-2025 |
| E13 | https://www.prokopievlaw.com/post/spain-s-aepd-issues-guidance-on-agentic-ai-systems-and-gdpr-compliance-february-2026 · https://www.lexology.com/library/detail.aspx?g=37b80f0b-19fc-4d4d-9e82-6b7efeff3b94 · https://www.mondaq.com/unitedstates/data-protection/1759416/aepd-publishes-guidance-on-agentic-artificial-intelligence-from-a-data-protection-perspective |
| E14 | https://www.privacyengine.io/blog/gdpr-data-transfers-agentic-ai/ · https://www.dastra.eu/en/article/ai-agents-in-data-processing-what-challenges-for-compliance-and-governance/59952 |
| E15 | MCP deep_research analysis; framework refs: https://eur-lex.europa.eu/eli/reg/2016/679/oj · https://edpb.ec.europa.eu/sites/default/files/consultation/edpb_guidelines_05_2020_controllers_and_processors.pdf |
| E16 | https://ai-act-service-desk.ec.europa.eu/en/ai-act/article-57 · https://artificialintelligenceact.com/article-57-ai-regulatory-sandboxes/ |
| E17 | https://docs.superintelligence.io/artificial-superintelligence-alliance/archive/artificial-superintelligence-alliance-update-on-asi-token-merger · https://crypto.news/fet-price-extends-gains-as-ai-token-rally-and-asi-roadmap-lift-demand-how-high-can-it-go/ · https://coinstats.app/ai/a/fundamental-analysis-fetch-ai |
| E18 | https://www.panewslab.com/en/articles/019d2e7c-6c0d-777f-9ee7-8e7cfa93669c · https://rnwy.com/blog/virtuals-protocol-trust-scoring · https://coinbureau.com/review/virtuals-protocol-review · https://levex.com/en/blog/eliza-framework-blockchain-ai-agents · https://markets.financialcontent.com/bpas/article/tokenring-2026-2-6-the-rise-of-agentic-capital-how-ai16z-and-autonomous-trading-swarms-are-remaking-solana |
| E19 | https://ui.adsabs.harvard.edu/abs/2025arXiv250803858W/abstract · https://github.com/charleslwang/MI9-Eval · https://www.marktechpost.com/2026/03/18/nvidia-ai-open-sources-openshell-a-secure-runtime-environment-for-autonomous-ai-agents/ · https://blogs.nvidia.com/blog/secure-autonomous-ai-agents-openshell · https://github.com/NVIDIA/OpenShell · https://arxiv.org/html/2512.16856v1 |
| E20 | https://www.legalnodes.com/article/progressive-decentralization-playbook · https://www.starknet.io/blog/starknet-decentralization-a-roadmap-in-broad-strokes/ · https://xmtp.org/decentralization · https://blockster.com/the-graph-technical-roadmap-2026 |

---

## Gap Coverage Summary

| Gap | Status | Evidence | Coverage |
|-----|--------|----------|----------|
| G1 (refresh) | ✅ Confirmed | E1, E7 | Stage 1 confirmed Feb 2026; fault proofs Oct 2024 + Security Council Apr 2025. No Stage 2 timeline. |
| G2 (High) | ✅ Covered | E8, E9, E10 | Art.79 full text + CASP obligations + agent wallet interpretation. Full application 10 Jul 2027. Agent accounts → human beneficial owner. No explicit AMLR guidance on AI agents yet. |
| G3 (Medium) | ✅ Covered | E4, E5, E17 | ASI Alliance current state; ASI:Chain DevNet/TestNet timeline; no stablecoin/MiCA/managed permeability. |
| G4 (Medium) | ✅ Covered | E20 | Industry patterns: XMTP, Starknet, The Graph, ONINO. Common 3-phase pattern maps to N4Y.AI Phase 1→4. |
| G5 (Medium) | ✅ Covered | E11, E12, E16 | GPAI effective Aug 2, 2025; N4Y.AI = deployer; enforcement Aug 2, 2026; Code of Practice; Art.57 sandboxes by Aug 2, 2026. |
| G6 (High) | ✅ Covered | E13, E14, E15 | AEPD Feb 2026 guidance (71pp); Rule of 2; controller/processor in multi-agent HC; cross-sandbox data transfer risks; DPIA required. |
| G7 (Medium) | ✅ Covered | E1, E19 | MI9 (runtime governance, 6 components), OpenShell (kernel-level sandbox), DeepMind 4-layer defense framework. State-of-art for circuit breakers. |
| G8 (Medium) | ✅ Covered | E18 | Bittensor/Virtuals/ElizaOS all permissionless; no sandbox/regulatory compliance. Virtuals developing identity (ERC-8004) but trust gap significant. |

---

## Conflicts Between Sources

1. **AMLR application date:** E8 (Cursor WebSearch) = 10 July 2027; E10 (MCP analysis) = June 30, 2025. **Resolution:** 10 July 2027 is the general application date from AMLR text; some provisions applied earlier. E8 is more authoritative (direct regulatory source).
2. **N4Y.AI AI Act classification:** E3 = "Provider Art.16-23"; E12 = "Deployer". **Resolution:** depends on whether N4Y.AI trains/fine-tunes models (Provider) or only integrates third-party GPAI (Deployer). Both may apply simultaneously for different functions.
3. **E10/E15 confidence:** Both are analytical interpretations from MCP model with April 2024 cutoff, not regulatory guidance. Marked Medium confidence accordingly.

---

## Remaining Holes

- **G2:** No ESMA/EBA formal guidance specifically addressing AI agent wallet accounts under Art.79. Analysis is based on text interpretation.
- **G4:** N4Y.AI's own Phase 1→4 roadmap details not found in external sources (internal architecture doc).
- **G7:** No single reference describes the exact "managed permeability" mechanism as designed for N4Y.AI; MI9/OpenShell/DeepMind are external frameworks, not N4Y.AI-specific implementations.
- **OI-047/OI-048:** These open issues require synthesis at draft stage, not additional evidence.
