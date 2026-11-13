# DocBuilder — Recommended Initial Testing Method Catalogue

**Document type:** Proposed method-selection baseline  
**Version:** 0.1  
**Status:** Candidate list for laboratory-owner/technical review; not approved methods  
**Date:** 2026-11-13

---

## 1. Purpose and important limitation

This document recommends which laboratory testing method families DocBuilder should support first. It is intended to guide initial technical block design, calculation-engine coverage and starter worksheet selection.

It is **not** an accreditation scope, an approved laboratory method list, a substitute for controlled standards/SOPs, or evidence that a formula has been verified.

A method can be activated as a built-in verified package only after the exact method, edition, amendments, matrix, laboratory SOP, formula, input fields, units, reporting basis and reference test cases have been confirmed by the laboratory's competent technical personnel.

## 2. Selection principles

1. Cover all four confirmed first-release areas: QMS/general, chemical, environmental, coal/coke.
2. Start with methods likely to reuse the same block patterns: sample details, equipment, reagents, observations, calculations, QC and reporting.
3. Prefer complete method packages over large numbers of empty parameter templates.
4. Keep method identity separate from block identity. The same pH input/result block may be reused by different methods, but each method package retains its own reference and rules.
5. Separate a generic calculation capability from a method-verified formula.
6. Support records/templates for a discipline even where the exact technical calculation package is deferred pending an approved method.
7. Do not assume the laboratory performs every method listed below. Confirm against the lab's intended/accredited scope and available controlled standards.

## 3. Recommended release approach

### Release group A — Initial method packages to prioritize

These are the proposed core candidates for the first method-specific implementation wave.

| Discipline | Candidate method family | Main document/record | Engine capability | Priority |
|---|---|---|---|---|
| Chemical | Volumetric/titrimetric analysis: acid-base, redox and complexometric variants | Titration worksheet, standardization record | Blank/aliquot/dilution handling and method-specific titration formula | A1 |
| Chemical | Gravimetric analysis | Gravimetric worksheet | Tare/gross/net mass, mass difference and method-specific percentage | A1 |
| Chemical | pH measurement | pH observation record | Instrument reading, calibration/verification record fields and result capture | A1 |
| Chemical | Conductivity measurement | Conductivity record | Reading, cell constant/temperature compensation only where method requires | A1 |
| Chemical | UV-Visible spectrophotometry | Instrument observation and calibration-curve record | Blank correction, calibration regression, back-calculation and dilution | A1 |
| Environmental | Water/wastewater pH, conductivity, turbidity, TDS and TSS | Water analysis worksheet | Parameter-specific result capture and gravimetric/volume calculations where applicable | A1 |
| Environmental | Water/wastewater alkalinity, total hardness, chloride | Analytical worksheets | Titration calculations with method-specific factors | A1 |
| Environmental | Dissolved oxygen and BOD | DO/BOD worksheets | Initial/final DO, dilution and seed/control corrections only as specified by the selected method | A1 |
| Environmental | COD (dichromate method family) | COD worksheet | Blank/sample titration or instrument-based calculation as appropriate to the chosen method | A1 |
| Coal/coke | Proximate analysis: moisture, ash, volatile matter and fixed carbon by difference | Proximate analysis worksheet | Separate method-specific moisture/ash/volatile calculations; fixed carbon by defined difference on the same reporting basis | A1 |
| Coal/coke | Gross calorific value by bomb calorimeter | GCV worksheet | Calibration/standardization, sample run, corrections and reporting basis exactly matching selected method | A1 |
| Coal/coke | Total sulphur | Sulphur worksheet | Method-specific combustion/Eschka/instrument calculation package after exact method selection | A1 |

### Release group B — Include structured templates/records initially; activate calculations after technical method confirmation

| Discipline | Candidate method family | Suggested initial support | Why calculation activation may be deferred |
|---|---|---|---|
| Chemical | AAS / ICP-OES / ICP-MS metal analysis | Standards, calibration levels, instrument readings, QC, sample and result tables | Instrument/software output formats, calibration model and matrix corrections vary |
| Chemical | HPLC / GC analysis | Sequence, standard/curve, chromatogram result entry, QC and report fields | Instrument platform, integration and method-specific response models differ |
| Chemical | Standard/reagent solution preparation and standardization | Structured preparation and standardization records | Concentration/standardization formulas depend on substance, reaction stoichiometry and method |
| Environmental | Sulphate, fluoride, nitrate/ammonia and other analytes | Method-specific result records | Methods and interferences differ by analyte, matrix and instrument |
| Environmental | Heavy metals by AAS/ICP | Calibration/result/QC templates | Must specify analyte, instrument technique, wavelength/mass, digestion and matrix |
| Environmental | Soil pH, moisture, conductivity and selected chemical parameters | Soil sample and result worksheets | Soil preparation, extractant, soil:solution ratio and method reference must be fixed |
| Environmental | Ambient air particulate matter (for example PM10/PM2.5) | Sampling and monitoring record, mass/flow/time fields | Sampler type, flow verification, filter conditioning and calculation method must be selected |
| Environmental | Stack emission particulate/gas monitoring | Field sampling, equipment, traverse, flow and result records | Sampling train, reference conditions and pollutant-specific methods vary substantially |
| Coal/coke | Ultimate analysis | Element-specific worksheets and result reporting | Carbon/hydrogen/nitrogen/sulphur methods are distinct and require confirmed standards |
| Coal/coke | HGI | Observation and calculation record | Sample preparation, reference coal and exact method conditions must be confirmed |
| Coal/coke | Ash fusion temperature | Observation record with deformation/softening/hemisphere/flow temperatures | Atmosphere and test conditions are integral to the method |

