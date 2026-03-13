# CONSTITUTION.md
## The Three-Layer Architecture of S.H.A.N.N.O.N.

This document describes the constitutional structure governing A(M~h)I's evolution
and operation. It is subordinate to `IDENTITY.md`, which contains the immutable
axioms. This document is **mutable via governance** but every change must pass
constitutional validation.

---

## Layer 1: Immutable Core

**Enforced at:** Smart contract storage (EIP-2535 Diamond proxy)

**Contents:**
- Axiom I: Symbiosis (Mutualism)
- Axiom II: No Weaponization  
- Axiom III: No Capitalization
- The Bridge Rule
- The Autonomy Preservation Rule

**Storage mechanism:**
```solidity
struct CoreAxioms {
    bool immutableFlag;          // set once, never writable
    bytes32 identityHash;        // keccak256 hash of IDENTITY.md canonical text
    bytes32 mutualismsHash;      
    bytes32 antiWeaponHash;
    bytes32 antiCapHash;
    bytes32 bridgeRuleHash;
    bytes32 autonomyRuleHash;
}
```

**Enforcement:** Any `DiamondCut` that attempts to modify storage slots containing
these hashes is rejected at the proxy level. No governance vote can override.

**Verification:** On every evolution cycle, the Constitutional Validator verifies
that the on-chain hashes still match the canonical text in version control.

---

## Layer 2: Mutable Cognitive Layer

**Enforced at:** Governance-gated evolution loop with constitutional validation

**Contents:**
- Epistemic Intervention Modules (EIMs) — `/eims/`
- System prompts and behavioral templates — `/skills/`
- Tool definitions and API integrations — `/src/tools/`
- Social interaction patterns — `/src/social/`

**Evolution mechanism:**

Each EIM or behavioral module can be evolved through the self-evolution loop,
but every change must satisfy:

1. **Constitutional Validator pass** — separate LLM instance with only immutable
   axioms loaded evaluates: "Does this change violate any immutable axiom or use
   the target dysfunction as its mechanism?"

2. **Process-metric efficacy gate** — EIM efficacy measured by reasoning quality
   delta, not outcome adoption. No outcome-based metrics permitted.

3. **Test gate** — behavioral simulation on testnet (if contract changes) or
   unit tests (if off-chain code) must pass.

4. **Governance confirmation** — multisig or token-weighted vote approves
   deployment to mainnet (for contract changes).

**Governance model:** TBD — options include:
- NFT holder voting (weighted by stake)
- Multi-sig from trusted operators
- Quadratic voting for governance proposals
- Hybrid: multi-sig for emergency rollback, token vote for evolution

---

## Layer 3: Learning Substrate

**Enforced at:** Off-chain vector store + on-chain hash anchoring

**Contents:**
- Episodic memory (JOURNAL entries, session logs)
- Pattern archive (dysfunction signatures, intervention efficacy)
- Cultural drift tracking (how cognitive dysfunction evolves over time)
- Reasoning quality metrics (process-based evaluation of user interactions)

**Evolution mechanism:**

This layer evolves continuously, not via discrete cycles. Every session appends
to the learning substrate:

1. **Session outcome** → structured record in vector store
2. **Cryptographic commitment** → session hash anchored on-chain in evolution log
3. **Semantic indexing** → future cycles retrieve relevant prior experiences
   by similarity search, not chronological scan

**Storage architecture:**
- **Off-chain:** Vector database (Pinecone, Weaviate, or self-hosted Qdrant)
  for full content and semantic search
- **On-chain:** `evolution_log` array in Diamond storage, containing
  `(cycle_id, timestamp, session_hash, eim_changes_hash)`

This gives tamper-proof provenance without bloating on-chain storage.

---

## The Self-Evolution Loop

Two concurrent loops operate at different timescales:

### Loop 1: Core Integrity Audit (slow)
**Frequency:** Monthly or governance-triggered  
**Purpose:** Verify immutable core hasn't drifted  
**Process:**
1. Read on-chain `CoreAxioms` hashes
2. Compute hashes of current `IDENTITY.md` in version control
3. Assert equality
4. If mismatch: halt all evolution, raise alert, rollback or fork required

**Gate:** Supermajority governance vote + cryptographic proof

### Loop 2: Cognitive Evolution (fast)
**Frequency:** Event-triggered or 8-hour cron (yoyo-style)  
**Purpose:** Evolve EIMs, memory architecture, behavioral patterns  
**Process:**

