# Evidence Pack: L2/D09 Governance | v2.0 | 05.04.2026

> Previous evidence: E1–E9 (migrated from v3.0, 01.04.2026).
> New evidence: E10–E28 (deep research pass, 05.04.2026).
> Cursor WebSearch + MCP deep_research (Perplexity sonar-deep-research).

---

## Existing Evidence (E1–E9, unchanged)

| ID | Источник | Дата | Тип | Ключевой claim | Confidence |
|----|----------|------|-----|----------------|------------|
| E1 | Optimism Collective (Season 8, 2025) | Aug 2025 | doc | Bicameral: Token House (token-weighted) + Citizens' House (1p1v); 4 stakeholder groups; optimistic approval with veto | High |
| E2 | Arbitrum DAO Constitution | 2023 | doc | Security Council 12 members (2 cohorts); 9/12 emergency; 7/12 non-emergency; 6-month rotation; max 2 terms | High |
| E3 | Curve Finance veToken model | 2020 | doc | veToken = lock × time; non-transferable; linear decay; 4-year max; gauge voting | High |
| E4 | a16z DAO governance attacks | 2024 | doc | Flash loan ($182M Beanstalk), whale capture (Steem), voter apathy (4-5% Compound), malicious proposals ($3.1M YAM) | High |
| E5 | MetaDAO futarchy (Solana) | 2024-2025 | doc | Robin Hanson futarchy; 20-60 participants; 200+ trades/3h; "IQ sufficient to not burn $220M treasury" | Med |
| E6 | research_L2_security_governance.md | Mar 2026 | doc | Phase 8 resolutions: 4-level AIP, Security Council specs, veQi parameters, Citizens' Chamber timing, TAC specs | High |
| E7 | ICANN multistakeholder model | 2024 | doc | Governments + business + civil society + technical; transparency, inclusiveness, consensus | High |
| E8 | Wyoming DAO LLC / Swiss Foundation models | 2021+ | doc | Wyoming DAO LLC: algorithmically managed; Swiss Foundation + DAO: ETH/Solana model | High |

---

## New Evidence (E10–E28)

