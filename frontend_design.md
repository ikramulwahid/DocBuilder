# DocBuilder — Frontend Design and Interaction Specification

**Document status:** Revised draft for owner review  
**Revision:** 2.0  
**Prepared:** 13 November 2026  
**Product authority:** `project_details.md`  
**Decision authority:** `project_decisions.md`  
**Related catalogues:** `block_list.md`, `document_master_list.md`

---

## 1. Purpose and design principles

This file defines how DocBuilder should look and behave. It replaces the original frontend proposal wherever that proposal conflicts with the accepted product scope.

DocBuilder is a local-first document authoring and laboratory record-building PWA. Its interface should be professional, precise and optimized for people creating structured laboratory documents, worksheets, forms and registers.

### 1.1 Core UX principles

1. Make document creation and editing the primary experience.
2. Keep the user in control of document content, metadata and informational status fields.
3. Separate Design, Fill and Preview modes clearly.
4. Reuse the same underlying document representation in editor, preview, print and export.
5. Show calculation inputs, units, formula sources and validation outcomes clearly.
6. Never imply that DocBuilder has approved, authorized, controlled or verified a document merely because it displays an approval field or status label.
7. Keep essential application functions available offline after first load and cache preparation.
8. Make local save state and backup status visible without introducing cloud/account features.
9. Prefer reusable components and schemas over one-off screens for individual document types.
10. Avoid operational laboratory management features that are outside product scope.

### 1.2 Product boundary reflected in the UI

The UI includes document/template authoring, completed records, registers, local master data, formulas, technical content, preview, export and backup. It does not include user accounts, roles, approval routing, notification centres, controlled-document distribution, obsolete archives, sample-lifecycle Kanban, equipment alerts, CAPA task management, analytics dashboards or AI.

A user may create documents about any of those subjects using generic blocks and templates. That does not create the corresponding operational management system.

---

## 2. Design foundations

### 2.1 Visual direction

Use a clean, calm, professional laboratory-workspace aesthetic. The interface may be information-dense, but tables, formulas and field labels must remain readable.

### 2.2 Themes

| Theme | Purpose | Behaviour |
|---|---|---|
| Light — Lab Clean | Default working theme | White surfaces, soft grey-blue app background |
| Dark | Low-light working | Dark application chrome; document page remains white in print/preview |
| Print / Paper Preview | Output inspection | A4 page appearance without editor chrome |
| High Contrast | Accessibility option | Strong contrast and non-colour-only state indicators; final WCAG target to be specified |

### 2.3 Colour system

Suggested initial palette:

- Primary: deep teal-blue `#0B5C8E` for actions, selection and focus.
- Application background: soft grey-blue `#F3F6F9` in light mode.
- Surfaces: white cards/canvas with slate text and borders.
- Validation: distinguish errors, warnings and informational messages using text/icons as well as colour.
- Formula-derived values: visually distinct but still legible in monochrome printing.

Document appearance must be configurable separately from application theme.

### 2.4 Typography

| Use | Suggested fonts |
|---|---|
| Application UI | Inter or Segoe UI |
| Document body | Arial or Calibri, configurable per template |
| IDs/formulas/technical identifiers | JetBrains Mono or IBM Plex Mono |

Fonts must have suitable fallback stacks and export-safe handling.

### 2.5 Layout and density

- Desktop-first responsive layout.
- Collapsible left navigation and a compact top bar.
- 8-point spacing grid and restrained corners/shadows.
- Comfortable and Compact density options.
- Tablet layout should preserve the editing workflow where practical.
- Mobile layout should support document viewing and basic record entry where feasible; exact mobile support is a detailed-specification item.

### 2.6 Keyboard shortcuts

| Shortcut | Action |
|---|---|
| `/` | Open block insertion menu when editor has focus |
| `Ctrl+S` | Save now / request save |
| `Ctrl+K` | Command/search palette |
| `Ctrl+Z` / `Ctrl+Y` | Undo / Redo |
| `Ctrl+D` | Duplicate selected block |
| `Ctrl+Shift+P` | Open Preview mode, if not conflicting with browser/OS shortcuts |

Shortcuts must not hijack ordinary text-entry behaviour.

---

## 3. Application shell and navigation

### 3.1 Primary navigation

