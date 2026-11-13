# DocBuilder — Project Roadmap

**Document type:** Development roadmap and phase-gate plan  
**Version:** 1.0  
**Status:** Proposed execution sequence; no implementation phase started  
**Baseline date:** 2026-11-13  
**Product brief:** `project_details.md`  
**Decision register:** `project_decisions.md`  
**Progress log:** `project_progress.md`

---

## 1. Purpose

This roadmap defines the recommended order for taking DocBuilder from planning through detailed specification, implementation, verification and first release.

It is a staged plan, not a claim that work has been completed. A phase is complete only when its deliverables and exit criteria have been verified and the result is recorded in `project_progress.md`.

The roadmap must not be used to expand the accepted product scope. All work must comply with `project_details.md` and `project_decisions.md`.

## 2. Roadmap principles

1. Resolve data contracts and dependencies before building large numbers of UI blocks.
2. Build the PWA/offline and local-data foundation before feature implementation depends on persistence.
3. Define one canonical block registry and shared block runtime.
4. Separate templates, documents, records and registers in both data model and UI.
5. Make completed-record preservation a core data-integrity capability.
6. Build the calculation framework before implementing method-specific calculations.
7. Implement technical methods only from identified source material and laboratory expert verification.
8. Use one shared document representation for editor, preview, print and exports.
9. Test offline behaviour and backups continuously, not only at the end.
10. Every phase must have measurable acceptance criteria and evidence.

## 3. Phase overview

| Phase | Name | Main outcome | Gate |
|---|---|---|---|
| 0 | Planning and specification reconciliation | Consistent authoritative specifications | P0-GATE |
| 1 | PWA and local data foundation | Installable offline shell and reliable local persistence | P1-GATE |
| 2 | Core document, template and editor system | Blank documents, templates and Design/Fill/Preview modes | P2-GATE |
| 3 | Block registry and customization | Native blocks, visual builder and advanced schema builder | P3-GATE |
| 4 | Records, registers and master data | Structured records, grid views and shared local libraries | P4-GATE |
| 5 | Formula and calculation engine | Safe formulas and verified-method infrastructure | P5-GATE |
| 6 | First-release laboratory content | Functional QMS, chemical, environmental and coal/coke method sets | P6-GATE |
| 7 | Rendering, export and backup | PDF, DOCX, CSV, Excel, JSON and ZIP | P7-GATE |
| 8 | Integration, hardening and release | Verified offline-capable first release | P8-GATE |

The exact dependency order of portions of Phases 3–7 may be adjusted during detailed planning, but the gates and product boundaries must remain intact.

---

## 4. Phase 0 — Planning and specification reconciliation

**Status:** Not started as a formal repository work package.

### Work packages

- P0-01: Owner-review and accept the four project-control files and three revised product specifications as one coherent baseline.
- P0-02: Resolve owner comments and formally baseline the normalized block catalogue, including all 220 original IDs and active/content-only/deferred/excluded classifications.
- P0-03: Resolve the five duplicate document-title pairs and formally baseline the 324-entry document catalogue without deleting or renumbering IDs.
- P0-04: Formally baseline the frontend design against the accepted PWA, document, template, record, register and customization requirements.
- P0-05: Build and review a traceability matrix linking priority document templates to block IDs, block-definition versions and applicable methods.
- P0-06: Finalize the first-release starter-template subset and the priority technical method list for each of the four disciplines.
- P0-07: Complete detailed PWA, data model, document/template/record/register, custom-block, formula, master-data, rendering and backup specifications.
- P0-08: Select a technology stack and libraries based on the approved requirements.
- P0-09: Define test, quality, performance and acceptance strategy.

### Required deliverables

- Accepted project-control files.
- Reconciled block catalogue.
- Normalized document catalogue and duplicate-resolution map.
- Prioritized starter-template list.
- Four-discipline method list and technical-content responsibility plan.
- Architecture/data-model specifications.
- Technology selection record.
- Test strategy and gate templates.

