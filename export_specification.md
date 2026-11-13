# DocBuilder — Export, Print and Backup Specification

**Document type:** Proposed engineering specification  
**Version:** 0.1  
**Status:** Recommended for owner review; not yet approved  
**Date:** 2026-11-13

---

## 1. Purpose

Define how DocBuilder generates professional PDF, DOCX, CSV, Excel and A4 print output, and how it creates/restores JSON and ZIP workspace backups.

The key requirement is that all document outputs originate from one canonical structured document model. Each output format has its own renderer/adapter; no format should become the hidden source of truth for another.

## 2. Confirmed output requirements

| Output | Required use |
|---|---|
| PDF | Shareable, print-ready document output |
| DOCX | Editable Microsoft Word-compatible document output |
| A4 print | Browser print layout, including Save as PDF through the print dialog |
| CSV | Flat export of selected tabular data |
| Excel/XLSX | Multi-sheet structured workbook export |
| JSON backup | Portable structured workspace backup, including attachments by the recommended approach |
| ZIP backup | Structured workspace plus separate binary attachments and checksums |

Importing arbitrary Word/Excel templates is not part of the confirmed requirement in this specification. Backup restore is required. General DOCX/XLSX import should be separately approved and specified if desired.

## 3. Canonical render model

The same normalized document model must feed:

1. in-app document preview;
2. HTML print renderer;
3. PDF renderer;
4. DOCX renderer;
5. structured CSV/XLSX data exporters.

The model should describe semantic blocks, field values, tables, images, headers/footers, page settings and calculation outputs. It should not store a PDF or DOCX as the only representation of document content.

Each renderer should declare support for every block type. If a renderer cannot represent a block, it must produce a visible export warning or a documented fallback; it must not silently omit content.

## 4. Recommended implementations by format

| Output | Recommended implementation | Notes |
|---|---|---|
| PDF | pdfmake browser API | Suitable for semantic PDF generation with tables, repeated table headers, headers/footers, images/SVG, fonts and page breaks. Prototype pagination and complex tables before final selection. |
| A4 print | HTML renderer + dedicated print CSS | Use browser print dialog; hide app panels and controls; define A4 page size, margins, page-break rules and repeating table headers where supported. |
| DOCX | `docx` library browser API | Generate WordprocessingML from canonical blocks; use paragraphs, tables, styles, headers/footers, page breaks and images. |
| XLSX | ExcelJS browser document-based workbook API | Create sheets for document metadata, field values, each table/register block and calculation details. Benchmark because browser API builds workbook in memory and has no streaming writer. |
| CSV | App-owned RFC 4180 serializer | One dataset/table per CSV file; UTF-8. For multi-table export, generate a ZIP containing individual CSV files and an index/manifest. |
| JSON | Versioned workspace JSON serializer | Include structured records, definitions and attachment payloads as Base64, with size warning. |
| ZIP | JSZip | Include manifest, workspace JSON, binary attachments in separate files and checksums. |

All essential dependencies must be bundled and available offline. Do not load PDF/DOCX/XLSX code or fonts from a remote CDN at export time.

## 5. PDF export

### 5.1 Page model

- A4 portrait is the default page size; landscape and other supported page sizes may be configured per document/template.
- Page size and margins are document settings, not hard-coded assumptions in each block.
- Header/footer layouts should use the document's configured fields and shared render model.
- Page number and total page count should be rendered where supported.
- The PDF must include all visible document content unless the user explicitly excludes a block in export settings.

Exact default margins, typography and header/footer measurements remain to be approved in the page-design specification.

### 5.2 Tables

- Support fixed and flexible column widths.
- Repeat table headers on continued pages where the PDF engine supports it.
- Avoid splitting a row across pages where possible.
- Provide a defined fallback for very wide tables (landscape, scale-to-fit with warning, or split table); do not silently shrink text to unreadable sizes.
- Support explicit keep-together/allow-split behaviour at table and row level where technically feasible.
- Support cell alignment, number formats, borders, fills and multiline content.

### 5.3 Formula output

Display the formula/result in a readable form. Where appropriate, include calculation inputs, units, method reference, intermediate results and the reported result. The renderer must use the stored calculation output/provenance, not re-run an independent formula implementation during export.

### 5.4 Images and charts

Support common raster formats and SVG/vector output as appropriate. Validate image type and size. Maintain aspect ratio by default. For charts, embed an SVG or high-quality image generated from the same chart data/configuration.

### 5.5 Fonts

Bundle fonts locally with appropriate licenses. Test glyph coverage for symbols, superscripts/subscripts, Greek characters and common scientific notation. Do not depend on system fonts being present in the same way on every device.

## 6. A4 print output