| Navigation item | Contents | Boundary |
|---|---|---|
| Home | Recent documents, recently used templates, quick create, local storage usage and an inline last-backup reminder | Backup reminder is informational only; no push notifications, approval KPIs, overdue dashboards or operational alerts |
| Documents | Individual documents and completed records | Search, filter, open, duplicate, export |
| Templates | Starter templates and user-created templates | Versioning and explicit selective updates |
| Registers | Local structured registers and their records | Grid/table and individual detail views |
| Master Data | Laboratory profile, personnel, equipment, chemicals, reagents, methods, matrices and units | Local reference data only |
| Blocks | Native block catalogue and user-created blocks | 13 user-facing palette categories; AUTO IDs are shared services, not a palette category |
| Technical Tools | Formula builder, method configurations and supported calculators | No operational monitoring dashboard |
| Settings | Appearance, document defaults, storage, backup/restore, import/export and help | No users/roles/accounts |

### 3.2 Top bar

The top bar should contain:

- Application name and current workspace/document context.
- Global search/command palette.
- Local save-state indicator.
- Undo/redo when editing.
- Theme toggle.
- Help/shortcuts.
- Export action when a document/record is open.

Do not show login, role switching, notification bell, approval inbox, share/collaboration or controlled-copy controls.

### 3.3 Home screen

The Home screen should be a starting point, not a laboratory-management dashboard.

Recommended content:

- New blank document.
- Create from template.
- Open recent documents.
- Recently used templates.
- Recently accessed registers.
- Pinned/favourite templates.
- Local storage usage indicator.
- Last backup date and a direct backup action.
- Offline/online and app-update status.

Do not show pending approvals, overdue calibrations, sample status KPIs, CAPA tasks, due-review calendars or operational notifications.

---

## 4. Document library

### 4.1 Document list

Use a searchable/filterable table or list with configurable columns such as:

- Document title.
- Displayed document ID.
- Type/category.
- Discipline.
- Informational status (optional, user-defined).
- Created/modified date.
- Source template (where applicable).
- Record state (for records only: draft/completed/amended, according to record rules).

The status field is informational and must not trigger locking or workflow behaviour.

### 4.2 Document actions

- Create blank document.
- Create from template.
- Open/edit.
- Duplicate.
- Save as template.
- Export PDF/DOCX.
- Export structured data to CSV/Excel.
- Delete with clear confirmation and defined recovery/backup implications.

There is no controlled-copy distribution, obsolete archive or approval action.

### 4.3 New document flow

1. Select category/type/discipline.
2. Select a starter template or blank document.
3. Enter optional metadata (title, document ID, dates, department, prepared/reviewed/approved names/designations, status and references).
4. Open in Design or Fill mode as appropriate.

The document gets an independent copy of its starting template structure. Later template edits do not change it automatically.

---

## 5. Template gallery and template designer

### 5.1 Template gallery

Organize templates by document type and discipline, including:

- Manual, policy and quality document.
- SOP/procedure.
- Form.
- Register.
- Worksheet.
- Technical record.
- Report.
- Checklist.
- Plan/schedule.
- General document.

Provide search, category filters, tags, favourites and template preview.

### 5.2 Template detail

Show:

- Template title, category, discipline and purpose.
- Included block list.
- Required inputs and optional fields.
- Method/standard references where configured.
- Formula configuration references where applicable.
- Page/rendering preview.
- Template definition version and compatibility notes.
- Use Template / Duplicate Template actions.

### 5.3 Template designer

Use the same three-panel editor as document design mode. Allow:

- Adding/reordering/configuring blocks.
- Optional reusable sections.
- Default values and field visibility rules.
- Required/optional settings.
- Table column/schema configuration.
- Header/footer/page setup.
- Formula binding configuration.
- Template definition version creation.

A template may protect its structure from accidental changes in Fill mode through template-design settings. This is not an approval lock or document-control state.

### 5.4 Selective template updates

When a template has changed, an existing document may be selected for explicit update. Show a comparison of added, removed and changed blocks/fields. Preserve document-specific content and user data. Require explicit conflict resolution. Never propagate template changes automatically.

---

## 6. Document editor — core workspace

### 6.1 Three-panel layout

| Panel | Contents |
|---|---|
| Left — Block Library | Search, category groups, P1/core blocks, favourites, recently used, drag/insert |
| Centre — Document Canvas | A4 page canvas, semantic blocks, page guides, headers and footers |
| Right — Properties | Selected block settings, field schema, validation, defaults, formulas, data references and rendering options |

Suggested initial desktop proportions: approximately 25% / 40% / 35%, adjustable or collapsible. Exact widths should be tested with long property forms and wide tables.

### 6.2 Editor modes

**Design mode**
- Add, remove, move and duplicate blocks.
- Configure block properties and schemas.
- Configure tables, visibility rules and default values.
- Set page layout, document metadata presentation, headers and footers.
- Create/reuse sections and save as template.

