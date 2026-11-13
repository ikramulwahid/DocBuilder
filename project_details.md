# DocBuilder — Project Details and Permanent Product Brief

**Document type:** Primary project brief / product requirements baseline  
**Version:** 1.0  
**Status:** Draft for owner review; product direction accepted  
**Baseline date:** 2026-11-13  
**Repository:** https://github.com/ikramulwahid/DocBuilder  
**Default branch at baseline:** `main`

---

## 1. Purpose of this document

This is the primary, durable description of DocBuilder. It defines the product purpose, boundaries, confirmed requirements, architectural principles and working rules for anyone designing or developing the application.

Read this document before planning or implementing a feature. Use `project_decisions.md` for the history and detail of decisions, `project_roadmap.md` for planned work and sequencing, and `project_progress.md` for the current factual project state.

This document describes the intended product. It does not claim that the application has already been implemented or that the detailed technical specifications are complete.

## 2. Product definition

**DocBuilder is an installable, offline-capable, single-user, browser-based document authoring and laboratory record-building application for ISO/IEC 17025 testing laboratories.**

It enables a laboratory to design, create, customize, populate, calculate, preview, save, print and export laboratory documentation using reusable blocks, templates, structured records, local master-data libraries and calculation tools.

DocBuilder is intended to be substantially more capable than a conventional word processor or static template collection. Its central value is the ability to assemble reusable document structures, capture structured laboratory information, use verified or user-defined calculations, and produce professional output.

### 2.1 Initial laboratory scope

The first release must support these four areas:

1. QMS and general laboratory documentation.
2. Chemical testing.
3. Environmental testing.
4. Coal and coke testing.

Each area must have meaningful, functional support in the first release. The exact initial list of test methods and the technical depth of each discipline must be agreed in a separate method-by-method specification. Additional disciplines should be addable later without redesigning the core application.

## 3. Explicit product boundaries

DocBuilder is **not** intended to become any of the following as part of the confirmed product:

- A Laboratory Information Management System (LIMS).
- A sample lifecycle, work-allocation or laboratory production-management platform.
- A document-control system.
- A server-backed QMS or compliance-management platform.
- A multi-user or collaborative application.
- An approval or authorization workflow engine.
- A cloud synchronization service.
- An AI-powered document generator.
- An equipment-maintenance or calibration-alert management system.

The application must not introduce login, user accounts, role-based permissions, server-side business logic, a remote business-data database, cloud synchronization or AI functionality without a separate explicit owner decision.

### 3.1 Content is not workflow

DocBuilder may create documents containing document numbers, status labels, issue/revision details, prepared-by/checked-by/reviewed-by/approved-by names, designations, dates, signature images, revision-history tables, approval tables, compliance checklists and audit-related content.

These are ordinary editable document content or records. Their presence does not mean DocBuilder enforces document approval, authorization, controlled distribution, audit trails or a controlled-document lifecycle.

A user-entered status such as “Approved” is informational only. It must not automatically lock a document or trigger a workflow.

## 4. Confirmed product requirements

| Area | Requirement |
|---|---|
| Delivery | Installable Progressive Web App (PWA) |
| Offline operation | Core application works offline after the first successful load and caching of required assets |
| Hosting | Static HTTPS hosting for initial delivery and application updates; no business-data backend |
| Users | Single user and local workspace |
| Login/authentication | None |
| Backend/API | None for normal product operation |
| Primary storage | IndexedDB |
| Static asset cache | Service worker and Cache API |
| Documents | Blank/individual documents and documents created from templates |
| Templates | Reusable, editable templates with explicit versions |
| Template updates | Existing documents remain unchanged; users may selectively apply updates with preview/conflict handling |
| Completed records | A completed record is preserved as a snapshot; an amendment creates a separate copy/record linked to the original where appropriate |
| Registers | Structured record collections with grid/table views and individual record views |
| Master data | Shared local libraries reusable by documents and records |
| Blocks | Native blocks, visual user-created blocks and advanced schema-defined blocks |
| Calculations | Built-in method-specific calculations and user-created formulas |
| Formula scope | Across blocks within the same document; no cross-document formula references in the confirmed scope |
| Units/validation | Explicit, method- and field-configured; never inferred from generic block names |
| AI | Excluded |
| Document control | Excluded |
| Approval workflow | Excluded |
| Approval/signature content | Allowed as ordinary document content; no approval events or enforcement |
| Export | PDF, DOCX, CSV, Excel and A4 print output |
| Backup | Both JSON and ZIP workspace backup/restore |
| Customization | All three levels: developer-built native blocks, visual designer, advanced declarative schema editor |