Print uses the HTML renderer and CSS `@page` rules. The print view should:

- render only the document, not application chrome;
- set A4 page size and agreed margins;
- preserve page breaks and block keep-together settings as supported;
- avoid clipping fixed-width tables;
- use print-safe colours and backgrounds;
- include configured headers/footers/page numbers where supported by the target browser;
- show a pre-print warning for unsupported/overflowing content.

Browser print output may vary by browser and print settings. The app should provide a PDF file export for a more controlled downloadable artifact. The print dialog's Save as PDF option remains a user-controlled browser feature.

## 7. DOCX export

### 7.1 Semantic mapping

| DocBuilder content | DOCX representation |
|---|---|
| Document title/subtitle | Paragraphs with named styles |
| Section headings | Heading styles with outline levels |
| Rich text | Runs with bold/italic/underline, links and supported inline styles |
| Lists | Numbered/bulleted paragraphs |
| Callouts | Styled table/paragraph with semantic label |
| Basic/dynamic tables | Word tables with widths, borders, cell formatting and repeated header where supported |
| Images | Inline image with size/aspect ratio |
| Header/footer | DOCX section header/footer |
| Page/section break | Corresponding Word break |
| Formula | Text/formula notation and result; mathematical vector image only where necessary |
| Chart/diagram | Embedded image or supported drawing representation |

### 7.2 Compatibility

DOCX output must be tested in current Microsoft Word desktop and LibreOffice. The export should remain editable. Avoid advanced OOXML features that are not supported consistently unless the feature is essential and tested.

### 7.3 Limitations

PDF and DOCX are different layout systems. Semantic and content parity is required, but pixel-identical pagination is not a realistic requirement. The app should identify blocks with unsupported or approximate DOCX rendering.

## 8. Excel/XLSX export

### 8.1 Workbook structure

For a completed document or record, recommend these sheets where relevant:

1. `Document Info` — document ID/title/type, metadata and export timestamp.
2. `Field Values` — structured scalar fields with block ID, block label, field key, value and unit.
3. One sheet per structured table/register block — each table exported as a real worksheet table with column headings and typed cells.
4. `Calculation Details` — formula/method ID and version, input references, input values/units, intermediate values, final result and rounding/reporting policy.
5. `References` — standard/method identifiers and edition metadata, not proprietary standard text.

Sheet names must be sanitized and kept within Excel's naming limits. Resolve duplicate names deterministically.

### 8.2 Data typing

- Numeric values should be numeric cells where the original field is numeric.
- IDs, codes and values with significant leading zeros should be text.
- Dates should be date cells with an explicit display format and preserved ISO representation in document metadata where needed.
- Units should be in explicit columns or metadata; do not append units to numeric cell values if doing so prevents calculation.
- Empty/missing values must remain distinguishable from zero.

### 8.3 Formula values

Do not turn DocBuilder formulas into live Excel formulas by default. Export the calculated numeric value and include the formula expression/method metadata as text in `Calculation Details`. This prevents differences in function support, floating-point arithmetic, formula recalculation and workbook security from changing the laboratory result outside DocBuilder.

An optional future “export as Excel formulas” feature would need separate approval and validation.

### 8.4 Styling and constraints

Use readable column widths, frozen header rows, filters, consistent number formats and clear worksheet titles. Avoid overly elaborate formatting. Protect neither the workbook nor its sheets by default unless requested separately.

ExcelJS browser generation is document-based and builds the workbook in memory; the browser streaming writer is not included. Set tested size limits, show progress, and benchmark large registers before offering export at scale.

## 9. CSV export

- CSV is one flat table per file.
- Use UTF-8 encoding; consider a UTF-8 BOM option for Excel compatibility if the owner accepts it.
- Apply RFC 4180 quoting/escaping rules for commas, quotes and newlines.
- Include headers and stable field keys or a companion schema/manifest for complex exports.
- Preserve numeric values and IDs correctly.
- Treat untrusted text beginning with spreadsheet formula trigger characters (`=`, `+`, `-`, `@`, tab or carriage return) to prevent CSV formula injection when opened in spreadsheet software. Numeric negative values must remain numeric, not be blindly prefixed as text.
- For a document containing multiple structured tables, offer “Export all tables as ZIP” containing each CSV plus a manifest mapping the files to their source block/register.

## 10. JSON backup

The recommended JSON backup is a self-contained workspace export, not simply an export of the current document.

It should include:

- backup format version;
- app and data schema versions;
- workspace metadata;
- documents and block instances;
- templates and template versions;
- custom block schemas and definitions;
- formula definitions and method package versions;
- register definitions and records;
- master-data records;
- record snapshots and amendment relationships;
- attachment metadata and Base64 payloads;
- integrity metadata where practical.

