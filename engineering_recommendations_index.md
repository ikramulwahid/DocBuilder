# DocBuilder — Engineering Recommendations Package

**Package version:** 0.1  
**Prepared:** 2026-11-13  
**Status:** Proposed for owner review; not an implementation authorization

## Purpose

This package recommends the five engineering areas identified as prerequisites for detailed technical planning. It supplements the approved product direction and revised project-control documents; it does not replace them.

## Documents

| File | Subject | Main recommendation |
|---|---|---|
| `data_architecture_spec.md` | Local data model, IndexedDB, entities, snapshots, migrations, backup/restore boundaries | Dexie over IndexedDB; separate document/template/record models; preserved completed snapshots; stable field IDs |
| `technology_stack_recommendation.md` | Framework, PWA, editor, data grid, calculations, export and tests | React + TypeScript + Vite PWA; Dexie; custom block runtime; focused libraries |
| `calculation_engine_specification.md` | Built-in methods, user formulas, units, precision, provenance and verification | Restricted AST + decimal arithmetic + method-specific test packages |
| `initial_testing_method_catalogue.md` | Candidate initial test methods for QMS, chemical, environmental, coal/coke | Method families prioritized; exact standards and editions require lab technical confirmation |
| `export_specification.md` | PDF, print, DOCX, CSV, Excel, JSON and ZIP | One canonical document model with separate semantic output adapters |

## Relationship to existing project files

These documents are proposed engineering detail and should be referenced by, not silently merged into, the approved files:

- `project_details.md` remains the permanent product brief.
- `project_decisions.md` remains the decision register.
- `project_roadmap.md` remains the implementation sequence.
- `project_progress.md` remains the factual status log.
- `block_list.md`, `document_master_list.md` and `frontend_design.md` remain the detailed product catalogues/design specifications.

After the owner reviews and accepts the recommendations, the corresponding decisions and roadmap references can be incorporated into the project-control files. No GitHub changes are included in this package.

## Important status distinctions

- **Confirmed product decisions:** offline-capable single-user PWA; no backend/login/AI/document-control/approval workflow; IndexedDB local persistence; documents/templates/records/registers; same-document formula scope; four initial laboratory areas; PDF/DOCX/CSV/Excel/JSON/ZIP.
- **Recommendations in this package:** technology selections, exact data-store design, formula AST/runtime design, initial method candidates and file-generation libraries.
- **Still requiring owner/technical approval:** browser/OS support, technical method list and editions, rounding/unit policies, backup encryption and restore modes, file/export limits, and final package versions.

## No implementation has started

These are specification and recommendation documents only. They do not create a repository, modify GitHub, install dependencies, scaffold the application or begin Phase 1.