## 5. Main user capabilities

### 5.1 Create documents

Users can:

- Create a blank document.
- Create a document from a reusable template.
- Insert, remove, move, configure and reuse blocks.
- Customize document metadata and page design.
- Save, duplicate and export documents.
- Convert a suitable document into a reusable template.

A document created from a template becomes an independent instance. Subsequent template edits must not silently change existing documents.

### 5.2 Design and manage templates

Users can create, edit, duplicate, organize and reuse templates for manuals, policies, SOPs, procedures, forms, registers, worksheets, reports, plans, checklists and other document types.

Templates must have stable identities and explicit versions. Users may selectively apply a newer template version to an existing document only after reviewing the proposed changes and resolving conflicts. The update must not overwrite unrelated user edits.

### 5.3 Populate records and registers

Users can populate documents with actual laboratory information and maintain structured local records.

A register is a collection of individual records, not merely a static table. The user must be able to view records in a grid/table and open an individual record. Register records can also be presented or printed through document templates where specified.

### 5.4 Use shared master data

Reusable local master-data libraries should include, at minimum, configurable categories such as:

- Laboratory profile and organization details.
- Personnel and designations.
- Equipment and instruments.
- Chemicals and reagents.
- Testing methods and standards.
- Sample types and matrices.
- Units and measurement parameters.

The detailed schema, reference-versus-snapshot policy and update behaviour must be specified for each category.

### 5.5 Perform calculations

Two calculation sources are required:

1. **Built-in method-specific calculations:** configured against identified methods and standard editions, with technically reviewed formulas, inputs, units, intermediate calculations, rounding rules and test cases.
2. **User-created formulas:** authored through a visual formula builder and advanced expression editor, with access to compatible fields/calculations across blocks in the same document.

The formula engine must not execute arbitrary JavaScript or other unrestricted user code. A user-defined formula must not be represented as a verified standard method merely because it calculates a result.

### 5.6 Customize blocks

Three customization levels are required:

1. **Native blocks:** developed and maintained by the application.
2. **Visual custom blocks:** created by users through a no-code visual designer.
3. **Advanced custom blocks:** created through a declarative schema editor for fields, types, validations, layout and supported formula bindings.

The advanced schema editor is not a general-purpose code execution environment. Custom blocks must use the shared block runtime, data contracts, formula engine and rendering system.

## 6. Application layout and user experience

The intended primary editor is a three-panel desktop-first workspace:

- **Left panel:** searchable, grouped block library, including favourites and recently used blocks.
- **Centre panel:** document canvas with page boundaries and A4 preview.
- **Right panel:** properties and configuration for the selected block.

The editor must provide three modes:

| Mode | Purpose |
|---|---|
| Design | Build and configure the structure of a document or template |
| Fill | Enter actual values, observations and record data |
| Preview | Inspect the output as a formatted document before printing/export |

The interface should provide light and dark working themes and a print/paper preview. The final rendering must remain faithful to the same underlying document model.

The original design's navigation, dashboard, document library, template gallery, block library, registers, settings, editor and export screens remain useful starting points, subject to the explicit exclusions in this brief.

## 7. Local-first and offline architecture

### 7.1 Delivery and offline behaviour

DocBuilder is a static PWA. The user must initially load the application from a secure HTTPS origin (or a development localhost origin) so the browser can obtain the app files and install the service worker. After the first successful load and caching of all required assets, core features must work without internet access.

The service worker/Cache API cache application assets. IndexedDB is the source of truth for user data. The two stores must not be confused.

Core offline functionality includes document creation/editing, template use, local master data, register operations, local calculations, document preview, PDF/DOCX/CSV/Excel export and backup/restore, subject to browser capability and locally bundled dependencies.

Connectivity may be needed for initial delivery and future static application updates. There is no cloud sync or remote user-data service.

### 7.2 Data storage

- IndexedDB: documents, templates, block instances, custom schemas, formulas, registers, records, master data, attachment blobs and relationships.
- Cache API: application shell, scripts, styles, fonts and required static assets.
- localStorage: small non-critical preferences only, if needed.
- OPFS: optional only after a separate compatibility/performance evaluation; not assumed as a required baseline.

The application should request persistent browser storage where supported, monitor storage usage where possible and warn users about storage/backup risks. Browser storage is not a substitute for external backup files.

### 7.3 Privacy and security boundary