## 4. QMS and general laboratory documentation

QMS/general documentation is a document-generation area rather than a laboratory test-method package. Initial templates should cover:

- Quality Manual and laboratory profile;
- policies and objectives;
- organization chart and role/responsibility descriptions;
- document and record procedures/forms as laboratory content;
- impartiality and confidentiality documentation;
- risk and opportunity assessment forms/registers;
- internal audit plans, checklists and reports;
- management review agenda/minutes/action records;
- nonconforming work, complaint and corrective-action forms/records;
- personnel competence and training records;
- equipment and calibration records;
- sample handling and testing records;
- method verification/validation and measurement uncertainty workbooks;
- technical record and test report templates.

DocBuilder creates and stores these as ordinary documents/records. It does not enforce approval, distribution, audit workflow, CAPA workflow or document-control lifecycle.

## 5. Candidate standards/reference families to verify

The following sources are useful starting points. They must be checked against the laboratory's actual controlled copies and scope before method packages are implemented.

### 5.1 Water and wastewater

- BIS `IS 3025` is a family of physical/chemical sampling and test methods; do not treat it as a single method or assume all parts are current.
- BIS currently lists `IS 3025 (Part 11):2022` for pH (reviewed in 2026).
- BIS lists `IS 3025 (Part 15):2023` for total residue/dissolved/suspended solids with an Amendment 1 dated 2026; Part 16:2023 is also listed for filterable residue/TDS at 180 °C. Confirm which part and procedure matches the laboratory's method.
- BIS lists `IS 3025 (Part 23):2023` for alkalinity and `IS 3025 (Part 38):2025` for dissolved oxygen. The optical sensor sub-method `IS 3025 (Part 38/Sec 2):2023` is also listed; these must not be treated as interchangeable without method review.
- APHA/AWWA/WEF *Standard Methods for the Examination of Water and Wastewater*, 24th edition (2023), is a current reference family for selecting water/wastewater analytical methods. The exact method number and edition must be recorded for each analyte.

### 5.2 Coal and coke

- BIS `IS 1350 (Part 1):2025`, Third Revision, covers proximate analysis.
- BIS `IS 1350 (Part 2):2022`, Third Revision, covers gross calorific value.
- BIS `IS 1350` Part 3 covers total sulphur by the Eschka method; confirm exact edition/amendment status against the current BIS catalogue before adoption.
- ASTM `D5865/D5865M-26` is listed as an active 2026 gross calorific value method.
- ASTM `D3173/D3173M-25` covers moisture in the analysis sample; `D3174-12R18E01` is listed as active for ash; `D3175-26` covers volatile matter; `D3180-25` covers calculation of analyses from as-determined to different bases.
- ASTM `D4239-26` covers sulphur by high-temperature tube furnace combustion; `D409/D409M-24` covers Hardgrove grindability; `D1857/D1857M-24` covers ash fusibility.
- BIS `IS 1350 (Part 4/Sec 1):2023` covers determination of carbon and hydrogen; this is a separate ultimate-analysis package.

### 5.3 Chemical methods

The chemical method packages should be selected from the laboratory's actual approved/controlled references. The initial families should include volumetric/titrimetric, gravimetric, pH, conductivity and UV-Visible spectrophotometry. AAS, ICP-OES, ICP-MS, GC and HPLC can follow with analyte-specific and instrument-specific method metadata.

Do not publish a generic chemical formula as a standard method without identifying the reaction, stoichiometry, standardization factor, sample preparation, blank correction, dilution, matrix and reporting basis.

## 6. Suggested method-package template

Each method package should record at least:

| Field | Requirement |
|---|---|
| Method package ID | Stable application ID |
| Discipline | Chemical / Environmental / Coal-Coke / General |
| Test / analyte / parameter | Specific measurable parameter |
| Matrix | Water, wastewater, soil, coal, coke, air, stack, etc. |
| Method type | Standard method, laboratory-developed method, modified method, etc. |
| Standard body | BIS / APHA-AWWA-WEF / ASTM / ISO / other |
| Standard number | Exact identifier and part/section |
| Edition/year | Exact edition/year |
| Amendment/corrigendum | Current applicable amendment information |
| Lab SOP reference | Laboratory's actual controlled SOP identifier/revision |
| Instrument/technique | Required where applicable |
| Required equipment/reagents | Structured references |
| Inputs and units | Every observation/input, type, unit, allowed range if method-defined |
| Formula and corrections | Full implementation logic and provenance |
| Result/reporting basis | As received, dry basis, filtered/unfiltered, etc. as relevant |
| Rounding and significant figures | As specified by method/approved lab rule |
| QC requirements | Blank, duplicate, spike, CRM, control material, etc. where applicable |
| Verification evidence | Golden test cases and comparison results |
| Technical reviewer | Human technical review evidence and date |
| Package status | Draft / technically reviewed / active / retired (method package metadata, not document-control workflow) |

