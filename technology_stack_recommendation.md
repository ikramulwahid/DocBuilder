# DocBuilder — Technology Stack Recommendation

**Document type:** Proposed engineering recommendation  
**Version:** 0.1  
**Status:** Recommended for owner review; not yet approved  
**Review date:** 2026-11-13

---

## 1. Recommendation summary

Use a TypeScript-first React application built with Vite, delivered as a static installable PWA. Use IndexedDB through Dexie for local persistence. Build the document canvas and block renderer as DocBuilder-owned components; use focused libraries for rich text, data grids, drag-and-drop, calculations and file generation.

Avoid a large all-in-one UI framework or a general-purpose word processor as the application's central document model. DocBuilder's differentiator is its structured block model and laboratory-aware data/ calculation layer.

## 2. Recommended stack

| Layer | Recommended technology | Role | Notes |
|---|---|---|---|
| Language | TypeScript with strict compiler settings | Application and domain code | Require strict null checks, no implicit any, exhaustive unions for block types |
| UI framework | React 19.3 | Component system and editor UI | Pin exact compatible release in lockfile |
| Build/dev server | Vite 8.x (current stable patch at implementation time) | Fast development and static production build | Check supported Node range at the time of installation |
| Runtime/build tool | Node.js 26 Active LTS, npm bundled with it | Local development, build and test tooling | If a required package is not compatible, use the latest supported LTS branch and document the reason |
| PWA | `vite-plugin-pwa` 2.x + Workbox | Manifest, service worker, precache and update flow | Use prompt-based update, not forced reload while editing |
| Local database | Dexie.js 4.x over IndexedDB | Typed object stores, indexes, transactions and migrations | Dexie is a wrapper; IndexedDB remains the actual persistence layer |
| Domain validation | Zod 4.x | Validate app-owned objects and input boundaries | Do not use it as a substitute for a portable custom-block schema format |
| Custom block schema | JSON Schema 2020-12 + Ajv 8.x | Validate user-defined block schemas and custom record payloads | Maintain an explicit allowed schema subset; do not let schemas execute code |
| UI state | Zustand 5.x or current compatible stable release | Transient editor/UI state | IndexedDB remains the durable source of truth; do not duplicate persisted workspace data in a long-lived UI store |
| Rich text | Tiptap React / ProseMirror | Rich-text editing inside rich-text blocks | Do not use it as the document's overall block hierarchy |
| Tables / grids | TanStack Table v9 | Headless grid state, sorting, filtering and pagination | Implement DocBuilder's own cell editors, validation and styling |
| Drag and drop | dnd-kit current stable React packages | Reordering blocks and supported sortable items | Confirm package/API compatibility before pinning; keep keyboard alternatives |
| Charts | Recharts or another React SVG chart library after a small rendering spike | Charts and graphs in the document model | Charts must export as vector/image reliably and work offline |
| Formula parsing | math.js parser/AST, restricted to an allowlisted grammar | Parse user expression syntax | Do not call unrestricted `evaluate()` on untrusted user formulas |
| Decimal arithmetic | decimal.js, or math.js BigNumber with a documented precision policy | Avoid binary floating-point surprises | Keep units and rounding separate from numeric arithmetic |
| PDF generation | pdfmake 0.3.x | Browser-side PDF creation | Has table, header/footer, image/SVG, page-break and font features; test current release for browser bundle size and layout |
| Print | Semantic HTML renderer + print CSS | Browser print and Save as PDF | Use same canonical document model, not a separate content implementation |
| DOCX | `docx` 9.x | Browser-side Word document generation | Map supported blocks into paragraphs, tables, headers, footers and page breaks |
| XLSX | ExcelJS 4.x browser document-based workbook API | Styled workbook generation | Browser build does not include streaming writer; benchmark memory and enforce export limits |
| CSV | Small application-owned RFC 4180 serializer | Text/CSV output | Keep CSV output deterministic and guard against spreadsheet formula injection |
| ZIP | JSZip 3.x | ZIP backup and multi-file CSV exports | Generate Blob output in browser; large archives need memory testing |
| Unit testing | Vitest | Domain, formula, data model and renderer tests | Run on every relevant package/PR |
| UI/component tests | React Testing Library + user-event | Component interaction tests | Focus on accessible user behaviour |
| Browser/E2E tests | Playwright | Installability-related flows where supported, editor, storage, export and offline scenarios | Service-worker testing is Chromium-specific in Playwright; add real-browser checks for target browsers |
| Lint/format | ESLint + Prettier | Static quality and consistent formatting | Keep rules strict but avoid noisy rules that do not find real defects |

