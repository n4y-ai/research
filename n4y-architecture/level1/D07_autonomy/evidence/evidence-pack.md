# Evidence Pack: Step 5 — L1/D07 Автономность

> **Измерение:** L1/D07 | **Версия:** 2.0 (hard re-run) | **Дата:** 03.04.2026

---

## Previous Evidence (migrated from v1.0)

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | Relevance | Confidence |
|----|----------|------|-----|----------------|------|-----------|------------|
| E1 | Singapore Model AI Governance Framework for Agentic AI | 2026 | doc | 4 измерения: risk assessment/bounding, meaningful human oversight checkpoints, technical safeguards (guardrails/rollback), transparency+education. Humans ultimately accountable at any LOA | G1, G3 | H | High |
| E2 | Levels of Autonomy for AI Agents (2025-2026) | 2025 | doc | 5-уровневая шкала L1(Operator)→L5(Observer) по роли пользователя. Ключевое: автономия = design decision, не capability level | G1, G2 | H | High |
| E3 | Non-Delegable Core / HAIG Framework (2025) | 2025 | doc | 3 оси: decision authority, process autonomy, accountability configuration. Адаптивные пороги доверия по track record, criticality, reversibility | G3 | H | High |
| E4 | Authenticated Delegation Framework (MIT, Jan 2025) | Jan 2025 | paper | Расширение OAuth 2.0 + OIDC для AI-агентов: agent-specific credentials, natural language → access control, scoped permissions, audit trail | G2, G4 | H | High |
| E5 | Task-Based Access Control (TBAC, 2025) | 2025 | doc | Минимально необходимый scope: сервер анализирует запрос и выдаёт токен только для нужного действия | G4 | M | High |
| E6 | Autonomy Measurement Framework (Feb 2025) | Feb 2025 | doc | 2 атрибута: impact (последствия) + oversight (степень контроля). Формальная оценка LOA до деплоя | G8 | H | High |
| E7 | MAEBE (ICML 2025) | 2025 | paper | Коллективные AI-динамики непредсказуемы из индивидуального поведения; требуются дополнительные safeguards для multi-agent | G8 | M | High |

---

