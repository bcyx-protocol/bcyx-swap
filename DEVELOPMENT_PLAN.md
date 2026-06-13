# BCYX-SWAP Development Plan

## Grant Summary

BCYX-SWAP will validate ephemeral private coordination for BTC to sBTC interoperability as an open-source primitive for the Stacks ecosystem.

The project is scoped as an 8-week Getting Started grant with two equal milestones. Milestone 1 delivers a working testnet/local prototype of the core BCYX primitives. Milestone 2 completes the end-to-end demonstration, batch coordination, benchmarking, public reporting, and external technical feedback loop.

| Field | Value |
| --- | --- |
| Total Grant Amount | $7,000 |
| Duration | Approximately 8 weeks |
| Payment Schedule | 50% / 50% |
| Milestone 1 Payment | $3,500 |
| Milestone 2 Payment | $3,500 |
| Primary Outcome | Validate ephemeral private coordination for BTC to sBTC interoperability |

---

## Protocol Alignment

This development plan adapts the BCYX protocol documentation to a BTC and Stacks sBTC proof-of-concept.

BCYX is not treated as a bridge, custody system, or permanent balance sheet. It is a short-lived coordination layer that receives private swap intent, commits it, matches it, proves correctness, and then relies on Bitcoin and Stacks as the correspondent settlement domains.

The implementation should preserve the core BCYX design principles:

* Public verification, private coordination
* Ephemeral coordination state
* Correspondent-chain settlement finality
* Commitment-based swap intents
* Shielded coordination pools
* Nullifier-based replay protection
* Accumulator-based batching
* Zero-knowledge proof verification
* Selective disclosure by default
* Safe timeout and refund behavior

Reference materials:

* `bcyx-protocol/docs/README.md`
* `bcyx-protocol/docs/architecture.md`
* `bcyx-protocol/docs/swapping-mechanism.md`
* `bcyx-protocol/docs/settlement-model.md`
* `bcyx-protocol/docs/accumulators.md`
* `bcyx-protocol/docs/zk-verification.md`
* `bcyx-protocol/docs/selective-disclosure.md`

---

## Target PoC Flow

```txt
User creates BTC to sBTC intent
  -> commitment is generated
  -> intent enters shielded coordination pool
  -> compatible intents are matched or routed
  -> BTC-side settlement condition is observed
  -> sBTC-side settlement condition is prepared
  -> nullifier and accumulator roots are updated
  -> proof validates swap and settlement predicates
  -> verifier accepts or rejects public proof inputs
  -> Bitcoin and Stacks reach compatible final state
  -> BCYX coordination state is finalized or discarded
```

The persistent truth lives on Bitcoin and Stacks. BCYX only keeps the temporary state needed to coordinate, prove, finalize, or recover a swap.

---

## In Scope

* BTC to sBTC swap lifecycle model
* Commitment schema for private swap intents
* Nullifier derivation and spent-state tracking
* Shielded coordination pool prototype
* Intent matching and batch construction
* Mock Bitcoin settlement adapter
* Mock Stacks sBTC settlement adapter
* Atomic coordination and timeout/refund state machine
* Accumulator roots for commitments, nullifiers, proofs, and settlement state
* Local proof verification harness
* Selective disclosure predicates for validity, deadline, and unspent status
* End-to-end demo and technical report

## Out of Scope

* Mainnet funds
* Production sBTC minting or redemption
* Production bridge infrastructure
* Custodial liquidity operation
* Decentralized coordinator consensus
* Generalized cross-chain messaging
* Multi-asset support beyond BTC and sBTC

---

## Work Packages

## 1. Protocol and State Model

Define the canonical BTC to sBTC swap lifecycle and the minimal ephemeral state BCYX needs to track.

### Deliverables

* Swap state machine
* State transition tests
* Timeout and refund rules
* Persistent, ephemeral, and invalidated state definitions
* Technical lifecycle documentation

### Target States

```txt
IntentCreated
Committed
Queued
Matched
BitcoinLocked
BatchPrepared
ProofGenerated
Verified
StacksSettlementPrepared
Completed
Refunded
Expired
Rejected
```

## 2. Commitments, Nullifiers, and Selective Disclosure

Implement the privacy primitives that allow the coordinator to prove a swap without exposing the full execution graph.

### Deliverables

* Commitment schema and serializer
* Commitment preimage model
* Nullifier derivation
* Nullifier registry
* Selective disclosure predicate interface
* Tests for duplicate use, invalid preimages, expired intents, and predicate failure

### Commitment Inputs

```txt
swap_id
asset_in = BTC
asset_out = sBTC
btc_amount
sbtc_amount
bitcoin_lock_reference
stacks_recipient_commitment
expiry_height
disclosure_policy
nonce
```

### Public Proof Inputs

