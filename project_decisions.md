# DocBuilder — Project Decisions Register

**Document type:** Decision register / ADR log  
**Version:** 1.0  
**Status:** Accepted baseline decisions; detailed implementation choices remain open  
**Baseline date:** 2026-11-13  
**Product brief:** `project_details.md`

---

## 1. Purpose and rules

This register records product and architecture decisions that affect DocBuilder's scope or design.

- `Accepted` means the owner has explicitly agreed to the decision.
- `Proposed` means a recommendation is awaiting owner approval.
- `Pending specification` means the direction is known but implementation details remain to be defined.
- `Superseded` means a later decision replaces it; the historical entry must remain visible.

Do not silently change an accepted decision. A change requires a new decision entry that identifies the affected earlier decision, the reason, the impact and the owner's approval.

Detailed implementation choices (such as exact browser versions, library selections, database indexes and formula grammar) may be settled in technical specifications when they do not change an accepted product boundary.

## 2. Accepted product decisions

### DEC-001 — Browser-based PWA delivery

- **Status:** Accepted
- **Date:** 2026-11-13
- **Decision:** DocBuilder will be delivered as an installable Progressive Web App.
- **Rationale:** Browser delivery avoids requiring a native desktop installer while allowing an installable, app-like experience.
- **Consequences:** Initial distribution and application updates require a secure web origin; the service worker caches required assets for subsequent offline use. Browser/OS compatibility must be tested.

### DEC-002 — Local-first, single-user, no backend

- **Status:** Accepted
- **Date:** 2026-11-13
- **Decision:** DocBuilder is a single-user local workspace with no login, backend, server database, cloud sync or multi-user service.
- **Rationale:** The application should work independently on the user's device and avoid server infrastructure.
- **Consequences:** IndexedDB is the primary data store. Data does not automatically synchronize across browsers/devices. Backup and restore are essential. There is no DocBuilder account-based access control.

### DEC-003 — No document-control system

- **Status:** Accepted
- **Date:** 2026-11-13
- **Decision:** DocBuilder provides document authoring and record creation only; it will not enforce document control.
- **Rationale:** The product is a document builder, not a controlled-document management platform.
- **Consequences:** No controlled-copy distribution, obsolete-document enforcement, mandatory approval, authorization workflow, enforced revision lifecycle or approval-trigger automation. Document numbers, statuses, issue/revision details and revision-history tables may still exist as ordinary editable content.

### DEC-004 — Templates, individual documents, records and registers

- **Status:** Accepted
- **Date:** 2026-11-13
- **Decision:** Support reusable templates, blank/individual documents, populated/completed records and repeatable registers.
- **Rationale:** Laboratories need both document design and ongoing creation of structured records.
- **Consequences:** Template, document, record and register models must be distinct but interoperable. Registers are collections of structured records with grid/table and individual detail views.

### DEC-005 — Approval information is content, not workflow

- **Status:** Accepted
- **Date:** 2026-11-13
- **Decision:** Prepared/checked/reviewed/approved names, designations, dates and signatures may be included as ordinary customizable document content. No application-enforced approval workflow is included.
- **Rationale:** Laboratory documents often need these sections even when the authoring application does not manage approvals.
- **Consequences:** Approval/signature blocks are content/layout/input capabilities only. No approval events, approver roles, status transitions, authorization checks or workflow triggers.

### DEC-006 — Built-in and user-created calculations

- **Status:** Accepted
- **Date:** 2026-11-13
- **Decision:** Support both technically specified method-specific calculations and user-created formulas.
- **Formula scope:** User formulas may reference fields/calculations across blocks within the same document. Cross-document and cross-register formula references are not included in the confirmed scope.
- **Rationale:** The application must serve real laboratory worksheets while remaining configurable.
- **Consequences:** A safe expression engine, unit-aware values, dependency resolution, formula provenance, validation and versioning are required. Arbitrary JavaScript execution is prohibited. User-created formulas must not be labelled as verified standard methods.

### DEC-007 — AI excluded

- **Status:** Accepted
- **Date:** 2026-11-13
- **Decision:** No AI functionality or external AI provider integration will be included.
- **Rationale:** AI is not required for the intended core product.
- **Consequences:** `AUTO-018` remains traceable in the source catalogue but is excluded from active product functionality.

### DEC-008 — First-release laboratory disciplines

- **Status:** Accepted
- **Date:** 2026-11-13
- **Decision:** The first release covers QMS/general laboratory documentation, chemical testing, environmental testing, and coal/coke testing.
- **Rationale:** These are the initial target areas.
- **Consequences:** All four areas must have meaningful functional support in release 1. The exact initial method list and technical depth must be specified and agreed before the relevant methods are declared complete.

### DEC-009 — Export and print formats