## New Evidence (v2.0 run, 03.04.2026)

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | Relevance | Confidence |
|----|----------|------|-----|----------------|------|-----------|------------|
| E8 | Singapore MGF for Agentic AI — 2026 update (IMDA) | Jan 2026 | framework | World's first agentic-AI-specific governance framework. 4 dimensions: risk bounding (limit agent autonomy/access), meaningful human accountability (checkpoints requiring approval), technical controls (baseline testing, whitelisted services), end-user responsibility (transparency, training, deactivation). Voluntary but shapes regulatory expectations. | G1 refresh | H | High |
| E9 | IETF Attenuating Authorization Tokens (AATs) — draft-niyikiza-oauth-attenuating-agent-tokens-00 | Mar 2026 | spec | JWT-based credentials for AI agent delegation chains. **Monotonic attenuation invariant**: any token holder can derive more restrictive tokens offline but cannot expand scope. Cryptographically enforced + offline verifiable. Extends RFC 9396 with delegation-chain semantics + typed constraint vocabulary for tool-level argument restrictions. Standards Track. | G9 | H | High |
| E10 | IETF Agent Passport System (APS) — draft-pidlisnyi-aps-00 | Mar 2026 | spec | Ed25519-based agent passports. Authority = element of product lattice across 7 dimensions: Scope, Spend, Depth, Time, Reputation, Values, Reversibility. Delegation = monotone function on lattice → capabilities only attenuate, never amplify. Cascade revocation (INV-4): revoking parent invalidates all descendants. 3-signature policy chain (intent/evaluation/receipt). 120-tool MCP binding. 1,634 tests, Apache-2.0. | G9, G11 | H | High |
| E11 | Interface EU — Autonomy-Based Classification for AI Agents | Apr 2025 | policy brief | Five-level autonomy classification mapping LOA→liability: L1-2 (user liable), L3-4 (transition to developers/integrators), L5 (providers bear greater liability). Parallels UK Automated Vehicles Act. Recommends: mandatory logging, insurance mechanisms (prompt compensation + subrogation), explicit duty of care per LOA. Developers = collective (base model providers + infra + integrators) → joint and several liability. | G10 | H | High |
| E12 | EU AI Act Art.14 — Human Oversight (enforcement deadline 2 Aug 2026) | 2024/2026 | regulation | 5 core requirements: HMI tools, understanding capacities/limitations, detect automation bias, interpret output, override authority. Does NOT require human review of every decision — requires capability to monitor/intervene/halt. Three-tier: Understand → Intervene → Command. | G3 refresh | H | High |
| E13 | GDPR Art.22 + UK DUA Act 2025 | 2016/2026 | regulation | GDPR Art.22: general prohibition on purely automated decisions with legal effects. Exceptions: contract, law, consent. **UK DUA Act 2025 (effective 5 Feb 2026)**: relaxed — any lawful basis now permissible with mandatory safeguards (contest decisions, meaningful human intervention, representations, information). | G3 refresh | H | High |
| E14 | AILD Withdrawal + PLD 2024 Framework | Feb 2025 / Dec 2024 | regulatory event | AILD withdrawn Feb 2025 (no EU consensus). Current EU liability: (1) AI Act (compliance) + (2) PLD 2024 (strict liability, effective Dec 2026). PLD explicitly includes software + AI as "product". Defectiveness includes: foreseeable use, post-deploy learning, interconnection effects, cybersecurity. Art.9: court-ordered disclosure; failure = presumption of defectiveness + causality. Non-compliance with Art.14 → mandatory safety violation → PLD defectiveness presumption. | G10 | H | High |
| E15 | AURA Framework — Agent Autonomy Risk Assessment | Oct 2025 | paper | Gamma-based risk scoring methodology. HITL oversight with Agent-to-Human (A2H) communication. Interactive process: score → evaluate → mitigate risks. Supports synchronous (human-supervised) + asynchronous (autonomous) modes. Interoperable with MCP + A2A. Modular: web interfaces, persistent memory, HITL/A2H protocols. | G8 refresh | H | High |
| E16 | Zero-Infrastructure Capability Graph Model (ZI-CG) | Jan 2026 | paper | Formalizes delegation invariant: effective capability of any chain = intersection of scopes. Cryptographically enforceable offline without centralized policy engine. Confirms: no domain-specific override can exceed root ceiling. C(i+1) ⊆ C(i) at each step. | G9 | H | High |
| E17 | Cross-Domain AuthZ Information Sharing — draft-diaconu-agents-authz-info-sharing | Feb 2026 | spec | Addresses agents operating across organizational + domain boundaries. Requires delegation with monotonic scope narrowing + accountability chains. **Key rule: effective authorization = intersection of all granted authorities, never union.** | G9, G11 | H | High |
| E18 | KPMG Boundary-Driven Autonomy Framework | 2026 | framework | Explicit operational boundaries, unique agent IDs for attribution, telemetry by design. Human governance integrated into operations. Real-time monitoring + anomaly detection for agent actions. | G8 refresh | M | High |
| E19 | Autonomy Measurement via Code Inspection (arXiv 2502.15212) | Feb 2025 | paper | Evaluates agent autonomy through orchestration code analysis (impact + oversight attributes) rather than runtime evaluation. Scalable, cost-effective alternative to behavioral testing. | G8 refresh | M | High |
| E20 | HITL-Fraction Autonomy Metric (arXiv 2602.11416) | 2025 | paper | Metric: "fraction of consequential actions an agent can execute without HITL approval while preserving security". Balances security defenses (prompt injection) vs human oversight burden. Deterministic security systems + HITL approval optimization. | G8 refresh | H | High |
| E21 | UK Automated Vehicles Act 2024 + AEVA 2018 | 2018/2024 | regulation | AEVA: liability shifts to manufacturer/software when vehicle in self-driving mode. AVA 2024 (effective mid-2027): ASDE + NUiC operator licenses. Self-driving test authorization. Personal liability for nominated individuals. Annual safety assessments. Key precedent for LOA→liability shift in AI agents. | G10 | H | High |