**Fill mode**
- Enter actual values and observations.
- Display required fields and validation feedback.
- Resolve local master-data references.
- Evaluate declared calculations across blocks in the same document.
- Add repeatable rows/field groups/sections where configured.
- Save an in-progress document or create a completed-record snapshot.

**Preview mode**
- Inspect A4 output with headers, footers, page numbers and page breaks.
- Inspect table continuation and image/chart sizing.
- View calculation outputs and units.
- Check validation warnings/errors.
- Open export/print actions.

### 6.3 Canvas behaviour

- Page boundaries and margins are visible in Design/Preview modes.
- Drag handles and insertion controls must be keyboard accessible.
- Block toolbars may include duplicate, move, configure and delete.
- Conditional visibility is configured through the shared rule engine.
- Block-level design constraints are separate from document approval or status.
- Long content and wide tables must have defined overflow behaviour.

### 6.4 Right properties panel

Depending on the selected block, expose:

- Label/title and help text.
- Required/optional state.
- Default value.
- Field type and precision.
- Units and allowed values.
- Input validation.
- Formula binding and source fields.
- Local master-data/register reference.
- Conditional visibility rules.
- Repeatable behaviour.
- Table columns and row settings.
- Export/render settings.

Do not expose workflow routing, approver assignment, audit-event configuration or notifications.

### 6.5 Editor status bars

The bottom bar may show:

- Word count.
- Page count.
- Validation error/warning count.
- Current zoom and paper size.
- Local persistence state.

Do not show formal revision state or approval progress.

---

## 7. Block library and category presentation

The normalized block registry has 14 ID namespaces. The visible block palette should have 13 user-facing categories; `AUTO-*` IDs describe shared services/capabilities and are not presented as a separate block category.

| Palette category | Included namespaces |
|---|---|
| Structure | STR |
| Metadata | META |
| SOP sections | SOP |
| Tables | TBL |
| Form inputs | FRM |
| Chemical | CHEM |
| Environmental | ENV |
| Coal/coke | COAL |
| Equipment/calibration | EQB |
| Quality control/uncertainty | QC |
| Preparation/review/approval content | APR (content only) |
| Compliance/accreditation content | NABL |
| Charts/visualization | VIS |

The palette should not expose excluded IDs APR-007, AUTO-008, AUTO-010, AUTO-011, AUTO-017 or AUTO-018. Shared services such as formulas, validation, numbering, file assets, repeatable data and conditional rules are accessed through compatible blocks and properties.

---

## 8. Block-specific interface requirements

| Block group | Interface elements and behaviour |
|---|---|
| Structure | Rich-text toolbar, heading levels, list indentation, callouts, divider, page break, table of contents, image/attachment display |
| Metadata | Editable title, ID, revision label, dates, department, person names/designations, optional free-text status, applicability and references |
| SOP sections | Pre-titled section presets with editable content and optional numbering; reusable in other document types |
| Tables | Spreadsheet-like grid, typed columns, add/remove rows/columns, repeatable rows, formula cells and conditional rows |
| Form inputs | Text, number, decimal, date/time, dropdown, multi-select, radio, checkbox, uploads, optional signature-like input, auto-ID, QR/scanner input and repeatable field group |
| Chemical | Reagent preparation, standardization, titration, gravimetric, pH, conductivity, absorbance, calibration data, dilution, concentration and analytical results; formula behaviour comes from selected method configurations |
| Environmental | Sampling location/time/conditions, preservation, field measurements, water/air/stack/soil/noise records and configured analytical calculations |
| Coal/coke | Sample preparation, moisture, ash, volatile matter, fixed carbon, GCV/NCV, sulphur, HGI, ash fusion and ultimate analysis, configured by method/basis |
| Equipment | Equipment details/specifications, calibration records, intermediate verification, maintenance/breakdown/usage records and certificate attachments; no alerts or due-date tracker |
| QC/uncertainty | Blank/duplicate/replicate/spike/CRM/PT records, control-chart data and views, uncertainty components/budget/calculation; no operational QC dashboard or notifications |
| Preparation/review/approval content | Name, designation, date, optional typed/drawn signature content and ordinary approval tables; no workflow or signatory verification |
| Compliance/accreditation | Clause/reference fields, checklists, scope tables, findings, nonconformity/root-cause/corrective-action/effectiveness content and evidence references; no CAPA/compliance workflow |
| Visualization | Line, bar, pie, scatter, histogram, trend, calibration/control charts and structured flowchart/diagram output |

---

## 9. Registers and master data

### 9.1 Registers hub