No document content, laboratory records, formulas, master data or attachments should be transmitted to a server during normal operation. There is no DocBuilder login or application-level authorization. Access to the browser profile and device is governed by operating-system and browser security, not by DocBuilder.

## 8. Record preservation and history

DocBuilder must distinguish three separate concepts:

1. **Autosave and undo/redo:** normal editing behaviour.
2. **Internal recovery snapshots:** local recovery points used to restore work after accidental changes or failures; not formal document revisions.
3. **Completed-record preservation:** when a user marks a record as completed, preserve the completed state as a snapshot. Subsequent amendments create a separate amended copy/record, retaining a link to the original where appropriate.

Completed-record snapshots are a data-integrity feature, not a claim of legal immutability, tamper-proof storage, electronic approval or formal document control. The exact completion criteria, amendment workflow and deletion/retention behaviour require a detailed specification.

Document revision-history tables, where required, are ordinary editable content unless the user explicitly requests a future application-managed feature.

## 9. Block catalogue and source traceability

The original `block_list.md` contains 220 unique block IDs. Every original ID must remain traceable in the normalized catalogue. Do not silently delete, renumber or reuse an existing ID.

The 14 original ID namespaces represented in the normalized catalogue are:

`STR`, `META`, `SOP`, `TBL`, `FRM`, `CHEM`, `ENV`, `COAL`, `EQB`, `QC`, `APR`, `NABL`, `VIS`, `AUTO`.

The old frontend wording “13 block categories” is not a reliable taxonomy definition and must be reconciled with the normalized registry. The ID namespaces, user-facing groups and architectural layers are different concepts and should not be used interchangeably.

Every original ID must be classified as one of:

- Active native block.
- Active user-configurable block/preset.
- Shared application service or capability used by blocks.
- Deferred/future capability.
- Explicitly excluded capability, retained for traceability.

Overlap should be resolved through shared data models and services rather than by duplicating implementations.

### 9.1 Important block-scope interpretations

- Approval, signature and revision-history blocks may create document content; they do not run an approval or document-control workflow.
- `AUTO-018` AI Content Generator is excluded.
- `AUTO-010` approval trigger and any workflow-enforcement interpretation are excluded.
- `AUTO-017` must not imply an application-managed audit trail. A static reference/content block may be considered if useful.
- Automatic numbering may be retained as an authoring utility only if its scope is clearly defined and it does not imply document control.
- Automatic revision management and controlled-document lifecycle functions are excluded.
- Calculations, conditional rules, repeatable fields, image uploads, attachments, graphs and data links should use shared services.

## 10. Document catalogue and templates

The original `document_master_list.md` contains 324 entries with 324 unique document IDs across 20 categories. It remains the source catalogue until a normalized version is explicitly adopted.

The five exact duplicate-name pairs identified during review must be reconciled without losing original IDs:

| Entry 1 | Entry 2 | Shared name |
|---|---|---|
| QMS-015 | AUD-001 | Internal Audit Procedure |
| QMS-016 | AUD-012 | Management Review Procedure |
| SOP-009 | MU-002 | Measurement Uncertainty Budget |
| CR-011 | CMP-012 | Customer Satisfaction Analysis |
| CR-012 | CMP-004 | Complaint Investigation Report |

For each pair, determine whether the entries should share one canonical template with alternate catalogue references or whether their actual scopes require separate templates. Do not assume that identical titles always mean identical content.

The first release should provide fully developed starter templates for an agreed priority subset. Other catalogue entries may remain available as catalogue items or basic starting structures until their templates are developed. The application must not present unfinished generic shells as technically complete templates.

The document catalogue is a product planning inventory. It is not, by itself, proof that every listed item is mandatory under ISO/IEC 17025 or NABL requirements.

## 11. Technical accuracy requirements

For each built-in technical method/calculation, the detailed specification must identify:

- Discipline, matrix/sample type and parameter.
- Test method and standard/document reference, including edition/revision.
- Applicability and limitations.
- Input fields, data types and units.
- Formula and formula version.
- Conversion factors, corrections and intermediate calculations.
- Rounding, significant digits and reporting rules.
- Acceptance criteria and their source, where applicable.
- Validation rules, error states and boundary cases.
- Worked examples and automated test vectors.
- Technical reviewer/verification evidence and approval of the specification before release.

Never invent a method, formula, limit, acceptance criterion, unit or standard edition from a block name. Unspecified technical details must remain explicitly pending.

## 12. Rendering and export requirements

