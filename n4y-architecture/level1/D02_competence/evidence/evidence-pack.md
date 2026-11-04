# Evidence Pack: Step 5 — L1/D02 Компетентностное

> **Измерение:** L1/D02  
> **Версия:** 3.0 (deep-research re-run)  
> **Дата:** 03.04.2026  
> **Источники:** Cursor WebSearch (12+6 queries) + MCP deep_research (Perplexity/Sonar, effort=high)  
> **Предыдущий пак:** v2 (E1-E9); настоящий пак расширяет до E1-E33

---

## Evidence Table

| ID | Источник | URL | Дата | Тип | Ключевой claim | Gap(s) | Relevance | Confidence |
|----|----------|-----|------|-----|----------------|--------|-----------|------------|
| E1 | W3C Verifiable Credentials v2.0 (Recommendation) | https://www.w3.org/TR/vc-data-model-2.0/ | 2024 | norm | Трёхсторонняя модель Issuer → Holder → Verifier; selective disclosure; JSON-LD; расширяемый тип credentials. Trust model application-specific — не предписывает, кто является доверенным issuer | G1, G5, G13 | H | High |
| E2 | Open Badges 3.0 (Final Release v1.4.1, 1EdTech) | https://www.imsglobal.org/spec/ob/v3p0 | May 2024 | norm | Стандарт для recognition of achievements; W3C VC-совместим; ESCO alignment через `alignment` URIs; supports skills, competencies, certifications, micro-credentials | G5, G8 | H | High |
| E3 | European Digital Credentials for Learning (EDCL) | https://europa.eu/europass/en | актуальная | doc | Инициатива ЕС на базе W3C VC + ESCO; ESCO 3 039 occupations, 13 939 skills; EQF-нормализация уровней; Europass EDCI инфраструктура для кросс-граничного обмена | G6, G15 | H | High |
| E4 | SingularityNET — AI Agent Safety Credentials | — | актуальная | doc | VC для безопасности и fairness AI-агентов; provable claims о поведении; разделение компетенции владельца и capabilities агента | G1, G3, G5 | H | High |
| E5 | SFIA (Skills Framework for the Information Age) v8 | https://sfia-online.org/ | 2023 | doc | 6 ordinal levels per competency, 121 навык в IT; organization-specific numerical mapping; no universal ESq_base formula | G2, G12 | M | High |
| E6 | O*NET (US Department of Labor) | https://www.onetonline.org/ | актуальная | doc | 1 016 профессий с детальными skill profiles; DWA (Detailed Work Activities) для Course-Skill Atlas | G2 | M | High |
| E7 | ESCO v1.2 (European Commission) | https://esco.ec.europa.eu/ | May 2024 | norm | 13 939 skills/competences; мультиязычная таксономия; +35 occupations, +42 skills, +196 knowledge concepts в v1.2; DigComp digital skills labeling | G2, G14 | H | High |
| E8 | SWE-bench Pro (Scale AI) | https://scale.com/research/swe_bench_pro | 2025 | bench | 1 865 problems from 41 repos (public/held-out/commercial); long-horizon tasks (hours-days); GPT-5 top at 23.3% Pass@1 (Pro), 70-79% on Verified; data contamination controls via copyleft repos + 3-checkpoint human verification | G7 | H | High |
| E9 | Open Badges 3.0 production adoption (1EdTech, 2025-2026) | https://content.1edtech.org/badge-count-2025/ | 2025-2026 | doc | Field shifted "from experimentation to broad adoption"; Credly 30M+ badges (enterprise); Badgr (academic); Accredible (EdTech/Coursera) | G8 | H | High |
| E10 | GAIA Benchmark (Meta/HuggingFace) | https://gaia-benchmark-leaderboard.hf.space/ | 2024-2026 | bench | 466 questions, 3 difficulty levels; tests reasoning, multi-modality, web browsing, tool-use. **Top agents Mar 2026: testManus 92.36%, OPS-Agentic-Search 92.36%** — matching human baseline (~92%). GPT-4 with plugins scored 15% at launch (2023) | G9 | H | High |
| E11 | AgentBench (Tsinghua University, ICLR 2024) | https://arxiv.org/abs/2308.03688 | Aug 2023 | paper | 8 practical domains (OS, databases, knowledge graphs, web, gaming, smart home); GPT-4 65-87% on selected tasks; multi-domain heterogeneous evaluation | G9 | H | High |
| E12 | WebArena (CMU/Salesforce, NeurIPS 2024) | https://arxiv.org/abs/2307.13854 | Jul 2023 | paper | 812 web tasks (e-commerce, CMS, forums); human ≈78%; **top AI: OpAgent (Qwen3-VL+RL) 71.6%** surpassing GPT-5/Claude agents; realistic temporal sequences | G9 | H | High |
| E13 | OSWorld (CMU/Salesforce Research) | https://arxiv.org/abs/2404.07972 | Apr 2024 | paper | 369 real OS tasks (Ubuntu/Windows/macOS); file I/O, multi-app workflows; multimodal (vision); July 2025 major upgrade with AWS support; unique OS-level grounding niche | G9 | H | High |
| E14 | ERC-8126: AI Agent Registration & Verification | https://eips.ethereum.org/EIPS/eip-8126 | Jan 2025 | draft-std | Ethereum standard for agent self-registration with on-chain VCs; EIP-712 signing; 4 verification layers (ETV/SCV/WAV/WV); **ZKP via Private Data Verification; unified 0-100 risk score** (5 tiers); optional quantum-resistant crypto; gasless micropayments via x402 | G13 | H | Medium |
| E15 | zkMe Agent Certification Credential (ACC) | https://docs.zk.me/hub/what/zkkya/acc | 2025 | doc | 8-domain agent certification with ZKP selective disclosure; **three-stage lifecycle: self-assessment → third-party audit → crypto credential issuance**; dynamic revocation; W3C standards-based; part of 5-credential zkKYA stack (APC/AIC/ACC/ARC/APF) | G13, G15 | H | Medium |
| E16 | Accredible OB 3.0 + W3C VC Launch | https://accredible.com/newsroom/accredible-launches-support-for-open-badge-3-0-and-w3c-verifiable-credentials | Jan 2026 | doc | Production support for OB 3.0 + W3C VC with automatic format conversion; **ACE extension** for structured credit hours, academic level, competency alignments; $800M+ in recommended college credit | G8 | H | High |
| E17 | Velocert OB3 + EDC Dual-Format + eIDAS | https://velocert.com/2026/01/16/announcing-open-badge-credentials-obv3/ | Jan 2026 | doc | Simultaneous issuance in OB v3 + European Digital Credentials; dual signing (JWS + EDC-compliant); **JAdES e-seal for eIDAS compliance** — organizational legal identity + cross-border validity; RFC published for community review | G8 | H | High |
| E18 | ACEI: AI Competency Erosion Index (Springer) | https://link.springer.com/chapter/10.1007/978-3-032-11748-9_6 | Jan 2026 | paper | Neurobiologically-grounded decay model; **ratio-based: AI dependency as multiplicative degradation factor**; links individual exponential decay to organizational S-curve; based on synaptic plasticity/myelination. Framework (not yet validated); multi-phase validation needed | G10 | H | Medium |
| E19 | Agent Drift + Provider Drift Analysis | https://prassanna.io/blog/agent-drift/ ; https://dev.to/irparent/eval-drift-the-silent-quality-killer-for-ai-agents-50ok | 2025-2026 | doc | Agent drift: duration-dependent performance collapse (SWE-bench 70%→23% on long tasks). **Provider drift: Stanford/Berkeley study GPT-4 code gen 52%→10% in 3 months** (unannounced). 91% of ML models degrade over time; only 5% of production agents have mature monitoring. EU AI Act mandates continuous monitoring by Aug 2026 | G10 | H | High |
| E20 | Course-Skill Atlas (Nature Scientific Data) | https://www.nature.com/articles/s41597-024-03931-8 | 2024 | paper | NLP alignment of 3M+ course syllabi to DoL DWAs; **generates numerical alignment scores** aggregating into skill profiles for institutions/majors; production-grade course-to-competency quantification | G12 | H | High |
| E21 | EASEC Framework — Transversal Competencies | https://easec.eu/framework/ | 2025 | doc | 32 transversal competencies, 5 domains (Intellectual/Technical/Work/Interpersonal/Linguistic); **aligned with ESCO v1.2.1 + EQF**; 5 proficiency levels; results validated against ESCO + EQF descriptors → assigned level 1-5; observable, measurable criteria | G12 | H | High |
| E22 | ToIP TRQP V2.0: Trust Registry Query Protocol | https://www.lfdecentralizedtrust.org/blog/toip-announces-public-review-02-of-the-trust-registry-query-protocol-trqp-specification-v2.0 | Dec 2025 | draft-std | **"DNS for trust"** — lightweight protocol for querying trust registries at scale; PARC model (Principal/Action/Resource/Context); Authorization + Recognition queries; supports W3C DIDs, KERI AIDs, HTTPS URLs as identifiers | G13 | H | High |
| E23 | ToIP Issuer Governance Requirements Guide | https://trustoverip.org/blog/2024/04/02/new-issuer-governance-requirements-guide-for-verifiable-credentials-public-comment-needed/ | Apr 2024 | doc | **7 core requirements for VC issuers:** Transparency, Auditability/Accountability, Interoperability, Revocation/Expiry, Privacy, Verification of Issuer/Holder, Validation of Claims. Governance Stack WG specification | G13 | H | High |
| E24 | Cedefop NQF Online Tool (41 countries) | https://cedefop.europa.eu/en/tools/nqfs-online-tool/ | 2025 | tool | Cross-border NQF comparison for **41 countries** (EU + candidates + partners); visual side-by-side qualification level mapping; quality assurance metadata; biennial data collection by Cedefop/EC/ETF | G15 | M | High |
| E25 | Corgent Virtual Trust Oracle (Cortensor Network) | https://docs.cortensor.network/roadmap/technical-roadmap-2026-network-launch-and-agentic-ai-infrastructure/corgent-2026-roadmap-virtual-trust-oracle-on-cortensor | Q1 2026 | doc | Data-driven trust oracle for AI agents; three services: Delegation-as-a-Service, **Validation-as-a-Service (Proof of Inference/PoUW)**, Arbitration-as-a-Service; collects validation datasets from live agent interactions on Cortensor Testnet | G13 | M | Medium |
| E26 | ArXiv 2511.02841: AI Agents with DIDs and VCs | https://arxiv.org/abs/2511.02841 | Nov 2025 | paper | Equips AI agents with self-sovereign identities using W3C DIDs + third-party VCs; proves identity ownership + cross-domain trust; demonstrates DID+VC framework for agent certification | G13 | H | High |
| E27 | W3C VC Working Group Charter (March 2026) | https://www.w3.org/2026/03/vc-wg-charter.html | Mar 2026 | norm | New WG charter: maintenance of existing VC specs + new render methods recommendation; completion expected July 2026; active standards development continues | G8, G13 | M | High |
| E28 | SkillsBench 2026 | https://arxiv.org/abs/2602.12670 | Feb 2026 | paper | 86 tasks across 11 domains; curated skills + deterministic verifiers; measures whether agent skills actually improve performance; re-benchmarking capability assessment | G9, G10 | H | High |
| E29 | ERC-8004: Trustless Agents | https://eips.ethereum.org/EIPS/eip-8004 | Jan 2026 | draft-std | Agent discovery and trust across organizational boundaries; **pluggable trust models**: reputation, stake-secured re-execution, ZK-ML proofs, TEE oracles; on-chain Identity/Reputation/Capability registries via ERC-721 NFTs | G13 | H | Medium |
| E30 | AgentTrust AQVC (quality-oracle) | https://github.com/vitaliiserbynassisterr/quality-oracle | 2025 | doc | Challenge-response verification issuing W3C VCs (AQVC format); **6-axis scoring**: accuracy, safety, reliability, process quality, latency, schema quality; applicable to AI agents and MCP servers | G13 | M | Medium |
| E31 | VARIA Project (EU Horizon 2022-2024) | https://www.varia-project.eu/ | 2022-2024 | project | Maps EQF levels + ESCO skills into Open Badges 3.0 with numerical confidence scores: **`competency_confidence = (level_score × weight) + (skill_specificity_score × (1-weight))`**; directly addresses EQF/ESCO → numerical score conversion; not yet widely deployed | G12 | H | Medium |
| E32 | EU AI Act Article 4 + AI Continent Action Plan | https://digital-strategy.ec.europa.eu/en/policies/ai-talent-skills-and-literacy | Apr 2025 | norm | **AI literacy mandatory** for AI system providers/deployers (effective Feb 2, 2025); 40+ AI literacy initiatives in AI Office repository; ARISA project for AI skills development; AI Continent Action Plan (Apr 2025) to expand EU AI specialist pool | G14 | M | High |
| E33 | ESCO LOD Ontology + API | https://ec.europa.eu/esco/lod/static/model.html ; https://data.europa.eu/esco/api/latest/ | 2024 | norm | OWL-based ontology v2.0.0; SKOS + SKOS-XL + ADMS; three pillars (Occupation/Skill/Qualification); **download: RDF, TTL, JSON-LD, XML, CSV**; JSON-LD context at data.europa.eu; ESCO API active (1000 req/hr); SKOS mapping to ISCED-FoET, ISCO08 | G14 | H | High |

