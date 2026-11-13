# DocBuilder — Data Architecture Recommendation

**Document type:** Proposed engineering specification  
**Version:** 0.1  
**Status:** Recommended for owner review; not yet approved  
**Date:** 2026-11-13  
**Applies to:** DocBuilder local-first PWA

---

## 1. Purpose

This document recommends how DocBuilder should store, relate, version, recover, export and restore its local data. It translates the accepted product decisions into a proposed data architecture. It does not represent an implemented database.

The architecture must support documents, templates, completed records, registers, custom blocks, formula definitions, method packages, master data and attachments without a backend.

## 2. Governing principles

1. **Local-first:** IndexedDB is the authoritative store for user-created workspace data.
2. **Offline-capable:** Core reads, writes, calculations, rendering and exports must not require a network connection after the PWA is installed and its required assets are cached.
3. **One canonical document model:** The editor, preview and export adapters consume the same structured document representation.
4. **Stable identity:** Persistent objects use stable IDs; human-readable labels are never used as foreign keys.
5. **Explicit versioning:** App database schema, document schema, template version, block schema, formula definition and method package versions are separate concepts.
6. **No hidden document control:** There are no application-enforced approval events, controlled-document states, distribution workflows or audit-trail service.
7. **Historical record preservation:** Completing a laboratory record creates a preserved application snapshot. Later amendments create a separate related record.
8. **Visible persistence state:** The interface distinguishes saving, saved and save-failed states.
9. **Portable backups:** A user must be able to export and restore the complete local workspace.
10. **No silent data loss:** Migrations, restore and template updates are previewed, validated and recoverable.

## 3. Proposed storage technologies

| Technology | Responsibility | Recommendation |
|---|---|---|
| IndexedDB | Primary structured database and binary attachments | Use through Dexie.js; keep the IndexedDB model independent from UI components |
| Cache API | PWA static assets only | Managed by Workbox/service worker |
| `localStorage` | Small non-critical preferences only | Never store documents, records or attachments here |
| OPFS | Optional future large-file optimization | Do not make it a first-release dependency; browser compatibility and backup behaviour must be proven first |
| `Blob` / `File` | In-memory representation of attachments and export results | Persist attachment binary data in IndexedDB or an explicitly selected file storage mechanism |

The IndexedDB standard is designed for transactional storage of structured objects for offline web applications. Dexie provides a typed schema and transaction-oriented wrapper over IndexedDB. See the references at the end of this document.

## 4. Database ownership and workspace model

### 4.1 Workspace

The first release should use one default local workspace per PWA origin/browser profile. Do not add multi-user accounts, cloud workspaces or server synchronization.

A workspace metadata record should include:

- `workspaceId`
- `workspaceName`
- `createdAt`
- `modifiedAt`
- `dataSchemaVersion`
- `minimumCompatibleAppVersion`
- `backupFormatVersion`
- locale and display preferences where appropriate

A separate multi-workspace/profile feature is not required for the first release. Backup/restore is the portability mechanism.

### 4.2 Stable IDs

Use `crypto.randomUUID()` where available in the secure PWA context, with a tested UUID fallback only if the supported browser matrix requires it. IDs must not be recycled after deletion.

Every persistent object should have, as appropriate:

- `id`
- `createdAt`
- `updatedAt`
- `schemaVersion` or `definitionVersion`
- `name` / `label` for display, never as identity

Do not use document numbers, equipment codes, sample IDs or template names as database primary keys; users may edit or reuse them.

## 5. Proposed IndexedDB object stores

The following is a logical schema proposal. Exact key paths and compound indexes are to be finalized in the database design work package.

