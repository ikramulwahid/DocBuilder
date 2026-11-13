# DocBuilder — Revised Block Catalogue and Architecture

**Document status:** Revised specification draft — pending owner review  
**Revision:** 2.0  
**Prepared:** 13 November 2026  
**Source repository:** `ikramulwahid/DocBuilder`, branch `main`  
**Source files reviewed:** `block_list.md`, `document_master_list.md`, `frontend_design.md`

---

## 1. Purpose and scope

This specification is the revised canonical block catalogue for DocBuilder. It preserves and maps all 220 original block IDs, resolves overlapping names, distinguishes insertable content and data-entry blocks from shared application services, and identifies which capabilities are active, content-only, deferred or excluded. It is governed by `project_details.md` and `project_decisions.md`.

It is a product and software-design specification. It is **not** a statement that a particular ISO/IEC 17025 or NABL requirement is satisfied, and it does not prescribe technical test methods or laboratory acceptance limits.

### 1.1 Rules for this revision

1. No original block ID is silently deleted.
2. Existing block IDs are retained as `legacy_block_id` values for migration and traceability.
3. New records must have a globally unique `block_uid`, separate from document numbers.
4. Similar names do not automatically mean identical behaviour. Each overlap is resolved in Section 5.
5. A shared service is not presented as an insertable block unless it has a user-facing block/interface.
6. Technical calculations, units, limits and table schemas must be method- and matrix-configurable. Unknown values remain explicitly unconfigured.
7. Existing master-document IDs and block IDs belong to different namespaces, even when their text is identical.
8. Retaining an original ID does not require implementing the original capability if it conflicts with an accepted product decision; record the content-only, deferred or excluded mapping explicitly.
9. No block may introduce document-control enforcement, approval workflow, operational laboratory tracking, notifications, cross-document formula execution or AI functionality.

### 1.2 Important ID namespace correction

The original lists reuse identifiers such as `SOP-001`, `CHEM-001`, `ENV-001`, `COAL-001` and `NABL-001` for both documents and blocks. These identifiers are not safe as a single primary key.

Use separate namespaces:

- Document record: `document_uid` (for example, `DOCUID-...`) and displayed `document_id` (for example, `SOP-001`).
- Block definition: `block_uid` (for example, `BLK-SOP-001`) and `legacy_block_id` (the original catalogue ID, for example, `SOP-001`).
- Block instance: `block_instance_id`, unique within the stored document content.

Do not change a laboratory's displayed document numbers to solve a block-catalogue collision. The namespace is an internal data-model requirement.

---

## 2. Corrected architecture and terminology

The original list mixes five different kinds of things. They should be represented separately in code and in the catalogue.

| Architecture kind | Meaning | Examples | Insertable in a document? |
|---|---|---|---|
| **Content block** | Content displayed in a document | Heading, rich text, warning, page break, attachment display | Yes |
| **Input/control block** | Interface used to enter or select a value | Text field, date picker, upload, signature capture | Yes, when used in a form or fillable document |
| **Structured domain block** | A typed laboratory/business data structure with a configured editor and renderer | Sample details, equipment details, titration observations, coal GCV observations | Yes, as a composite block or table/form preset |
| **Visualization block** | Displays data in a visual form | Line chart, control chart, calibration graph | Yes |
| **Application service** | Reusable logic used by one or more blocks | Formula engine, numbering, conditional rules, file-asset service | No, not by itself; it supports blocks |

A sixth concept, **template section**, is a preconfigured combination of blocks. For example, a “Purpose” SOP section is a template/preset for a heading and rich-text content, not a new editor engine.

### 2.1 Core shared services

The following services should be implemented once and reused:

| Service | Responsibility | Used by |
|---|---|---|
| Document metadata service | Stores editable document identity, title, dates, department, optional user-entered status and applicability | META blocks, headers, footers, templates |
| Person reference service | References local personnel records and ordinary document roles/designations | Prepared/checked/reviewed/approved content, authorization descriptions |
| Template/schema version service | Versions reusable templates and block schemas for compatibility and selective update | Template update preview, block compatibility and migration |
| Recovery/snapshot service | Stores internal recovery points and completed-record snapshots as distinct mechanisms | Undo/redo recovery, completed records and amended copies |
| Table engine | Column definitions, row data, validation, sorting, repeatable rows and pagination metadata | TBL blocks and technical tables |
| Form/control engine | Typed user inputs, required state, defaults and validation | FRM blocks and fill mode |
| Formula engine | Parses, validates and evaluates approved formula definitions | Calculated fields, technical calculations, formula tables |
| Rule/condition engine | Evaluates visibility and conditional behaviour | Conditional fields, rows and sections |
| Numbering service | Generates unique IDs according to configured scope and format | Document number and field-level Auto ID |
| File-asset service | Stores file metadata, bytes/blob references, hashes and preview information | Uploads, attachments, certificates and images |
| Reference catalogue | Stores ISO clauses, NABL references, standards, editions and internal references as locally maintained reference data | Metadata and compliance/document-content blocks |
| Technical-method configuration | Associates a block configuration with a test method, matrix, edition, units and calculation definition | Chemical, environmental, coal and QC blocks |
| Completed-record preservation | Captures a completed record and its required source/configuration snapshots; amendments create separate linked copies | Completed technical records and register entries |
| Chart renderer | Renders chart definitions from typed data series | Line, trend, calibration and control charts |
| Document renderer | Produces print/PDF page layout, pagination, headers, footers and block rendering | All document blocks |
| Local persistence and backup | Stores records locally and exports/imports versioned backup data | Whole application |

---

## 3. Standard block-definition record

Every catalogue entry should use a common definition schema. Not every property is required for every block, but the schema should be consistent.

| Property | Required | Description |
|---|---:|---|
| `block_uid` | Yes | Unique internal block-definition ID, prefixed/namespaced as `BLK-...` |
| `legacy_block_id` | Where applicable | Original ID from `block_list.md`; retain unchanged |
| `canonical_name` | Yes | Approved name shown in the block library |
| `architecture_kind` | Yes | Content, input/control, structured-domain, visualization or service-interface |
| `category` | Yes | Structure, metadata, SOP, table, form, chemical, environmental, coal, equipment, QC, approval, compliance, visualization or automation |
| `description` | Yes | User-facing purpose |
| `data_model` | As applicable | Data schema/model used by the block |
| `input_schema` | As applicable | Field definitions, types and constraints |
| `validation_profile` | As applicable | Required state, ranges, formats and cross-field rules |
| `method_config_id` | Technical blocks | Applicable method/matrix/edition-specific configuration |
| `renderers` | Yes | Editor, preview, PDF/print and (where applicable) compact/table renderers |
| `allowed_contexts` | Yes | Document types/categories where it may be used |
| `capabilities` | Yes | Examples: repeatable, conditional, calculated, linkable, uploadable |
| `version` | Yes | Definition version for migration and reproducibility |
| `lifecycle_state` | Yes | Active, deprecated or retired; retirement must include migration notes |

A block instance in a document must also store its own instance ID, definition ID/version, configured properties, content/value and references to data records. Do not store the entire catalogue definition inside every document instance.

---

## 4. Revised catalogue — all original IDs retained

The tables below list every original ID in `block_list.md`. “Kind” identifies the architectural role, not a development-priority ranking.

### 4.0 Scope-state legend

- **Active — content/data:** included as document content, input, structured data or visualization; this does not imply an operational management workflow.
- **Active — shared service:** local reusable logic consumed by in-scope blocks.
- **Deferred:** retained for traceability but not promised in the first release; requires an explicit roadmap work package.
- **Excluded:** conflicts with an accepted product boundary; ID retained for migration/traceability only.

The original inventory is retained in full. Individual block rollout is governed by `project_roadmap.md` and the selected first-release method/template lists; the old P1/P2 shortlist is historical prioritization input, not a commitment to implement all 50 items in a single release.

### 4.1 STR — Document structure blocks