---

## Gap Coverage Matrix

| Gap | Описание | Критичность | Covered by | Coverage Assessment |
|-----|----------|-------------|------------|---------------------|
| G7 | SWE-bench Pro как benchmark-оракул для Agent Capability VC | High | E8 | **Strong.** 1 865 problems, contamination controls, long-horizon tasks documented. Top GPT-5 score 23.3% (Pro) vs 70-79% (Verified). Missing: formal score→VC translation spec, revocation semantics |
| G8 | Open Badges 3.0 production adoption + W3C VC integration | Medium | E2, E9, E16, E17, E27 | **Strong.** Accredible + Velocert production launches (Jan 2026); ACE extension; eIDAS JAdES e-seal; W3C VC WG active through Jul 2026. Missing: formal ESCO+OB3+VC JSON-LD context; interop test suite |
| G9 | Новые benchmark-стандарты 2025-2026 (GAIA, AgentBench, WebArena, OSWorld) | High | E10, E11, E12, E13, E28 | **Strong.** Four complementary benchmarks with 2026 leaderboard data. GAIA agents reach human-level (92.36%). WebArena OpAgent 71.6%. SkillsBench (86 tasks/11 domains) adds skills-focused evaluation. Missing: unified scoring rubric, VC claim aggregation standard |
| G10 | Decay-модель ESq: деградация без re-benchmarking | Medium | E18, E19, E28 | **Moderate.** ACEI exponential decay framework (multiplicative degradation factor); provider drift: GPT-4 code 52%→10% in 3 months; 91% models degrade. EU AI Act continuous monitoring by Aug 2026. Missing: agent-specific empirical study over 12+ months; concrete re-cert intervals |
| G11 | ESr formal status: каноничный термин или proposed? | Medium | E19 (partial) | **Weak.** "ESr" NOT found in external literature. Closest external: "dynamic capability gating" (NIST Zero Trust), "just-in-time access" (JiTA), runtime policy enforcement. **ESr is Logos-internal; needs standardization proposal or alignment to existing terminology** |
| G12 | ESq_effective algorithm: capability_multiplier formula | High | E5, E20, E21, E31 | **Moderate.** VARIA: `competency_confidence = (level_score × weight) + (skill_specificity_score × (1-weight))`. Course-Skill Atlas: NLP numerical alignment scores. EASEC: 5-level from EQF, observable criteria. SFIA: 6 ordinal levels. **No canonical ESq_base or capability_multiplier formula published.** Logos must design this |
| G13 | Benchmark oracle governance: кто выдаёт Agent Capability VC | High | E1, E14, E15, E22, E23, E25, E26, E29, E30 | **Strong.** Rich ecosystem emerging: ERC-8126 (ZKP risk scoring), zkMe ACC (3-stage lifecycle with audit), ToIP TRQP V2.0 ("DNS for trust"), ToIP Issuer Governance (7 requirements), ERC-8004 (pluggable trust models), ArXiv DID+VC for agents, Corgent oracle. Missing: benchmark-specific oracle governance spec |
| G14 | ESCO v1.2 update status; SKOS/RDF в VC claims | Medium | E7, E32, E33 | **Strong.** ESCO v1.2 current (May 2024); ontology v2.0.0 (OWL/SKOS/JSON-LD); API active (1000 req/hr); JSON-LD context → VC-compatible claim embedding. AI Act Art. 4 mandates AI literacy. v1.3 timeline uncertain |
| G15 | EQF/NQF cross-border recognition in digital agent context | Low | E3, E24 | **Moderate.** Cedefop NQF tool covers 41 countries; EDCI infrastructure operational (OB3+VC); EQF 8 levels EU-wide. **AI agent credentials NOT yet integrated into EQF/EDCI.** No formal EQF profile for agent competencies |

