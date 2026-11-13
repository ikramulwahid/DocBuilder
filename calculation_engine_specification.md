# DocBuilder — Calculation Engine Specification

**Document type:** Proposed functional and technical specification  
**Version:** 0.1  
**Status:** Recommended for owner and technical review; not yet approved  
**Date:** 2026-11-13

---

## 1. Purpose

Define a shared calculation engine for laboratory worksheets and records. It must support both developer-supplied method-specific calculations and user-created formulas, while keeping results reproducible, explainable and tied to their inputs and method/formula version.

This document defines the engine. It does not approve any laboratory method or formula. Formula packages must be technically checked against the laboratory's controlled method and verified with reference test cases before being labelled verified.

## 2. Two separate sources of calculations

### 2.1 Built-in method-specific calculation packages

A built-in package is authored or configured by the development team and technically reviewed by a qualified laboratory subject-matter expert before release.

It must identify:

- method package ID and version;
- discipline and test/parameter;
- sample matrix and applicable scope;
- standard/source identifier, title, edition and amendments;
- applicable method/SOP revision supplied or confirmed by the laboratory;
- input fields, units, types and required status;
- formula(s), constants and calculation sequence;
- blank, dilution, aliquot, correction and basis handling where applicable;
- intermediate results;
- result unit and reporting basis;
- significant digits/decimal places and rounding rule;
- acceptance or QC checks, if they are method-defined;
- limitations, prerequisites and known exclusions;
- validation/test-case set and technical reviewer/approval metadata as content of the package, not as an application workflow.

A standard reference alone does not mean the formula package is verified. Verification requires a controlled source, a correct implementation and reviewed test evidence.

### 2.2 User-created formulas

Users can create formulas using:

- a visual formula builder with field pickers, operators, supported functions and unit hints;
- an advanced expression editor for users comfortable with formula syntax.

Both editors must create the same constrained formula representation and use the same evaluator. The advanced editor must not permit arbitrary JavaScript, browser API access, imports or user code execution.

A user-created formula must be labelled as user-defined. It must not be described as an official method or verified standard formula unless separately reviewed and packaged as a built-in method.

## 3. Scope of formula references

Confirmed scope: formulas may reference fields/calculations across blocks **within the same document only**.

A formula reference is identified using stable internal IDs, conceptually:

`documentId + blockInstanceId + fieldKey`

The visual formula builder may show readable aliases such as `SampleMass`, but the stored definition must resolve to stable IDs.

Not included in the first release:

- formula links to another document;
- formula links directly to a register record;
- formula links directly to a changing master-data value;
- cross-workspace formulas.

If an external value is needed, the user must copy/reference it into the current document as a documented input/snapshot before calculation.

## 4. Formula model

Each formula definition should include:

| Property | Meaning |
|---|---|
| `formulaId` | Stable formula identity |
| `formulaVersion` | Explicit definition version |
| `origin` | `builtin-method` or `user-defined` |
| `expression` | Stored constrained expression |
| `astVersion` | Expression parser/AST format version |
| `inputs` | Declared stable field references and expected types/units |
| `output` | Output field key, type and unit |
| `functionsAllowed` | Allowed function identifiers or function-set version |
| `rounding` | Method/formula-specific rounding/reporting policy |
| `precision` | Internal decimal precision policy |
| `preconditions` | Required ranges, non-zero denominators, required values etc. |
| `warnings` | Non-blocking conditions |
| `methodReference` | Required for built-in packages; optional for user formulas |
| `testCases` | Expected outputs and tolerances |
| `createdAt` / `updatedAt` | Local metadata |

The exact storage schema will be finalized in the data-model specification.

## 5. Expression syntax recommendation

Start with a small, documented grammar rather than attempting to reproduce all of Excel or JavaScript.

Recommended supported elements:

- numeric constants (decimal and scientific notation);
- field references inserted by the formula builder;
- parentheses;
- addition, subtraction, multiplication and division;
- exponentiation with explicit restrictions on exponent type/range;
- comparison operators for conditions where needed;
- a controlled function set such as `abs`, `sqrt`, `min`, `max`, `sum`, `average`, `round`, `floor`, `ceil` and `if` only after its semantics are specified;
- approved aggregate operations over explicitly scoped repeatable table columns.

