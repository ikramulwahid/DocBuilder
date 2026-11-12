# LabDoc Builder: Frontend Design Specification

**Product:** Block-based document builder for ISO/IEC 17025:2017 (NABL) testing laboratories
**Scope:** Frontend only. No login or backend. Data is held in local state and browser storage (localStorage / IndexedDB), with JSON import/export for backup.
**Inputs:** Document master list (20 categories, about 300 documents) and block list (13 block categories)

---

## 1. Design Foundations

### 1.1 Overall feel
Clean, calm and professional, like a lab LIMS crossed with Notion. The interface is dense but readable, because users work with tables and forms all day.

### 1.2 Themes

| Theme | Use | Notes |
|---|---|---|
| Light "Lab Clean" (default) | Daily work | White canvas, soft grey-blue app background |
| Dark | Long sessions, low light | Document canvas stays white so output matches the printed page |
| Print / Paper preview | Preview and export | A4 page, black on white, no UI chrome |
| High contrast | Accessibility | Optional, WCAG AAA targets |

### 1.3 Colour system

**Core palette**
- Primary: deep teal-blue `#0B5C8E` (actions, selection, focus)
- Neutrals: slate greys for panels, borders and text
- Surface: white cards on a `#F3F6F9` app background (light theme)

**Status colours**

| Status | Colour |
|---|---|
| Draft | Grey |
| In Review | Amber |
| Approved | Green |
| Obsolete | Red |
| Overdue | Deep red |

**Block category tags (one hue each)**

| Category | Hue |
|---|---|
| Structure | Slate |
| Input | Blue |
| SOP | Indigo |
| Table | Teal |
| Chemical | Purple |
| Environmental | Green |
| Coal | Brown / charcoal |
| Equipment | Orange |
| QC / Uncertainty | Pink |
| Approval | Gold |
| Compliance | Red |
| Automation | Cyan |

### 1.4 Typography
- **UI:** Inter or Segoe UI
- **Document body:** Arial or Calibri (common in labs), configurable per template
- **Document IDs, formulas, code-like values:** JetBrains Mono or IBM Plex Mono

### 1.5 Layout rules
- Collapsible left navigation rail plus a top bar
- 8-pt spacing grid, 6–8 px corner radius, subtle shadows
- Desktop-first; tablet supported for lab-floor data entry; phone is view-only
- Density setting: Comfortable / Compact

### 1.6 Keyboard shortcuts

| Shortcut | Action |
|---|---|
| `/` | Open block menu in editor |
| `Ctrl+S` | Save |
| `Ctrl+K` | Command palette / global search |
| `Ctrl+Z` / `Ctrl+Y` | Undo / Redo |
| `Ctrl+D` | Duplicate selected block |

---

## 2. Screen Catalogue

### A. Shell and Navigation

| Screen | Contents | Purpose |
|---|---|---|
| App shell | Left rail (Home, Documents, Templates, Registers, Blocks, Compliance, Reports, Settings); top bar with global search, notification bell, theme toggle, role switcher | Navigation on every screen |
| Command palette | Search documents, blocks, commands | Fast access |
| Role switcher (mock) | Preparer / Checker / Reviewer / Approver / Quality Manager / Admin | Demonstrates workflow without login |

### B. Home and Dashboard

| Screen | Contents | Purpose |
|---|---|---|
| Dashboard | KPI cards (total docs, drafts, pending review, approved, due for review), "My tasks", recent documents, review-due calendar | Daily starting point |
| Compliance overview | Coverage by ISO 17025 clause, documents per category, progress rings | NABL readiness at a glance |
| Notifications centre | Review reminders, approval requests, calibration-due alerts | Reminders |

### C. Document Library

| Screen | Contents | Purpose |
|---|---|---|
| Master Document List | Table with ID, name, type, category, revision, status, owner, review date; filters, saved views, bulk actions | Digital version of the master list |
| Category browser | Tree of the 20 categories with counts | Browse by category |
| Card / grid view | Thumbnails with status badges | Visual browsing |
| Document detail panel | Metadata, revision history, linked documents, attachments, audit trail | Quick look without opening the editor |
| New Document wizard | 1) Choose category and type. 2) Pick template or blank. 3) Fill metadata (auto ID, department, ISO clause). 4) Open editor | Guided creation |

### D. Template Gallery

| Screen | Contents | Purpose |
|---|---|---|
| Template gallery | Cards by type (SOP, Form, Register, Worksheet, Report, Checklist, Plan, Policy) with preview | Start from proven structures |
| Template detail | Block list, sample preview, "Use template" button | Choose with confidence |
| Template builder | Same editor with locked and unlocked regions | Admins create reusable templates |
| Pre-built packs | Chemical Lab Pack, Environmental Pack, Coal Pack, QMS Pack | Fast onboarding per lab type |

### E. Document Editor (core screen)

**Three-panel layout**