### Exit criteria — P0-GATE

- No contradiction remains between the project brief, decision register, block catalogue and frontend scope.
- All original block IDs and document IDs are traceable.
- Explicitly excluded capabilities are marked and cannot be accidentally implemented as active requirements.
- All four first-release disciplines have an agreed initial functional scope.
- No technical method is claimed as verified without a source and competent technical review plan.
- Implementation tasks have dependencies, acceptance criteria and test evidence requirements.

---

## 5. Phase 1 — PWA and local data foundation

### Work packages

- P1-01: Establish the selected frontend/toolchain and reproducible development/build environment.
- P1-02: Implement PWA manifest, service worker and versioned static-asset caching.
- P1-03: Implement offline readiness detection and first-load guidance.
- P1-04: Implement IndexedDB repository/data-access layer.
- P1-05: Implement schema versioning, transactional operations and migration/rollback framework.
- P1-06: Implement save-state reporting and autosave primitives.
- P1-07: Implement local storage usage monitoring and persistent-storage request where supported.
- P1-08: Implement application preferences and initial navigation shell.

### Exit criteria — P1-GATE

- A clean install/first load succeeds from the supported origin.
- Required application assets are cached and the app restarts offline.
- Core shell and local data operations do not call a backend.
- IndexedDB records survive page reload and browser restart in the supported environment.
- Failed writes are reported; the UI never shows “saved” before the transaction succeeds.
- Migration tests prove data is preserved or safely rolled back.
- Updating app assets does not delete user data.
- Supported browsers/OS and known limitations are documented.

---

## 6. Phase 2 — Core document, template and editor system

### Work packages

- P2-01: Define and implement the canonical document model.
- P2-02: Implement blank document creation, save, duplicate and deletion rules.
- P2-03: Implement template model, versions and document instantiation.
- P2-04: Implement the three-panel editor shell.
- P2-05: Implement Design, Fill and Preview modes.
- P2-06: Implement page setup, A4 canvas, margins, zoom and page boundaries.
- P2-07: Implement basic metadata and document identity fields as content.
- P2-08: Implement ordinary user-editable status fields without workflow/locking.
- P2-09: Implement undo/redo, autosave feedback and internal recovery snapshots.
- P2-10: Implement completed-record snapshot and amended-copy model.

### Exit criteria — P2-GATE

- A user can create and edit a blank document.
- A user can create a document from a template.
- The created document is independent of its source template.
- Editing a template does not change existing documents.
- Template update preview can identify changes; selective merge/conflict handling is verified before that function is released.
- Design, Fill and Preview modes share one document model.
- Informational status fields do not lock documents or initiate workflows.
- Completed records are preserved as snapshots; amendments create separate linked copies.
- Recovery snapshots are clearly distinguished from completed-record snapshots and manual revision-history content.

---

## 7. Phase 3 — Block registry and customization

### Work packages

- P3-01: Implement canonical block registry with stable original IDs.
- P3-02: Define common block-definition and block-instance contracts.
- P3-03: Implement core structure blocks: headings, rich text, lists, callouts, page breaks and dividers.
- P3-04: Implement shared table engine and table presets.
- P3-05: Implement common input controls and validation bindings.
- P3-06: Implement metadata/content blocks, references and attachment/image presentation.
- P3-07: Implement reusable services: conditional logic, repeatable data, numbering (if approved by detailed spec), attachments and chart rendering.
- P3-08: Implement visual custom-block designer.
- P3-09: Implement advanced declarative schema editor and validation.
- P3-10: Implement custom-block versioning and compatibility rules.
- P3-11: Implement block library search, grouping, favourites and recent use.

### Exit criteria — P3-GATE

- Every original block ID has a traceable canonical registry entry and scope classification.
- Duplicate/overlapping capabilities share common services.
- Native, visual custom and schema-defined blocks use the same document and rendering contracts.
- Custom schemas cannot execute arbitrary scripts.
- Schema changes do not silently corrupt existing documents or completed records.
- Out-of-scope AI, approval workflow, audit trail and document-control behaviours are absent.
- Every released block has defined properties, validation, persistence and export behaviour.