| Original ID | Revised canonical name | Kind | Corrected behaviour |
|---|---|---|---|
| STR-001 | Document Title | Content block | Displays the document title. Default to a live binding to document metadata; allow a deliberate fixed/override title only when the template permits it. |
| STR-002 | Document Subtitle | Content block | Displays an optional subtitle; may bind to metadata or contain local text. |
| STR-003 | Section Heading | Content block | Heading with semantic level and numbering options. |
| STR-004 | Subsection Heading | Content block | Nested heading with semantic level and numbering options. |
| STR-005 | Paragraph / Rich Text | Content block | Rich text with supported inline formatting, links and paragraph alignment. |
| STR-006 | Numbered List | Content block | Ordered list with configurable numbering style and nesting. |
| STR-007 | Bullet List | Content block | Unordered list with configurable bullet style and nesting. |
| STR-008 | Definition Block | Content block | Term-definition pair or a repeatable glossary-style list. |
| STR-009 | Note Callout | Content block | Informational note; visual style is a renderer choice. |
| STR-010 | Warning Callout | Content block | Warning/caution content; must not imply a safety classification not configured by the author. |
| STR-011 | Information Callout | Content block | General informational callout. |
| STR-012 | Highlight Callout | Content block | Emphasized text; must remain accessible in print and monochrome output. |
| STR-013 | Page Break | Content block | Starts a new rendered page; it is a layout instruction, not a data record. |
| STR-014 | Horizontal Divider | Content block | Draws a configurable horizontal separator. |
| STR-015 | Table of Contents | Content block | Generates entries from semantic headings and configured heading levels. |
| STR-016 | Document Footer | Content block | Displays configured footer fields; page numbering and metadata are bound dynamically. |
| STR-017 | Document Header | Content block | Displays configured header fields; document metadata can be bound dynamically. |
| STR-018 | Page Number | Content block | Dynamic page number and total pages, if supported by the chosen output renderer. |
| STR-019 | Image Display | Content block | Displays an existing image asset; it does not perform file selection/upload itself. |
| STR-020 | Attachment Display | Content block | Displays an attachment reference, filename/description and optional link/print representation; uses the shared file-asset service. |

### 4.2 META — Document metadata blocks

These are metadata fields or metadata views. The canonical values live in the document metadata/version model, not as unrelated copies embedded in every block.

| Original ID | Revised canonical name | Kind | Corrected behaviour |
|---|---|---|---|
| META-001 | Document Number | Metadata field | Reads/writes the document's displayed identifier; generation is delegated to the numbering service. |
| META-002 | Document Title Metadata | Metadata field | Canonical title value. STR-001 can display this value. |
| META-003 | Revision Identifier | Metadata/content field | Optional user-entered revision label displayed as ordinary document content; no application-managed revision lifecycle is implied. |
| META-004 | Issue Date | Metadata field | Date the version is issued, with explicit date semantics. |
| META-005 | Effective Date | Metadata field | Date from which the version is intended to apply. |
| META-006 | Review Due Date | Metadata field | Scheduled review date; not the same as effective or approval date. |
| META-007 | Department | Metadata field | Department or functional area reference. |
| META-008 | Prepared By | Metadata/content field | Displays the preparer's name/designation, optionally selected from the local personnel library; ordinary editable content only. |
| META-009 | Reviewed By | Metadata/content field | Displays the reviewer's name/designation, optionally selected from the local personnel library; no review event is generated. |
| META-010 | Approved By | Metadata/content field | Displays the approver's name/designation, optionally selected from the local personnel library; no approval event is generated. |
| META-011 | Document Status | Metadata/content field | Optional, freely editable informational status text or configured label; it never triggers a transition, lock or workflow. |
| META-012 | Revision History Table | Content/data block | A manually editable table included in the document. Internal recovery snapshots are separate and are not shown as formal revisions. |
| META-013 | Distribution List Content | Content/data block | An ordinary editable list/table of intended holders or recipients. No controlled-copy distribution, acknowledgement or notification is managed by the application. |
| META-014 | ISO Clause Reference | Reference field | Stores one or more references to ISO clauses using the shared reference catalogue. |
| META-015 | Document Applicability | Metadata field | Selects applicable laboratory, discipline, department, site or activity from configured values. |

### 4.3 SOP — Standard operating procedure section presets

SOP entries are semantic section presets. They should use the same heading, rich-text, table, formula, list and reference engines as other content. The preset supplies an initial title, help text and optional suggested structure.

| Original ID | Revised canonical name | Kind | Corrected behaviour |
|---|---|---|---|
| SOP-001 | Purpose Section | Template section | Heading plus editable purpose content. |
| SOP-002 | Scope Section | Template section | Heading plus applicability/scope content. |
| SOP-003 | Responsibility Section | Template section | Heading plus role/responsibility content; may reference person/role records. |
| SOP-004 | Definitions Section | Template section | Heading plus repeatable term-definition entries. |
| SOP-005 | Abbreviations Section | Template section | Heading plus abbreviation/meaning entries. |
| SOP-006 | References Section | Template section | Heading plus structured references and optional safe links. |
| SOP-007 | Principle of Method Section | Template section | Heading plus method-principle content. |
| SOP-008 | Required Equipment Section | Template section | Heading plus equipment references or equipment table. |
| SOP-009 | Apparatus and Glassware Section | Template section | Heading plus apparatus list/table. |
| SOP-010 | Chemicals and Reagents Section | Template section | Heading plus chemical/reagent references or table. |
| SOP-011 | Standard Solutions Section | Template section | Heading plus configured solution-preparation content/records. |
| SOP-012 | Safety Precautions Section | Template section | Heading plus safety content; no default hazard classification is inferred. |
| SOP-013 | Environmental Conditions Section | Template section | Heading plus configured conditions and limits. |
| SOP-014 | Sample Requirements Section | Template section | Heading plus sample type, quantity, condition and acceptance information. |
| SOP-015 | Sample Preparation Procedure Section | Template section | Instructional procedure content. Actual sample-preparation observations belong in a technical record block. |
| SOP-016 | Procedure Steps Section | Template section | Ordered, optionally numbered procedure steps. |
| SOP-017 | Calculation Formula Section | Template section | Displays an approved formula definition and explanatory variables; calculation execution belongs to the formula engine. |
| SOP-018 | Result Interpretation Section | Template section | Text and configured interpretation rules/criteria, where specified. |
| SOP-019 | Quality Control Requirements Section | Template section | Describes required QC activities and references configured QC plans/records. |
| SOP-020 | Acceptance Criteria Section | Template section | Displays method-configured acceptance criteria; no universal limits are assumed. |
| SOP-021 | Measurement Uncertainty Section | Template section | Explains/references uncertainty approach and associated records. |
| SOP-022 | Reporting Requirements Section | Template section | Specifies reporting content/format and any configured reporting rules. |
| SOP-023 | Troubleshooting Section | Template section | Problem/cause/action content, preferably in a table. |
| SOP-024 | Records Generated Section | Template section | Lists records produced by the procedure and their identifiers/retention references. |
| SOP-025 | Annexure Section | Template section | Adds appendices/annexures using normal document blocks. |

### 4.4 TBL — Table blocks

All table blocks use one table engine. A table preset defines a default schema and behaviour; it does not require a separate table implementation.