```txt
commitment_root
nullifier
batch_root
settlement_root
bitcoin_settlement_reference
stacks_settlement_reference
protocol_parameters
```

### Private Witness Inputs

```txt
commitment_preimage
swap_secret
amounts
recipient_data
matching_metadata
route_metadata
nonce
```

## 3. Shielded Coordination Pool and Matching

Build the local/test coordination node that accepts private commitments and matches compatible intents.

### Deliverables

* Intent submission API or CLI
* Shielded pool storage model
* Matching rules
* Batch membership queue
* Deterministic matching tests
* Coordinator logs suitable for a public demo without leaking private metadata

## 4. Bitcoin and Stacks Settlement Adapters

Model correspondent-chain settlement without claiming production custody or production sBTC integration.

### Deliverables

* Mock Bitcoin lock event adapter
* Mock Bitcoin confirmation-depth policy
* Mock Stacks sBTC settlement event adapter
* Settlement receipt model
* Timeout/refund adapter behavior
* Tests for settled, expired, rejected, and replayed settlement references

## 5. Accumulators and Batch Verification

Compress private coordination state into auditable roots.

### Deliverables

* Commitment accumulator
* Nullifier accumulator
* Proof accumulator
* Settlement accumulator
* Inclusion proof format
* Batch root generation
* Batch verification tests

## 6. Proof Verification Harness

Build a local proof-verification prototype that validates BCYX-SWAP predicates before settlement finalization.

### Deliverables

* Proof input specification
* Constraint or predicate model
* Local prover/verifier harness
* Single-swap verification
* Invalid proof rejection
* Batch proof verification
* Documentation of proof assumptions and limitations

## 7. Demo, Benchmarks, and Reporting

Publish artifacts that make the PoC externally reviewable.

### Deliverables

* Public demo video
* End-to-end walkthrough
* Benchmarks
* Technical report
* Updated architecture documentation
* External reviewer feedback summary

---

## Milestone 1: BCYX-SWAP Testnet Prototype

| Field | Value |
| --- | --- |
| Target Date | 4 weeks after grant start |
| Payment Percent | 50% |
| Amount | $3,500 |
| Adoption Metric | N/A |

### Description

Deliver a working BTC to sBTC coordination prototype implementing the core BCYX primitives: commitment-based swap intents, shielded coordination pools, intent matching, nullifier protections, and atomic coordination logic.

The implementation will operate in local/test environments using mocked settlement adapters where necessary. The purpose of this milestone is to prove that private intent can become verifiable coordination state without exposing counterparties, routes, or private execution metadata.

### Week-by-Week Plan

| Week | Focus | Outputs |
| --- | --- | --- |
| Week 1 | Protocol skeleton | State machine, commitment schema, nullifier model, repository structure |
| Week 2 | Coordination node | Intent submission, shielded pool, matching rules, deterministic tests |
| Week 3 | Settlement modeling | Mock Bitcoin adapter, mock Stacks sBTC adapter, timeout/refund logic |
| Week 4 | Prototype integration | Local coordination flow, docs, demo recording, milestone validation |

### Success Criteria

* Public GitHub repository updated with prototype implementation.
* Users can create and submit private BTC to sBTC swap commitments.
* Coordination nodes can match compatible intents inside a shielded pool.
* Nullifiers prevent duplicate execution of the same commitment.
* Atomic coordination flow executes successfully in local/test environments.
* Timeout or refund path is represented and tested.
* Technical documentation describing architecture and swap lifecycle is published.
* Public demo video shows the prototype in operation.

### Required Evidence

* Source code for the coordinator prototype
* Tests for commitments, nullifiers, matching, and state transitions
* Example commands or scripts for running the local prototype
* Architecture and lifecycle documentation
* Demo video link
* Milestone summary in the repository

---

## Milestone 2: End-to-End BCYX-SWAP Demonstration

| Field | Value |
| --- | --- |
| Target Date | 8 weeks after grant start |
| Payment Percent | 50% |
| Amount | $3,500 |
| Adoption Metric | At least 3 external developers, reviewers, or ecosystem contributors provide feedback through GitHub issues, discussions, pull requests, or public technical review |

### Description

Validate the complete BCYX-SWAP proof-of-concept by demonstrating private BTC to sBTC coordination from intent creation through successful coordination completion.

This milestone extends the prototype with accumulator-backed batch coordination, proof verification, benchmark reporting, and a public technical report. The goal is to evaluate whether ephemeral coordination is viable as a privacy-preserving Bitcoin interoperability primitive for the Stacks ecosystem.

### Week-by-Week Plan

| Week | Focus | Outputs |
| --- | --- | --- |
| Week 5 | Accumulators | Commitment, nullifier, proof, and settlement roots |
| Week 6 | Proof verification | Local proof harness, proof input docs, invalid proof rejection |
| Week 7 | End-to-end demo | Full intent-to-completion flow, batch coordination demo, recovery tests |
| Week 8 | Publication | Benchmarks, technical report, finalized docs, external feedback collection |