| Store | Main purpose | Important relationships / indexes |
|---|---|---|
| `workspaceMeta` | Workspace identity and backup metadata | Unique workspace ID |
| `documents` | Document envelope, title, category, type, status-as-content, metadata and source template reference | Index by name, category, modified date, source template |
| `documentBlocks` | Ordered block instances and their content/configuration | Index by `documentId`, block type, position |
| `templates` | Stable template identity and descriptive metadata | Index by name, document category, document type |
| `templateVersions` | Immutable template definition versions | Index by `templateId`, version, current flag |
| `customBlockDefinitions` | User-created visual and schema-defined blocks | Index by category, name, schema version |
| `registerDefinitions` | Register structure, columns/fields, view configuration | Index by category, name |
| `registerRecords` | Individual structured entries in registers | Compound indexes by register, record date, key fields |
| `masterDataRecords` | Equipment, personnel, chemicals, reagents, methods, units and other reusable data | Compound indexes by `masterDataType`, display code/name |
| `formulaDefinitions` | User-authored reusable formula definitions and metadata | Index by scope, name, version |
| `methodPackages` | Built-in method-specific calculation packages and reviewed metadata | Index by discipline, analyte/parameter, standard reference, version |
| `recordSnapshots` | Completed record snapshots and amendment relationships | Index by source document/record, completion date, amended record ID |
| `attachments` | Binary blobs, filename, MIME type, byte size, checksum and metadata | Index by owner type/owner ID, checksum |
| `preferences` | Theme, density, editor preferences and other non-critical preferences | Key-value |

### 5.1 Native block definitions

Native block definitions should be shipped with the application as versioned source-controlled manifests/code, not edited directly in the IndexedDB store. The runtime may cache a manifest locally for diagnostics, but the authoritative native definition belongs to the application release.

### 5.2 Custom block definitions

Visual and advanced custom block definitions are user data and belong in `customBlockDefinitions`. Every custom block must record its schema version and the renderer/runtime capability version it requires.

### 5.3 Document content storage

Keep each document envelope in `documents` and its ordered block instances in `documentBlocks`. Use transactions when a change spans both stores. Each block instance should have:

- a stable `blockInstanceId`
- `documentId`
- `blockTypeId` (original catalogue ID or namespaced custom ID)
- `definitionVersion`
- `position` / order key
- `configuration`
- `content` / field values
- `visibility` and layout settings where applicable
- references to attachments and other permitted local entities

Store structured rich-text JSON, not arbitrary HTML, as the primary content representation. HTML is a rendering/export representation and must be sanitized.

## 6. Document, template and record relationships

### 6.1 Document from template

When a user creates a document from a template, copy the chosen template version and its block tree into an independent document instance. Store:

- `sourceTemplateId`
- `sourceTemplateVersionId`
- `sourceTemplateVersionNumber`
- an optional source-template label snapshot

Later template edits must not affect the document automatically.

### 6.2 Selective template update

When a user explicitly updates a document from a newer template version:

1. Load the document's source template version and the selected target template version.
2. Compare stable block identities/keys and their properties.
3. Show added, removed, moved and changed blocks.
4. Identify conflicts where the document has changed content that the template also changes.
5. Require the user to choose how to handle each conflict.
6. Apply the selected update in one transaction.
7. Preserve the pre-update document state for local recovery.

Never overwrite user-entered record values merely because a template field default changed.

### 6.3 Completed record snapshots

Recommended behaviour:

- A user can mark a document/record as `Completed` (or another user-defined informational state).
- Completion creates a self-contained snapshot of the document structure, values, calculation outputs, formula/method versions, referenced master-data values and attachment references/checksums.
- The completed snapshot is preserved by the application as a separate record-snapshot entity.
- Editing after completion starts a separate amendment/copy linked to the original snapshot; it must not silently mutate the preserved original.
- The snapshot may be exported independently.

**Important limitation:** A local browser application without a server-side trust anchor cannot claim that a snapshot is tamper-proof, legally immutable or independently audited. The feature protects against ordinary accidental edits within the application; it is not a cryptographic evidence system.

### 6.4 Recovery snapshots versus document revision history

Keep these separate:

