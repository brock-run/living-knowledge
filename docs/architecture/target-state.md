# Living Knowledge architecture: target state

**Status:** proposed domain-knowledge architecture. [Current state](current-state.md) is design-only. The [component map](component-map.md) ties each target node to its source and first decision gate.

```mermaid
flowchart LR
  LK02["LK02 Identified source versions and evidence"] --> LK03["LK03 Candidate assertions and conflicts"]
  LK03 --> LK04["LK04 Steward review and effective-time policy"]
  LK04 --> LK05["LK05 Immutable domain-canon snapshot"]
  LK05 --> LK06["LK06 Access and authority filters before ranking"]
  LK06 --> LK07["LK07 Context bundle, answer, or publication pin"]
  LK02 --> LK08["LK08 Exact version-level dependency ledger"]
  LK04 --> LK08
  LK07 --> LK08
  LK08 --> LK09["LK09 Rebuildable indexes and lineage projections"]
  LK08 --> LK10["LK10 Explained ripple review"]
```

PKM Canon can supply source-faithful versions, but Living Knowledge alone decides which domain assertions are approved. Candidate research is a labeled reviewer mode. An answer uses a named canon, permitted audience and purpose, access, and effective time before relevance ranking; its pin records exact claims and evidence actually used. A historical pin remains auditable but does not grant a new reader access.

The dependency ledger is the governing version-level record for ripple. SQL views, search indexes, and OpenLineage/PROV-O mappings are projections that can be rebuilt and reconciled after lag. Storage and orchestration choices remain contingent on the bounded pilot and operational spike; no target box implies an accepted implementation decision.