All output formats must use a shared document representation so that the editor, preview, printing and exports do not maintain unrelated content models.

Required outputs:

- PDF.
- Editable DOCX.
- A4 print-ready output.
- CSV for appropriate tabular data.
- Excel workbook export for tables/registers and structured whole-document data.
- JSON backup.
- ZIP backup including structured data and attachments.

Excel export is structured data export, not a promise to recreate the visual page layout of the original document. Each export type requires a defined mapping for rich text, formulas, values, units, dates, tables, images and attachments.

Pagination, repeated table headings, page breaks, headers/footers, image scaling, long content, formula output and overflow must be specified and tested for each supported block type.

## 13. Explicitly excluded functionality

The following items are out of scope unless the owner changes the product decision:

- LIMS/sample workflow or sample tracking Kanban.
- Operational work allocation or analyst task inbox.
- Calibration due-date alerting/notification centre.
- Equipment-maintenance alert system.
- Enforced document review/approval workflow.
- Enforced document distribution and acknowledgement.
- Controlled-copy/obsolete-document lifecycle.
- Application-managed audit trail or CAPA workflow.
- Multi-user roles, login and permissions.
- Cloud database, cloud backup or synchronization.
- AI content generation or external AI integration.

A user may still create ordinary documents, forms, registers and checklists describing these subjects.

## 14. Quality attributes

DocBuilder should be:

- **Offline-capable:** core functionality does not depend on a network after first load.
- **Data-conscious:** autosave, transactional persistence, clear save status and safe migrations.
- **Recoverable:** JSON/ZIP backup and tested restore.
- **Technically trustworthy:** method-specific calculations are specified and verified before being labelled verified.
- **Extensible:** new disciplines and block presets do not require rewriting the core.
- **Consistent:** one canonical block registry and shared services.
- **Portable:** users can export their workspace and structured data.
- **Predictable:** template/schema updates do not silently rewrite existing documents or completed records.
- **Testable:** each feature has explicit acceptance criteria and automated tests where practical.

## 15. Working rules for coding and design agents

Any AI coding/design agent or developer working on DocBuilder must:

1. Read this file, `project_decisions.md`, `project_roadmap.md` and `project_progress.md` before starting work.
2. Read the relevant original source specifications and detailed technical specifications before changing an affected area.
3. Treat this file as the authority for product scope and boundaries. Treat `project_decisions.md` as the authority for recorded decisions. Do not override either based on assumptions.
4. Preserve all original document IDs and block IDs unless the owner explicitly approves a migration/renumbering plan.
5. Keep the project local-first and backend-free. Do not add login, roles, cloud sync, AI, document control or approval workflow without explicit approval.
6. Do not invent laboratory formulas, standards, units, limits, acceptance criteria or technical procedures.
7. Separate document content from application-enforced workflows.
8. Keep completed-record snapshots independent from later edits to templates, schemas, formulas or master data.
9. Use shared services and data contracts; avoid duplicate implementations of the same capability.
10. Execute all verification tasks the agent can perform itself, including unit/integration tests, browser checks, offline checks, build checks and export checks where available. Reserve owner actions only for genuinely owner-controlled decisions, consent or inaccessible physical environments.
11. Report exactly what changed, what was tested, the results, remaining limitations and any owner-only actions. Never claim a check was performed if it was not.
12. Update `project_progress.md` after completing an approved work package. Propose decision changes in `project_decisions.md`; do not silently rewrite accepted decisions.
13. Do not edit the GitHub repository, replace source specifications or begin implementation unless the owner has requested that work.

## 16. Source documents and related specifications

Original repository planning sources:

- `block_list.md` — original block catalogue and P1/P2 suggestions.
- `document_master_list.md` — proposed 20-category document inventory.
- `frontend_design.md` — original frontend and screen design proposal.

Related architecture specifications prepared during planning:

- `DocBuilder_Revised_Block_Catalogue.md` — normalized interpretation of original block IDs and overlaps; must be reconciled with the decisions in this project brief before being adopted as authoritative.
- `DocBuilder_Final_Product_Technical_Architecture.md` — product/technical architecture baseline; must be updated to incorporate the accepted recommendations recorded in `project_decisions.md` before being treated as fully consistent.

The four project-control files are:

- `project_details.md` — this primary product brief.
- `project_decisions.md` — accepted decisions and decision history.
- `project_roadmap.md` — planned work and phase gates.
- `project_progress.md` — factual execution history and current state.

---

**End of project brief.**