| Original ID | Revised canonical name | Kind | Corrected behaviour |
|---|---|---|---|
| TBL-001 | Basic Table | Structured-domain block | General rows and columns with typed cells. |
| TBL-002 | Data Entry Table | Structured-domain block | Editable tabular inputs with per-column type and validation. |
| TBL-003 | Equipment Table | Structured-domain block | Preset for equipment references/details; column schema configurable. |
| TBL-004 | Chemical Inventory Table | Structured-domain block | Preset for chemicals, grade, identifiers, quantity and configured storage fields. |
| TBL-005 | Reagent Table | Structured-domain block | Preset for reagent/solution identity and preparation attributes. |
| TBL-006 | Sample Details Table | Structured-domain block | Preset for sample identity and sample attributes. |
| TBL-007 | Observation Table | Structured-domain block | Repeatable observations with typed columns and units. |
| TBL-008 | Calculation Table | Structured-domain block | Inputs, intermediate values and outputs tied to a versioned calculation definition. |
| TBL-009 | QC Results Table | Structured-domain block | QC observations and evaluations linked to a QC plan/criteria. |
| TBL-010 | Calibration Table | Structured-domain block | Calibration points/readings and associated instrument/method references. |
| TBL-011 | Personnel Table | Structured-domain block | Person/role references and configured personnel attributes. |
| TBL-012 | Revision History Table Preset | Structured-domain block | Generic table preset only. META-012 is a separate manually editable revision-history table preset; neither block is an application-generated history or version service. |
| TBL-013 | Approval Information Table | Structured-domain/content block | An ordinary editable table for names, designations, dates and optional signature images; no approval events or workflow. |
| TBL-014 | Comparison Table | Structured-domain block | Side-by-side or row-wise comparison of compatible values. |
| TBL-015 | Repeatable Rows Table | Structured-domain block | Add/remove repeatable rows using the shared repeatable-data mechanism. |
| TBL-016 | Conditional Rows Table | Structured-domain block | Displays rows according to rules from the shared condition engine. |
| TBL-017 | Summary Table | Structured-domain block | Displays derived/aggregated values with source references. |
| TBL-018 | Formula-Based Table | Structured-domain block | Table with formula-defined columns/cells evaluated by the formula engine. |

### 4.5 FRM — Form and input controls

| Original ID | Revised canonical name | Kind | Corrected behaviour |
|---|---|---|---|
| FRM-001 | Text Input | Input/control block | Single-line text with length/format constraints. |
| FRM-002 | Multiline Text Input | Input/control block | Multi-line text with length/format constraints. |
| FRM-003 | Number Input | Input/control block | Numeric input with explicit integer/decimal precision rules. |
| FRM-004 | Decimal Input | Input/control block | Decimal input with configured precision and range. |
| FRM-005 | Date Picker | Input/control block | Date-only value with configured allowed range. |
| FRM-006 | Time Picker | Input/control block | Time-only value with configured format/time-zone semantics where relevant. |
| FRM-007 | Date-Time Picker | Input/control block | Date/time value stored with an explicit time-zone policy. |
| FRM-008 | Single-Select Dropdown | Input/control block | One value from a configured option set. |
| FRM-009 | Multi-Select | Input/control block | Multiple values from a configured option set. |
| FRM-010 | Radio Selection | Input/control block | One value from a short configured option set. |
| FRM-011 | Checkbox | Input/control block | Boolean or multi-option selection; schema defines which. |
| FRM-012 | File Upload Control | Input/control block | Selects/imports a file into the shared file-asset service; distinct from displaying an attachment. |
| FRM-013 | Image Upload Control | Input/control block | Selects an image asset; STR-019 displays the selected asset. |
| FRM-014 | Signature Capture Control | Input/control block | Captures typed/drawn signature data where enabled. It does not by itself constitute an approval event. |
| FRM-015 | Initials Input | Input/control block | Captures initials as a typed value or configured mark. |
| FRM-016 | Field-Level Auto ID | Input/control block | Requests a unique value from the numbering service for a field-specific numbering scope. |
| FRM-017 | Calculated Field | Input/control block | Read-only or controlled-output field evaluated by the formula engine from declared inputs. |
| FRM-018 | Conditional Field | Input/control block | Shows/enables a field when a rule-engine condition evaluates true. |
| FRM-019 | Barcode/QR Input or Scanner | Input/control block | Captures/scans/accepts an identifier. Code generation is a separate output capability. |
| FRM-020 | Repeatable Field Group | Input/control block | Repeats a group of fields; distinct from repeating table rows or document sections. |

### 4.6 CHEM — Chemical testing blocks

These are specialized technical presets/composite blocks. Method-specific inputs, units, calculations, limits and reporting rules must be configured against the laboratory's selected method and edition.

| Original ID | Revised canonical name | Kind | Corrected behaviour |
|---|---|---|---|
| CHEM-001 | Chemical Information | Structured-domain block | Chemical identity, grade, supplier/lot and configured hazard/storage references. |
| CHEM-002 | Reagent Preparation Record | Structured-domain block | Records actual reagent preparation, quantities, date, preparer and resulting batch/ID. |
| CHEM-003 | Standard Solution Preparation Record | Structured-domain block | Records preparation inputs, target concentration, final volume and traceability. |
| CHEM-004 | Solution Standardization Record | Structured-domain block | Records standardization observations, calculation and assigned concentration. |
| CHEM-005 | Titration Observation Record | Structured-domain block | Stores titration readings and replicate observations; no formula assumed without method configuration. |
| CHEM-006 | Titration Calculation | Structured-domain block | Evaluates the selected titration method's versioned formula and outputs. |
| CHEM-007 | Gravimetric Observation Record | Structured-domain block | Stores sample/container weights and observation stages. |
| CHEM-008 | pH Measurement Record | Structured-domain block | Stores pH readings, temperature, instrument and calibration references as configured. |
| CHEM-009 | Conductivity Measurement Record | Structured-domain block | Stores conductivity readings, temperature, instrument and cell/calibration references as configured. |
| CHEM-010 | Spectrophotometer Reading Record | Structured-domain block | Stores wavelength, absorbance and relevant instrument/sample details. |
| CHEM-011 | Calibration Curve Dataset | Structured-domain block | Stores calibration standards, levels and response values. VIS-005 renders the graph from this dataset. |
| CHEM-012 | Chemical Recovery Calculation | Structured-domain block | Calculates recovery for a configured chemical/analytical study. QC-005 is the QC spike-recovery assessment profile. |
| CHEM-013 | Dilution Calculation | Structured-domain block | Uses a configured dilution model with explicit units and input validation. |
| CHEM-014 | Concentration Calculation | Structured-domain block | Uses a configured concentration model; method and unit conversions must be explicit. |
| CHEM-015 | Analytical Result | Structured-domain block | Stores final reported analytical value, unit, qualifier, method reference and source calculation/result provenance. |

### 4.7 ENV — Environmental testing blocks

| Original ID | Revised canonical name | Kind | Corrected behaviour |
|---|---|---|---|
| ENV-001 | Sampling Location | Structured-domain block | Stores location description and optional coordinates; coordinates are not inferred. |
| ENV-002 | Sampling Date and Time | Structured-domain block | Stores sample collection time with explicit time-zone policy. |
| ENV-003 | Sampling Conditions | Structured-domain block | Stores configured field/weather/environmental observations. |
| ENV-004 | Sample Collection Record | Structured-domain block | Records collection device, technique, sampler and collection details. |
| ENV-005 | Sample Preservation Record | Structured-domain block | Records preservative, container, preservation time and storage conditions where configured. |
| ENV-006 | Field Measurement Record | Structured-domain block | Stores field readings such as pH or temperature with units and instrument references. |
| ENV-007 | Water Analysis Record | Structured-domain block | Configurable water-analysis observation/result preset; parameters and methods are not hard-coded. |
| ENV-008 | BOD Calculation | Structured-domain block | Executes a selected BOD method/configuration; formula and correction rules must be configured and verified. |
| ENV-009 | COD Calculation | Structured-domain block | Executes a selected COD method/configuration; formula and correction rules must be configured and verified. |
| ENV-010 | TDS Calculation | Structured-domain block | Executes the selected TDS calculation or records an instrument result, as configured. |
| ENV-011 | TSS Calculation | Structured-domain block | Executes a selected TSS method/configuration with configured weighing and blank corrections. |
| ENV-012 | Heavy Metal Results | Structured-domain block | Stores analyte-specific results, units, method, instrument and qualifiers. |
| ENV-013 | Air Monitoring Results | Structured-domain block | Configurable ambient/other air monitoring data schema; sampling context and method must be selected. |
| ENV-014 | Stack Monitoring Results | Structured-domain block | Configurable stack monitoring observations and calculated/reportable values. |
| ENV-015 | Soil Analysis Results | Structured-domain block | Matrix-specific soil results, units, preparation and method references. |
| ENV-016 | Noise Monitoring Results | Structured-domain block | Stores noise observations, measurement settings, location and time context. |