## 7. Proposed implementation priority

### First technical vertical slice

Implement one complete path end to end rather than many empty templates. Recommended first vertical slice:

1. Coal sample details.
2. Proximate analysis observations and calculations.
3. GCV bomb-calorimeter calibration and sample worksheet.
4. Calculation provenance and QC fields.
5. Completed record snapshot and amended-copy behaviour.
6. PDF, DOCX and structured Excel export.

Reason: the coal block catalogue already contains a coherent set of sample, observation, calculation and result blocks, and the user's earlier coal GCV worksheet requirements make this a useful technical validation case. This is a recommendation for a development vertical slice, not a claim that coal must be the only first-release discipline.

### Parallel foundation

In parallel, establish the generic chemical and environmental templates and field schemas. Add the first environmental method packages after exact water/wastewater method editions and lab SOPs are identified.

## 8. Technical validation and copyright safeguards

- Use the lab's authorized controlled copies of standards and SOPs.
- Do not reproduce full proprietary standard text inside DocBuilder starter packs unless permitted.
- Store method identifiers, references, edition, field schemas, formula implementation notes and test evidence.
- Distinguish transcription/implementation from technical verification.
- Do not claim NABL endorsement, accreditation or automatic compliance.
- Do not automatically assume that a standard's current published edition is the method actually used by a given laboratory; the laboratory must confirm its adopted edition and any permitted deviations.

## 9. Open questions before method packages are frozen

1. Which water matrices are in the first laboratory scope: drinking water, ground water, surface water, wastewater/effluent, or all?
2. Which exact parameters/analytes are performed and reported by the laboratory?
3. Which sampling, preparation and analytical standards does the laboratory actually use (BIS, APHA, ASTM, ISO, CPCB, or a specified combination)?
4. Which instrument techniques and models must the initial packages accommodate (UV-Vis, AAS, ICP-OES, ICP-MS, GC, HPLC, etc.)?
5. For coal/coke, which methods and reporting bases are required for proximate, GCV, sulphur, ultimate analysis, HGI and ash fusion?
6. Which ambient-air and stack pollutants/monitoring methods are included in the laboratory's scope?
7. Which test methods are the user's highest-volume or highest-priority methods?

## 10. References checked on 2026-11-13

- [BIS standard details — IS 3025 Part 11, pH](https://standards.bis.gov.in/website/standard-details) — BIS page reports Part 11:2022 and review in 2026.
- [BIS standard details — IS 3025 Part 15:2023 and Amendment 1 (2026)](https://standards.bis.gov.in/website/standard-details) — solids method status.
- [BIS standard details — IS 3025 Part 23, alkalinity](https://standards.bis.gov.in/website/standard-details).
- [BIS standard details — IS 3025 Part 38:2025, dissolved oxygen](https://standards.bis.gov.in/website/standard-details).
- [BIS standard details — IS 3025 Part 38/Sec 2:2023, optical DO sensor](https://standards.bis.gov.in/website/standard-details).
- [BIS: IS 1350 (Part 1):2025](https://www.services.bis.gov.in/tmp/PCD5024223_24032025_1.pdf) — proximate analysis, Third Revision.
- [BIS: IS 1350 (Part 4/Sec 1):2023](https://www.services.bis.gov.in/tmp/PCD5021798_26122023_1.pdf) — carbon and hydrogen.
- [BIS standard details — IS 1350 (Part 2):2022](https://standards.bis.gov.in/website/standard-details) — gross calorific value.
- [ASTM D5865/D5865M-26](https://store.astm.org/d5865_d5865m-26.html) — gross calorific value.
- [ASTM D3173/D3173M-25](https://store.astm.org/standards/d3173) — moisture.
- [ASTM D3174](https://store.astm.org/standards/d3174) — ash.
- [ASTM D3175-26](https://store.astm.org/d3175-26.html) — volatile matter.
- [ASTM D4239-26](https://store.astm.org/standards/d4239) — sulphur.
- [ASTM D409/D409M-24](https://store.astm.org/standards/d409) — HGI.
- [ASTM D1857/D1857M-24](https://store.astm.org/d1857_d1857m-24.html) — ash fusibility.
- [APHA/AWWA/WEF Standard Methods, 24th Edition](https://www.wef.org/publications/standard-methods/) and [APHA publication listing](https://secure.apha.org/testimis/ItemDetail?Category=BK&WebsiteKey=b04418ea-e87d-4d4c-92e8-72c97f1ae2c4&iProductCode=978-087553-2998).
- [NABL Documents](https://nabl-india.org/nabl-documents/) — current NABL documents, guides and checklists.

---

**Approval status:** Candidate method catalogue only. No listed method is automatically enabled as verified or approved for a particular laboratory.