| ID | Источник | Дата | Тип | Ключевой claim | Gap(s) | URL | Confidence |
|----|----------|------|-----|----------------|--------|-----|------------|
| E10 | World ID 4.0 (Worldcoin/World) | Dec 2025 | protocol doc | ~18M verified users; account-based architecture with multi-key support and one-time-use nullifiers; World Vote Mini App enables 1p1v on Worldchain via Agora; AgentKit (Mar 2026) with x402 for AI agent human-backing. Regulatory risk: Kenya halted ops (Jan 2026). | G9, G1 | https://world.org/blog/engineering/introducing-world-id-4.0 | High |
| E11 | Holonym / Human Passport (ex-Gitcoin Passport) | Feb 2025 | industry report | Holonym acquired Gitcoin Passport for $10M; rebranded as Human Passport. Credential aggregation: GitHub, ENS, POAPs, social, phone, govt ID, anonymous biometrics. PoP reduced attacker influence >80% in grant rounds. 120+ projects, $512M secured (Mar 2026). | G9 | https://www.biometricupdate.com/202502/holonym-acquires-gitcoin-passport-in-proof-of-personhood-expansion | High |
| E12 | MACI v3.0 roadmap (PSE/Ethereum Foundation) | 2025 | protocol doc | ZK-SNARK encrypted voting; receipt-free (makes bribery economically irrational); key-change mechanism invalidates coerced votes. v3.0: custom voice credits, custom gatekeeping per poll, hash-chains, offchain relay. 2026 target: 100K concurrent voters. GG24 production deployment. Gatekeepers: Semaphore, Zupass, Gitcoin Passport, Hats. | G9, G8 | https://maci.pse.dev/roadmap | High |
| E13 | Cryptographic PoP Framework (ePrint 2026/333) | 2026 | academic | Two credential types: Personhood Credentials (PHCs, authority-issued) + Verifiable Relationship Credentials (VRCs, peer-to-peer reputation). ZKP-based verification. | G9 | https://eprint.iacr.org/2026/333 | Med |
| E14 | Humanity Protocol (PHC + VRC on Hedera) | 2025 | protocol doc | Non-invasive palm recognition + ZKP + Hyperledger AnonCreds on Hedera. Self-sovereign: biometric data stays local. Dual credentials: PHC (personhood) + VRC (verifiable relationships). | G9 | https://www.lfdecentralizedtrust.org/blog/proof-of-personhood-in-action-the-first-person-credentials-demo-on-hedera | Med |
| E15 | SBT Governance Framework (DeSpread Research) | 2024 | academic | SBTs = non-transferable on-chain records; enable 1p1v + expertise-weighted governance. Voting rights = f(participation score, contribution quality). Require PoP binding for Sybil resistance. | G9, G12 | https://research.despread.io/sbt-series-last/ | Med |
| E16 | Optimism Season 8: 4-Stakeholder Citizenship | Aug 2025 | governance doc | Citizens' House subdivided: end-users (PoP + 3/6 months Superchain activity, up to 1,000), apps (gas usage, 100+), chains (revenue, 15+). Optimistic approvals: auto-pass unless vetoed. DAB: 7 elected developers for expert technical filter. Citizenship "remains an experiment." | G9, G10, G1 | https://www.optimism.io/blog/governance-in-season-8-the-next-phase | High |
| E17 | Optimism Veto & Conflict Resolution Framework | 2025 | governance doc | Both houses have veto over DAB protocol upgrades. 7-day veto window; 4 stakeholder groups can object. Dynamic veto thresholds based on # groups signaling. If veto met → appeals phase. Optimistic approval = bias toward action reduces deadlock. | G10 | https://docs.optimism.io/governance/gov-faq | High |
| E18 | Hierarchical Multisig & Separation of Powers (Optimism) | 2025 | governance doc | 2-of-2 top-level (Foundation + SC); nested: 10-of-13 SC + 5-of-7 Foundation. Cryptographic encoding of separation of powers. Either sub-multisig can veto; both must approve execution. | G10 | https://www.optimism.io/blog/season-9-from-experiment-to-organization | High |
| E19 | Dynamic Voting Power Quorum (Arbitrum DVP) | Feb 2025 | governance proposal | DVP adjusts quorum thresholds dynamically based on actual participation levels, maintaining governance operability as participation fluctuates. Prevents permanent deadlock from declining turnout. | G10, G12 | https://forum.arbitrum.foundation/t/constitutional-dvp-quorum-for-arbitrumdao-implementation-parameters/30484/27 | Med |
| E20 | Arbitrum SC Constitutional Amendment | Sep 2025 | governance proposal | Extends terms to 2 years; annual elections (Sep only); lowers nominee threshold 0.2%→0.1%; adds `rotateMember()` function (18-day timelock); nominee key rotation (3-day notice). 16 candidates in Mar 2026; 19 in Sep 2025. | G11, G3 | https://forum.arbitrum.foundation/t/constitutional-aip-security-council-election-process-improvements/29848 | High |
| E21 | Arbitrum SC Emergency Action (Oct 2025) | Oct 2025 | incident report | Stylus bug on Sepolia (block 204060366): native vs virtual stack depth → gas consumption error → chain divergence risk. War room at 12:45; 11/12 members joined; payload prepared by contributors for independent SC verification; onchain fix at 2:30 PM; 2h total response. No funds at risk. | G11, G3 | https://forum.arbitrum.foundation/t/security-council-emergency-action-10-13-2025/30093 | High |
| E22 | OpenZeppelin SC Best Practices Guide | 2025 | industry guide | Membership: technical security + crisis management + legal/compliance. Geographic distribution of signers. Safe-of-safes model. Tiered thresholds: pause = simple majority, parameter changes = supermajority. Regular key rotation (quarterly/biannual). Pause guardian for rapid halt. | G11 | https://www.openzeppelin.com/news/security-council-best-practices-guide | High |
| E23 | DAO Voter Apathy: Comprehensive Solutions (DAOtimes) | 2025 | industry analysis | 17% global turnout; top 20% hold 78% voting power. L2 migration → 8-15% gain. Snapshot: 12,500+ DAOs. Delegation (Compound, Uniswap, ENS, Optimism). Quadratic voting: 100+ DAOs, 30% adoption growth. SBTs: 200+ DAOs. Gamification (Aavegotchi XP → 23% gain). Financial incentives: +12% avg. Combined: 12-30% improvement. | G12 | https://daotimes.com/a-list-of-possible-solutions-to-dao-voter-apathy | High |
| E24 | ArbitrumDAO Apr 2025: 59.83% Participation | Apr 2025 | governance analytics | 59.83% onchain, 53.78% offchain. Factors: SC election focal points, standardized voting schedules, Foundation infrastructure support, revenue diversification (Timeboost), proposal quality. Delegated voting power +12.5% YoY; participation per proposal +28% YoY. | G12, G3 | https://forum.arbitrum.foundation/t/arbitrum-governance-analytics-april-report/29205 | High |
| E25 | ENS Delegation Incentives Pilot | Oct 2025 | governance proposal | 90K ENS / 90 days. 10% to active delegates (>7/10 votes), 90% to delegators. Per-delegate cap 1%; per-delegator cap 5%; time-held factor (180d cap) for Sybil resistance. | G12 | https://discuss.ens.domains/t/rfc-delegation-increase-incentives-system/21546 | Med |
| E26 | Optimism Anti-Capture Commission | 2025 | governance doc | 26 members (top 100 delegates, >70% participation); 10M OP delegated; Citizens' House veto hotline. Internal threshold: 10-of-26 Snapshot → 1.6M OP can swing. 1B OP unlocking next 2 yrs. Proposed futures: lighter group / on-chain kill-switch / sunset. S7 retro rewards: delegates 6,000 OP; citizens 250-2,000 OP; ACC lead 5,000 OP. | G12, G8 | https://gov.optimism.io/t/the-future-of-the-anticapture-commission/9934 | High |
| E27 | Moonwell Governance Attack (Mar 2026) | Mar 2026 | incident report | $1,808 → 40.17M MFAM → proposal MIP-R39 (admin transfer: 7 markets + comptroller + oracle) → $1.08M at risk (600:1 leverage). Thwarted: 66.7% votes against + Break Glass Guardian (2-of-3 multisig veto). Root cause: MFAM price collapse but governance quorum params unchanged. | G8 | https://crypto.news/moonwell-hit-by-governance-attack-1-08m-at-risk-for-1800-spend/ | High |
| E28 | Drift Protocol Social Engineering Attack (Apr 2026) | Apr 2026 | incident report | $285M loss on Solana DEX. Durable nonce social engineering: pre-signed transactions + fabricated CarbonVote Token (seeded liquidity, wash trading) → oracle manipulation. "Critical vulnerability was not smart contract bug but social engineering of multisig signers + zero-timelock SC migration." North Korean actors suspected (TRM Labs). | G8 | https://thehackernews.com/2026/04/drift-loses-285-million-in-durable.html | High |

