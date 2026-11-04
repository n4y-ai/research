# Evidence Pack: Step 5 — L1/D01 Идентификационное

> **Измерение:** L1/D01  
> **Версия:** 3.0 (full deep-research pass for G7–G13)  
> **Дата:** 02.04.2026  
> **Источники:** E1–E7 (migrated from v3.0, close G1–G6); **E8–E23 (new, close G7–G13)**

---

## Evidence Table

| ID | Источник | Дата | Тип | Ключевой claim | Gaps | Relevance | Confidence |
|----|----------|------|-----|----------------|------|-----------|------------|
| E1 | W3C DID v1.1 (Working Draft → CR) | Jan–Mar 2026 | norm | DID — URI вида `did:method:specific-id`, разрешаемый в DID Document. Стандарт method-agnostic: 100+ зарегистрированных DID-методов. | G1,G5 | H | High |
| E2 | W3C Verifiable Credentials v2.0 (Recommendation) | May 2025 | norm | Трёхсторонняя модель Issuer → Holder → Verifier. Поддержка selective disclosure (ZKP-compatible). JSON-LD. | G2,G5 | H | High |
| E3 | SingularityNET + Privado ID | Mar 2025 | doc | Первый децентрализованный реестр доверия AI-агентов (AI Agent Trust Registry). On-chain DIDs + VC + anonCred для AI-агентов. | G1 | H | High |
| E4 | Fetch.ai — Almanac Contract | актуальная | doc | Агенты регистрируются через Cosmos-based Almanac contracts. Каждый агент получает уникальный верифицируемый идентификатор. | G1,G4 | H | High |
| E5 | Worldcoin World ID | актуальная | doc | Iris-based биометрия → ZKP-based World ID. Privacy-preserving. | G3 | M | Med |
| E6 | Human Challenge Oracle (HCO) | 2025 | paper | Continuous rate-limited human verification. Time-bound challenges, криптографически привязанные к идентичности. | G3 | M | Med |
| E7 | eIDAS 2.0 — EU 2024/1183 | 2024 | norm | Регуляторная рамка ЕС: электронные кошельки для VC, pseudonymous credentials. (Полное описание → L2/D08) | G5 | M | High |
| **E8** | **cheqd MCP-enabled Agentic Trust Solution** | **Apr 2025** | **doc** | **MCP-powered AI agent identity: agents hold tamper-proof VCs, identities cryptographically provable. MCP server enables agents to read/write DIDs, issue VCs, manage trust registries. Scoped granular agent permissions. Joined DAIAA. ASI Alliance integration (Fetch.ai + SingularityNET).** | **G8** | **H** | **High** |
| **E9** | **cheqd Trust Graph** | **Nov 2025** | **doc** | **Dynamic replacement for static Trust Registries: living interconnected network showing trust flow between people, organizations, and AI agents. Adapts automatically as credentials issued/verified/revoked. Hierarchical, contextual, reputation-based trust assessment.** | **G8** | **H** | **High** |
| **E10** | **TRAIN Validation Engine (Fraunhofer / cheqd)** | **2025** | **doc** | **Trust Registry ANchor INfrastructure: evaluates VC trustworthiness by recursive DID-chain verification to root authority. Policy compliance + anchor validation + credential verification. TDZM (DNS-anchored trust zones). Blends decentralized (DID/VC) AND centralized (DNS/X.509) trust chains.** | **G8** | **H** | **High** |
| **E11** | **cheqd MCP Toolkit** | **Mar 2025** | **code** | **`@cheqd/mcp-toolkit` (TypeScript, Apache 2.0, v1.4.1-develop.2): DID CRUD, VC issuance/verification, DID-linked resources, ZK proof verification, DIDComm connections. `whois` prompt validates agent trust chains via `list-credentials` → `verify-trust-registry` → TRAIN recursive verification. 6 contributors.** | **G8** | **H** | **High** |
| **E12** | **AIP: Agent Identity Protocol** | **Mar 2026** | **paper** | **Invocation-Bound Capability Tokens (IBCTs): fuse identity + attenuated authorization + provenance binding. Compact mode (JWT, 0.049ms Rust verify, 356 bytes) for single-hop MCP. Chained mode (Biscuit + Datalog, 340–380 bytes/block) for multi-hop delegation. 600/600 adversarial attacks rejected. 0.086% overhead with Gemini 2.5 Flash. IETF draft-prakash-aip-00. Reference implementations: Python + Rust.** | **G10, G13** | **H** | **High** |
| **E13** | **Agentic JWT (A-JWT) / Secure Intent Protocol** | **Sep 2025 (paper); Dec 2025 (IETF)** | **paper + norm** | **Extends OAuth 2.0 for non-deterministic LLM agents. Agent checksums (prompt + tools + config), workflow-aware token binding, new OAuth grant type (`agent_checksum`), per-agent proof-of-possession. Blocks scope-violating requests, replay attacks, impersonation, prompt-injection. IETF Internet-Draft draft-goswami-agentic-jwt-00.** | **G10, G13** | **H** | **High** |
| **E14** | **A2A Agent Card Identity Proposals** | **Feb–Mar 2026** | **doc** | **Two proposals in A2A project: (1) Issue #1672: `verifiedIdentity` field with ECDSA P-256 certificates, AgentID (getagentid.dev) reference implementation. (2) Issue #1497: Three-tier verification (self-asserted → domain-verified → organization-verified), mandatory AgentCard signing, delegation chain security, revocation endpoints. DID/JWT auth demo in a2a-js. Current A2A verifies hosting domain via TLS, NOT agent identity.** | **G10** | **H** | **Med** |
| **E15** | **OWASP Top 10 for Agentic Applications 2026** | **Dec 2025** | **norm** | **Agent Identity Abuse = #3 risk (ASI03). 100+ security experts. Threats: fake helper agent impersonation, cached credential reuse, transitive privilege inheritance. Mitigations: unique bounded identity per agent, short-lived credentials, session isolation, re-authorization for privilege escalation, detect transitive privilege inheritance + revoke stale entitlements. Microsoft published Copilot Studio alignment (Mar 2026).** | **G13** | **H** | **High** |
| **E16** | **did:webvh v1.0 — DIF** | **2025 (v1.0 release)** | **norm** | **did:web + Verifiable History. Append-only DID Log (signed + hash-chained entries). Pre-rotation keys (post-quantum safe). Witness networks (Certificate Transparency-style duplicity detection). SCID (Self-Certifying Identifier) enables host-independent portability while preserving full history. Cryptographic agility (versioned spec upgrades, per-entry method params). Three complete implementations: TypeScript, Python, Go; Rust in development. Applicable for audit trails, forced rotation, compliance logging.** | **G7** | **H** | **High** |
| **E17** | **W3C DID Core v1.1 — Candidate Recommendation** | **Mar 5, 2026** | **norm** | **First major update since v1.0 (2022). Updated syntax, refined data model, clearer serialization rules. CR exit criteria: 2 consumers + 1 producer and 2 producers + 1 consumer interoperating. Comment period until Apr 5, 2026. Primarily editorial + clarification — no breaking changes identified. Agent-specific DID use cases underrepresented in spec discussions.** | **G9** | **H** | **High** |
| **E18** | **Dock Labs DID MCP Server** | **Mar 2026** | **doc** | **MCP server for DID operations: issue credentials, manage DIDs, handle presentation requests via natural language. Least-privilege by design — agents get scoped functions only. Demonstrates MCP as integration pattern for DID infrastructure. Compatible with DID v1.1.** | **G9** | **M** | **Med** |
| **E19** | **ERC-7093: Social Recovery Interface** | **2024+** | **norm** | **Flexible social recovery decoupled from identity verification. Guardian types: individuals (no blockchain needed), NFT/SBT holders, WebAuthn/Passkeys, DKIM signatures, OpenID tokens, ZKP. Customizable recovery policies without smart contract upgrades. Eliminates single points of failure.** | **G11** | **H** | **High** |
| **E20** | **DID-KR: DID Key Recovery Extension** | **Mar 2026 (Editor's Draft)** | **norm** | **Three complementary recovery types: (A) Social ZKP — Feldman VSS + Schnorr proofs + proactive share refreshment; (B) Deterministic Seedling — HD seeds + VDF time-locks for inheritance/dead-man-switch; (C) MPC-Mediated — fROST threshold signatures + epoch-based state sync. Guardian privacy via salted hashes + pseudonymous IDs. Reference impl: sirraya-did-kr v2.1.2, 100% test pass, share gen 0.33ms, ZKP verify 0.26ms.** | **G11** | **H** | **High** |
| **E21** | **W3C Data Integrity BBS Cryptosuites v1.0** | **Apr 2025 (CR Draft)** | **norm** | **BBS+ signatures for VCs: selective disclosure (disclose specific claims only) + unlinkable derived proofs (proofs cannot be linked to original signature → no cross-verifier tracking). Pairing-based crypto with BLS12-381. Requires 2+ independent implementations for CR advancement.** | **G12** | **H** | **High** |
| **E22** | **Benchmarking Selective Disclosure for VCs (University of Bologna)** | **2025** | **paper** | **SD-JWT (EUDIW standard) does NOT fully meet EU Reg 2024/1183 privacy requirements — linkability remains a concern. Formalized security/privacy properties and threat models. Key privacy objectives: unlinkability, minimality of information, non-duplication prevention. BBS+ achieves unlinkability; SD-JWT does not.** | **G12** | **H** | **High** |
| **E23** | **ERC-8004: Trustless Agents (Ethereum mainnet)** | **Jan 2026** | **norm** | **On-chain agent identity registry (ERC-721), reputation registry (0-100), validation registry (stake/zkML/TEE). 24K+ agents first week. MetaMask/Google/Coinbase co-authored. Multi-chain: Ethereum, Arbitrum, Celo, BNB.** | *(supplementary)* | **M** | **High** |

---

## Source URLs

| ID | URL |
|----|-----|
| E1 | https://www.w3.org/TR/2026/CR-did-1.1-20260305/ |
| E2 | https://www.w3.org/TR/vc-data-model-2.0/ |
| E3 | https://singularitynet.io/ (+ Privado ID partnership) |
| E4 | https://fetch.ai/docs/references/contracts/uagents-almanac/almanac-overview |
| E5 | https://worldcoin.org/world-id |
| E6 | (academic — 2025 publication on HCO) |
| E7 | https://eur-lex.europa.eu/eli/reg/2024/1183 |
| E8 | https://cheqd.io/blog/pioneering-trust-in-the-age-of-ai-introducing-cheqds-mcp-enabled-agentic-trust-solution/ |
| E9 | https://cheqd.io/blog/why-cheqd-is-changing-from-trust-registry-to-trust-graph/ |
| E10 | https://docs.cheqd.io/product/studio/trust-registries/train/validate |
| E11 | https://github.com/cheqd/mcp-toolkit |
| E12 | https://arxiv.org/abs/2603.24775 ; https://www.ietf.org/archive/id/draft-prakash-aip-00.html |
| E13 | https://arxiv.org/abs/2509.13597 ; https://datatracker.ietf.org/doc/html/draft-goswami-agentic-jwt-00 |
| E14 | https://github.com/a2aproject/A2A/issues/1497 ; https://github.com/a2aproject/A2A/issues/1672 |
| E15 | https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026 |
| E16 | https://identity.foundation/didwebvh/v1.0 ; https://blog.identity.foundation/dif-celebrates-v1-0-release-of-did-webvh/ |
| E17 | https://www.w3.org/TR/2026/CR-did-1.1-20260305/ ; https://github.com/w3c/transitions/issues/774 |
| E18 | https://www.biometricupdate.com/202603/dock-labs-launches-ai-agent-mcp-server-for-digital-identity |
| E19 | https://eips.ethereum.org/EIPS/eip-7093 |
| E20 | https://github.com/sirraya-labs/did-kr ; https://pypi.org/project/sirraya-did-kr/2.1.2/ |
| E21 | https://w3.org/TR/vc-di-bbs |
| E22 | https://cris.unibo.it/handle/11585/1038253 |
| E23 | https://eips.ethereum.org/EIPS/eip-8004 |

---

## Gap Coverage Matrix

| Gap | Закрыт? | Ключевые evidence | Примечание |
|-----|---------|-------------------|------------|
| G7 (Medium) — did:webvh | **closed-by-evidence** | E16 | v1.0 released (upgraded from v0.5). Verifiable history, pre-rotation, witness mechanisms, portability with SCID. Three implementations (TS/Py/Go). Applicable for Logos audit trail: append-only DID Log with hash chains. Complements did:cheqd for forced-rotation and compliance use cases. |
| G8 (High) — cheqd MCP Agentic Trust | **closed-by-evidence** | E8, E9, E10, E11 | Four independent sources: blog (architecture), Trust Graph (dynamic trust), TRAIN (recursive validation to root authority with DNS/X.509 bridge), MCP Toolkit (open-source, TypeScript, DID CRUD + VC + whois trust chain). Directly integrable with N4Y.AI as external attestation layer via MCP. |
| G9 (Medium) — W3C DID v1.1 | **closed-by-evidence** | E17, E18 | CR published Mar 5, 2026. Changes primarily editorial: updated syntax, refined data model, clearer serialization. No breaking changes. Comment period until Apr 5, 2026. did:cheqd compatibility not at risk — no structural incompatibilities identified. Dock Labs MCP server confirms DID v1.1 operability. |
| G10 (High) — Runtime verification | **closed-by-evidence** | E12, E13, E14 | Three complementary protocols: (1) AIP/IBCT for verifiable delegation across MCP and A2A (0.086% overhead, 100% attack rejection), (2) A-JWT for intent-bound agent authorization with PoP, (3) A2A Agent Card proposals for three-tier verification. Concrete handshake: IBCT compact (single-hop JWT) or chained (Biscuit multi-hop) with scope attenuation + provenance. |
| G11 (Medium) — Social recovery | **closed-by-evidence** | E19, E20 | Two complementary frameworks: (1) ERC-7093 for flexible guardian types (WebAuthn, DKIM, ZKP, OpenID — no blockchain required), (2) DID-KR with three recovery types (Social ZKP via Feldman VSS, Seedling Inheritance via VDF time-locks, MPC via fROST thresholds). Production reference implementation with sub-ms performance. Significantly beyond Shamir 3-of-5. |
| G12 (Medium) — Binding VC privacy | **closed-by-evidence** | E21, E22 | BBS+ (W3C CR Draft) provides unlinkable derived proofs — the only mechanism that fully prevents cross-verifier tracking of Binding VC. SD-JWT is linkable (confirmed by Bologna benchmark). Requirements for Logos Binding VC: use BBS+ signatures, derived/proxy DIDs, short-lived binding credentials, per-agent DIDs with separate binding VCs. |
| G13 (High) — Threat model | **closed-by-evidence** | E12, E13, E15 | Three-layer coverage: (1) AIP Section 6 — seven adversary classes with prevention/detection/not-addressed matrix; unique defenses against delegation depth violation + audit evasion. (2) OWASP ASI03 — Agent Identity Abuse #3 risk; mitigations: bounded identity, short-lived creds, session isolation, transitive privilege detection. (3) A-JWT — agent checksums, workflow binding, PoP against replay/impersonation. |

---

## Source Conflicts and Tensions

1. **AIP vs. OAuth 2.1 for MCP auth**: AIP paper (E12) explicitly argues OAuth 2.1 is insufficient for multi-hop delegation (no holder-side attenuation, no delegation chain, no provenance). However, MCP adopted OAuth 2.1 in 2026 and the AIP authors position AIP as *complementary* (OAuth for transport auth, AIP for delegation chain). **Resolution:** Both are needed — OAuth 2.1 for single-hop client→server, AIP/IBCT for multi-agent delegation.

2. **did:webvh vs. did:cheqd**: E16 (did:webvh) provides verifiable history that did:cheqd lacks natively. AIP paper (E12 §2.1) criticizes DID methods for blockchain dependencies and circular trust bootstrapping. **Resolution:** did:webvh is complementary, not competing — optimal for audit trail use cases where full DID Document history is required (e.g., compliance logging). did:cheqd remains primary for persistent agent identity.

3. **SD-JWT vs. BBS+ for Binding VC**: E22 (Bologna benchmark) shows SD-JWT fails unlinkability requirement; E21 (BBS+ W3C spec) provides unlinkability but is still in CR (not yet Recommendation). **Resolution:** BBS+ is the technically correct choice for privacy-preserving Binding VCs, but requires monitoring W3C CR advancement. SD-JWT acceptable as interim with linkability risk acknowledged.

4. **Centralized vs. decentralized Agent Card verification**: E14 shows tension between ECDSA P-256 centralized CA approach (Issue #1672) and DID-based self-sovereign approach (a2a-js PR #31). **Resolution:** Both valid for different deployment contexts; Logos architecture should use DID-based approach consistent with overall design philosophy.

5. **DID-KR maturity**: E20 is Editor's Draft (Mar 2026), not yet a stable specification. Reference implementation exists but limited adoption data. **Confidence note:** Technical design sound; production readiness requires monitoring.