Initially disallow:

- assignments and variable definitions;
- function definitions;
- arbitrary property/member access;
- object/array construction by user expression;
- imports and custom units created from formulas;
- arbitrary recursion;
- network, filesystem, browser, DOM or global-object access;
- arbitrary code or plugin execution.

Formula references and supported functions must be inserted through the UI where possible to reduce syntax errors.

## 6. Evaluation architecture and safety

Recommended pipeline:

1. Parse the expression text into an AST.
2. Traverse and validate every AST node against DocBuilder's explicit allowlist.
3. Resolve every symbol to a declared same-document field reference.
4. Validate input types and units.
5. Build a dependency graph for calculated fields.
6. Detect missing references and cycles.
7. Evaluate the validated AST through a controlled interpreter using decimal arithmetic.
8. Apply method/formula-specific output conversion and rounding.
9. Save the displayed result and its provenance.

Do not execute `eval`, `new Function`, or unrestricted user code. If math.js is used as the parser, its AST is only parsed input; the application must validate it and evaluate it through its own restricted evaluator. Do not assume a library's sandbox alone is a complete security boundary.

## 7. Numeric precision and rounding

### 7.1 Internal arithmetic

Use arbitrary-precision decimal arithmetic (recommended: decimal.js) rather than JavaScript binary floating-point for user-visible laboratory calculation chains.

The project must define:

- internal significant-digit precision;
- intermediate operation precision;
- division precision;
- when rounding is allowed;
- final rounding mode;
- handling of negative zero and scientific notation.

Do not select one global final rounding rule for all methods. The method or user formula configuration must define output rounding and significant figures. If the source method does not define a rule, leave the policy explicitly pending technical review; do not invent one.

### 7.2 Display versus stored value

Store the unrounded/precision-preserved calculation value where appropriate, plus the reported value and its formatting policy. The exact decimal representation should be serialized as a string, not converted to a JavaScript number for storage.

## 8. Unit system

Build a controlled unit registry with:

- unit code and canonical symbol;
- dimension (mass, volume, length, temperature, time, pressure, concentration, energy, etc.);
- conversion factor and offset where applicable;
- allowed prefixes;
- quantity-specific display rules;
- method-specific restrictions where required.

Rules:

- values are stored with their unit identity;
- compatible conversions are explicit and deterministic;
- incompatible dimensions block calculation;
- no silent conversion between mass concentration and amount concentration;
- temperature conversions must account for offsets;
- do not mix measurement bases (as-received, air-dried, dry, dry ash-free) without an explicit operation and source basis;
- report units must be part of the output definition.

A unit conversion library may be evaluated, but DocBuilder should own the approved unit codes and method-specific constraints. User formulas must not be allowed to create or override unit definitions.

## 9. Dependency graph and evaluation order

Calculated fields may depend on ordinary inputs or other calculated fields within the same document. The engine should:

1. create a directed dependency graph;
2. topologically order independent calculations;
3. evaluate only affected descendants when an input changes;
4. identify cycles and report the involved fields;
5. preserve a calculation trace for each evaluated output.

For repeatable tables, formula scope must be explicit: same-row, each-row, table aggregate or selected rows. Do not silently aggregate across every row.

## 10. Error and warning model

Use separate result states:

- `not-configured`
- `waiting-for-input`
- `invalid-input`
- `invalid-unit`
- `dependency-error`
- `circular-reference`
- `domain-error`
- `calculation-error`
- `calculated`
- `calculated-with-warning`

Examples of blocking errors: divide by zero, missing mandatory input, non-numeric value, incompatible units, invalid square-root domain, missing formula field, circular dependency.

Examples of warnings: value outside a user-configured advisory range, an optional QC observation omitted, or a method-specific caution. Warnings must not silently become pass/fail accreditation decisions.

## 11. Calculation provenance

Each calculated output should be traceable to:

- input values and units;
- source block/field IDs;
- formula ID and version;
- method package ID/version, when applicable;
- standard/SOP reference and edition;
- engine version;
- intermediate values;
- unit conversions;
- rounding/reporting policy;
- calculation timestamp;
- warnings/errors.

For completed record snapshots, preserve this provenance so a later formula package update does not rewrite the historical calculation.

