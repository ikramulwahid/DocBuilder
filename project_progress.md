# DocBuilder — Project Progress Log

**Document type:** Factual project status and activity log  
**Version:** 1.0  
**Baseline date:** 2026-11-13  
**Project brief:** `project_details.md`  
**Decision register:** `project_decisions.md`  
**Roadmap:** `project_roadmap.md`

---

## 1. Current project status

< STATUS: PLANNING BASELINE DRAFTED — IMPLEMENTATION NOT STARTED >

DocBuilder is currently at the planning/specification stage. The original GitHub repository contains three Markdown planning files only. No application source code, package manifest, build configuration, automated tests or implementation has been identified in the repository during the baseline review.

The owner has confirmed the overall product purpose, scope and major architectural decisions. Four coordinated project-control documents have now been drafted locally. They have not yet been committed to GitHub.

### Current summary

| Area | Status | Notes |
|---|---|---|
| Product purpose | Confirmed | Laboratory document builder and record authoring application |
| Product boundaries | Confirmed | Local-first, single-user; no backend, LIMS, document control, workflow or AI |
| First-release disciplines | Confirmed | QMS/general, chemical, environmental, coal/coke |
| PWA/offline direction | Confirmed | Installable PWA; initial online load; then core offline operation |
| Data architecture direction | Confirmed | IndexedDB primary local store; detailed schema pending |
| Templates/documents/records/registers | Confirmed | Separate concepts with defined relationships |
| Completed-record preservation | Confirmed | Preserve snapshot; amendment creates separate copy/record |
| Block customization | Confirmed | Native, visual custom and advanced schema-defined blocks |
| Calculation direction | Confirmed | Built-in method-specific plus user-created formulas within one document |
| Master data | Confirmed | Shared local master-data libraries |
| Exports/backups | Confirmed | PDF, DOCX, A4 print, CSV, Excel, JSON and ZIP |
| Initial method list | Pending | Must be agreed per discipline |
| Detailed schemas/formula grammar | Pending | Engineering specification required |
| Repository implementation | Not started | No source implementation identified |
| Automated verification | Not started | No test/build setup identified |

## 2. Repository baseline

**Repository:** https://github.com/ikramulwahid/DocBuilder  
**Default branch:** `main`  
**Reviewed branch state:** 2026-11-13  
**Repository state during review:** Three tracked Markdown files; one branch (`main`); recursive tree listing was not truncated.

### 2.1 Original repository files

| File | Purpose | Baseline observation |
|---|---|---|
| `block_list.md` | Original block inventory | 220 original block IDs; all unique |
| `document_master_list.md` | Proposed laboratory document inventory | 324 document IDs; all unique; 20 categories |
| `frontend_design.md` | Frontend screen/UI proposal | 281 lines; includes some functionality later excluded by owner decisions |

### 2.2 Document catalogue findings

The 324 entries include five exact duplicate-name pairs. Both IDs in every pair must be preserved until their scope is reconciled:

| ID 1 | ID 2 | Duplicate title |
|---|---|---|
| QMS-015 | AUD-001 | Internal Audit Procedure |
| QMS-016 | AUD-012 | Management Review Procedure |
| SOP-009 | MU-002 | Measurement Uncertainty Budget |
| CR-011 | CMP-012 | Customer Satisfaction Analysis |
| CR-012 | CMP-004 | Complaint Investigation Report |

The existence of a catalogue entry does not prove that the document is legally/standard-mandated or that a complete template has been developed.

### 2.3 Original block catalogue findings

- 220 block IDs were found.
- All 220 IDs were unique.
- The normalized catalogue uses 14 ID namespaces: STR, META, SOP, TBL, FRM, CHEM, ENV, COAL, EQB, QC, APR, NABL, VIS and AUTO.
- The original frontend statement “13 block categories” requires reconciliation with the normalized registry.
- Original IDs must remain traceable even when a capability is content-only, deferred or excluded.

## 3. Planning work completed

### 2026-11-13 — Product and architecture clarification

The owner confirmed:

- Installable PWA, offline after first load.
- Single-user, local storage, no backend/login.
- No document-control system.
- Templates, individual documents, completed records and repeatable registers.
- Approval information limited to names, designations and dates as document content; no workflow.
- Built-in verified method-specific and user-created calculations.
- No AI.
- Initial QMS/general, chemical, environmental and coal/coke disciplines.
- PDF, DOCX, A4 print, JSON backup/restore and CSV/Excel export.
- Three customization levels.
- Both JSON and ZIP backups.
- Template changes do not propagate automatically; selected documents may be updated explicitly.
- Registers are structured record collections with grid/table views.
- Formula references are limited to blocks in the same document.
- Shared local master-data libraries.
- CSV/Excel support for table/register data and structured whole-document data.

