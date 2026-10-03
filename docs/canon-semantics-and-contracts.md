# Living Knowledge: canon, snapshots, and impact contracts

**Status:** Proposed architecture (the repository has no application code yet).
**Source:** *Living Domain Knowledge System - Initial Brainstorming*; this
document narrows its ontology, assertion, publication, and OpenLineage ideas
into contracts that can interoperate with PKM Canon and CanonFlow.

## Canon and admission

A **canon** is a governed, versioned selection accepted as authoritative for
a named purpose, scope, context, and time under explicit admission/revision
rules. Living Knowledge owns a **domain knowledge canon**: steward-approved
claims, definitions, relationships, and knowledge-object versions. Approval
establishes organizational authority within a domain and effective interval;
it is not a guarantee of timeless or universal truth.

Keep the brief's three layers distinct:

1. **Source layer:** PKM Canon packages or other identified source versions;
   faithful capture is source-canonical, not domain-approved.
2. **Candidate layer:** extracted concepts/assertions, competing evidence,
   confidence, conflicts, and review tasks; candidates cannot be served as
   approved domain facts without an explicit label/policy.
3. **Approved layer:** steward decision, policy version, effective interval,
   supersession history, and exact supporting evidence. Documentation, search
   indexes, context bundles, and answers derive from this layer.

`canonical` is always qualified: “canonical source element” and “approved
domain-canon assertion” describe different statuses. A source correction,
extraction, or model output must not silently promote a candidate.

## Product binding of portable contracts

Living can implement `CanonStore[Ref, Commit, Artifact]` without inheriting
PKM Canon's `CanonicalPackage` return type. A possible binding is:

```python
CanonStore[KnowledgeVersionRef, ApprovedAssertionCommit, ApprovedAssertionVersion]
```

The commit validates evidence, reviewer authority, context/effective-time
rules, and immutable identity; it returns `ApprovedAssertionVersion`. A
separate adapter may bind the same port to an Iceberg table snapshot. Store
commit and canon admission are distinct: approval must be represented by an
auditable decision and selected into a canon snapshot. Do not force an Iceberg
physical table shape into the shared interface.

| Portable contract | Living binding |
| --- | --- |
| `CanonDescriptor` | Domain, owner/steward, authority policy version, audience, scope |
| `VersionRef` | Assertion/definition/knowledge-object version plus source refs |
| `Snapshot` | Immutable approved selection; record Iceberg snapshot IDs and policy version |
| `PublicationPin` | Documentation or context-bundle ID, immutable `AuthorityContext` (including audience and use purpose), canon snapshot, exact claims/evidence, retrieval results/config, generation and review versions |
| `DependencyEdge` | Source element supports assertion; assertion supports object; object/context bundle supports publication/answer |
| `AuthorityContext` | Principal, audience, domain, use purpose, effective time, classification |

A snapshot closes over the **approved selection**, not every candidate in the
warehouse. It must identify the policy and effective time used to select it.
Iceberg snapshot IDs preserve physical reproducibility; the canon snapshot
manifest preserves semantic selection and approval. Each publication pins both
where applicable. A documentation release points to the exact versions it
used, not merely the latest table or search index. A pin preserves audit
history and the audience for which the answer was produced; it does not
authorize old content for a new reader.

## Retrieval contract

Apply access, domain authority, approved status, audience/use context, and
effective time before semantic ranking. Return exact version/evidence refs,
the canon snapshot ID, and a reason for abstention or exclusion. A source
passage may be retrievable as evidence but must be labeled as a source passage,
not an approved assertion. Candidate retrieval is a separate reviewer mode.

## Provenance, lineage, and ripple

The [dedicated application design](provenance-lineage-ripple.md) specifies the
shared semantics, Living's exact dependency path, OpenLineage mapping, and
implementation checks.

**Provenance** answers where one claim/version came from and who approved it.
**Lineage** records durable directed dependencies through source sync, parsing,
claim extraction, review, index build, context assembly, and publication.
**Ripple** is a reverse query over those dependencies after a change, with
snapshot comparison and policy deciding whether a dependent is stale, needs
review, or remains valid in its original context.

The brainstorming brief's [OpenLineage](https://openlineage.io/docs/spec/object-model/)
plan is a useful projection for runs,
datasets, dataset versions, and index/context-bundle builds. Retain an
internal version-level dependency ledger for exact assertion/evidence/output
links. Emit OpenLineage from durable events or an outbox so a missing lineage
backend does not silently erase audit evidence. Use its run/dataset links to
explain pipeline provenance, while the internal ledger powers precise ripple.

Example: a PKM Canon source node changes; its prior version is linked to three
Living assertions. Reverse traversal identifies those assertions, their
knowledge objects, documentation releases, and pinned context bundles. A
steward sees which evidence was actually used and decides whether the claims
remain valid. No output is automatically rewritten.

## Initial slice

1. Define source, candidate, approved, and superseded statuses with steward
   events and temporal fields.
2. Implement an immutable approved-selection snapshot manifest and a
   documentation publication pin.
3. Record exact source-to-assertion and assertion-to-publication edges.
4. Gate retrieval by authority and context, with a reviewer-only candidate
   path.
5. Add reverse-impact queries and OpenLineage run/dataset emission from the
   same durable events.

The portable vocabulary and store shape are further specified in PKM Canon's
`docs/specs/canon-contracts-v0.1.md`. The two products should share contract
tests once both have working adapters, while keeping separate storage and
admission implementations.