Show the local registers created by the user or installed from templates, such as equipment, chemical, reagent, sample receipt, personnel, PT and other registers.

### 9.2 Register views

Each register should support:

- Grid/table view.
- Individual record detail view.
- Draft records may be edited according to their template/schema. A completed-record snapshot is read-only; an amendment creates a separate linked editable copy and never overwrites the original snapshot. Deletion/retention policy is defined separately.
- Search, filter, sorting and configurable columns.
- Record creation from a template.
- Individual record print/export.
- CSV/Excel export.
- Completed-record snapshot and amendment behaviour: completion preserves the captured record; subsequent changes must be made through a separate linked amended copy, never by editing the completed snapshot in place.

A register is not a workflow board. Do not add Kanban states, automated assignments, reminders or operational status transitions unless a new product decision authorizes them.

### 9.3 Master-data screens

Provide local editors for:

- Laboratory profile and branding.
- Personnel and designations.
- Equipment and instruments.
- Chemicals and reagents.
- Testing methods and standard editions.
- Sample types and matrices.
- Units and measurement parameters.
- User-defined reference lists.

Every master-data record needs a stable internal identifier. Documents may use a live reference while being edited and capture the relevant values into a completed-record snapshot.

---

## 10. Formula and technical tools UI

### 10.1 Formula builder

Provide:

- Formula name and description.
- Same-document field picker grouped by block.
- Supported operators and function catalogue.
- Input/output unit configuration.
- Intermediate expression/result preview.
- Sample/test values.
- Validation messages and dependency display.
- Save formula definition and version.

Formula references may cross blocks within the same document, but not other documents or registers. Arbitrary JavaScript or user-supplied executable code is prohibited.

### 10.2 Verified method configuration

Display:

- Method name and identifier.
- Standard and edition.
- Matrix/parameter.
- Input fields and units.
- Formula and correction steps.
- Reporting basis and rounding.
- Test vectors and expected outputs.
- Technical source/review metadata.

Do not display a “verified” indicator until the configuration has passed its documented technical review and tests. User-created formulas must be labelled separately.

### 10.3 Calculators

Standalone calculators may provide convenience, but any result inserted into a technical record must preserve its inputs, formula/configuration identity and units. A standalone calculator does not replace the method-specific worksheet/record.

---

## 11. Reports, rendering and exports

### 11.1 Document output

The export dialog should offer:

- PDF.
- Editable DOCX.
- A4 print.
- CSV for selected table/register data.
- Excel workbook for selected tables/registers or structured whole-document data.

CSV/Excel output is structured data, not an exact visual reproduction of the document page. Whole-document Excel export should map blocks/sections to clearly named sheets/tables and preserve source block IDs where possible.

### 11.2 PDF and print preview

- A4 is the initial paper preset; margins/orientation should be configurable.
- Headers/footers and page numbers should use the selected document's content/metadata.
- Table headers repeat when configured; row splitting/continuation must be deterministic.
- Images and charts maintain aspect ratio and readable labels.
- Page breaks must not create unintended blank pages.
- Attachments must be embedded, appended, referenced or clearly reported as not embeddable; never silently omitted.
- Preview must use the same renderer contract as PDF/print as far as technically possible.

### 11.3 DOCX output

DOCX export should prioritize editable structure: headings, paragraphs, tables, images, headers/footers and supported fields. Advanced interactive behaviours may be represented as static content. Any unsupported block must be reported before export.

### 11.4 Export validation

The export flow should show warnings for unsupported or lossy mappings, missing attachments, unconfigured calculations and validation errors. The user may proceed only after reviewing applicable warnings; critical data loss should block export.

---

## 12. Backup, restore and local storage UX

### 12.1 Backup screens

Provide separate actions for:

- JSON structured-data backup.
- ZIP complete workspace backup, including attachments and a manifest.

Show last backup date, estimated data size and whether attachments are included.

### 12.2 Restore flow

1. Select JSON or ZIP backup.
2. Validate file format, schema version, IDs and attachment checksums.
3. Show a restore summary and compatibility warnings.
4. Let the user choose replace or merge where supported by the backup specification.
5. Require confirmation before changing existing data.
6. Preserve/create a safety backup before a destructive replace when technically possible.
7. Report restored, skipped and conflicted items.

### 12.3 Storage indicators

Show approximate local storage usage and a warning when browser quota/persistence is uncertain. Provide a direct backup action. Do not imply that browser storage is permanent or protected from device/browser deletion.

---

## 13. Settings and help

Settings should include:

