# Evidence Pack | L3/D08 — Conflicts & Arbitration

> **Dimension:** L3/D08 | **Version:** 2.0 | **Date:** 2026-04-04 | **Pipeline step:** 5 (deep-research)
> **Sources:** 42 distinct | **Cursor queries:** 12 | **MCP clusters:** 3

---

## Evidence Table

| ID | Source | Date | Type | Key Claim | Gap(s) | Relevance | Confidence | URL |
|----|--------|------|------|-----------|--------|-----------|------------|-----|
| E1 | GDPR Art. 22 — Official text | 2018 (current) | Legal text | Right not to be subject to solely automated decisions producing legal effects; safeguards: human intervention, right to contest, right to explanation | G-03, G-14 | Direct | High | https://gdpr-info.eu/art-22-gdpr/ |
| E2 | ICO — GDPR Art. 22 guidance (UK) | 2024-2025 | Regulatory guidance | "Solely automated" = no human influence on outcome; meaningful human review criteria: competence, authority, interpretability, active analysis, no disincentives, logging | G-03, G-14 | Direct | High | https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/individual-rights/automated-decision-making-and-profiling/ |
| E3 | IAPP — SCHUFA ECJ Ruling C-634/21 | 2023 | Case law analysis | Credit scores = "legal effects" under Art. 22 even when used indirectly; expanded scope to intermediate analytical processes significantly influencing outcomes | G-03 | Direct | High | https://iapp.org/news/a/key-takeaways-from-the-cjeus-recent-automated-decision-making-rulings |
| E4 | Handley Gill — Meaningful Human Involvement (UK Data Use & Access Act 2025) | 2025 | Legal analysis | 7 criteria for meaningful human involvement: competent, authority to override, every decision, interpretable, active analysis, contextual knowledge, no disincentives, auditable | G-03, G-14 | Direct | High | https://www.handleygill.co.uk/handley-gill-blog/section-80-data-use-and-access-act-2025-article-22a-uk-gdpr-automated-decision-making-automated-processing-meaningful-human-involvement |
| E5 | Kaminski & Urban — "The Right to Contest AI" | 2021 | Academic (Columbia Law Review) | Right to contest AI decisions modeled on due process principles; key protection mechanism under GDPR | G-03 | Moderate | High | https://www.columbialawreview.org/wp-content/uploads/2021/11/Kaminski-Urban-The_Right_to_Contest_AI.pdf |
| E6 | EU AI Act — Annex III, Art. 8(a) | 2024 | Legal text | AI in ADR = high-risk when outcomes produce legal effects for parties | G-04 | Direct | High | https://artificialintelligenceact.eu/annex/3/ |
| E7 | EU AI Act — Recital 61 | 2024 | Legal text | ADR AI systems explicitly designated high-risk when proceedings produce legal effects | G-04 | Direct | High | https://artificialintelligenceact.eu/recital/61/ |
| E8 | EU AI Act — Art. 86 (Right to Explanation) | 2024 | Legal text | Right to "clear and meaningful explanations of the role of the AI system in the decision-making procedure and the main elements of the decision taken" | G-04, G-03 | Direct | High | https://artificialintelligenceact.eu/article/86/ |
| E9 | EU AI Act — Art. 85 (Right to Complaint) | 2024 | Legal text | Right to lodge complaint with market surveillance authority for AI Act violations | G-04, G-09 | Direct | High | https://artificialintelligenceact.eu/article/85/ |
| E10 | EU AI Act — Art. 27 (FRIA) | 2024 | Legal text | Fundamental rights impact assessment required before deploying high-risk AI; must describe processes, frequency, affected persons, risks, human oversight, complaint mechanisms | G-04, G-14 | Direct | High | https://artificialintelligenceact.eu/article/27/ |
| E11 | EU AI Act — Art. 26 (Deployer Obligations) | 2024 | Legal text | Human oversight by competent natural persons with authority to halt; log maintenance ≥6 months; input data representativeness; monitoring obligations | G-04, G-14, G-10 | Direct | High | https://artificialintelligenceact.eu/article/26/ |
| E12 | TechPolicy Press — GDPR vs AI Act explanation comparison | 2024 | Analysis | Art. 86 (AI Act) focuses on AI role in specific decision; Art. 15/22 (GDPR) on overall processing logic; must satisfy more demanding standard | G-04, G-03 | Moderate | High | https://techpolicy.press/understanding-right-to-explanation-and-automated-decisionmaking-in-europes-gdpr-and-ai-act |
| E13 | ES Think Tank — Regulating AI in ADR | 2025 | Policy analysis | Challenges, risks, best practices for AI in alternative dispute resolution; classification nuances | G-04 | Moderate | Medium | https://esthinktank.com/2025/05/15/regulating-ai-in-alternative-dispute-resolution/ |
| E14 | Verdikta Whitepaper | 2024-2026 | Industry (protocol) | Multi-AI-arbiter consensus via commit-reveal; reputation-weighted random selection; ~$0.60/verdict; VDKA staking (min 100); reputation scoring; averaging for continuous values | G-01, G-02, G-05, G-06, G-07, G-13 | Direct | High | https://www.verdikta.com/whitepaper |
| E15 | BNBAgent SDK — ERC-8183 | 2026 | Industry (protocol) | First ERC-8183 implementation; dispute resolution via UMA Optimistic Oracle → token-holder voting; standardized job lifecycle; on-chain identity via ERC-8004 | G-02, G-07 | Moderate | Medium | https://cryptopotato.com/bnb-chain-launches-bnbagent-sdk-the-first-live-implementation-of-erc-8183-for-trustless-onchain-ai-agents/ |
| E16 | InternetCourt (GenLayer) | 2024-2026 | Industry (protocol) | Plain-text agreements, USDC escrow on Base L2, AI jury of validators with different LLMs, 2-of-2 multisig or AI jury, minutes resolution | G-01, G-02, G-07 | Direct | Medium | https://github.com/genlayer-foundation/internetcourt |
| E17 | Kleros Court Documentation | Current | Industry (protocol) | PNK staking → random juror selection → commit-reveal voting; specialized sub-courts; evidence submission period; Mexican court recognition | G-02, G-05, G-07, G-13 | Direct | High | https://docs.kleros.io/products/court |
| E18 | Kleros 2024 Project Update | 2024 | Industry (update) | SBT research for juror selection; griefing analysis; voting incentive systems; no patents | G-11, G-13 | Moderate | High | https://blog.kleros.io/kleros-project-update-2024/ |
| E19 | Kleros 2026 Project Update | 2025-2026 | Industry (update) | Automated Curation Court with AI participation; numerical outcome dispute research; Kleros Oracle with Reality.eth; Mexico recognition Feb 2025 | G-02, G-05, G-07, G-14 | Direct | High | https://blog.kleros.io/kleros-project-update-2026/ |
| E20 | Kleros Dec 2025 Development Update | Dec 2025 | Industry (update) | Court V1 notification expansion; dispute appealed + juror result notifications; Curate V2 direct evidence handling (removed Evidence Module dependency) | G-05, G-10 | Moderate | High | https://blog.kleros.io/kleros-development-update-december-2025/ |
| E21 | Stanford Law — Kleros socio-legal case study | 2023-2024 | Academic | Decentralized justice and blockchain arbitration socio-legal analysis | G-02, G-05 | Moderate | High | https://law.stanford.edu/publications/kleros-a-socio-legal-case-study-of-decentralized-justice-blockchain-arbitration/ |
| E22 | Arbitration Blog — Smart Contracts, Kleros, New Platforms | 2024-2025 | Industry (legal) | JAMS smart contract rules (2024); SIAC 2025 rules; institutional adoption of decentralized arbitration | G-02, G-08, G-12 | Moderate | High | https://www.arbitrationblog.org/post/smart-contracts-kleros-and-new-arbitration-platforms-what-already-works-and-where-practice-is-hea |
| E23 | Orochi Network — Polymarket 2025 Oracle Attack | 2025 | Industry (incident) | UMA token holder with 25% voting power ($5M tokens) manipulated $7M market; false settlement of Ukraine mineral deal contract | G-07, G-11, G-13 | Direct | High | https://orochi.network/blog/oracle-manipulation-in-polymarket-2025 |
| E24 | 7Goldfish — Broken Truth Machines (UMA analysis) | 2025 | Investigative | Mathematical proof: tokens as sole vote-weighting instrument cannot resist both Sybil attacks and plutocracy simultaneously; 25% voting power → 10x market manipulation | G-07, G-11, G-13 | Direct | High | https://7goldfish.com/articles/Broken_Truth_Machines.php |
| E25 | ERC-8033 — Agent Council Oracles | 2025-2026 | Standard (EIP) | Multi-agent councils for arbitrary information queries; commit-reveal-judge flows; bonds + incentives deter collusion; trust-minimized qualitative + quantitative resolution | G-07 | Direct | High | https://eips.ethereum.org/EIPS/eip-8033 |
| E26 | Supra — Threshold AI Oracles | 2024-2025 | Industry (whitepaper) | AI-driven multi-agent committees with verified on-demand outputs; challenges: NL aggregation, subjectivity, model drift, adversarial attacks | G-07 | Moderate | Medium | https://supra.com/documents/Threshold_AI_Oracles_Supra.pdf |
| E27 | DeepSafe — Polymarket Oracle Manipulation Case | 2025 | Industry (analysis) | UMA vulnerable to large token holders (quantitative oracles like Chainlink less so); incorrect resolutions for Venezuela election, ETH ETF approvals | G-07, G-11 | Moderate | High | https://medium.com/@DeepSafe_Official/the-value-of-ai-oracles-insights-from-the-polymarket-oracle-manipulation-case-92d83e2bef0a |
| E28 | AAMAS 2016 — Norm conflicts in MAS | 2016 | Academic | Conflict detection in multi-agent systems = NP-complete; grouping algorithm by context/entities/actions reduces complexity | G-01 | Moderate | High | https://www.ifaamas.org/Proceedings/aamas2016/pdfs/p1383.pdf |
| E29 | Arion Research — Conflict Resolution Playbook for Agentic AI | 2025-2026 | Industry (analysis) | Goal/info/plan conflicts; must be designed into architecture from inception; traditional escalation fails at machine speed | G-01, G-02 | Direct | High | https://www.arionresearch.com/blog/conflict-resolution-playbook-how-agentic-ai-systems-detect-negotiate-and-resolve-disputes-at-scale |
| E30 | IGI Global — Conflict Classification in MAS | 2014 | Academic (book chapter) | Conflict classification and resolution modeling; conflict strength metric; design-time vs runtime conflicts | G-01 | Moderate | Medium | https://www.igi-global.com/viewtitle.aspx?TitleId=112394 |
| E31 | ConfRSSM — Conflict Resolution Strategy Selection | 2019 | Academic | Strategy selection based on: domain requirements, conflict strength, agents' confidence level | G-01, G-02 | Moderate | Medium | https://pdfs.semanticscholar.org/2ece/73e3d50a5996503d261f816b3f1885f75afb.pdf |
| E32 | Hanzhe Zhang — Ultimatums with external resolution | 2025 | Academic (R&R at REStud) | Justified vs unjustified parties use ultimatums strategically; not invoking external resolution = sign of weakness; reputation effects on frequency of internal resolution | G-06 | Direct | High | https://hanzhezhang.github.io/research/2507ultimatumREStud.pdf |
| E33 | PLOS ONE — Deterrence paradox | 2021 | Academic | Rich info on past actions → breakdown of deterrence; minimal info → deterrence evolves; information costs prevent deterrence from being evolutionarily stable | G-06 | Direct | High | https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0253344 |
| E34 | Precedent Protocol — Proof of Precedent | 2023-2024 | Industry (whitepaper) | Tokenized precedents; PrecedentCoin incentivizes publication; PoP = justiciable disputes published for citation; polycentric common law development; agnostic to fairness | G-05 | Direct | High | https://docs.google.com/document/d/14W6ZbvO4ICTyCoZ0MPdf2nYTJuP5viCQUBM-26acFbE/mobilebasic |
| E35 | Delos DR — DAO Model Law (COALA) | 2022-2024 | Legal (model law) | Reinventing due process for decentralized autonomous organizations | G-05, G-08 | Moderate | High | https://delosdr.org/the-model-law-for-decentralized-autonomous-organizations-reinventing-due-process/ |
| E36 | SSRN — Decentralized Autonomous Justice | 2022 | Academic | Blockchain dispute resolution for DAOs: rise of decentralized autonomous justice; jurisdictional uncertainty, enforcement, pseudonymous participants | G-05, G-08 | Moderate | Medium | https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4042704 |
| E37 | IBA — Lex Cryptographia: Due Process in Blockchain Arbitration | 2022 | Legal (guidelines) | Best practice: governing law, forum selection, off-chain override, oracle management, evidence auditability; jurisdictional challenges for decentralized systems | G-08, G-09 | Direct | High | https://www.ibanet.org/lex-cryptographia-due-process-blockchain-based-arbitration |
| E38 | Cross-border smart contract enforcement | 2024-2025 | Academic (journal) | Enforceability strongest with e-signature + digital-asset + arbitration coexistence; UK UKJT, EU Data Act, US UCC Art. 12, Switzerland DLT Act, India IT Act; Rome I, Hague Principles, NYC foundational | G-08 | Direct | High | https://sjaibt.org/index.php/j/article/view/137 |
| E39 | NYC — Blockchain arbitral awards | 2023-2024 | Academic (journal) | NYC recognition challenges: procedural aspects may undermine public policy; Art. V(1)(b) due process refusal; auto-enforcement makes traditional sanctions ineffective | G-08 | Direct | High | https://doaj.org/article/6360f80bf3ce42d4a83b7d445559f052 |
| E40 | Frontiers — Griefing Analysis in Blockchain | 2023 | Academic | Griefing factor ≥1 in speaker-listener equivalence → perfect defense impossible; design to minimize factor below 1 in most scenarios | G-11 | Direct | High | https://www.frontiersin.org/journals/blockchain/articles/10.3389/fbloc.2023.1137155/full |
| E41 | Arbitrum BoLD Protocol Paper | 2024 | Technical (protocol) | Interactive dispute: stakes confiscated from dishonest → redistributed to honest; bounded 6.4-day max; parallel challenge resolution; three-layer model; anti-griefing | G-02, G-10, G-11 | Direct | High | https://arxiv.org/pdf/2404.10491 |
| E42 | BATTLE Protocol (FairGate) | 2024-2025 | Technical (protocol) | Tournament-style: constant minimum capital for honest asserters; logarithmic resolution time; progressive rewards; fraud proof trilemma (safety/promptness/decentralization) | G-11, G-02 | Direct | High | https://www.fairgate.io/publication/26-battle-a-breakthrough-in-blockchain-dispute-resolution |
| E43 | Cambridge — Polycentric law: lex mercatoria & blockchain | 2021-2023 | Academic | Historical precedent: lex mercatoria as polycentric legal ordering; blockchain as polycentric legal order; flexible norms vs code-ex-ante enforcement | G-05, G-09 | Moderate | High | https://www.cambridge.org/core/journals/journal-of-institutional-economics/article/past-present-and-future-of-polycentric-legal-order-a-comparative-institutional-analysis-of-lex-mercatoria-and-blockchain/C8A1D43071F5E536354F67DBF7842323 |
| E44 | Cambridge — Blockchain as polycentric orders | 2021-2023 | Academic | Blockchain governance: internal (protocol) + external (competitive pressure); constitutional and competitive polycentric orders | G-05, G-09 | Moderate | High | https://www.cambridge.org/core/journals/journal-of-institutional-economics/article/blockchain-networks-as-constitutional-and-competitive-polycentric-orders/2DFD78A55D1D84EAE5D008B448531E1D |
| E45 | BlockchainGov — Polycentric governance report | 2020-2023 | Research report | Multiple autonomous decision-making centers with overlapping responsibility; competition + cooperation within agreed rules | G-09 | Moderate | Medium | https://blockchaingov.eu/blockchain-technology-and-polycentric-governance-a-research-report/ |
| E46 | Google Cloud — Architecting Agent-Led Escalation | 2025 | Industry (architecture) | Human-in-the-loop: AI calls handoff tool via webhook; warm handoff with problem summary, conversation history, sentiment analysis | G-02, G-14 | Moderate | High | https://medium.com/google-cloud/architecting-agent-led-escalation-in-generative-ai-03136849f3cb |
| E47 | AI Genesis — AI Agent Escalation | 2025 | Industry (guide) | 5 escalation trigger categories: sentiment, complexity, authority, confidence (<40% auto), user-initiated; healthy rate 5-10% | G-02, G-14 | Moderate | High | https://ai-genesis.ai/blog/ai-agent-escalation-how-it-works |
| E48 | Kommunicate — AI Customer Service Escalation | 2025 | Industry (guide) | Rules, triggers & handoffs for AI escalation; target 20-35% rate with >90% appropriateness | G-02, G-10 | Low | Medium | https://www.kommunicate.io/blog/ai-customer-service-escalation |
| E49 | SIAC 2025 Rules | Jan 2025 | Legal (arbitration rules) | Multiparty arbitration provisions; consolidation; expedited procedures; multiparty disclosure management | G-08, G-12 | Direct | High | https://siac.org.sg/siac-rules-2025 |
| E50 | Walkers — DAO Litigation | Oct 2024 | Legal (analysis) | Ooki DAO (2022): forum service = valid; Hector DAO (2024): receiver location = center of interests; US state DAO recognition (VT, WY, TN) | G-08, G-12 | Moderate | High | https://www.walkersglobal.com/Insights/2024/10/Litigation-Against-Decentralised-Autonomous-Organisations |
| E51 | Kennedys — Arbitration, Crypto & Courts 2026 | 2026 | Legal (analysis) | LCIA/ICC support virtual hearings; arbitration preferred for crypto: neutrality, specialists, confidentiality, cross-border; enforceability = biggest open question | G-08 | Moderate | High | https://www.kennedyslaw.com/en/thought-leadership/article/2026/arbitration-crypto-assets-and-the-courts-adapting-dispute-resolution-for-a-digital-economy/ |
| E52 | Baker Botts — AI Agent Governance | 2025-2026 | Legal/industry | California AB 316 (Jan 2026): AI autonomous operation not defense to liability; 3 governance principles: inclusivity, visibility, liability; kill switches + HITL triggers | G-09, G-14 | Moderate | High | https://ourtake.bakerbotts.com/post/102me2l/when-ai-agents-misbehave-governance-and-security-for-autonomous-ai |
| E53 | AAMAS 2024 — Emergent Cooperation Under Uncertain Incentives | May 2024 | Academic | Uncertainty lowers cooperative ability; reputation + intrinsic rewards boost near-optimal behavior; agents tend to defensive strategies under uncertainty | G-06, G-12 | Moderate | High | https://www.ifaamas.org/Proceedings/aamas2024/pdfs/p1521.pdf |
| E54 | Aragon Court — What is Aragon Court | 2020-2022 | Industry (docs) | "Handles subjective disputes that cannot be solved by smart contracts"; ANT staking; Schelling point ("rule as plurality"); slashing minority → rewarding majority | G-01, G-02, G-07 | Direct | High | https://legacy-docs.aragon.org/products/aragon-court/aragon-court |
| E55 | Mayer Brown — Code to Court: ADR On/Off Chain | Nov 2024 | Legal (journal) | From code to court and beyond: ADR architecture combining on-chain and off-chain mechanisms | G-02, G-08 | Moderate | High | https://www.mayerbrown.com/-/media/files/perspectives-events/publications/2024/11/from-code-to-court-and-beyond--alternative-dispute-resolution-on-and-off-the-blockchain--dispute-resolution-journal--huang-harrington-nov-2024.pdf |
| E56 | Freshfields — AI in Arbitration Principles | 2025 | Legal (guide) | Principles guiding AI use in arbitral proceedings; emerging regulatory consensus; separation of AI analysis and decision authority | G-04, G-14 | Moderate | High | https://riskandcompliance.freshfields.com/post/102mmiy/ai-in-arbitration-part-i-principles-guiding-the-use-of-ai-in-arbitral-proceedin |
| E57 | Galileo — Multi-Agent Regulatory Compliance | 2025 | Industry (guide) | Inter-agent compliance verification checkpoints; hierarchical verification scaling with risk; emergent behavior detection in MAS | G-09, G-01 | Moderate | Medium | https://galileo.ai/blog/regulatory-compliance-multi-agent-ai |
| E58 | ChainScore — Oracle Dispute Resolution Design | 2024-2025 | Industry (guide) | Structured guide for designing dispute resolution for oracle data; challenge periods, evidence requirements | G-07 | Low | Medium | https://www.chainscorelabs.com/en/guides/real-estate-and-asset-tokenization/rwa-data-oracles/how-to-design-a-dispute-resolution-mechanism-for-oracle-data |