---

## Source Conflicts and Uncertainties

1. **SWE-bench Pro scoring discrepancy**: E8 reports GPT-5 at 23.3% (Scale AI primary source), while MCP pass cited earlier 2024 snapshots with different models at 30-40% on original SWE-bench. Resolved: these are different benchmark subsets (Pro vs. Verified/original).

2. **GAIA leaderboard variance**: Cursor pass (E10) shows top agents at 92.36% (Mar 2026), MCP pass cites 2024 data showing Claude 3.5 Sonnet at 52%. Resolved: rapid progress from 2024→2026; 2026 scores from official HuggingFace leaderboard are authoritative.

3. **ESCO v1.3 release**: MCP pass indicates v1.3 announced for Q1-Q2 2024 but uncertain; Cursor pass found no v1.3 confirmation. Status: **unresolved — v1.3 may or may not have shipped by April 2026.**

4. **VARIA Project scoring formula (E31)**: Confidence Medium — project is EU Horizon R&D, not yet in wide deployment. Formula is directional but not industry-validated.

5. **ACEI framework (E18)**: Confidence Medium — proposed framework, multi-phase validation research required; not yet a validated measurement tool.

6. **Blockchain-based standards (E14, E15, E29)**: All in draft status; ERC-8126/ERC-8004 are Ethereum proposals not yet ratified. zkMe ACC is proprietary. Confidence Medium for all three.

---

## Remaining Holes

| Area | What's Missing | Recommendation |
|------|---------------|----------------|
| G7 score→VC translation | No formal spec for converting SWE-bench Pro % → Agent Capability VC claims | Logos must design: threshold model (pass/fail at X%) or continuous scoring in VC claim |
| G9 unified scoring | No cross-benchmark normalization rubric | Propose multi-benchmark capability profile (radar chart): SWE-bench Pro (code), GAIA (general), WebArena (web), OSWorld (OS) |
| G10 re-cert intervals | No empirical agent-specific decay study | Commission longitudinal study: re-test same agents on SWE-bench/GAIA at 3/6/12/24 months |
| G11 ESr canonicity | Term is Logos-internal only | Define ESr formally in architecture doc; cross-reference with NIST ZTA "dynamic capability enforcement" |
| G12 ESq formula | No published ESq_base or capability_multiplier standard | Design Logos-specific: ESq_base = f(EQF_level, ESCO_skill_count, credential_recency); capability_multiplier = normalized_benchmark_score |
| G15 agent in EQF | EQF/EDCI have no agent competence profile | Long-term: propose agent competency EQF extension to EC |