- **Undo/redo:** short-lived editor interaction history.
- **Recovery snapshot:** an application-generated local restore point to recover from accidental edits or failed operations.
- **Document revision-history content:** a user-editable table/section inside the document.

Do not create an application-managed formal document revision or approval history.

## 7. Master-data references and snapshots

A document may refer to a local master-data record such as equipment or a reagent. The document must support two distinct behaviours:

1. **Live reference:** display current master-data values while the document is still being prepared, with an obvious indication that values are coming from a shared record.
2. **Captured snapshot:** when a completed record is created, copy the relevant referenced values into its snapshot so later edits to the master-data record do not rewrite the completed record.

The snapshot should record the master-data ID and the captured values. If a referenced master-data item is deleted later, the completed record must still display the captured values.

Do not assume all master-data categories have the same fields. Each category requires its own schema, validation and reference behaviour.

## 8. Formula and method data relationships

- A formula may reference fields/calculations in different blocks **within the same document only**.
- Formula references must use stable `blockInstanceId` + `fieldKey` identifiers, not labels or screen positions.
- A formula must not reference another document, register record or workspace-level master-data field as a calculation input in the confirmed first-release scope unless that value has first been copied into the current document as a snapshot/input.
- Built-in method packages and user formulas must have distinct origin/type markers.
- Completed record snapshots must preserve the exact method/formula package version and calculation result.

## 9. Attachments

Each attachment record should include:

- `attachmentId`
- original filename
- MIME type detected/verified at import
- byte size
- SHA-256 checksum
- created/imported date
- owning entity type and ID
- optional user-entered description
- binary Blob or archive reference

Do not trust only the filename extension. Validate the actual file signature/MIME where practical. Define a configurable file-size limit after browser testing; do not promise unlimited uploads.

For normal operation, attachments should be stored as binary data in IndexedDB. Do not convert every attachment to Base64 for routine storage; Base64 is acceptable inside the single-JSON backup format where needed.

## 10. Persistence, transactions and autosave

### 10.1 Save behaviour

Use debounced autosave for active editing, with an explicit Save action as an additional user control. Autosave must:

- write the document and dependent block changes transactionally;
- show `Saving`, `Saved` and `Save failed` states;
- update `updatedAt` only on successful commit;
- surface quota and transaction errors;
- avoid silently discarding pending edits during navigation or reload.

A reasonable starting debounce for testing is 500–1,000 ms after the last input, with flush-on-blur/navigation where safe. The final value is a performance decision, not a data-integrity rule.

### 10.2 Multi-tab behaviour

Although the product is single-user, the same workspace might be open in multiple tabs. Use a simple coordination mechanism (for example, BroadcastChannel plus an optimistic document update token) to detect stale edits. If a conflict is detected, warn and offer reload/compare/recovery rather than silently applying last-write-wins to a whole document.

### 10.3 Transactions

Use IndexedDB transactions for operations that must succeed together, including:

- document envelope + block changes;
- completed snapshot creation + amendment state update;
- template update application;
- register record + attachment relationship update;
- workspace restore.

## 11. Database schema migrations

Maintain separate versions for:

1. application release;
2. IndexedDB schema;
3. document content schema;
4. native block definitions;
5. custom block schemas;
6. formula engine/runtime;
7. method package definitions;
8. backup format.

Migration rules:

- migrations are deterministic, versioned and idempotent where feasible;
- migrations are tested against representative prior-version workspaces;
- take or require a validated backup before any destructive migration;
- never silently discard unknown custom fields;
- if a migration is unsupported, retain the source data and present a clear recovery/export route;
- do not activate an incompatible app update until migration preflight succeeds.

## 12. Backup and restore contract

### 12.1 JSON backup

Recommend one self-contained JSON file containing workspace metadata, all structured records, templates, documents, custom schemas, formulas, method-package snapshots and attachments encoded as Base64. Because Base64 increases size and a large JSON object may consume substantial memory, warn before creating/restoring large JSON backups and set tested file-size limits.