### 4.8 COAL — Coal testing blocks

| Original ID | Revised canonical name | Kind | Corrected behaviour |
|---|---|---|---|
| COAL-001 | Coal Sample Details | Structured-domain block | Sample identity, source, condition, preparation state and relevant metadata. |
| COAL-002 | Coal Sample Preparation Record | Structured-domain block | Records actual crushing/pulverizing/preparation activity. SOP-015 remains the instructional procedure-section preset. |
| COAL-003 | Moisture Observation Record | Structured-domain block | Stores configured tare, sample and drying weights/observations. |
| COAL-004 | Moisture Calculation | Structured-domain block | Applies the selected method's calculation, basis and rounding rules. |
| COAL-005 | Ash Observation Record | Structured-domain block | Stores crucible/sample/final weights and method-specific observation details. |
| COAL-006 | Ash Calculation | Structured-domain block | Applies configured method, basis and calculation rules. |
| COAL-007 | Volatile Matter Observation Record | Structured-domain block | Stores configured mass observations and furnace/time/temperature context. |
| COAL-008 | Volatile Matter Calculation | Structured-domain block | Applies selected method's calculation and corrections, if applicable. |
| COAL-009 | Fixed Carbon Calculation | Structured-domain block | Derives fixed carbon using a configured basis and configured constituent inputs. |
| COAL-010 | Calorific Value Observation Record | Structured-domain block | Stores bomb calorimeter measurements, masses, energy-equivalent data and correction inputs required by the selected method. |
| COAL-011 | Gross Calorific Value Calculation | Structured-domain block | Uses a method-specific, versioned GCV calculation; all corrections and units must be explicit. |
| COAL-012 | Net Calorific Value Calculation | Structured-domain block | Uses a configured NCV model with stated basis and required constituent/moisture inputs. |
| COAL-013 | Total Sulphur Results | Structured-domain block | Stores method-specific sulphur observations and final results. |
| COAL-014 | HGI Results | Structured-domain block | Stores HGI test observations and the selected calculation/standard configuration. |
| COAL-015 | Ash Fusion Results | Structured-domain block | Stores characteristic temperatures and method/instrument details. |
| COAL-016 | Ultimate Analysis | Structured-domain block | Stores configured ultimate-analysis constituents, basis, units and method references. |

### 4.9 EQB — Equipment and calibration blocks

| Original ID | Revised canonical name | Kind | Corrected behaviour |
|---|---|---|---|
| EQB-001 | Equipment Details | Structured-domain block | References or creates an equipment record with unique equipment identity. |
| EQB-002 | Equipment Specifications | Structured-domain block | Stores range, resolution, accuracy and other configured specification fields with units. |
| EQB-003 | Equipment Calibration Details | Structured-domain block | References calibration provider/certificate, dates, scope and results. |
| EQB-004 | Calibration Results | Structured-domain block | Stores calibration observations/results and uncertainty fields where applicable. |
| EQB-005 | Calibration Due Date | Structured-domain block | Stores and displays a user-entered calibration due date as part of an equipment/calibration record. It does not monitor due dates, issue alerts or schedule work. |
| EQB-006 | Intermediate Verification Record | Structured-domain block | Records verification readings, criteria and assessment. |
| EQB-007 | Equipment Maintenance Record | Structured-domain block | Records planned/actual maintenance and outcomes. |
| EQB-008 | Equipment Breakdown Record | Structured-domain block | Records breakdown, impact, action and return-to-service details. |
| EQB-009 | Equipment Usage Log | Structured-domain block | Appends dated usage entries with user/sample/method references as configured. |
| EQB-010 | Equipment Status | Structured-domain block | Stores a user-editable informational equipment-status value within a record. No status-transition workflow or event/audit log is generated. |
| EQB-011 | Calibration Certificate Attachment | Structured-domain block | Associates a file asset with an equipment/calibration record; file handling is shared. |
| EQB-012 | Equipment Acceptance Record | Structured-domain block | Records acceptance checks and authorization before use. |

### 4.10 QC — Quality control and measurement uncertainty blocks

| Original ID | Revised canonical name | Kind | Corrected behaviour |
|---|---|---|---|
| QC-001 | QC Plan | Structured-domain block | Stores QC activity, frequency, sample type, responsible role and configured criteria. |
| QC-002 | Blank Result | Structured-domain block | Stores blank observations/results with method-specific limits. |
| QC-003 | Duplicate Result | Structured-domain block | Stores paired/duplicate observations and configured evaluation. |
| QC-004 | Replicate Result | Structured-domain block | Stores replicate series and configured precision evaluation. |
| QC-005 | Spike Recovery Assessment | Structured-domain block | QC-specific spike recovery using configured spike/expected/result values and acceptance criteria; calculation kernel may be shared with CHEM-012. |
| QC-006 | CRM Result | Structured-domain block | Stores certified/reference value, observed value, uncertainty and configured evaluation. |
| QC-007 | Control Chart Dataset and Rules | Structured-domain block | Stores ordered QC observations, centre/limit/rule configuration and evaluation. VIS-004 is its graphical presentation. |
| QC-008 | QC Acceptance Evaluation | Structured-domain block | Applies configured QC criteria/rules and records pass, fail or review outcome. |
| QC-009 | QC Investigation Record | Structured-domain block | Records out-of-control investigation, impact, cause and disposition. |
| QC-010 | PT Results | Structured-domain block | Stores proficiency-testing round, provider, assigned value, result and related metadata. |
| QC-011 | Z-Score Calculation | Structured-domain block | Evaluates a configured PT scoring definition with declared inputs and rounding. |
| QC-012 | Uncertainty Components | Structured-domain block | Stores uncertainty source, type, distribution, value, unit and supporting evidence. |
| QC-013 | Uncertainty Budget | Structured-domain block | Aggregates component values and sensitivity/conversion data under a selected model. |
| QC-014 | Uncertainty Calculation | Structured-domain block | Computes combined/expanded uncertainty through an explicitly configured model. |
| QC-015 | Traceability Information | Structured-domain block | Stores links to standards, reference materials, calibrations and measurement traceability evidence. |

### 4.11 APR — Approval and signature blocks

| Original ID | Revised canonical name | Kind | Corrected behaviour |
|---|---|---|---|
| APR-001 | Prepared By | Content block/preset | Displays editable preparer name, designation and date; may reuse META-008/person-reference data. No workflow event. |
| APR-002 | Checked By | Content block/preset | Displays editable checker name, designation and date as ordinary document content. No workflow event. |
| APR-003 | Reviewed By | Content block/preset | Displays editable reviewer name, designation and date; may reuse META-009/person-reference data. No workflow event. |
| APR-004 | Approved By | Content block/preset | Displays editable approver name, designation and date; may reuse META-010/person-reference data. No workflow event. |
| APR-005 | Signature Display/Capture | Content/input block | Uses the shared typed/drawn signature capture or an image asset as ordinary document content. It does not authenticate a signer or create a legally verified electronic signature. |
| APR-006 | Signature/Approval Date Field | Content/input block | Displays a user-entered date associated with a prepared/checked/reviewed/approved section. No event timestamp is generated. |
| APR-007 | Approval Sequence Section | Template section/content preset | Provides a static, editable sequence/table for prepared, checked, reviewed and approved information. It does not execute a workflow. |
| APR-008 | Authorization Details | Content/data block | Presents manually maintained authorization information as document content or a local reference; no permission enforcement. |
| APR-009 | Report Authorization Section | Content block/preset | Presents the report signatory name, designation, date and optional signature as content; does not authorize or release a report. |
| APR-010 | Revision History/Approval Note | Content block/preset | Provides an editable section to describe a revision and its stated approval information; no revision approval enforcement. |

### 4.12 NABL — Compliance and accreditation blocks