### Success Criteria

* End-to-end swap coordination demo is completed and publicly available.
* Batch coordination and accumulator mechanisms are demonstrated.
* Proof verification accepts valid public inputs and rejects invalid or replayed state.
* Benchmarks documenting coordination performance and limitations are published.
* Architecture documentation and protocol specifications are finalized for the PoC.
* Public technical report summarizes outcomes, trade-offs, risks, and future directions.
* At least 3 external developers, reviewers, or ecosystem contributors provide feedback through GitHub issues, discussions, pull requests, or public technical review.

### Required Evidence

* End-to-end demo video or reproducible demo script
* Batch coordination example
* Accumulator root output examples
* Proof verification logs or test outputs
* Benchmark report
* Final technical report
* Links to external feedback

---

## Benchmark Plan

The benchmark report should measure practical PoC limits rather than make production claims.

| Metric | Description |
| --- | --- |
| Intent creation latency | Time to create and store a private commitment |
| Matching latency | Time to match compatible intents |
| Batch construction latency | Time to create a batch and update roots |
| Proof generation time | Time for local proof or predicate verification |
| Verification time | Time to verify single and batched proof inputs |
| Batch size impact | Performance at small batch sizes such as 1, 5, 10, and 25 swaps |
| Failure handling latency | Time to detect invalid, expired, or replayed state |
| State footprint | Size of commitment, nullifier, proof, and settlement records |

---

## Testing Strategy

* Unit tests for state transitions
* Unit tests for commitment creation and serialization
* Unit tests for nullifier derivation and duplicate rejection
* Matching tests for deterministic behavior
* Adapter tests for mocked Bitcoin and Stacks settlement events
* Accumulator tests for root updates and inclusion proofs
* Proof tests for valid, invalid, expired, replayed, and mismatched witnesses
* End-to-end tests for happy path, refund path, and invalid proof rejection

---

## Security and Privacy Checklist

* BCYX does not hold long-lived custody of BTC or sBTC.
* Swap coordination state expires after completion, refund, or rejection.
* Commitments do not expose owners, recipients, routes, or private execution metadata.
* Nullifiers prevent replay and duplicate settlement.
* Public proof inputs expose roots, nullifiers, protocol parameters, and settlement references only.
* Private witness data is never logged in demo or test output.
* Completed swaps cannot be refunded.
* Refunded swaps cannot be completed.
* Expired swaps cannot settle after timeout.
* Rejected proofs do not finalize settlement state.
* Settlement references cannot be replayed across batches.
* Batch roots are deterministic and auditable.
* Any mocked settlement assumption is documented clearly.

---

## Documentation Deliverables

* README update describing the BTC to sBTC PoC
* Architecture document
* Swap lifecycle document
* Local setup guide
* Demo walkthrough
* Proof input and accumulator specification
* Benchmark report
* Final technical report

---

## External Feedback Plan

External review is part of Milestone 2 because the primitive is still being validated.

Target reviewer groups:

* Stacks developers
* Bitcoin interoperability developers
* ZK proof engineers
* Wallet or coordination infrastructure builders
* Open-source reviewers interested in privacy-preserving settlement

Acceptable feedback channels:

* GitHub issues
* GitHub discussions
* Pull requests
* Public technical review posts
* Public comments on the demo or report

The final report should summarize what feedback was received, which issues were accepted, and which risks remain unresolved.

---

## Open Questions

* Which proof system should be used for the first non-mock implementation?
* What Bitcoin lock construction should the PoC model first: Taproot script, multisig escrow, or another test-only condition?
* Which Stacks interface should represent sBTC settlement in the mock phase?
* What confirmation depth should be used for local and testnet demonstrations?
* Should the first demo prioritize peer-to-peer matched intents or one-sided liquidity routing?
* Which selective disclosure predicates are essential for the initial Stacks ecosystem review?

---

## Non-Goals Before Mainnet

* No real BTC custody
* No production sBTC minting or redemption
* No mainnet deployment
* No unsupported privacy guarantees beyond demonstrated PoC properties
* No claim of trustless production interoperability before external security review
* No hidden liquidity operator assumption
* No permanent BCYX balance sheet or custody layer

---

## Final Deliverable

At the end of the 8-week grant, BCYX-SWAP should provide a public, reproducible proof-of-concept showing:

```txt
private BTC to sBTC intent
  -> commitment
  -> shielded coordination
  -> matching
  -> nullifier protection
  -> batch accumulation
  -> proof verification
  -> correspondent-chain settlement completion or safe refund
```

The expected outcome is not a production bridge. It is a credible technical validation of ephemeral private coordination as an open-source primitive for BTC to sBTC interoperability on Stacks.