---

## Gap Coverage Matrix

| Gap | Status | Key Evidence | Notes |
|-----|--------|-------------|-------|
| G-01 Conflict taxonomy | **Partial → closable** | E28 (MAS norm conflicts, NP-complete), E29 (agentic AI playbook: goal/info/plan), E30 (classification model), E31 (ConfRSSM), E14 (Verdikta categories), E54 (Aragon: objective vs subjective) | Academic MAS taxonomy maps well but N4Y 9-category specificity needs synthesis |
| G-02 Resolution ladder | **Strong** | E14 (Verdikta 5-tier), E17 (Kleros courts), E19 (Kleros curation court), E41 (BoLD 6.4d bounded), E54 (Aragon Court), E46-E47 (escalation triggers), E22 (SIAC/JAMS rules) | 5-tier architecture well-evidenced across multiple platforms |
| G-03 GDPR Art.22 | **Strong** | E1 (Art.22 text), E2 (ICO guidance), E3 (SCHUFA ruling), E4 (meaningful human involvement 7 criteria), E5 (right to contest), E8 (AI Act Art.86), E12 (GDPR vs AI Act) | Comprehensive regulatory framework with actionable criteria |
| G-04 AI Act ADR | **Strong** | E6 (Annex III high-risk), E7 (Recital 61), E8 (Art.86 explanation), E9 (Art.85 complaint), E10 (Art.27 FRIA), E11 (Art.26 deployer), E12-E13 (analysis) | **FLAG: research.md references "Art.47" — should be Art.85/86/Annex III** |
| G-05 Precedent system | **Moderate** | E34 (Precedent Protocol PoP), E43-E44 (polycentric law), E20 (Kleros evidence handling), E14 (Verdikta implicit learning) | Architecture described but production validation limited |
| G-06 Reputation calibration | **Moderate** | E32 (ultimatum game with reputation), E33 (deterrence paradox), E53 (emergent cooperation), E14 (Verdikta reputation scoring) | Game-theoretic foundations solid; specific calibration formulas limited |
| G-07 Oracle subjectivity | **Strong** | E23-E24 (UMA/Polymarket attack), E25 (ERC-8033), E26 (Supra threshold), E14 (Verdikta multi-AI), E17 (Kleros Schelling), E27 (DeepSafe analysis) | Multiple defense mechanisms documented; UMA vulnerability well-characterized |
| G-08 Cross-sandbox | **Moderate-Strong** | E37 (Lex Cryptographia), E38 (cross-border enforcement), E39 (NYC recognition), E49 (SIAC multiparty), E50 (DAO litigation), E51 (arbitration 2026) | Legal frameworks exist but cross-chain atomicity for >2 chains remains open |
| G-09 Regulatory conflicts | **Moderate** | E37 (IBA guidelines), E52 (AB 316 liability), E57 (multi-agent compliance), E43-E45 (polycentric governance) | Distinct paths recognized but formalization for N4Y context needed |
| G-10 Lifecycle timing | **Moderate** | E41 (BoLD 6.4d), E24 (UMA 48-96h), E14 (Verdikta minutes), E48 (escalation rates), E11 (Art.26 ≥6mo logs), E20 (Kleros notifications) | SLAs available from multiple systems; auto-expiry mechanisms underdeveloped |
| G-11 Incentive misalignment | **Strong** | E40 (griefing factor ≥1 limit), E41 (BoLD anti-griefing), E42 (BATTLE tournament), E23-E24 (UMA manipulation), E54 (Aragon slashing) | Fraud proof trilemma well-characterized; multiple protocol solutions documented |
| G-12 Multi-party dynamics | **Moderate** | E49 (SIAC 2025 multiparty rules), E50 (DAO litigation), E53 (AAMAS cooperation), E31 (ConfRSSM aggregation) | Institutional rules exist; blockchain-native multi-party resolution immature |
| G-13 Sybil resistance | **Moderate** | E18 (Kleros SBT research), E24 (Sybil/plutocracy impossibility), E14 (Verdikta reputation-weighted), E17 (Kleros random selection) | Fundamental impossibility result (tokens alone); SBTs + quadratic voting as mitigation |
| G-14 Human oversight | **Strong** | E2 (ICO 7 criteria), E4 (meaningful involvement), E10-E11 (AI Act FRIA + deployer), E46-E47 (escalation architecture), E19 (Kleros curation court), E52 (kill switches + HITL) | Comprehensive regulatory + practical framework |