| Original ID | Revised canonical name | Kind | Corrected behaviour |
|---|---|---|---|
| NABL-001 | ISO Clause Reference | Reference field | Reuses the reference catalogue; may point to an ISO clause. It is not a separate clause-data model from META-014. |
| NABL-002 | NABL Requirement Reference | Reference field | References a specific NABL document/requirement and edition/version where recorded. |
| NABL-003 | Compliance Status | Structured-domain block | Stores a locally assessed status against a specific requirement and evidence set. |
| NABL-004 | Evidence Reference | Structured-domain block | Links to a document, record, attachment or other evidence item. |
| NABL-005 | Assessment Finding | Structured-domain block | Records a finding, source, clause/requirement, evidence and disposition. |
| NABL-006 | Nonconformity Details | Structured-domain block | Stores NC statement, evidence, classification and linked records as configured. |
| NABL-007 | Root Cause Analysis | Structured-domain block | Records investigation method, cause and supporting rationale. |
| NABL-008 | Corrective Action | Structured-domain block | Provides editable action description, responsible-person text, user-entered target date, implementation evidence and informational status fields; no CAPA task assignment, reminders or workflow. |
| NABL-009 | Effectiveness Verification | Structured-domain block | Records verifier, evidence, outcome and verification date. |
| NABL-010 | Scope of Accreditation | Structured-domain block | Stores scope entries such as discipline, matrix, parameter, method, range and location as configured. |

### 4.13 VIS — Charts, graphs and visualization blocks

| Original ID | Revised canonical name | Kind | Corrected behaviour |
|---|---|---|---|
| VIS-001 | Line Chart | Visualization block | Generic line-chart renderer for compatible numeric series. |
| VIS-002 | Bar Chart | Visualization block | Generic bar-chart renderer. |
| VIS-003 | Pie Chart | Visualization block | Generic proportion chart; use only when the input data is suitable for part-to-whole representation. |
| VIS-004 | Control Chart Renderer | Visualization block | Renders QC-007 data and configured limits/rules; does not independently own the QC observations. |
| VIS-005 | Calibration Curve Renderer | Visualization block | Plots CHEM-011 calibration dataset and configured fit/curve details. |
| VIS-006 | Scatter Plot | Visualization block | Generic x/y scatter renderer. |
| VIS-007 | Histogram | Visualization block | Displays a distribution using configured binning. |
| VIS-008 | Trend Chart | Visualization block | Time-ordered line/trend configuration with date/time axis and optional trend analysis. Shares chart rendering infrastructure with VIS-001. |
| VIS-009 | Image/Diagram Display | Visualization block | Displays a static diagram/image asset; FRM-013 is the upload control and STR-019 is the general inline image content block. |
| VIS-010 | Flowchart | Visualization block | Displays a structured process diagram; data/editing format must be defined separately from image upload. |

### 4.14 AUTO — Automation capabilities and service interfaces

AUTO entries are capabilities/services, not automatically independent document blocks. Only those with a user-visible editor or output need a block-library entry.

| Original ID | Revised canonical name | Kind | Corrected behaviour |
|---|---|---|---|
| AUTO-001 | Calculation Runtime | Application service | Executes approved calculation definitions; consumed by FRM-017, TBL-018 and technical calculation blocks. |
| AUTO-002 | Formula Definition and Evaluation Engine | Application service | Validates/parses formula definitions, resolves declared inputs and evaluates versioned formulas. |
| AUTO-003 | Conditional Rule Engine | Application service | Evaluates logical conditions against typed field/data values. |
| AUTO-004 | Local Data Lookup/Autofill | Application service/interface | Resolves data from explicitly configured local records; no server/database fetch is assumed. |
| AUTO-005 | Linked Document Reference | Input/control or content interface | Links a document by stable internal ID and displays selected metadata; snapshot/live-link behaviour is configured. |
| AUTO-006 | Linked Register Reference | Input/control or content interface | Links a register and selected record/entry by stable ID. |
| AUTO-007 | Document Numbering Service | Application service | Generates document identifiers according to configurable prefix, scope, sequence and collision rules. |
| AUTO-008 | Automatic Document Revision Numbering | Excluded capability | Formal automatic document-revision lifecycle is outside scope. Preserve this ID as an excluded legacy capability; user-entered META-003 values and ordinary recovery snapshots remain available. |
| AUTO-009 | Date/Time Default Provider | Application service | Supplies configured current date/time defaults; stored value and its provenance must be clear after entry. |
| AUTO-010 | Approval Trigger | Excluded capability | Application-triggered approval workflows and transitions are excluded. Approval information may still be authored as ordinary content through APR/META blocks. |
| AUTO-011 | Notification/Reminder Service | Excluded capability | Product-level reminders, approval notifications and operational alerts are excluded. Ordinary static reminder text may be authored in documents. |
| AUTO-012 | QR Code Generator | Application service/interface | Generates QR/barcode output from a configured payload; distinct from FRM-019 scanning/input. |
| AUTO-013 | Data Validation Engine | Application service | Applies schema, field, table, cross-field and domain validation. |
| AUTO-014 | Conditional Visibility Renderer | Application service | Applies AUTO-003 rules to visible/enabled state; not a second rule engine. |
| AUTO-015 | Repeatable Section Mechanism | Application service/interface | Repeats a document section; distinct from FRM-020 field groups and TBL-015 rows. |
| AUTO-016 | Cross-Document Data Linking | Deferred/excluded capability | Cross-document formula/data dependencies are outside confirmed formula scope. Simple user-selected document references may be represented as ordinary links/content; no live cross-document computation. |
| AUTO-017 | Audit Trail Reference | Excluded application service | DocBuilder does not create an application-managed audit trail. A user may author an ordinary reference or table about an external audit record, but this ID does not create event logging. |
| AUTO-018 | AI Content Generator | Excluded capability | AI functionality and external AI integrations are excluded from the product. Retain the original ID only for catalogue traceability. |

---

### 4.15 Capability-state register for legacy service IDs

The following statuses resolve the original automation entries against the accepted product decisions. All original IDs remain available for traceability; excluded entries must not appear as active UI controls or runtime services.

| Original ID | Revised state | Product treatment |
|---|---|---|
| AUTO-001 | Active — shared service | Local calculation execution runtime. |
| AUTO-002 | Active — shared service | Safe formula definition, parsing and evaluation; no arbitrary code execution. |
| AUTO-003 | Active — shared service | Conditional rule evaluation within supported document/template contexts. |
| AUTO-004 | Active — shared service | Autofill from explicitly selected local master-data/register records. |
| AUTO-005 | Active — content/interface | User-selected link/reference to another local document; no automatic formula dependency. |
| AUTO-006 | Active — content/interface | User-selected reference to a local register/record. |
| AUTO-007 | Active — shared service | Configurable identifier generation for documents/fields where enabled; not revision control. |
| AUTO-008 | Excluded | Automatic document revision lifecycle/numbering. User-entered revision content remains possible. |
| AUTO-009 | Active — shared service | Current date/time defaults, captured as values when inserted. |
| AUTO-010 | Excluded | Approval triggers, workflow transitions and approval events. |
| AUTO-011 | Excluded | Reminder, notification and operational alert services. |
| AUTO-012 | Active — shared service/interface | Local QR/barcode generation for supported payloads. |
| AUTO-013 | Active — shared service | Schema, input, table and calculation validation. |
| AUTO-014 | Active — shared service | Applies conditional visibility from AUTO-003. |
| AUTO-015 | Active — shared service/interface | Repeatable document sections within the current document. |
| AUTO-016 | Deferred | General cross-document data-linking capability beyond simple explicit document/register references; no cross-document formulas in the confirmed scope. |
| AUTO-017 | Excluded | Application-managed audit/event trail. |
| AUTO-018 | Excluded | AI content generation or external AI integration. |

All non-AUTO original entries are retained as content, input, structured-data or visualization definitions. Their implementation priority is governed by the roadmap and selected method/template packages; their presence in this catalogue is not a promise that every block ships in the first release.

---

## 5. Definitive overlap and duplicate-resolution register

This section closes the open questions raised in Appendix A. The entries remain separate where they serve different roles; shared services provide common behaviour.