---

## 8. Phase 4 — Records, registers and master data

### Work packages

- P4-01: Define register and register-record schemas.
- P4-02: Implement register hub and grid/table view.
- P4-03: Implement individual record detail/edit view.
- P4-04: Implement filtering, sorting, search and configurable columns.
- P4-05: Implement local master-data libraries.
- P4-06: Implement live-reference versus snapshot behaviour by master-data category.
- P4-07: Implement record insertion/reference in documents and templates.
- P4-08: Implement completed-record preservation and amended-copy association.
- P4-09: Define delete/archive/retention behaviour without implying document control.

### Exit criteria — P4-GATE

- A register is stored as structured records, not only as an embedded static table.
- Users can create, view, search, filter and edit records in a grid and individual detail view.
- Reusable master data can be referenced from documents and registers.
- Completed records preserve the values actually used, even if master data changes later.
- An amendment does not overwrite the original completed snapshot.
- No sample workflow, due-date alerts, notifications or operational tracking dashboards are introduced.

---

## 9. Phase 5 — Formula and calculation engine

### Work packages

- P5-01: Define safe formula grammar and parser.
- P5-02: Define typed values, units and dimensional compatibility rules.
- P5-03: Implement field binding across blocks within the same document.
- P5-04: Implement dependency graph and cycle detection.
- P5-05: Implement recalculation, validation and error reporting.
- P5-06: Implement precision, rounding and formatting rules.
- P5-07: Implement result provenance and formula-version capture.
- P5-08: Implement visual formula builder.
- P5-09: Implement advanced formula editor using the same safe expression model.
- P5-10: Define method-package contract for verified built-in calculations.
- P5-11: Build generic calculation test harness and worked-example framework.

### Exit criteria — P5-GATE

- Formulas can reference compatible fields/calculations in different blocks of the same document.
- Cross-document formula references are not available.
- Circular dependencies are detected and reported.
- Invalid types, missing values, incompatible units and out-of-range values are handled explicitly.
- Formula evaluation does not execute arbitrary code.
- Inputs, formula version, units, intermediate results, rounding and output provenance can be reproduced.
- User-created formulas are clearly distinguished from verified method-specific calculations.
- Core engine tests cover normal, boundary, invalid and precision cases.

---

## 10. Phase 6 — First-release laboratory content

All four disciplines are required in release 1. Technical method selection must be finalized in Phase 0 and verified content must be supplied/reviewed by laboratory experts.

### Work packages

- P6-01: QMS/general documentation starter templates and blocks.
- P6-02: Chemical testing block presets, initial method packages and worksheets.
- P6-03: Environmental testing block presets, initial method packages and worksheets.
- P6-04: Coal/coke block presets, initial method packages and worksheets.
- P6-05: Shared equipment, reagent, sample, QC and result schemas needed by the agreed methods.
- P6-06: Method reference/edition traceability.
- P6-07: Technical review of each built-in calculation and its worked examples.
- P6-08: Document master-list starter-template priority set.

### Exit criteria — P6-GATE

- Each of the four disciplines has functional, tested blocks and templates for its agreed initial method set.
- Each built-in calculation is tied to an identified method/source and edition/revision.
- Units, formula, corrections, intermediate values, rounding and result presentation are defined.
- Laboratory technical experts have reviewed the method-specific content and test examples.
- No method, limit or formula is labelled verified without documented evidence.
- Remaining document-catalogue entries are clearly labelled as later work or basic starter structures, not completed templates.

---

## 11. Phase 7 — Rendering, export and backup

### Work packages