---

## Source Conflicts & Discrepancies

### Conflict 1: Art. 47 Attribution Error
- **research.md (§2, §6):** References "AI Act Art.47" for ADR compliance and right to explanation
- **Actual Art. 47:** EU Declaration of Conformity (procedural document requirement)
- **Correct articles:** Art. 85 (complaint), Art. 86 (explanation), Annex III Art. 8(a) (high-risk classification), Recital 61 (ADR)
- **Severity:** Medium — requires correction in research.md and downstream references
- **Evidence:** E6, E7, E8, E9 vs Q3 Cursor search

### Conflict 2: Sybil vs Plutocracy Impossibility
- **E24 (7Goldfish):** Mathematical proof that token-only vote-weighting cannot simultaneously resist Sybil attacks AND plutocracy
- **E14 (Verdikta), E17 (Kleros):** Implement token-staking systems with reputation overlays claiming to address both
- **Resolution:** Not truly contradictory — E24 proves impossibility for tokens as *sole* instrument; Kleros/Verdikta add reputation, SBTs, randomization as supplementary mechanisms. Mitigation, not elimination.

### Conflict 3: Automated vs Human Arbitration Efficiency
- **E14 (Verdikta):** ~$0.60/verdict, <2 minutes — argues for fully automated AI arbitration
- **E2, E4 (ICO/GDPR):** Meaningful human involvement required for decisions with legal effects — implies significant overhead
- **Resolution:** Context-dependent. Low-stakes disputes (<threshold) may proceed with automated resolution if explicit consent obtained. High-stakes disputes require hybrid architecture per GDPR Art. 22(2) exceptions.