| Overlap identified | Corrected relationship | Decision |
|---|---|---|
| Document Title: STR-001 / META-002 | META-002 owns the canonical document-title value; STR-001 renders it. STR-001 may be fixed text only where a template explicitly allows an override. | **Shared value, separate metadata and display roles.** |
| Revision History: META-012 / TBL-012 | META-012 is a manually editable revision-history table. TBL-012 is a generic table preset that may be configured for other history-style content. Neither creates application-managed document revision history. | **Manual document content; internal recovery is separate.** |
| Prepared/Reviewed/Approved: META-008/009/010 and APR-001/003/004 | META fields and APR presets display editable names, designations and dates. APR-005/FRM-014 may capture a typed/drawn signature as ordinary content. No approval event, actor authentication or workflow state exists. | **Shared person references; content only.** |
| ISO Clause Reference: META-014 / NABL-001 | Both use the same reference catalogue. META-014 is document metadata; NABL-001 is a compliance/reference interface. | **One reference model, multiple contexts.** |
| Control Chart: QC-007 / VIS-004 | QC-007 owns observations, limits, rules and evaluations. VIS-004 renders a chart from that dataset. | **Data model plus visualization.** |
| Calibration Curve / Calibration Graph: CHEM-011 / VIS-005 | CHEM-011 owns calibration points and fit metadata. VIS-005 renders the graph. | **Data model plus visualization.** |
| Line Chart / Trend Graph: VIS-001 / VIS-008 | Both use one chart renderer. VIS-008 adds time-series defaults and optional trend-specific configuration. | **Separate configurations, shared renderer.** |
| Sample Preparation: SOP-015 / COAL-002 | SOP-015 describes instructions. COAL-002 records an actual coal sample-preparation activity. | **Procedure versus execution record.** |
| Recovery: CHEM-012 / QC-005 | Share calculation functions where mathematically identical, but retain distinct schemas, contexts, criteria and reporting. | **Shared calculation kernel, separate domain blocks.** |
| Image: STR-019 / FRM-013 / VIS-009 | FRM-013 selects/uploads an image; STR-019 displays an inline image; VIS-009 displays a diagram/visualization. All use the file-asset service. | **Upload versus content display versus visualization.** |
| Signature: FRM-014 / APR-005 | FRM-014 captures typed/drawn signature content. APR-005 places that content in a prepared/checked/reviewed/approved section; no identity assurance or approval event is implied. | **Shared capture component, document content only.** |
| Barcode/QR: FRM-019 / AUTO-012 | FRM-019 scans/accepts an identifier; AUTO-012 generates a code from a configured payload. | **Input/scanning versus generation.** |
| Auto ID: FRM-016 / AUTO-007 | FRM-016 requests a field-level ID; AUTO-007 provides numbering rules/service for document IDs and other scopes. | **Shared numbering service, different scopes.** |
| Calculation: FRM-017 / TBL-018 / AUTO-001 / AUTO-002 | FRM-017 is a calculated field; TBL-018 is a table preset with formula cells; AUTO-001 executes; AUTO-002 defines/parses/evaluates formulas. | **Field/table interfaces use common calculation services.** |
| Repeatable: FRM-020 / AUTO-015 / TBL-015 | FRM-020 repeats fields; TBL-015 repeats rows; AUTO-015 repeats document sections. All use common repeatable-data concepts but distinct renderers and scopes. | **Three distinct repeat scopes, shared mechanism.** |
| Conditional: FRM-018 / TBL-016 / AUTO-003 / AUTO-014 | FRM-018 controls field visibility; TBL-016 controls row visibility; AUTO-003 evaluates rules; AUTO-014 applies results to rendering. | **One rule engine, multiple targets.** |
| Attachment/File Upload: STR-020 / FRM-012 / EQB-011 | FRM-012 uploads/selects; STR-020 displays an attachment; EQB-011 associates a certificate asset with equipment/calibration data. All use one file-asset service. | **Shared storage, distinct user-facing functions.** |

---

## 6. Corrected P1/P2 mapping

The former “recommended master block library” is a priority shortlist, not a second catalogue. Its names should map to the stable block definitions above. A priority label does not create another block ID.

### 6.1 P1 mapping

| Former P1 name | Canonical mapping |
|---|---|
| Heading | STR-003 / STR-004 |
| Rich Text | STR-005 |
| Bullet List | STR-007 |
| Numbered List | STR-006 |
| Basic Table | TBL-001 |
| Dynamic Table | TBL-002 plus TBL-015 where repeatable rows are required |
| Text Field | FRM-001 |
| Number Field | FRM-003 |
| Decimal Field | FRM-004 |
| Date Picker | FRM-005 |
| Dropdown | FRM-008 |
| Checkbox | FRM-011 |
| File Upload | FRM-012 |
| Image Upload | FRM-013 |
| Purpose | SOP-001 |
| Scope | SOP-002 |
| Responsibility | SOP-003 |
| Definitions | SOP-004 |
| References | SOP-006 |
| Procedure Steps | SOP-016 |
| Equipment List | SOP-008 and/or TBL-003 |
| Chemical/Reagent List | SOP-010 and/or TBL-004/TBL-005 |
| Calculation Formula | SOP-017 plus AUTO-001/AUTO-002 as needed |
| Observation Table | TBL-007 |
| Result Table | TBL-017 or a configured TBL-001/TBL-002 schema; “Result Table” is a preset, not a separate original ID |
| QC Requirements | SOP-019 and QC-001 |
| Prepared/Reviewed/Approved | META-008/009/010 plus APR content presets; no workflow |
| Revision History | META-012 as a manually editable revision-history table; TBL-012 remains a generic table preset |
| Document Header | STR-017 |
| Document Footer | STR-016 |

### 6.2 P2 mapping

| Former P2 name | Canonical mapping |
|---|---|
| Sample Details | TBL-006 and/or COAL-001 / ENV-001..ENV-005 as applicable |
| Equipment Details | EQB-001 / EQB-002 |
| Calibration Details | EQB-003 / EQB-004 |
| Reagent Preparation | CHEM-002 / CHEM-003 / CHEM-004 |
| Titration Observation | CHEM-005 |
| BOD/COD Calculation | ENV-008 / ENV-009 |
| Coal Proximate Analysis | COAL-003..COAL-009, grouped in a template; not a new individual block ID |
| Measurement Uncertainty | QC-012..QC-014 and SOP-021 |
| Control Chart | QC-007 plus VIS-004 |
| Risk Assessment | No dedicated original block ID. Use a structured table/form initially; add a new ID only through a controlled catalogue revision. |
| Nonconformity | NABL-006 and/or CMP/audit document templates for user-authored findings, evidence and investigation content; no application-managed NC workflow. |
| Corrective Action | NABL-008 plus ordinary editable action/evidence fields in forms and registers; no application-managed CAPA tasks, assignments, reminders or effectiveness workflow. |
| Signature | FRM-014 for typed/drawn content; APR-005 for display in a document section; no verified electronic signature |
| ISO Clause Reference | META-014 / NABL-001 |
| Conditional Section | AUTO-003/AUTO-014/AUTO-015 (local document visibility/repeat behavior only) |
| Formula Engine | AUTO-001/AUTO-002 |
| Linked Document | AUTO-005 |
| Linked Register | AUTO-006 |
| QR Code | AUTO-012 |
| Chart / Graph | VIS-001..VIS-008; original list has no single `Chart / Graph` ID |

---

## 7. Data model and local storage requirements

The application is backend-free and single-user. All lookups resolve against local data. Internal identifiers must be separate from displayed laboratory document numbers.

### 7.1 Minimum local entities