### 2026-11-13 — Recommended product direction accepted

The owner accepted the recommended direction, including:

- Optional freely editable status field only; no workflow or document lock.
- Internal recovery snapshots plus a manually editable revision-history table; no formal document revision-control lifecycle.
- Approval-related blocks remain ordinary customizable content, including signature fields, without approval workflow.
- Completed records are preserved as snapshots; amendments create separate copies/records.
- No operational laboratory tracking/alert features beyond documents, templates, records and registers.
- All four initial disciplines receive functional technical support for an agreed priority method set.
- Fully developed starter templates are provided for a selected priority subset; the remainder stay in the catalogue for later development.
- The development team builds the framework while laboratory experts provide or verify method-specific technical content.

## 4. Planning artifacts drafted locally

The following files exist in the working artifact area but have not yet been committed to the GitHub repository:

| File | Description | Status |
|---|---|---|
| `DocBuilder_Revised_Block_Catalogue.md` | Revised interpretation of the 220 original block IDs, duplicate/overlap handling and shared services | Draft; requires reconciliation against accepted decisions |
| `DocBuilder_Final_Product_Technical_Architecture.md` | Earlier architecture baseline covering PWA, storage, document models, calculations, exports and remaining specifications | Draft; must be updated to reflect the later accepted recommendations |
| `project_details.md` | Primary permanent product brief | Drafted locally in this package |
| `project_decisions.md` | Accepted decision register | Drafted locally in this package |
| `project_roadmap.md` | Phase-based implementation plan | Drafted locally in this package |
| `project_progress.md` | This progress log | Drafted locally in this package |

The project-control files are the intended authoritative working set once reviewed and adopted. The two earlier architecture artifacts are supporting drafts and must not silently override the project-control files.

## 5. Current blockers and pending work

No code implementation should begin until the following foundational planning work is completed or explicitly accepted as staged work:

1. Reconcile block catalogue entries with the final no-document-control/no-workflow/no-AI decisions.
2. Normalize the 220 block IDs and define active, content-only, shared-service, deferred and excluded classifications.
3. Reconcile the five duplicate document-title pairs while preserving every document ID.
4. Select the fully developed first-release starter templates.
5. Agree the initial test-method list for QMS/general, chemical, environmental and coal/coke support.
6. Define method-specific technical inputs, formulas, units, corrections, limits and worked examples with laboratory expert verification.
7. Define detailed PWA/browser support and data-storage specifications.
8. Define document/template/record/register schemas, especially completed-record snapshots and amendments.
9. Define visual/advanced custom-block schemas and version compatibility.
10. Define safe formula grammar, unit model and calculation provenance.
11. Define PDF/DOCX/CSV/XLSX output contracts and block support matrix.
12. Define JSON/ZIP backup format, validation and restore conflict behaviour.
13. Select the technology stack and third-party libraries.
14. Define automated tests, browser/offline checks and release gates.

## 6. Next recommended work package

**P0-01 — Project baseline and source reconciliation.**

1. Owner reviews the four project-control files.
2. Once approved, place them in the repository root without deleting the three original source files.
3. Reconcile the revised block catalogue with `project_details.md` and `project_decisions.md`.
4. Build a traceability matrix for all 220 block IDs and 324 document IDs.
5. Resolve the five duplicate document titles and select priority templates/methods.
6. Update the roadmap/progress status with actual completion evidence.

No repository changes have been made as part of this draft.

## 7. Progress log entry template

Append a dated entry for each completed work package. Do not rewrite history or claim verification without evidence.

```markdown
### YYYY-MM-DD — [Work package ID and title]

- **Status:** Not started / In progress / Implemented-not-verified / Verified / Blocked
- **Branch/commit:** [branch and commit, if applicable]
- **Scope completed:** [specific files/features]
- **Decisions affected:** [DEC IDs or None]
- **Checks performed:** [exact commands/manual checks]
- **Results:** [pass counts, screenshots/evidence, failures]
- **Not completed / limitations:** [specific items]
- **Owner action required:** [only genuinely owner-controlled items, otherwise None]
- **Next work package:** [ID]
```

## 8. Status definitions

- **Not started:** No work has been performed.
- **In progress:** Work is actively being carried out.
- **Implemented — not verified:** Code/content exists but required checks have not been completed.
- **Verified:** Required acceptance criteria and tests have passed with evidence recorded.
- **Blocked:** Work cannot proceed because a dependency, decision or environment issue is unresolved.
- **Deferred:** Explicitly postponed; not silently treated as complete.
- **Excluded:** Outside accepted product scope.

---

**End of progress log.**