## 3. Architectural implementation choices

### 3.1 Build a custom block-based document model

The document's primary data format should be a normalized structured JSON model containing document metadata and ordered block instances. The React canvas renders this model. Rich text is a content type inside a block, not the root representation for the entire document.

This avoids forcing forms, calculations, registers, scientific observations and dynamic tables into a word-processing abstraction.

### 3.2 Use Tiptap narrowly

Use Tiptap for paragraph/rich-text blocks that require inline formatting, lists, links and similar authoring capabilities. Define a controlled extension set and store structured JSON. Sanitize imported HTML and links. Do not permit arbitrary HTML, scripts or untrusted embeds.

### 3.3 Build the visual layer with design tokens

Recommend CSS variables/design tokens and CSS Modules (or a similarly scoped CSS approach) rather than adopting a large component framework. The editor requires custom page canvas, dense panels, tables, block controls and predictable print styles. A light utility layer may be used, but the design system should remain owned by DocBuilder.

### 3.4 Keep the data grid headless

TanStack Table provides table state and row models; DocBuilder should own cell editors, typed columns, validation, row insertion/removal, repeatable-row behaviour and visual design. Do not make all register or worksheet content depend on a commercial grid package.

### 3.5 Keep file generation in adapters

Each output format should have its own adapter consuming the same structured document/record model. Do not derive PDF by screenshotting the screen, and do not derive DOCX from a raw screenshot or from a single HTML dump if it loses semantic structure.

## 4. Calculation-library recommendation

Use a controlled expression pipeline:

1. Parse expression text into a math.js AST.
2. Validate every node against an explicit allowlist.
3. Resolve only declared document field identifiers.
4. Evaluate the validated AST using a controlled interpreter and decimal.js arithmetic.
5. Apply unit compatibility/conversion through a DocBuilder-owned unit registry.
6. Apply method-specific rounding and reporting rules only after calculation.

The formula runtime must reject assignments, function definitions, property access, object construction, arbitrary imports, custom units from user expressions, access to browser APIs and unknown function names. Do not use JavaScript `eval`, `new Function`, or unrestricted expression evaluation.

math.js documentation explicitly discusses expression-parser security and high-precision BigNumber support; its security controls should be treated as guidance, not a replacement for DocBuilder's own AST allowlist and bounded interpreter.

## 5. Testing stack and required automated checks

| Test level | Tools | Coverage |
|---|---|---|
| Unit | Vitest | Formula parsing/evaluation, unit conversion, normalization, IDs, schemas, rounding, validation |
| Component | React Testing Library | Block controls, properties panel, tables, dialogs and save-state UI |
| Browser integration | Vitest Browser Mode where useful | IndexedDB interactions and DOM/browser API integration |
| E2E | Playwright | Create/edit/save/reload, template copy/update, register editing, backup/restore, exports |
| Offline E2E | Playwright Chromium plus manual target-browser checks | First-load readiness, disconnect network, cold/reopen offline, export offline, app update prompt |
| Regression | Golden fixtures | Stable PDF/DOCX/XLSX outputs, formula cases and representative workspaces |
| Accessibility | axe-core and keyboard review | Navigation, field labels, focus, keyboard block movement, dialogs |

## 6. Package and version policy

Versions listed here are current families/current release snapshots checked on 2026-11-13. Do not treat every patch number as a permanent requirement.