| Entity | Minimum purpose/fields |
|---|---|
| Workspace | Workspace UID, schema version, preferences, created/modified timestamps |
| Document | Internal UID, displayed document ID (optional), title, category/type, content tree, template-origin reference, optional free-text status, created/modified timestamps |
| Template | Template UID, title, category/type, current template-version UID, tags and timestamps |
| TemplateVersion | Version UID, template UID, schema/content definition, compatibility metadata, creation timestamp and change summary; this is template maintenance, not document control |
| BlockDefinition | `block_uid`, legacy ID, canonical name, schema/version, renderer references, capabilities and lifecycle state |
| BlockInstance | Instance UID, document/template UID, block UID/version, order, settings, content/value and permitted local data references |
| CustomBlockSchema | Schema UID/version, declarative fields, layout, validations, supported formula bindings and compatibility metadata |
| PersonRecord | Stable local UID, display name, designation and optional department/contact fields |
| MasterDataRecord | Category, stable UID, typed values, source/reference fields and update timestamps |
| Register | Register UID, title, record schema/version and associated document/template references |
| RegisterRecord | Record UID, register UID, schema version, values, state and timestamps |
| CompletedRecordSnapshot | Snapshot UID, source record/document UID, captured content/values, method/formula/schema/template references and completion timestamp |
| AmendedRecordLink | Original snapshot UID, amended record UID, reason/reference and amendment timestamp |
| RecoverySnapshot | Snapshot UID, source entity UID, saved state, created timestamp and recovery metadata; not a formal document revision |
| TechnicalMethodConfig | Method identifier/name, standard and edition, matrix, parameter, units, formula version, configured limits and validation profile |
| TableSchema | Column IDs, labels, types, units, required state, validation, formulas and display settings |
| FormulaDefinition | Formula ID/version, expression or structured formula, inputs, units, output, rounding and validation rules |
| ReferenceItem | Reference type, issuing body, identifier, title, edition/version/date and optional locator |
| FileAsset | Asset UID, filename, media type, size, hash, local blob/storage key, metadata and preview status |
| LocalLink | Source entity/UID, target entity/UID, link mode and optional captured snapshot values |

Do not create `ApprovalEvent` or application-managed `AuditEvent` entities as part of the confirmed product scope. The application may preserve ordinary user-authored names/dates and technical record values, but it does not attest to approval or provide an audit-trail service.

### 7.2 Local relationships

- A document may originate from a specific template version; later template edits do not alter it automatically.
- A template has one or more template versions; users may preview and selectively apply a newer version to a document.
- A document contains ordered block instances referencing versioned block definitions.
- A block instance may reference local master-data or register records using stable IDs.
- A completed-record snapshot captures the values and configuration needed to preserve what was completed.
- An amended record links back to the original completed snapshot and does not overwrite it.
- A file asset is stored once and may be referenced from multiple permitted documents/records.
- A formula may reference fields/calculations in other blocks within the same document only.

### 7.3 Live reference versus captured snapshot

Every master-data relationship must declare its behaviour:

- **Live reference:** display current source values in a working document where this is appropriate.
- **Captured snapshot:** retain the values used at the time of completion/export where historical reproducibility is required.
- **Reference plus snapshot:** retain the source UID and captured values together, so the origin is traceable while the completed record remains stable.

The exact default for each master-data category remains a detailed specification item. Completed records must preserve the values actually used, even if the source equipment/person/method record is later edited.

### 7.4 Persistence and backup

- Use IndexedDB or an equivalent structured local store for records and binary assets; localStorage is limited to small preferences/UI state.
- Cache application shell and required offline assets through the PWA service worker.
- JSON backup must carry a schema/version envelope and stable internal IDs; attachment inclusion/encoding must be explicit.
- ZIP backup must include structured data, a manifest, original attachments and integrity checksums.
- Import must validate schema version, IDs, relationships and assets before changing the workspace.
- Restore must offer a safe, explicit replace/merge strategy once its detailed rules are specified.
- Migrations must be versioned and non-destructive; preserve/offer a backup before migration.
- Browser storage is not a substitute for independent backups and is not described as tamper-proof storage.

---

## 8. Document content, recovery and completed-record behaviour

### 8.1 Optional informational status

META-011 is optional content. It may use a user-editable text value or a user-configured label set. The application must not impose a fixed approval lifecycle. A value such as “Approved”, “Draft” or “Obsolete” is informational only and must not lock a document, change permissions, initiate notifications or trigger transitions.

### 8.2 Names, designations, dates and signatures

META-008/009/010 and APR-001..APR-010 provide editable document content/presets. A user may enter or select names/designations and dates and may insert typed/drawn signature content or an image. The application does not authenticate the signatory, verify identity, attest that an approval occurred or claim compliance with a legally recognized electronic-signature regime.

### 8.3 Manual revision-history content

META-003 is an optional user-entered revision label. META-012 is a manually editable revision-history table. The application does not automatically enforce revision numbering, authorize revisions, distribute controlled copies or maintain an authoritative formal revision lifecycle.

### 8.4 Internal recovery snapshots

Recovery snapshots are generated for accidental-change/failure recovery. They are internal technical recovery points, not a formal document revision history. Their retention, frequency, storage limits and restore UX require detailed specification.

### 8.5 Completed-record snapshots and amendments

A user can mark a supported record/document instance as completed. Completion captures the record values and required source/configuration snapshots. Subsequent changes to the completed record must create a separate amended copy/record linked to the original; they must not silently overwrite the original snapshot. This feature is a data-integrity convenience, not a claim of legal immutability or tamper-proof storage. The completion action, allowed metadata changes, deletion/retention behaviour and amendment reason requirements remain to be specified.

---

## 9. Technical calculations and validation — safe specification

The original catalogue names calculation types but does not provide enough information to implement validated laboratory formulas. This revision deliberately does **not** invent formulas, corrections, units, limits or acceptance criteria.

### 9.1 Required calculation-definition fields

Every operational calculation should be configured with:

- Calculation ID and version.
- Test method/standard identifier and edition, where applicable.
- Matrix/sample type and parameter.
- Input field identifiers and units.
- Formula or structured calculation steps.
- Required correction terms and their sources.
- Output field, unit and reporting basis.
- Significant figures/rounding policy.
- Input range/type checks and missing-value behaviour.
- Formula test vectors with expected outputs and tolerances.
- Reviewer/approver and effective date for the configuration, if the lab's governance requires it.

### 9.2 Calculation families requiring method configuration

| Family | Catalogue IDs | Configuration required before release |
|---|---|---|
| Titration | CHEM-005, CHEM-006 | Method-specific factors, sample amount, titrant concentration/standardization, blanks/dilutions and reporting basis |
| Recovery | CHEM-012, QC-005 | Definitions of native/spiked amounts, spike amount, dilution/basis and acceptance criteria |
| Dilution/concentration | CHEM-013, CHEM-014 | Units, dilution stages, significant figures and permissible ranges |
| BOD/COD | ENV-008, ENV-009 | Exact method, blank/sample values, dilution factors, seed/control corrections where applicable, units and report basis |
| TDS/TSS | ENV-010, ENV-011 | Method/instrument distinction, sample volume, mass/blank corrections, drying/conditioning and units |
| Coal moisture/ash/volatile matter/fixed carbon | COAL-003..COAL-009 | Applicable standard/edition, weighing sequence, calculation basis, correction terms and reporting precision |
| Coal GCV/NCV | COAL-010..COAL-012 | Applicable standard/edition, calorimeter energy equivalent, sample mass, all required corrections, moisture/hydrogen basis, unit conversion and rounding |
| Z-score | QC-011 | Assigned value, standard deviation for proficiency assessment and provider/scoring convention |
| Measurement uncertainty | QC-012..QC-014 | Measurement model, input distributions, sensitivity coefficients, correlations, coverage factor and reporting policy |

Do not mark any calculation as validated merely because the software returns a number. Each configured calculation needs verified test vectors and domain review by an authorized technical user.

### 9.3 Units and acceptance criteria

- Store a numeric value separately from its unit.
- Use a controlled unit catalogue and explicit conversion definitions.
- Preserve the original observed value/unit as entered, even when a normalized value is also calculated.
- Distinguish method limits, laboratory acceptance criteria, instrument range and reporting limits.
- Do not hard-code a generic range or universal recovery window across all parameters/methods.
- Store qualifier values (for example, below a configured reporting limit) separately from the numeric result.

---

## 10. Technical block schemas — minimum common contract

Specialized CHEM/ENV/COAL/EQB/QC blocks may have their own detailed schemas, but each should follow a common envelope.