### 12.2 ZIP backup

Recommended structure:

```text
DocBuilder-Backup-<date>.zip
  manifest.json
  workspace.json
  attachments/
    <attachment-id>.<extension>
  checksums.json
```

`workspace.json` contains structured workspace data and references to attachment IDs. Binary files are stored separately. `manifest.json` contains backup format version, DocBuilder version, schema versions, creation time, workspace ID and record counts. `checksums.json` contains SHA-256 checksums for payload files.

### 12.3 Restore preflight

Before changing the current workspace:

1. Read and validate the manifest and format version.
2. Validate JSON/schema structure.
3. Verify attachment paths, MIME/size rules and checksums.
4. Check for unsupported newer schema versions.
5. Show workspace name, creation date, object counts and possible conflicts.
6. Require explicit confirmation of the restore mode.
7. Restore into a staging database/temporary object store where feasible, then switch/commit only after validation.
8. Preserve or prompt for a backup of current data before replacement.

**Recommendation for first release:** implement `Replace current workspace` and `Restore as a separate exported copy` first. Defer a sophisticated cross-workspace merge until conflict semantics are fully designed.

### 12.4 Backup encryption

Recommend an optional password-protected ZIP/JSON backup using Web Crypto AES-GCM with a password-based key derivation scheme and a unique salt/nonce per backup. This needs a separate threat-model and usability review. Do not claim that optional archive encryption protects data already stored in the active browser profile or replaces operating-system account/disk protection.

## 13. Storage quota and durability

- Request persistent storage (`navigator.storage.persist()`) when supported and report whether it was granted.
- Monitor estimated usage/quota where supported.
- Warn before large imports, uploads, backups and exports.
- Handle `QuotaExceededError` and transaction aborts explicitly.
- Provide an always-accessible backup action and a configurable reminder.
- Treat browser storage as recoverable working storage, not as the only copy of important laboratory records.

## 14. Acceptance criteria for this architecture

- Core documents, templates, register records and attachments persist after reload and browser restart.
- Core operations work with network disabled after PWA readiness.
- A completed record snapshot remains unchanged when its source document or master data is edited later.
- An amendment creates a separate related record.
- A template update never silently changes existing documents.
- A template update conflict is visible before changes are applied.
- Formula references use stable same-document field identities.
- Backup includes all data and attachments and validates checksums.
- Restore failure does not leave a partially replaced workspace.
- Schema migration tests cover at least the previous supported application/data version.
- Quota errors are visible and do not falsely report a successful save.

## 15. Items still requiring owner/engineering approval

1. Whether password-protected backup is required in the first release or deferred.
2. Whether `Restore as a separate exported copy` should be a true second local workspace or simply a downloaded validated backup.
3. Final attachment size/file-type policy after target-browser tests.
4. Final autosave debounce and recovery-snapshot retention limits.
5. Final browser and operating-system support matrix.
6. Exact master-data categories and reference/snapshot policy for each category.
7. Whether completed-record snapshots should include generated PDF/DOCX outputs automatically or only structured source data and attachments.

## 16. References

- [W3C Indexed Database API 3.0](https://www.w3.org/TR/IndexedDB/) — transactional browser database specification.
- [Dexie.js documentation](https://dexie.org/docs/Dexie.js) — IndexedDB wrapper and schema upgrades.
- [MDN Storage API](https://developer.mozilla.org/en-US/docs/Web/API/Storage_API) — storage estimate and persistence APIs.
- [MDN StorageManager.persist()](https://developer.mozilla.org/en-US/docs/Web/API/StorageManager/persist) — persistent storage request.
- [web.dev: PWA update guidance](https://web.dev/learn/pwa/update) — safe app/data update considerations.

---

**Approval status:** Proposed recommendation. Do not treat this document as an approved implementation baseline until accepted by the owner.