| Panel | Contents |
|---|---|
| Left: Block library | Searchable, grouped by the 13 block categories. Drag to add. P1 blocks pinned at the top, plus "Recently used" and favourites |
| Centre: Canvas | A4 page view with header (logo, doc no., revision, date), blocks, and footer (page X of Y, controlled-copy stamp). Each block has a drag handle, a "+" insert, and a hover toolbar (duplicate, delete, lock, comment, conditional visibility) |
| Right: Properties | Settings for the selected block: label, required, validation, formula, default value, visibility rules, data link |
| Top toolbar | Title, status badge, save indicator, undo/redo, Design / Fill / Preview toggle, zoom, comments, share/export |
| Bottom bar | Word count, page count, validation errors, revision number |

**Editor modes**
1. **Design mode:** build or edit the template structure.
2. **Fill mode:** the analyst enters data; calculations run live.
3. **Preview mode:** print-accurate A4 view.

**Supporting editor screens**
- Block settings drawer
- Formula editor (field picker, function list, test values)
- Conditional logic builder (If field = X, then show or hide)
- Comments and suggestions sidebar
- Version compare (side-by-side diff)

### F. Block-Specific UI

| Block group | Interface elements |
|---|---|
| Structure | Inline rich-text bar, heading levels, list indent, dividers, page breaks, callouts (note, warning, info, highlight) |
| Metadata | Header form for document number, revision, dates, department, prepared/reviewed/approved, status, ISO clause, applicability |
| SOP blocks | Pre-titled sections (Purpose, Scope, Responsibility, Definitions, References, Procedure Steps, etc.) with ISO-style numbering and "Add section" |
| Tables | Spreadsheet-like grid, add/remove rows and columns, column types (text, number, date, formula), repeatable rows, conditional rows |
| Form inputs | Live previews of text, number, decimal, date/time, dropdown, multi-select, radio, checkbox, upload, signature, initials, auto-ID, barcode/QR, repeatable group |
| Chemical | Titration table (initial/final burette, volume, auto result), calibration curve table with graph, dilution calculator, concentration calculator, recovery % |
| Environmental | Location with map pin or GPS fields, sampling date/time, field measurements table, BOD / COD / TDS / TSS calculators, heavy metal results, air, stack, soil and noise result tables |
| Coal | Crucible-weight tables for moisture, ash and volatile matter with auto %, fixed-carbon auto-calc, GCV/NCV, total sulphur, HGI, ash fusion, ultimate analysis |
| Equipment | Equipment details card, specification table, calibration status chip, due-date countdown, certificate upload, maintenance and breakdown entries, status toggle |
| QC and uncertainty | QC plan, blank/duplicate/replicate/spike/CRM tables, control chart with limit lines, PT Z-score gauge, uncertainty component list, budget table with combined and expanded U, traceability |
| Approval | Prepared / Checked / Reviewed / Approved cells with typed or drawn signature and date; multi-level workflow |
| Compliance | ISO clause picker, NABL requirement reference, compliance status pills, evidence link, NC and CAPA forms, root cause, effectiveness check, scope of accreditation |
| Visualization | Line, bar, pie, scatter, histogram, trend and control-chart blocks with a data-source picker; flowchart and diagram blocks |
| Automation | Linked document / register picker, auto-number and auto-revision badges, QR generator, approval trigger, notification rule, cross-document data link, AI draft generator |

### G. Registers and Logs

| Screen | Contents | Purpose |
|---|---|---|
| Registers hub | List of registers (Equipment, Sample Receipt, Reagent Preparation, Complaint, PT, etc.) | One place for all registers |
| Register view | Filterable data grid, add-entry drawer, export, row history | Day-to-day entries |
| Log view | Timeline or table for temperature, humidity, equipment usage | Frequent small entries |
| Calibration and due-date tracker | Calendar plus traffic-light list (green, amber, red) | Equipment compliance |
| Sample tracking board | Kanban: Received, In Testing, Reviewed, Reported, Retained / Disposed | Sample lifecycle |

### H. Workflow and Approvals

| Screen | Contents | Purpose |
|---|---|---|
| My tasks / inbox | Documents awaiting action with Accept / Return / Reject | Daily action queue |
| Approval workflow view | Stepper: Prepared, Checked, Reviewed, Approved, with dates and comments | Status clarity |
| Revision history | Revision table with change reason and approver, plus compare button | Document control (clause 8.3) |
| Distribution list | Who holds a controlled copy, issue date, acknowledgement | Copy control |
| Obsolete archive | Superseded documents, read-only with watermark | Prevents use of old versions |

### I. Compliance and Accreditation

| Screen | Contents | Purpose |
|---|---|---|
| ISO 17025 clause matrix | Clauses 4–8 against documents, with gap highlighting | Compliance mapping |
| NABL readiness checklist | Interactive checklist with evidence links and completion % | Assessment preparation |
| Scope of accreditation manager | Tests, ranges, methods, standards table | Scope control |
| NC and CAPA tracker | Nonconformities, root cause, action, effectiveness check, due dates | Audit follow-up |
| Internal audit planner | Annual programme calendar, checklists, reports | Clause 8.8 |
| Management review board | Agenda inputs, minutes, action tracker | Clause 8.9 |
| Risk and opportunity register | Likelihood x impact matrix with heat map | Clause 8.5 |