Warn about large JSON files because Base64 expansion and in-memory JSON serialization can increase memory usage. The ZIP backup is the recommended full-backup format for workspaces with many or large attachments.

## 11. ZIP backup

Recommended layout:

```text
DocBuilder-Backup-YYYY-MM-DD.zip
  manifest.json
  workspace.json
  checksums.json
  attachments/
    <attachment-id>.<safe-extension>
```

The manifest includes workspace ID, backup format version, DocBuilder version, schema versions, creation time, object counts and attachment count/total size. The checksum file maps each payload file to a SHA-256 digest.

Do not trust archive path names. Reject absolute paths, `..` traversal, duplicate conflicting paths, unexpected executable content and oversized expansion ratios. Validate all entries before restore.

## 12. Restore behaviour

The first release should implement:

- restore with validation and preview;
- replace-current-workspace, with an explicit pre-restore backup recommendation and clear warning;
- restore/export as a separate file without changing the current workspace.

A full “merge two independent workspaces” feature is deferred until ID collision, template, master-data and attachment conflict rules are specified.

Restore must be transactional/staged. If any required object or attachment fails validation, do not partially replace the live workspace.

## 13. Filename conventions

Recommend safe, predictable names:

- `DocumentID_DocumentTitle_YYYY-MM-DD.pdf`
- `DocumentID_DocumentTitle_YYYY-MM-DD.docx`
- `RegisterName_YYYY-MM-DD.xlsx`
- `RegisterName_YYYY-MM-DD.csv`
- `DocBuilder-Backup_YYYY-MM-DD_HH-mm.zip`

Sanitize filesystem-reserved characters and prevent path injection. Use local time or UTC consistently and record the export timestamp in metadata.

## 14. Export performance and memory management

- Load export libraries lazily but bundle them locally and precache them for offline operation.
- Generate large outputs in a Web Worker where library/runtime compatibility allows.
- Show progress and allow cancellation before the final file is written where possible.
- Set and test maximum attachment/record/workbook sizes.
- Avoid copying large binary buffers multiple times.
- Release object URLs and temporary blobs when no longer needed.
- Do not claim unlimited export size in a browser.

## 15. Acceptance tests

### PDF/print
- Multi-page document with headings and long paragraphs.
- Multi-page table with repeated headers.
- Wide table behaviour and overflow warnings.
- Header/footer and page numbering.
- Images, SVG charts, formula blocks and special scientific characters.
- A4 portrait/landscape and print preview.

### DOCX
- Document opens without repair prompts in Microsoft Word and LibreOffice.
- Tables, styles, page breaks, headers, footers and images remain editable.
- Unsupported block types produce a warning, not silent omission.

### XLSX/CSV
- Correct number/text/date types, leading zeros and empty values.
- One sheet per table and workbook metadata.
- Formula outputs exported as values plus traceable formula metadata.
- CSV quoting/newlines/Unicode and formula-injection cases.

### JSON/ZIP
- Round-trip all supported entity types and attachments.
- Checksum mismatch rejects restore.
- Unsupported backup schema is reported without changing live data.
- ZIP path traversal and unexpected entries are rejected.
- Large workspace memory/quota errors are handled visibly.

## 16. Open decisions

1. Should JSON include attachment payloads or only attachment metadata? This recommendation is self-contained JSON with embedded payloads, with ZIP preferred for large workspaces.
2. Should password protection/encryption be required for ZIP/JSON backups in v1?
3. Should CSV export include a UTF-8 BOM by default?
4. What maximum export/attachment sizes should be supported on target devices?
5. Should entire-document XLSX export include a separate sheet for every block's fields even when the block is not a table?
6. Which exact browser matrix must be supported for printing and PWA installation?

## 17. References

- [pdfmake package and feature list](https://www.npmjs.com/package/pdfmake) — PDF generation in browsers; tables, headers/footers, page breaks, SVG and fonts.
- [docx package](https://www.npmjs.com/package/docx) — JS/TS DOCX generation in browser and Node.
- [ExcelJS](https://github.com/exceljs/exceljs) — browser document-based workbook support; `writeBuffer` and limitation of browser streaming writer.
- [JSZip browser output](https://stuk.github.io/jszip/documentation/howto/write_zip.html) — Blob-based ZIP generation.
- [SheetJS data export documentation](https://docs.sheetjs.com/docs/solutions/output/) — workbook writing and CSV export; reviewed as an alternative, but ExcelJS is recommended where styled workbooks are important.
- [RFC 4180](https://www.rfc-editor.org/rfc/rfc4180) — common CSV format specification.

---

**Approval status:** Proposed recommendation. Do not treat as an approved export implementation baseline until accepted by the owner.