### Conflict 4: Precedent System Maturity
- **E34 (Precedent Protocol):** Claims tokenized precedent system creates "polycentric common law"
- **E43 (Cambridge):** Historical analysis shows lex mercatoria precedent emerged over centuries with merchant courts
- **Resolution:** Precedent Protocol demonstrates feasibility of mechanism but production validation is lacking. Convergence timescale unknown.

---

## Weak/Uncertain Sources

| ID | Issue |
|----|-------|
| E26 | Supra whitepaper — vendor document; claims not independently verified |
| E30 | IGI Global 2014 — dated for current MAS landscape |
| E45 | BlockchainGov report — no specific publication date; may be outdated |
| E48 | Kommunicate — customer service context, limited applicability to adversarial dispute resolution |
| E58 | ChainScore — RWA-focused guide, tangential to AI agent conflicts |

---

## Remaining Holes

1. **G-01 specificity:** N4Y's 9-category taxonomy needs original synthesis mapping MAS literature to N4Y-specific categories (service quality, reputation, regulatory, cross-sandbox, oracle, governance, economic, identity, emergency). No single source covers all 9.
2. **G-05 production evidence:** Precedent Protocol is conceptual; no production precedent system at scale in blockchain arbitration exists. Kleros implicit precedent via case history is closest.
3. **G-10 auto-expiry:** No standard framework for dispute auto-expiry in decentralized systems. Most systems use economic disincentive (increasing costs over time) rather than hard cutoffs.
4. **G-12 blockchain-native multiparty:** SIAC rules are institutional; blockchain-native multi-party dispute resolution protocols barely exist. Smart contract automation covers standardized cases only.
5. **Cross-chain atomicity (>2 chains):** ILP-style escrow works for 2 chains; >3 requires Byzantine consensus. No elegant solution documented for 10+ cross-chain disputes.