This is calculation traceability inside the local record, not an application audit trail or tamper-proof mechanism.

## 12. Method package structure

Recommended package components:

1. `method.json` — identity, discipline, analyte/parameter, matrix and reference metadata.
2. `fields.json` — input/result field schemas and units.
3. `formulas.json` — formulas, sequence, dependencies and rounding/reporting rules.
4. `validation.json` — ranges, mandatory fields, conditions and warnings.
5. `test-cases.json` — known input/output vectors and expected tolerances.
6. `references.md` — controlled source references, edition and technical notes.
7. `package-version.json` — package/runtime compatibility.

A built-in package must not embed or redistribute copyrighted standards text unless the project has permission. Keep method citations and implementation notes; the laboratory supplies or confirms the controlled method text.

## 13. Verification requirements for built-in calculations

Before a built-in formula package is labelled `technically-reviewed` or made available for production use:

- compare its formula and conditions against the controlled method/SOP;
- test nominal, minimum/maximum and boundary inputs;
- test blank correction, dilution/aliquot handling and conversion paths where applicable;
- test zero, negative and missing inputs;
- test unit mismatch and basis mismatch;
- test rounding and significant-figure rules;
- use independent reference calculations or known reference data;
- obtain technical sign-off from the laboratory's competent person;
- record the package version and review evidence.

The developer may write automated tests, but cannot replace laboratory technical approval.

## 14. Initial formula capability groups

| Group | Examples | Required engine capability |
|---|---|---|
| General arithmetic | Percentage, difference, ratio, average | Decimal arithmetic, field references |
| Dilution/concentration | C1V1=C2V2 variants, dilution factors | Unit checking, explicit concentration basis |
| Volumetric/titrimetric | Titrant volume, normality/molarity factors, blank correction | Method-specific constants and aliquot/dilution logic |
| Gravimetric | Mass difference and percentage calculations | Mass units, tare/gross/net and basis checks |
| Calibration curve | Regression, back-calculation and dilution | Data-series handling, model selection and regression diagnostics |
| Environmental water | BOD/COD/TDS/TSS and other configured parameters | Method-specific input schemas; no universal formulas |
| Coal | Moisture, ash, volatile matter, fixed carbon, GCV/NCV and basis conversions | Method-specific test conditions, corrections, reporting bases and precision |
| QC/statistics | Duplicate difference, recovery, RPD, Z-score, control limits | Statistical definitions and configured acceptance criteria |
| Measurement uncertainty | Components, sensitivity coefficients, combined/expanded uncertainty | Uncertainty model and covariance/coverage configuration appropriate to method |

This table is a capability roadmap, not a list of verified equations.

## 15. Acceptance criteria

- Same formula + same inputs + same method/runtime versions produces the same stored decimal output.
- Unit incompatibility is detected before calculation.
- Missing values are never silently treated as zero.
- Circular formulas are detected and identified.
- Formula dependencies are recalculated deterministically.
- Formula errors are visible and explain which field or operation failed.
- User formulas cannot call arbitrary JavaScript or browser APIs.
- Completed snapshots retain the formula/method version and inputs used.
- Every built-in method package has automated golden test cases and laboratory technical review evidence.

## 16. Open decisions

1. Exact function set and formula grammar.
2. Internal decimal precision.
3. Unit-code source and controlled unit catalogue.
4. Significant-figure and rounding conventions for each initial method.
5. Regression models and weighting options for calibration curves.
6. Whether formula authoring supports table-row aggregates in the first release.
7. Who provides the source SOPs and reference calculations for each method package.

## 17. References

- [math.js expression parsing](https://mathjs.org/docs/expressions/parsing.html) — AST parsing.
- [math.js security guidance](https://mathjs.org/docs/expressions/security.html) — risks and restricted parser functions.
- [math.js BigNumbers](https://mathjs.org/docs/datatypes/bignumbers.html) — arbitrary-precision numeric support.
- [decimal.js](https://github.com/MikeMcl/decimal.js) — arbitrary-precision decimal arithmetic.
- [NABL Documents](https://nabl-india.org/nabl-documents/) — current NABL guidance and policy documents; method packages should cite applicable documents only where relevant.

---

**Approval status:** Proposed recommendation. No calculation package is technically approved merely by inclusion in this specification.