- P7-01: Implement shared rendering/document representation.
- P7-02: Implement A4 page layout, page breaks and pagination.
- P7-03: Implement repeated table headings and multi-page table continuation.
- P7-04: Implement PDF generation and print stylesheet/output.
- P7-05: Implement editable DOCX export and feature support matrix.
- P7-06: Implement CSV export for tabular data/registers.
- P7-07: Implement Excel export for tables/registers and structured document data.
- P7-08: Implement versioned JSON workspace backup/restore.
- P7-09: Implement ZIP backup with manifest, JSON data and original attachments.
- P7-10: Implement backup validation, restore preview, safe replacement and/or merge according to the approved specification.
- P7-11: Implement visible backup status, an inline last-backup reminder and storage/quota warnings; do not add push notifications or an operational notification centre.

### Exit criteria — P7-GATE

- Preview, print and PDF output use the shared document model.
- Required core blocks render correctly in PDF and A4 print.
- DOCX output is editable and has a documented support/limitation matrix.
- CSV and Excel exports have defined field, unit, date and formula/value mapping.
- JSON and ZIP backup/restore round-trip the workspace, including custom blocks, formulas, records, master data and attachments.
- Invalid, incomplete, unsupported or corrupted backups are rejected safely without damaging current data.
- Backup restore conflict behaviour is tested and documented.

---

## 12. Phase 8 — Integration, hardening and release

### Work packages

- P8-01: Run full unit and integration test suites.
- P8-02: Run browser automation for supported browsers.
- P8-03: Run first-load, offline restart, offline update and cache-version tests.
- P8-04: Run data migration, storage exhaustion and recovery tests.
- P8-05: Run large document, table, attachment and register performance tests.
- P8-06: Run calculation verification suites for every built-in method.
- P8-07: Run visual regression and PDF/DOCX/Excel export checks.
- P8-08: Validate JSON/ZIP backup/restore and cross-origin/device migration by backup.
- P8-09: Review accessibility, keyboard navigation and responsive layouts.
- P8-10: Prepare user help, known limitations and release notes.
- P8-11: Perform final scope review against `project_details.md` and `project_decisions.md`.

### Exit criteria — P8-GATE

- All release-blocking tests pass.
- No unresolved critical/high defects remain without explicit owner acceptance.
- Offline core flows are demonstrated in the supported browser matrix.
- The application does not require a backend, login, AI service or network for normal local work after initial caching.
- All release 1 method packages have technical review evidence.
- PDF/DOCX/CSV/Excel outputs meet documented acceptance requirements.
- Full backup and restore are demonstrated successfully.
- Known limitations and unsupported features are disclosed.
- The final release scope remains within the accepted product boundaries.

---

## 13. Starter template prioritization method

The 324-entry master list must not be treated as a commitment to produce 324 fully populated templates immediately.

Choose the first-release template set using:

1. Necessity for the agreed four-discipline first-release workflows.
2. Frequency of use and practical value.
3. Dependencies on shared blocks, master data and calculation methods.
4. Availability of reliable source content and laboratory review.
5. Reusability across document types or disciplines.
6. Ability to test and maintain the template.

The selected list must include an owner-approved priority, scope, source/authoring responsibility, completeness status and acceptance criteria for each starter template.

## 14. Technical method prioritization method

For each discipline, create a method matrix containing:

- Method/test name and parameter.
- Matrix/sample type.
- Standard/method reference and edition.
- Required equipment and reagents.
- Observation fields and table schema.
- Formula and units.
- Corrections/conversion factors.
- QC and acceptance criteria where applicable.
- Reporting requirements.
- Worked examples and expected calculation outputs.
- Technical reviewer and verification evidence.

Only methods with complete specifications and reviewed test cases may be marked ready for release.

## 15. Change control for this roadmap

Roadmap changes may reorder or split work to address dependencies, but must not:

- Reintroduce excluded product capabilities.
- Remove an accepted release 1 discipline without owner approval.
- Drop original block/document IDs without an approved mapping/migration.
- Claim technical methods are verified without evidence.
- Mark a phase complete without its exit criteria and test evidence.

If a change affects an accepted product decision, record it first in `project_decisions.md` and obtain owner approval.

---

**End of roadmap.**
