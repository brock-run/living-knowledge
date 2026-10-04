# Reviewer guidance

Common standards: [Canonworks Reviewer Standards — CW-RS-0.2 draft](canonworks/reviewer-standards.md).
The local document is a generated, byte-identical snapshot of the CanonFlow
master, with its immutable source revision and SHA-256 in
[provenance](canonworks/reviewer-standards-source.json). It is readable offline.
This is a proposal for owner review, not ADR acceptance or a claim about
implemented capabilities. Read the actual base/head, local instructions, current
architecture, code and tests. Another repo's rule is not a local requirement.

## Living Knowledge applicability

This repository currently contains proposed contracts and lineage design, not a
running application. BRO-5 still owns the bounded pilot and accountable owners.

- RS-02/03/08: review source evidence, steward authority, approved knowledge and
  output pins as distinct design concepts. Check contract/architecture consistency
  and preserve the difference between proposed and implemented behavior.
- RS-04: permitted evidence, audience/access, effective time, freshness and
  abstention are pilot requirements to resolve and later verify. Do not describe
  proposed services or authorization as operational, or import another product's
  storage/deployment decisions as local requirements.
- RS-05/07: lifecycle, service and UI tests become applicable when executable
  components implement those paths. Do not request nonexistent application tests
  for a documentation-only change or use their absence to imply verification.
- RS-08: read [current](architecture/current-state.md),
  [target](architecture/target-state.md) and [map](architecture/component-map.md).
  ADR adoption remains governed by the official Notion ledger.

## Verification for the changed scope

- `node scripts/check-reviewer-standards.mjs`: read the versioned policy; verify the generated snapshot against its provenance.

- `node scripts/check-architecture.mjs`: diagram IDs and file links in the three
  files under `docs/architecture/` only; it does not check other docs or anchors.
- For other changed Markdown, resolve relative file links from the containing
  file and confirm each target exists. Inspect heading anchors in the target
  document and verify external references separately. Report this link/content
  review separately from the architecture checker.
- Inspect changed design/contract references, product boundaries and explicit
  implementation status. Cite source and branch/revision for material claims.
- No application test/lint target exists yet. Disclose that scope rather than
  inventing a passing service gate. Define supported executable checks with the
  first implementation slice; keep private domain material out of shared docs.

## Maintaining this reference

Propose common wording changes in CanonFlow's master through a PR and increment
CW-RS. Consumer updates use the committed source and regeneration procedure in
the common document; never hand-edit a generated snapshot. Review the pin and
content changes together. CI verifies local integrity, not upstream freshness
or adoption. Keep local exceptions here with their reason and decision link.
PR descriptions link relevant work and report actual checks, limitations and
feedback on the latest pushed revision.