- Laboratory profile and branding.
- Default document numbering formats (if enabled).
- Default page size, margins and typography.
- Theme, density and accessibility preferences.
- Block library enable/favourites/defaults.
- Storage information and backup/restore.
- Import/export preferences.
- Keyboard shortcuts and block guide.
- About, offline status and update information.

Do not include user/role administration, approval routing, notification preferences, cloud connection or AI provider settings.

---

## 14. Onboarding, empty states and error handling

First-run onboarding should:

1. Explain that data is stored locally in this browser profile.
2. Explain that initial load and later application updates require internet access, but core use is offline once cached.
3. Let the user configure laboratory name/profile and initial discipline interests.
4. Offer the starter-template catalogue.
5. Explain JSON/ZIP backup and encourage an initial backup after data is created.

Include clear empty states for documents, templates, registers and master data. Provide actionable messages for offline asset gaps, storage failures, invalid formulas, missing method configuration, restore conflicts and export limitations.

---

## 15. Accessibility and usability requirements

- Keyboard-operable navigation, menus, block insertion and table editing.
- Visible focus indicators.
- Proper labels and accessible error messages.
- Do not convey state by colour alone.
- Adequate contrast in all themes and exports.
- Sensible tab order through the three-panel editor.
- Wide tables must remain usable with keyboard and touch interactions.
- Respect browser zoom and text scaling.
- Support high-contrast mode as a tested enhancement.

---

## 16. Reusable UI component inventory

Buttons, status/metadata labels, category tags, data grid, sticky table headers, side panel, modal/dialog, tabs, accordion, breadcrumb, tooltip, searchable select, date/time picker, signature-like input, chart wrapper, formula chip, progress indicator for export/restore only, timeline for ordinary record content where appropriate, diff viewer for template updates, toast, skeleton loader, validation message and offline indicator.

No approval stepper, role chip, task inbox, notification centre, sharing control or collaboration comment system is required.

---

## 17. Suggested build order and technology notes

The authoritative implementation sequence is in `project_roadmap.md`. This design file does not mandate a specific library.

The original suggestions remain candidates for evaluation:

| Concern | Candidate approach | Selection status |
|---|---|---|
| Framework | React + TypeScript + Vite | Proposed; confirm in technical specification |
| Styling | CSS modules or utility styling with design tokens | Pending selection |
| Rich text/block editor | TipTap/ProseMirror or custom block renderer | Pending prototype/evaluation |
| Data grid | TanStack Table or comparable grid | Pending selection |
| Drag/drop | dnd-kit or accessible alternative | Pending selection |
| Charts | Recharts/Chart.js or comparable renderer | Pending selection |
| Formula engine | Safe expression parser or constrained math library | Pending security/technical evaluation |
| Local data | IndexedDB through a tested repository layer; Dexie is a candidate | Pending selection |
| PDF/DOCX | Libraries selected after pagination/export spike | Pending selection |
| PWA | Manifest, service worker and versioned static asset caching | Required architecture; library/tooling pending |

No library should be selected solely because it appears in the original proposal. Evaluate offline support, maintenance, bundle size, licensing, accessibility, export fidelity and browser compatibility.

---

## 18. Frontend acceptance criteria

The frontend design is implemented correctly when:

- Users can create blank documents and documents from templates.
- Documents are independent of later template edits unless explicitly updated by the user.
- Design, Fill and Preview modes operate on the same document model.
- The three-panel editor remains usable for wide tables and detailed properties.
- The visible palette uses the 13 user-facing categories; AUTO services are not presented as an extra block category.
- Informational status fields do not trigger workflows or locks.
- Prepared/checked/reviewed/approved and signature components are ordinary content only.
- Completed records are preserved as snapshots and amendments create separate linked copies.
- Registers provide structured record collections, grid/table views and individual record views.
- Formulas access compatible fields across blocks in the same document only.
- No login, user roles, approval workflow, notifications, controlled distribution, operational tracking, AI or app-managed audit trail is present.
- A user can preview and export supported content to PDF/DOCX/A4/CSV/Excel with clear warnings for unsupported mappings.
- JSON/ZIP backup and restore are discoverable and understandable.
- Essential application functions remain usable offline after required assets are cached.

---

## 19. Source traceability and status

This revision is based on the original `frontend_design.md`, the normalized 220-ID block registry, the 324-entry document catalogue, and the accepted requirements in `project_details.md` and `project_decisions.md`.

The original proposal's useful visual foundations, three-panel editor, Design/Fill/Preview modes, block-specific interfaces, themes, keyboard shortcuts and export goals are retained. Conflicting workflow, document-control, role, notification, operational tracking and AI features are removed or reinterpreted as ordinary document content.

**End of frontend design specification.**