### J. Technical Tools

| Screen | Contents | Purpose |
|---|---|---|
| Measurement uncertainty workspace | Component identification, budget table, combined and expanded U | MU documents |
| Method validation workspace | LOD, LOQ, linearity, precision, recovery, robustness, selectivity tabs with graphs | Validation reports |
| QC dashboard | Control charts, PT Z-scores, out-of-control flags | Ongoing QC |
| Calculators | Dilution, normality, concentration, percent recovery | Quick lab maths |

### K. Reports and Exports

| Screen | Contents | Purpose |
|---|---|---|
| Export dialog | PDF, DOCX, print, JSON/CSV for registers; watermark choice (Controlled / Uncontrolled / Draft) | Output |
| Print preview | A4 pagination, header/footer check | Final check |
| Analytics reports | Documents by status, overdue reviews, training coverage | Management reporting |

### L. Settings and Administration

| Screen | Contents | Purpose |
|---|---|---|
| Laboratory profile | Name, logo, address, NABL number, discipline (Chemical, Environmental, Coal) | Auto-fills headers |
| Document numbering rules | Prefixes (QMS-, SOP-, EQ-), format, next number | Auto ID |
| Header and footer designer | Logo, field placement, confidentiality note | Branding |
| Users and roles (mock) | Sample users with authority matrix | Workflow demo |
| Block library manager | Enable, disable, favourite blocks, set defaults | Admin control |
| Theme and appearance | Light, dark, accent colour, density, font | Personalisation |
| Data import / export | Backup or restore JSON, import master list | Portability without a backend |
| Help and shortcuts | Keyboard shortcuts, block guide, ISO clause cheat sheet | Onboarding |

### M. Onboarding, Empty States and System

- First-run onboarding: pick lab type, load master list, choose a theme
- Empty states with illustration and a "create first document" call to action
- Toasts for save, error and validation
- Unsaved-changes and delete confirmation dialogs
- 404 and offline notice

---

## 3. Reusable Component List

Buttons (primary, secondary, ghost), status badges, category tags, data grid with filters, sticky table headers, side drawer, modal, stepper, tabs, accordion, breadcrumb, avatar and role chip, tooltips, date-range picker, searchable select, signature pad, chart wrapper, formula chip, progress ring, calendar, kanban card, timeline, diff viewer, toast, skeleton loaders.

---

## 4. Key UX Behaviours

- **Inline validation:** red for out-of-range values, amber for warnings (e.g. recovery outside 80–120%).
- **Calculated fields:** visually distinct, with a light grey fill and a formula icon.
- **Locked sections:** approved documents are read-only; editing starts a new revision.
- **Auto header:** document header fills from metadata, so changing the revision updates every page.
- **Controlled-copy watermark:** applied automatically on export.
- **Autosave:** saved state shown in the toolbar.
- **Role-aware UI:** buttons and actions change with the selected mock role.

---

## 5. Block Library Priorities

**P1 (first release, about 30 blocks)**
Heading, Rich Text, Bullet List, Numbered List, Basic Table, Dynamic Table, Text Field, Number Field, Decimal Field, Date Picker, Dropdown, Checkbox, File Upload, Image Upload, Purpose, Scope, Responsibility, Definitions, References, Procedure Steps, Equipment List, Chemical/Reagent List, Calculation Formula, Observation Table, Result Table, QC Requirements, Prepared/Reviewed/Approved, Revision History, Document Header, Document Footer.

**P2 (second release, about 20 blocks)**
Sample Details, Equipment Details, Calibration Details, Reagent Preparation, Titration Observation, BOD/COD Calculation, Coal Proximate Analysis, Measurement Uncertainty, Control Chart, Risk Assessment, Nonconformity, Corrective Action, Signature, ISO Clause Reference, Conditional Section, Formula Engine, Linked Document, Linked Register, QR Code, Chart / Graph.

---

## 6. Suggested Build Order

1. **Phase 1 (P1 blocks):** shell, dashboard, document list, editor with structure, table, input, SOP, technical and approval blocks, preview, export.
2. **Phase 2 (P2 blocks):** laboratory, chemical, environmental and coal blocks, registers, QC and uncertainty, workflow screens.
3. **Phase 3:** compliance and NABL screens, linked documents and registers, charts, conditional logic, formula engine.

---

## 7. Suggested Frontend Stack (optional)

| Concern | Suggestion |
|---|---|
| Framework | React + TypeScript (Vite) |
| Styling | Tailwind CSS with a design-token file for themes |
| Editor engine | TipTap / ProseMirror, or a custom block renderer |
| Data grid | TanStack Table or AG Grid |
| Drag and drop | dnd-kit |
| Charts | Recharts or Chart.js |
| Formulas | mathjs or a custom expression parser |
| State and storage | Zustand plus IndexedDB (Dexie) |
| Export | html2pdf / print CSS for PDF, docx library for Word |