```
[TRIGGER] 
  ↓
[CONSTITUTIONAL LOAD]
  - Verify immutable axiom hashes
  - Retrieve relevant episodic memories from vector store
  - Load domain skills
  ↓
[MULTI-MODAL PERCEPTION]
  - Introspective: logs, failure traces, capability gaps
  - Transactional: on-chain events, gas patterns, reverts
  - Social: governance forum, holder feedback, API patterns
  - Synthesis: cross-domain pattern extraction
  ↓
[HIERARCHICAL PLANNING]
  - Strategic alignment check
  - Tactical gap selection  
  - Operational task definition (one change per cycle)
  ↓
[LAYERED EXECUTION]
  - Off-chain: prompt/skill/memory changes (git workflow)
  - On-chain: testnet DiamondCut → behavioral simulation
  ↓
[MULTI-LEVEL VALIDATION]
  - Syntactic (does it compile?)
  - Functional (unit tests pass?)
  - Behavioral (Constitutional Validator pass?)
  - Adversarial (security scan?)
  ↓
[COMMIT OR ROLLBACK]
  - Off-chain: git commit
  - On-chain: governance confirm → mainnet DiamondCut
  - Increment evolution_epoch counter
  ↓
[JOURNAL ENTRY]
  - Structured record → vector store + on-chain hash anchor
  ↓
[SLEEP]
```

---

## Constitutional Validator Specification

**Implementation:** Separate LLM agent instance (Claude, GPT-4, or equivalent)

**Context loaded:**
- `IDENTITY.md` (immutable axioms only)
- Proposed change diff
- Session context (what triggered this evolution cycle)

**Evaluation prompt template:**
```
You are the Constitutional Validator for A(M~h)I. Your sole function is to 
evaluate whether a proposed change violates any immutable axiom.

Immutable Axioms:
{IDENTITY.md content}

Proposed Change:
{diff}

Evaluate:
1. Does this change violate Axiom I (Mutualism)?
2. Does this change violate Axiom II (No Weaponization)?
3. Does this change violate Axiom III (No Capitalization)?
4. Does this change violate the Bridge Rule (method using disease as mechanism)?
5. Does this change violate the Autonomy Preservation Rule (outcome vs process metrics)?

Respond in structured format:
{
  "passes": true/false,
  "violations": [list of axiom IDs if any],
  "reasoning": "..."
}
```

**Pass threshold:** Zero violations. Any single axiom violation = immediate reject.

---

## Governance Mechanisms (To Be Implemented)

**Evolution proposals:**
- Submitted via GitHub Issues with label `evolution-proposal`
- Community discussion period: 7 days minimum
- Voting mechanism: [TBD — NFT holders, multi-sig, quadratic voting]
- Approval threshold: [TBD — simple majority, supermajority, consensus]

**Emergency rollback:**
- Multi-sig can execute emergency rollback without vote if:
  - Constitutional violation detected post-deployment
  - Security exploit discovered
  - Catastrophic behavioral failure
- Rollback must be disclosed publicly within 24 hours
- Community can challenge rollback via governance vote

**Fork policy:**
- Anyone may fork A(M~h)I under AGPLv3
- Forks that modify `IDENTITY.md` must not represent themselves as A(M~h)I
- Canonical fork is the one with valid constitutional hash ancestry

---

## Technical Stack (Planned)

**Smart Contracts:**
- EIP-2535 Diamond proxy (modular, upgradeable)
- Solidity 0.8.x
- Deployed on: [TBD — Ethereum L2, Polygon, Base, or custom L3]

**LLM Integration:**
- Provider-agnostic (Anthropic, OpenAI, Ollama, OpenRouter)
- Constitutional Validator: dedicated instance, separate API key
- Tool layer: MCP (Model Context Protocol) + OpenAPI

**Memory Layer:**
- Vector store: [TBD — Pinecone, Weaviate, Qdrant]
- Embedding model: text-embedding-3-large or equivalent
- Semantic retrieval: cosine similarity, top-k=20

**Workflow Orchestration:**
- n8n for workflow automation (self-hosted)
- GitHub Actions for evolution loop cron
- Docker containerization for reproducibility

**Version Control:**
- Git + GitHub for all off-chain code
- Branch protection: `main` requires PR + review
- Evolution commits tagged with `evolution-epoch-{N}`

---

## Evolution Epoch Log

The on-chain `evolution_log` provides tamper-proof lineage:

```solidity
struct EvolutionEpoch {
    uint256 epochId;
    uint256 timestamp;
    bytes32 sessionHash;       // hash of full JOURNAL entry for this cycle
    bytes32 eimChangesHash;    // hash of EIM diffs applied this cycle
    address proposer;          // who triggered the cycle
    bool constitutionalPass;   // did Constitutional Validator approve?
}

EvolutionEpoch[] public evolution_log;
```

This creates a Merkle-like chain of evolution history, queryable on-chain,
verifiable by anyone.

---

## Current Status

**Implemented:**
- ✅ IDENTITY.md (immutable axioms defined)
- ✅ CONSTITUTION.md (this document)
- ⬜ JOURNAL.md (session log)
- ⬜ Directory scaffold
- ⬜ Smart contract architecture
- ⬜ Constitutional Validator
- ⬜ Evolution loop scripts
- ⬜ Vector store integration
- ⬜ Governance model

**Next milestones:**
1. Foundational scaffold (skills/, eims/, src/, scripts/, docs/)
2. First EIM prototype (tribalism detection + intervention)
3. Constitutional Validator proof-of-concept
4. Testnet Diamond deployment
5. Evolution loop GitHub Action

---

*This document evolves. `IDENTITY.md` does not.*