---

## Source Conflict Notes

| Conflict | Details | Resolution |
|----------|---------|------------|
| PoP approach: centralized biometric (World ID) vs decentralized credential aggregation (Human Passport) vs social graph (BrightID) | Each has distinct attack surfaces: biometric spoofing vs credential farming vs social infiltration. Kenya regulatory action against World ID. | No single PoP is universal; hybrid approach recommended. All three have production validation. |
| Arbitrum SC election cadence: 6-month (current) vs annual (proposed amendment) | Constitutional amendment (Sep 2025) proposes annual Sep elections + 2-year terms. Mar 2026 election still ran under old rules pending onchain ratification. | Amendment status: temperature check passed; onchain vote pending. Both models documented. |
| Optimism Season 8 citizenship: scalable vs experimental | Official blog calls citizenship "an experiment"; CoinCentral reports 1,000 end-user cap. | Treat as production experiment with caps. Future scalability uncertain. |
| Governance attack defense: timelocks sufficient? | Moonwell had 24h timelock but attacker could still queue. Drift had zero-timelock SC migration. | Timelocks necessary but insufficient alone; requires multisig veto (Break Glass), behavioral screening, and social engineering training. |

---

## Gap Coverage Matrix

| Gap | Status Pre-Research | Evidence Added | Status Post-Research | Remaining Holes |
|-----|-------------------|----------------|---------------------|-----------------|
| G9 (Sybil/PoP) | open | E10, E11, E12, E13, E14, E15, E16 | closeable | N4Y-specific PoP stack selection (World ID vs Human Passport vs hybrid); MACI integration depth for Qi/Citizens' Chambers |
| G10 (Inter-chamber conflict) | open | E16, E17, E18, E19 | closeable | No production DAO has tested repeated bilateral veto deadlock; conference-committee mechanism untested on-chain |
| G11 (SC selection/rotation/removal) | open | E20, E21, E22 | closeable | N4Y-specific SC size/threshold calibration; geographic diversity enforcement mechanism |
| G12 (Participation/apathy) | open | E15, E19, E23, E24, E25, E26 | closeable | Conviction voting parameterization for N4Y; long-term sustainability of delegate compensation |
| G1 (Bicameral DAO) refresh | closed-by-evidence | E10, E16 | refreshed (2026) | RetroPGF 4-5 detailed mechanics not available |
| G3 (Security Council) refresh | closed-by-evidence | E20, E21, E24 | refreshed (2026) | Constitutional amendment final onchain vote outcome pending |
| G8 (Governance attacks) refresh | closed-by-evidence | E12, E26, E27, E28 | refreshed (2026) | AI-driven defense mechanisms still nascent; DAO-AI research pre-production |