| Common field | Purpose |
|---|---|
| `record_uid` | Stable identity of the technical record |
| `sample_uid` / `equipment_uid` | Link to the relevant sample/equipment where applicable |
| `method_config_uid` | Exact method/configuration used |
| `observed_at` | Observation date/time, with time-zone policy |
| `performed_by_uid` | Person reference for the activity |
| `reviewed_by_uid` | Optional person reference displayed as record content, where applicable; does not imply a workflow event |
| `raw_observations` | Original observations without destructive normalization |
| `calculated_values` | Output values with calculation-definition/version references |
| `units_and_basis` | Units and reporting basis for each value |
| `instrument_references` | Equipment/instrument IDs and relevant status/calibration references |
| `quality_control_references` | Blank/duplicate/CRM/PT/QC records as applicable |
| `attachments` | File-asset references |
| `record_state` | Working/completed state used for the completed-record snapshot behaviour; no approval workflow state |
| `snapshot/provenance` | References and captured configuration needed to reproduce the result |

This is a common envelope, not a claim that every technical block uses every field. Each block schema selects the applicable fields.

---

## 11. File and image handling

The original specification does not define accepted file types, size limits or storage behaviour. Those values must be set through application configuration and tested against the target environment; they should not be assumed in this catalogue.

Minimum requirements:

1. Store filename, media type, byte size, checksum/hash, creation time and originating record.
2. Define an explicit allowlist for importable file types; do not trust filename extension alone.
3. Configure maximum file size and total local-storage quota behaviour.
4. Preserve the original file; generated previews/thumbnails are derivatives.
5. PDF export must define how each attachment is represented: embedded image/page, attachment appendix, filename/reference, or hyperlink. Do not silently omit it.
6. Unsupported files must remain identifiable as attachments with a clear export notice.
7. The file-asset service is shared by FRM-012, FRM-013, STR-019, STR-020, EQB-011 and VIS-009.

---

## 12. Rendering and PDF requirements

Each block definition must identify its renderers and pagination behaviour.

Minimum rendering contract:

- A4 page layout as the initial paper preset; page size/margins must be configurable.
- Header/footer fields resolve from the document content/metadata state being rendered; completed-record output uses the captured snapshot values.
- Tables define whether rows can split, whether headers repeat, and how oversized rows are handled.
- Images/diagrams define maximum dimensions, aspect-ratio behaviour and page-break behaviour.
- Content blocks define keep-with-next/keep-together behaviour where practical.
- Page breaks are explicit layout instructions; they must not create duplicate blank pages.
- Long blocks must continue across pages or present a defined overflow treatment.
- Formula outputs and units must remain together and legible.
- Charts must render with readable labels/legends and not rely solely on colour to convey meaning.
- The print/PDF renderer must be tested independently from the interactive editor.
- Every supported block needs a rendering test covering normal, long-content and boundary/overflow cases.

A document preview is not considered print-verified until exported output has been inspected.

---

## 13. Applicability rules

A block should be available according to its architecture kind and configured applicability, not only a hard-coded category check.

Initial recommended policy:

| Block group | Default availability |
|---|---|
| STR structure/content blocks | All document categories, subject to template policy |
| META metadata | All documents; fields shown/required according to document type and lab configuration |
| SOP section presets | SOP/procedure templates by default; reusable in other documents where appropriate |
| TBL generic tables | All document categories |
| FRM input controls | Forms, worksheets, registers and fillable documents; may be used in other templates |
| CHEM technical blocks | Chemical methods/records; optionally reusable by another discipline if configured and justified |
| ENV technical blocks | Environmental sampling/testing records and applicable reports |
| COAL technical blocks | Coal/coke methods and records |
| EQB equipment blocks | Equipment/calibration/maintenance records and linked document templates |
| QC blocks | QC plans, technical records, PT/ILC and uncertainty contexts according to block purpose |
| APR content presets | Documents containing prepared/checked/reviewed/approved names, designations, dates or signature content; no workflow |
| NABL/compliance blocks | User-authored compliance matrices, assessment checklists and evidence documents; no app-managed compliance dashboard, CAPA workflow or accreditation tracking |
| VIS visualization blocks | Any document whose data and output format are compatible |
| AUTO services | Only in-scope local capabilities (formula, conditional rules, local lookup, numbering, date defaults, QR generation, validation and repeatable sections). AUTO-008, AUTO-010, AUTO-011, AUTO-016, AUTO-017 and AUTO-018 are excluded/deferred as described in Section 4.14. |

Templates may restrict availability. The block definition should not be duplicated just because it is used in multiple categories.

---

## 14. Detailed specifications still required

The product-level decisions on PWA delivery, single-user/local storage, no document control/workflow/AI, template behaviour, completed-record snapshots, register model, formula scope and exports are already recorded in `project_decisions.md`. The following are detailed specifications, not unresolved product-scope decisions.

| ID | Detailed item | Information/verification needed |
|---|---|---|
| DS-01 | Initial method catalogue | Owner/laboratory-approved priority method and standard editions for QMS/general, chemical, environmental and coal/coke content. |
| DS-02 | Method-specific calculations | Approved sources, exact formulas, corrections, units, basis, rounding, worked examples and technical verification evidence. |
| DS-03 | Specialized table schemas | Existing approved worksheets/forms, field names/types, mandatory values, units and relationships. |
| DS-04 | Acceptance criteria and limits | Method- and laboratory-specific criteria, ranges, reporting limits and qualifier rules; no generic values assumed. |
| DS-05 | Master-data schema details | Fields and live-reference/snapshot defaults for personnel, equipment, chemicals, reagents, standards, sample matrices and units. |
| DS-06 | File support and limits | Importable formats, size limits, storage/quota warnings, image formats and PDF representation of attachments. |
| DS-07 | Signature content | Whether typed/drawn signatures are sufficient as ordinary content or if additional legal-signature requirements are ever needed. No verification is currently in scope. |
| DS-08 | QR payload/scanning | Supported payload structure and whether scanner input is keyboard-based, camera-based or both. |
| DS-09 | Recovery and retention | Recovery snapshot frequency/retention, completed-record deletion/retention policy and amendment fields. |
| DS-10 | First-release implementation tiers | Which native blocks and detailed templates are delivered in each work package while ensuring all four initial disciplines receive functional support. |
| DS-11 | Custom schema language | Supported field types, conditional expressions, formulas, layout primitives and schema migration rules. |
| DS-12 | Renderer support matrix | Exact preview/PDF/DOCX/print support and fallback behaviour for each block type. |

No AI provider/model, approval-workflow routing, application audit-trail design or cross-document formula scope is awaiting specification; those capabilities are outside the accepted product scope.

---

## 15. Implementation acceptance criteria

The catalogue/architecture work is complete when all of the following are true:

- Every original ID in the source block list is present in Section 4 or has an explicit migration/deprecation record.
- Every block has a canonical name, architecture kind, description and definition version.
- Block identifiers are isolated from document-number identifiers.
- All overlap groups in Section 5 have an explicit resolution.
- Every legacy P1/P2 shortlist item maps to a canonical block/service or is explicitly identified as a preset/future capability; it does not override the release scope in `project_roadmap.md`.
- No technical formula, unit, limit or acceptance criterion is inserted without an approved method/lab configuration.
- Technical records retain method/configuration and calculation-version provenance.
- Historical finalized records cannot silently change when linked source records are edited.
- File assets are stored once and referenced consistently.
- Every supported block has an editor/fill/preview/PDF rendering contract appropriate to its role.
- Unresolved questions remain visible and are not represented as completed requirements.

---

## 16. Source traceability and limitations

This revision is derived from the three planning files present on the repository's `main` branch at the time of review. It normalizes and clarifies the block catalogue; it does not replace the document master list or the frontend screen catalogue.

The following need separate review before being treated as final product requirements:

- The detailed category/document inventory in `document_master_list.md`.
- The screen and navigation decisions in `frontend_design.md`.
- The actual standard/method catalogue and approved laboratory worksheets.
- Any regulatory/legal requirements relating to electronic signatures, records, retention or accreditation claims.

**Recommended use:** review and approve this document as the architectural baseline, then update the original `block_list.md` and the frontend design to reference this canonical catalogue. Do not begin implementing every technical block from its name alone.