- Pin exact dependency versions through the package manager lockfile.
- Use maintained stable releases, not alpha/beta releases, for production-critical storage/export/calculation dependencies.
- Check package license, last release, security advisories, browser support, bundle size and peer dependencies before adding a dependency.
- Do not load runtime libraries from a CDN; all essential runtime dependencies must be bundled and precached for offline use.
- Add new dependencies only when they remove substantial risk or implementation work.
- Keep an SBOM/dependency inventory and run vulnerability/dependency health checks as part of release verification.

At the time of this recommendation, the official React documentation reports React 19.3; Vite's release page identifies the 8.3 line as receiving regular patches; Node.js identifies 26.x as Active LTS. The exact versions should be rechecked when the project is actually scaffolded.

## 7. What not to add

Do not add by default:

- Next.js/SSR or server routes (not required for a static local-first app).
- Firebase/Supabase or hosted databases.
- Authentication/role-management systems.
- Cloud-sync libraries.
- AI SDKs.
- A full LIMS or workflow product.
- A heavyweight component library that dictates the entire editor.
- Arbitrary JavaScript/TypeScript execution for custom blocks or formulas.

## 8. Implementation spikes required before final package lock

Before declaring the stack frozen, run small proof-of-compatibility spikes for:

1. React + Vite + PWA plugin + Workbox prompt update and offline precache.
2. Dexie transaction/migration with structured objects and Blob attachments.
3. Tiptap rich-text JSON round-trip and safe paste sanitization.
4. TanStack Table with custom editable cells and at least 10,000 rows.
5. Formula parser AST allowlist + decimal arithmetic + unit conversion.
6. pdfmake A4 export with multi-page tables, repeated headings, image/SVG, formula output and local fonts.
7. docx browser `Packer.toBlob` export of tables, headers, footers, page breaks and images.
8. ExcelJS browser `writeBuffer` export of a styled workbook with multiple sheets.
9. JSZip creation and restore validation with binary attachments.

These are engineering feasibility tests; they do not start the product implementation.

## 9. Approval items

1. Confirm Node.js 26 Active LTS as the build-tool baseline, subject to the compatibility spike.
2. Confirm CSS Modules/design tokens rather than Tailwind as the primary styling approach.
3. Confirm the recommended PDF/DOCX/XLSX libraries, subject to export prototypes.
4. Confirm whether chart blocks are required in the first release or can follow the initial data blocks.
5. Confirm target browsers and operating systems before freezing browser API support.

## 10. References

- [React versions](https://react.dev/versions) — latest official React version.
- [Vite releases](https://vite.dev/releases) and [Vite guide](https://vite.dev/guide/) — supported release lines and Node requirements.
- [Node.js release schedule](https://nodejs.org/en/about/previous-releases) — Node LTS status.
- [vite-plugin-pwa documentation](https://github.com/vite-pwa/docs) — Workbox/PWA integration.
- [Dexie documentation](https://dexie.org/docs/Dexie.js) — IndexedDB wrapper.
- [Tiptap React guide](https://tiptap.dev/docs/editor/getting-started/install/react) — rich-text integration.
- [TanStack Table](https://tanstack.com/table/latest) — headless data-grid engine.
- [dnd-kit changelog](https://dndkit.com/changelog/) — current drag-and-drop APIs.
- [math.js security](https://mathjs.org/docs/expressions/security.html), [AST parsing](https://mathjs.org/docs/expressions/parsing.html) and [BigNumbers](https://mathjs.org/docs/datatypes/bignumbers.html).
- [decimal.js](https://github.com/MikeMcl/decimal.js) — arbitrary-precision decimal arithmetic.
- [pdfmake](https://www.npmjs.com/package/pdfmake) — browser-capable PDF generation.
- [docx](https://www.npmjs.com/package/docx) — browser-capable DOCX generation.
- [ExcelJS](https://github.com/exceljs/exceljs) — browser document-based XLSX generation.
- [JSZip](https://stuk.github.io/jszip/) — browser ZIP generation.
- [Vitest Browser Mode](https://vitest.dev/guide/browser/), [Playwright service workers](https://playwright.dev/docs/service-workers).

---

**Approval status:** Proposed recommendation. Do not treat this document as an approved implementation baseline until accepted by the owner.