- **Status:** Accepted
- **Date:** 2026-11-13
- **Decision:** Support PDF, editable DOCX, A4 print output, CSV and Excel export, plus JSON and ZIP backup/restore.
- **Rationale:** Laboratories require printable documents, editable files, structured data exports and complete local backups.
- **Consequences:** Each format needs a defined mapping. Excel export is structured data export; it is not required to reproduce the exact visual page layout of a document.

### DEC-010 — Three levels of block customization

- **Status:** Accepted
- **Date:** 2026-11-13
- **Decision:** Support (1) developer-built native blocks, (2) visual user-created blocks and (3) advanced declarative schema-defined blocks.
- **Rationale:** Users need flexibility beyond developer-provided presets.
- **Consequences:** All block types must use a shared registry/runtime and defined schema contracts. Advanced schemas must not execute arbitrary code.

### DEC-011 — Local shared master-data libraries

- **Status:** Accepted
- **Date:** 2026-11-13
- **Decision:** Provide reusable local master-data libraries.
- **Examples:** Laboratory profile, personnel, equipment, chemicals, reagents, methods/standards, sample types/matrices, units and measurement parameters.
- **Rationale:** Reusing common data reduces repeated entry and inconsistency.
- **Consequences:** Each category needs a defined schema and a live-reference versus historical-snapshot policy. Completed records must preserve the values actually used.

### DEC-012 — JSON and ZIP backup

- **Status:** Accepted
- **Date:** 2026-11-13
- **Decision:** Provide both JSON and ZIP workspace backup/restore.
- **Rationale:** JSON supports portable structured data; ZIP can carry structured data and original attachments without inflating all binary assets into a single text document.
- **Consequences:** Define a versioned JSON envelope, ZIP manifest, attachment paths/checksums, validation, restore safety and compatibility rules.

### DEC-013 — Template update policy

- **Status:** Accepted
- **Date:** 2026-11-13
- **Decision:** Existing documents remain unchanged when a template is edited. Users may explicitly select documents and apply updates after reviewing a comparison.
- **Rationale:** Template changes must not silently overwrite work already in progress or completed documents.
- **Consequences:** Template versions, diff/preview, conflict detection and selective merge rules are required.

### DEC-014 — Completed records are preserved snapshots

- **Status:** Accepted
- **Date:** 2026-11-13
- **Decision:** A completed record is preserved as a snapshot. Subsequent amendments create a separate amended copy/record linked to the original where appropriate.
- **Rationale:** Laboratory records should not be accidentally altered after completion.
- **Consequences:** Define the completion action, snapshot content, amendment relationship, allowed metadata changes, export behaviour and retention/deletion rules. This is not a claim of tamper-proof or legally immutable storage and is not a formal approval workflow.

### DEC-015 — Internal recovery snapshots, not formal document revisions

- **Status:** Accepted
- **Date:** 2026-11-13
- **Decision:** Provide normal autosave/undo/redo and internal recovery snapshots. A manually authored revision-history table may be included in a document. Do not introduce a formal application-managed document revision-control lifecycle.
- **Rationale:** Users need recoverability without creating a document-control system.
- **Consequences:** Recovery snapshots must be distinguished from completed-record snapshots and user-authored revision-history content. Exact retention and recovery UX remain to be specified.

### DEC-016 — No operational laboratory management features

- **Status:** Accepted
- **Date:** 2026-11-13
- **Decision:** Keep the product limited to document creation, templates, records and registers. Do not include operational monitoring/workflow features.
- **Excluded examples:** Sample lifecycle Kanban, analyst task management, calibration due-date alerts, maintenance alerts, notification centre, approval inbox, CAPA workflow and operational QMS dashboards.
- **Rationale:** Prevent scope drift into LIMS, asset management or QMS workflow products.
- **Consequences:** Equipment, sample, chemical and QC registers remain in scope as records, but their operational monitoring, alerts and workflow automation do not.

### DEC-017 — First release provides technical support for all four disciplines

- **Status:** Accepted
- **Date:** 2026-11-13
- **Decision:** Release 1 must include functional technical blocks/calculations for all four initial disciplines, for an explicitly agreed priority set of methods.
- **Rationale:** Each target discipline must receive meaningful support in the first release.
- **Consequences:** Do not claim every possible test method is included. The initial method list must be scoped, technically specified, supplied/reviewed by laboratory experts and verified before release.

### DEC-018 — Prioritized complete templates, not hundreds of empty shells

- **Status:** Accepted
- **Date:** 2026-11-13
- **Decision:** Provide fully developed starter templates for an agreed priority subset of the document master list. Keep remaining entries in the catalogue for later development; basic structures may be supplied only when clearly labelled as starter structures.
- **Rationale:** Quality and usability are more valuable than superficial coverage of every listed document.
- **Consequences:** Prioritize templates by first-release discipline, frequency/usefulness, dependencies and available verified content. Never imply a generic shell is a technically complete template.

### DEC-019 — Technical content uses a joint development/verification model

