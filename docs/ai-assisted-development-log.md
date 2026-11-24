# Lumen — AI-Assisted Development Log

Companion: [Architecture](architecture.md) · [Requirements traceability](requirements-traceability.md)

The shared-layer ways of working require a short weekly record of delegated AI work, suggestions rejected and why, and at least one mistake caught. Record actual activity and evidence; do not imply a recommendation was implemented, reviewed or deployed before it was. Update this file weekly and link relevant PRs, tests or design-review notes.

## Week 1 — Design and architecture

**Draft entry for owner review (based on the architecture drafting conversation):**

- Delegated: initial architecture proposal from the Lumen, Folio, Sentinel and shared-layer specifications; Mermaid diagram corrections; requirements traceability and technical design refinement.
- Rejected: replacing native Mermaid with SVG assets. The owner wanted editable Mermaid in the Markdown itself, with existing diagram content preserved.
- Mistake caught: the colored ER diagram used trailing semicolons on `classDef` and `class` directives, which broke parsing. The owner reported the failure; the directives were corrected without changing entity relationships. Earlier flowcharts also rendered differently across IDEs and GitHub, demonstrating that parsing success alone does not establish visual compatibility.
- Verification: see the delivered diagram-validation report; GitHub visual review of the new version is pending. Design remains Proposed until mentor review.
- Owner confirmation / evidence links: _Add actual PR, preview, test and review links here._

## Week 2 — Skeleton deployment

- Delegated work: _Pending._
- Suggestions rejected and why: _Pending._
- Mistake caught and how verified: _Pending._
- PR / test / deployment evidence: _Pending._

## Week 3 — Evaluation baseline

- Delegated work: _Pending._
- Suggestions rejected and why: _Pending._
- Mistake caught and how verified: _Pending._
- PR / test / evaluation evidence: _Pending._

## Week 4 — Improvement and final demo

- Delegated work: _Pending._
- Suggestions rejected and why: _Pending._
- Mistake caught and how verified: _Pending._
- PR / test / evaluation / demo evidence: _Pending._