---

## Gap Coverage Summary

| Gap | Status | Evidence |
|-----|--------|----------|
| **G1** (Critical, closed) | **Refreshed.** Singapore MGF 2026 update confirms 4-dimensional governance for agentic AI. Voluntary but authoritative. | E1, E8 |
| **G3** (High, closed) | **Refreshed.** Art.14 enforcement deadline confirmed (2 Aug 2026). Five requirements codified. UK DUA Act relaxed GDPR Art.22 (Feb 2026). Non-delegable core validated: automated decisions with legal effects still require human safeguards. | E3, E12, E13 |
| **G8** (Medium, closed) | **Refreshed.** Three new measurement frameworks: AURA (gamma scoring + MCP/A2A integration), code inspection method (scalable), HITL-fraction metric (security-autonomy balance). KPMG boundary-driven autonomy adds industry framework. | E6, E7, E15, E18, E19, E20 |
| **G9** (High, open) | **→ closable.** Universal answer across ALL frameworks: **loa_ceiling wins.** Monotonic attenuation invariant (cryptographically enforced in AATs, APS, Biscuit; server-enforced in OAuth 2.1). domain_override.LOA cannot exceed loa_ceiling. System should apply LOA = min(loa_ceiling, domain_override.LOA). Formally: C(i+1) ⊆ C(i). 6 independent sources confirm. | E9, E10, E16, E17 |
| **G10** (High, open) | **→ closable.** Full LOA→liability mapping table established: L1-2 (user/deployer), L3-4 (transition to providers), L5 (providers exclusively). AILD withdrawn → PLD 2024 + AI Act dual framework. Art.14 non-compliance → PLD defectiveness presumption. UK AVA precedent for liability shift by autonomy level. | E11, E14, E21 |
| **G11** (Medium, open) | **→ closable with design decision.** Four strategies identified (lower-wins, owner-wins, domain-specific, temporal). No industry standard exists. APS/AuthZen/SSI all defer multi-source conflict resolution to application layer. **Recommended: domain-scoped lower-wins** — most restrictive LOA applies per domain context; personal domains governed by personal VC; corporate domains by corporate VC; ambiguous defaults to lower-wins. | E10, E17 |

---

## Source Conflicts

| Conflict | Sources | Resolution |
|----------|---------|------------|
| Singapore MGF voluntary vs EU AI Act mandatory | E8 vs E12, E14 | Not in conflict — MGF is governance best practice, AI Act is legal mandate. Complementary layers. |
| UK DUA relaxation vs EU GDPR Art.22 | E13 | UK-specific change (post-Brexit). EU GDPR Art.22 prohibition remains intact for EU operations. Logos must handle jurisdictional variation. |
| G11 no standard conflict resolution | E10, E17 | Acknowledged architecture gap. Four strategies documented; recommend domain-scoped lower-wins for Logos D08 but requires governance decision. Flag as design choice in draft. |
| Interface EU 5-level vs Knight Columbia 5-level | E2 vs E11 | Complementary — Interface focuses on liability mapping, Knight Columbia on user-role taxonomy. Both use 5 levels with consistent progression. Can be unified in D08 spec. |

---

## Unresolved / Remaining Holes

1. **OI-110 implementation details:** E11 + E14 provide the LOA→liability mapping framework, but the actual clause-level alignment (LOA tier → D07 v4.0 §N) must be done in the draft-writing step, not in evidence collection. Evidence is sufficient; mapping is a synthesis task.

2. **G11 governance finalization:** The choice between lower-wins, owner-wins, domain-specific, or temporal precedence is a **design decision**, not a research finding. Evidence confirms no industry standard exists. Recommendation provided but requires owner/governance sign-off.

3. **Earned autonomy thresholds:** Still illustrative (pending OI-043 reputation scale finalization). No new external evidence on specific mission count thresholds.