- **Status:** Accepted
- **Date:** 2026-11-13
- **Decision:** The development team defines the technical framework and schema; the owner/laboratory subject-matter experts provide or verify method-specific content before it is released as verified.
- **Rationale:** The developer can implement calculation infrastructure but must not invent technical method requirements.
- **Consequences:** Every verified method package needs a traceable source, edition, formula, units, criteria where applicable, worked examples and test evidence reviewed by a competent technical expert.

## 3. Cross-cutting interpretation rules

1. An informational status field is not a workflow.
2. A signature capture or approval table is not proof that an approval process occurred.
3. A completed-record snapshot is not a formal controlled-document revision.
4. Internal recovery snapshots are not the document's revision history.
5. A register is an information collection; it does not automatically imply alerts, scheduling or operational tracking.
6. A user-created formula is not a verified standard method.
7. A local master-data reference must not silently change the values already captured in a completed record.
8. A custom block schema is declarative configuration, not permission to execute arbitrary code.
9. Retaining an original block ID does not require implementing an excluded capability. The ID may be mapped to a content-only interpretation, deferred capability or explicit exclusion.
10. A product feature must be evaluated against `project_details.md` before it is added to the roadmap.

## 4. Source catalogue findings to resolve

### 4.1 Original block IDs

The source `block_list.md` contains 220 unique IDs. All must remain traceable in the canonical registry. The revised catalogue contains 14 ID namespaces: `STR`, `META`, `SOP`, `TBL`, `FRM`, `CHEM`, `ENV`, `COAL`, `EQB`, `QC`, `APR`, `NABL`, `VIS`, `AUTO`.

The old frontend statement that the block library has 13 categories must be reconciled. ID namespaces, user-facing category groups and architecture layers are separate concepts.

### 4.2 Block mappings requiring scope adjustment

- `AUTO-018` AI Content Generator: explicitly excluded.
- `AUTO-010` Approval Trigger: excluded as an application workflow capability.
- `AUTO-017` Audit Trail Reference: no app-managed audit trail; a static document reference/content interpretation may be retained if useful.
- `AUTO-008` Auto Revision Number: no formal revision-management service. A user-entered revision field remains available as ordinary content.
- `APR-*` and `TBL-013`: content/layout/input blocks only; no approval state machine.
- `META-011` Document Status: optional user-editable content; no enforced status transitions or locks.
- `META-012` Revision History Table: ordinary editable table/content unless a later decision changes scope.
- `EQB-*`, `QC-*`, sample and chemical blocks: structured records are in scope; operational alerting/tracking is not.

These mappings must be reconciled in the canonical block specification before implementation.

### 4.3 Document master-list duplicate names

Five pairs have the same exact title in the source catalogue:

| First ID | Second ID | Name |
|---|---|---|
| QMS-015 | AUD-001 | Internal Audit Procedure |
| QMS-016 | AUD-012 | Management Review Procedure |
| SOP-009 | MU-002 | Measurement Uncertainty Budget |
| CR-011 | CMP-012 | Customer Satisfaction Analysis |
| CR-012 | CMP-004 | Complaint Investigation Report |

Preserve both IDs. Determine whether each pair uses one canonical template with multiple catalogue references or needs separate templates because of different scope/content.

## 5. Pending detailed specifications (not unresolved product decisions)

The following details remain open and must be specified before the associated functionality is implemented:

- Supported browsers/OS versions and PWA installation behaviour.
- IndexedDB schema, indexes, transaction boundaries and migration/rollback.
- Attachment size/type limits, storage and deletion behaviour.
- Autosave timing, undo/redo depth and recovery snapshot retention.
- Completed-record definition, snapshot fields, amendment relationship and deletion/retention policy.
- Template selective-update diff, merge and conflict-resolution rules.
- Exact master-data schemas and live-reference/snapshot policy per category.
- Formula grammar, supported operators/functions, unit model, precision and dependency handling.
- Initial method list for each of the four disciplines.
- Method-specific formulas, standards/editions, units, corrections, limits, QC and worked examples.
- Native block schemas, validation and rendering support.
- Visual custom-block designer and advanced declarative schema contract.
- PDF/DOCX/CSV/XLSX export mapping and rendering limitations.
- JSON/ZIP backup format, validation, replace/merge restore, encryption and backward compatibility.
- Test strategy and measurable performance/quality targets.
- Technology stack and third-party library selections.

These are to be resolved through research, engineering design and technical-subject-matter review. Do not silently convert an unverified proposal into an accepted product decision.

## 6. Decision change template

Use this template for new decisions:

```markdown
### DEC-XXX — Short decision title
- **Status:** Proposed / Accepted / Rejected / Superseded
- **Date:** YYYY-MM-DD
- **Context:** Why a decision is needed.
- **Options considered:** Option A; Option B; Option C.
- **Decision:** The selected option.
- **Rationale:** Why it was selected.
- **Consequences:** Product, architecture, migration and testing impacts.
- **Supersedes:** DEC-XXX, if applicable.
- **Owner approval/evidence:** How and when the owner approved it.
```

---

**End of decision register.**
