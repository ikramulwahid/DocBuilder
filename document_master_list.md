# DocBuilder — Document Master List and Template Catalogue

**Document status:** Revised draft for owner review  
**Revision:** 2.0  
**Prepared:** 13 November 2026  
**Source:** Original `document_master_list.md` in `ikramulwahid/DocBuilder` (`main`)  
**Related authority:** `project_details.md`, `project_decisions.md`, `project_roadmap.md`

---

## 1. Purpose and governing rules

This file is the canonical catalogue of documents users may create with DocBuilder. It preserves all 324 original document IDs and titles across the original 20 categories, while adding clearer template-priority and overlap guidance.

Important boundaries:
- This is a proposed laboratory document catalogue, not a claim that every entry is required by ISO/IEC 17025 or NABL in every laboratory.
- A catalogue entry does not mean a finished template already exists.
- DocBuilder creates documents, templates, completed records and registers. It does not enforce document control, approvals, controlled distribution, obsolete-document handling or QMS workflow.
- The presence of a document titled “Document Control Procedure”, “Approval Register”, “NABL Assessment Checklist” or similar does not add the corresponding management workflow to the application.
- The displayed document IDs below are separate from internal DocBuilder IDs and from block IDs. The application must use distinct internal UIDs for documents, templates, block definitions and block instances.
- Preserve all original IDs. Resolve duplicates through shared template definitions/aliases or context-specific variants; never silently delete or renumber an entry.
- Discipline/method configurations and technical formulas must be specified and reviewed independently; document titles alone are not sufficient technical requirements.

## 2. Catalogue fields

| Field | Meaning |
|---|---|
| Document ID | Original displayed catalogue identifier; preserved unchanged |
| Document title | Original document name; spelling may be normalized only with a recorded alias |
| Type | Original document type |
| Category | One of the original 20 catalogue categories |
| Starter-template priority | Proposed priority for building a complete starter template; not a regulatory requirement |
| Creation/record mode | To be assigned per entry: authorable template/document, structured record/register, externally issued/received item to attach/reference, or mixed use. Do not infer the mode from the title alone. |

## 3. Template-priority model

| Priority | Meaning |
|---|---|
| P1 candidate | Proposed starter-template candidate for the initial usable product; owner may revise the subset |
| Catalogue | Included in the 324-entry catalogue but not yet selected for a fully developed starter template |
| Method-dependent | Template/worksheet structure may be prioritized, but executable formulas and acceptance rules require a separately approved method configuration |

**Important:** The P1 candidate list below is a recommendation to make prioritization concrete. It is not yet a final owner-approved template backlog. Technical templates for chemical, environmental and coal/coke testing must not be called complete until the method list and technical content are verified.

## 4. Master document catalogue

### 1. Quality Management System (QMS)

| Document ID | Document title | Type | Starter-template priority | Notes |
|---|---|---|---|---|
| QMS-001 | Quality Manual | Manual | P1 candidate |  |
| QMS-002 | Quality Policy | Policy | P1 candidate |  |
| QMS-003 | Quality Objectives | Record | Catalogue |  |
| QMS-004 | Laboratory Organization Chart | Document | Catalogue |  |
| QMS-005 | Scope of Accreditation | Document | Catalogue |  |
| QMS-006 | Laboratory Profile | Document | Catalogue |  |
| QMS-007 | Management System Scope | Document | Catalogue |  |
| QMS-008 | Impartiality Policy | Policy | P1 candidate |  |
| QMS-009 | Confidentiality Policy | Policy | P1 candidate |  |
| QMS-010 | Document Control Procedure | SOP | P1 candidate |  |
| QMS-011 | Record Control Procedure | SOP | P1 candidate |  |
| QMS-012 | Risk and Opportunity Management Procedure | SOP | P1 candidate |  |
| QMS-013 | Nonconforming Work Procedure | SOP | P1 candidate |  |
| QMS-014 | Corrective Action Procedure | SOP | P1 candidate |  |
| QMS-015 | Internal Audit Procedure | SOP | P1 candidate | See duplicate-title resolution in Section 5 |
| QMS-016 | Management Review Procedure | SOP | P1 candidate | See duplicate-title resolution in Section 5 |
| QMS-017 | Continual Improvement Procedure | SOP | Catalogue |  |

### 2. Document Control and Registers

| Document ID | Document title | Type | Starter-template priority | Notes |
|---|---|---|---|---|
| DOC-001 | Master Document List | Register | P1 candidate |  |
| DOC-002 | Master Record List | Register | P1 candidate |  |
| DOC-003 | Document Revision History | Register | Catalogue |  |
| DOC-004 | Document Distribution Register | Register | Catalogue |  |
| DOC-005 | External Document Register | Register | Catalogue |  |
| DOC-006 | Obsolete Document Register | Register | Catalogue |  |
| DOC-007 | Document Approval Register | Register | Catalogue |  |
| DOC-008 | Document Change Request | Form | Catalogue |  |
| DOC-009 | Document Review Schedule | Register | Catalogue |  |
| DOC-010 | Record Retention Schedule | Register | Catalogue |  |

### 3. Personnel and Competency

| Document ID | Document title | Type | Starter-template priority | Notes |
|---|---|---|---|---|
| HR-001 | Personnel Qualification Records | Record | Catalogue |  |
| HR-002 | Employee CV / Resume | Record | Catalogue |  |
| HR-003 | Educational Qualification Records | Record | Catalogue |  |
| HR-004 | Experience Certificates | Record | Catalogue |  |
| HR-005 | Job Description | Document | P1 candidate |  |
| HR-006 | Personnel Authorization Matrix | Register | P1 candidate |  |
| HR-007 | Personnel Competency Matrix | Register | P1 candidate |  |
| HR-008 | Training Needs Identification | Form | Catalogue |  |
| HR-009 | Annual Training Plan | Plan | Catalogue |  |
| HR-010 | Training Attendance Record | Record | Catalogue |  |
| HR-011 | Training Evaluation Record | Form | Catalogue |  |
| HR-012 | Competency Assessment Procedure | SOP | Catalogue |  |
| HR-013 | Practical Competency Assessment | Form | Catalogue |  |
| HR-014 | Analyst Authorization Record | Record | Catalogue |  |
| HR-015 | Technical Manager Authorization | Record | Catalogue |  |
| HR-016 | Signatory Authorization Record | Record | Catalogue |  |
| HR-017 | Personnel Performance Evaluation | Record | Catalogue |  |
| HR-018 | Confidentiality Undertaking | Form | Catalogue |  |
| HR-019 | Impartiality Declaration | Form | Catalogue |  |
| HR-020 | Training Effectiveness Evaluation | Record | Catalogue |  |

### 4. Equipment and Calibration

| Document ID | Document title | Type | Starter-template priority | Notes |
|---|---|---|---|---|
| EQ-001 | Equipment Management Procedure | SOP | Catalogue |  |
| EQ-002 | Master Equipment List | Register | P1 candidate |  |
| EQ-003 | Equipment History Card | Record | P1 candidate |  |
| EQ-004 | Equipment Installation Record | Record | Catalogue |  |
| EQ-005 | Equipment Qualification Record | Record | Catalogue |  |
| EQ-006 | Equipment Calibration Procedure | SOP | Catalogue |  |
| EQ-007 | Equipment Calibration Schedule | Register | P1 candidate |  |
| EQ-008 | Equipment Calibration Certificate | Record | Catalogue |  |
| EQ-009 | Intermediate Verification Record | Record | Catalogue |  |
| EQ-010 | Daily Equipment Performance Log | Log | Catalogue |  |
| EQ-011 | Equipment Maintenance Schedule | Register | Catalogue |  |
| EQ-012 | Equipment Maintenance Record | Record | Catalogue |  |
| EQ-013 | Equipment Breakdown Report | Form | Catalogue |  |
| EQ-014 | Equipment Out-of-Service Tag | Form | Catalogue |  |
| EQ-015 | Equipment Repair Record | Record | Catalogue |  |
| EQ-016 | Equipment Revalidation Record | Record | Catalogue |  |
| EQ-017 | Reference Standard Register | Register | Catalogue |  |
| EQ-018 | CRM Register | Register | Catalogue |  |
| EQ-019 | Equipment Environmental Condition Log | Log | Catalogue |  |
| EQ-020 | Equipment Usage Log | Log | Catalogue |  |

### 5. Testing Methods and SOPs

| Document ID | Document title | Type | Starter-template priority | Notes |
|---|---|---|---|---|
| SOP-001 | Standard Operating Procedure | SOP | P1 candidate |  |
| SOP-002 | Test Method Verification Procedure | SOP | P1 candidate | Method-specific configuration and technical review required |
| SOP-003 | Test Method Validation Procedure | SOP | P1 candidate | Method-specific configuration and technical review required |
| SOP-004 | Method Selection and Review Procedure | SOP | Catalogue |  |
| SOP-005 | Standard Test Method Register | Register | Catalogue |  |
| SOP-006 | Test Method Verification Report | Report | Catalogue |  |
| SOP-007 | Test Method Validation Report | Report | Catalogue |  |
| SOP-008 | Measurement Uncertainty Estimation Procedure | SOP | P1 candidate | Method-specific configuration and technical review required |
| SOP-009 | Measurement Uncertainty Budget | Worksheet | Catalogue | See duplicate-title resolution in Section 5 |
| SOP-010 | LOD Study | Report | Catalogue |  |
| SOP-011 | LOQ Study | Report | Catalogue |  |
| SOP-012 | Linearity Study | Report | Catalogue |  |
| SOP-013 | Precision Study | Report | Catalogue |  |
| SOP-014 | Accuracy / Recovery Study | Report | Catalogue |  |
| SOP-015 | Robustness Study | Report | Catalogue |  |
| SOP-016 | Selectivity Study | Report | Catalogue |  |
| SOP-017 | Test Method Revision Record | Record | Catalogue |  |
| SOP-018 | Intermediate Precision Study | Report | Catalogue |  |

### 6. Sample Management

| Document ID | Document title | Type | Starter-template priority | Notes |
|---|---|---|---|---|
| SMP-001 | Sample Handling Procedure | SOP | Catalogue |  |
| SMP-002 | Sample Receipt Register | Register | P1 candidate |  |
| SMP-003 | Sample Registration Form | Form | P1 candidate |  |
| SMP-004 | Sample Identification Record | Record | Catalogue |  |
| SMP-005 | Sample Acceptance / Rejection Criteria | Document | Catalogue |  |
| SMP-006 | Sample Rejection Record | Record | Catalogue |  |
| SMP-007 | Sample Condition Inspection Record | Record | Catalogue |  |
| SMP-008 | Sample Storage Register | Register | Catalogue |  |
| SMP-009 | Sample Preservation Record | Record | Catalogue |  |
| SMP-010 | Sample Preparation Record | Record | Catalogue |  |
| SMP-011 | Sample Subsampling Record | Record | Catalogue |  |
| SMP-012 | Sample Transfer Register | Register | Catalogue |  |
| SMP-013 | Chain of Custody Form | Form | P1 candidate |  |
| SMP-014 | Sample Retention Register | Register | Catalogue |  |
| SMP-015 | Sample Disposal Record | Record | Catalogue |  |
| SMP-016 | Sample Return Record | Record | Catalogue |  |
| SMP-017 | Sample Tracking Sheet | Form | Catalogue |  |

### 7. Quality Control and Quality Assurance

| Document ID | Document title | Type | Starter-template priority | Notes |
|---|---|---|---|---|
| QC-001 | Quality Control Procedure | SOP | P1 candidate |  |
| QC-002 | Quality Control Plan | Plan | P1 candidate |  |
| QC-003 | Laboratory Control Sample Record | Record | Catalogue |  |
| QC-004 | Blank Sample Record | Record | Catalogue |  |
| QC-005 | Method Blank Record | Record | Catalogue |  |
| QC-006 | Duplicate Testing Record | Record | Catalogue |  |
| QC-007 | Replicate Testing Record | Record | Catalogue |  |
| QC-008 | Spike Recovery Record | Record | Catalogue |  |
| QC-009 | CRM Testing Record | Record | Catalogue |  |
| QC-010 | Control Charts | Record | Catalogue |  |
| QC-011 | Quality Control Acceptance Criteria | Document | Catalogue |  |
| QC-012 | Out-of-Control Investigation Report | Report | Catalogue |  |
| QC-013 | Proficiency Testing Procedure | SOP | Catalogue |  |
| QC-014 | Proficiency Testing Participation Register | Register | Catalogue |  |
| QC-015 | Proficiency Testing Result Evaluation | Report | Catalogue |  |
| QC-016 | Inter-Laboratory Comparison Record | Record | Catalogue |  |
| QC-017 | Quality Assurance Review Record | Record | Catalogue |  |
| QC-018 | Measurement Uncertainty Review Record | Record | Catalogue |  |

### 8. Customer and Contract Review

| Document ID | Document title | Type | Starter-template priority | Notes |
|---|---|---|---|---|
| CR-001 | Customer Enquiry Form | Form | Catalogue |  |
| CR-002 | Quotation / Offer | Document | Catalogue |  |
| CR-003 | Contract Review Procedure | SOP | Catalogue |  |
| CR-004 | Contract Review Form | Form | Catalogue |  |
| CR-005 | Customer Requirement Record | Record | Catalogue |  |
| CR-006 | Test Requirement Confirmation | Record | Catalogue |  |
| CR-007 | Outsourced Testing Approval | Form | Catalogue |  |
| CR-008 | Customer Communication Record | Record | Catalogue |  |
| CR-009 | Customer Complaint Register | Register | Catalogue |  |
| CR-010 | Customer Feedback Form | Form | Catalogue |  |
| CR-011 | Customer Satisfaction Analysis | Report | Catalogue | See duplicate-title resolution in Section 5 |
| CR-012 | Complaint Investigation Report | Report | Catalogue | See duplicate-title resolution in Section 5 |

### 9. Facilities and Environmental Conditions

| Document ID | Document title | Type | Starter-template priority | Notes |
|---|---|---|---|---|
| FAC-001 | Laboratory Facility Management Procedure | SOP | Catalogue |  |
| FAC-002 | Laboratory Layout Plan | Plan | Catalogue |  |
| FAC-003 | Laboratory Area Identification | Document | Catalogue |  |
| FAC-004 | Environmental Condition Monitoring Procedure | SOP | Catalogue |  |
| FAC-005 | Temperature Monitoring Log | Log | Catalogue |  |
| FAC-006 | Humidity Monitoring Log | Log | Catalogue |  |
| FAC-007 | Laboratory Housekeeping Checklist | Checklist | Catalogue |  |
| FAC-008 | Laboratory Cleaning Register | Register | Catalogue |  |
| FAC-009 | Pest Control Record | Record | Catalogue |  |
| FAC-010 | Access Control Register | Register | Catalogue |  |
| FAC-011 | Environmental Condition Excursion Report | Report | Catalogue |  |
| FAC-012 | Laboratory Safety Inspection Checklist | Checklist | Catalogue |  |
| FAC-013 | Ventilation / Fume Hood Performance Record | Record | Catalogue |  |
| FAC-014 | Restricted Area Access Register | Register | Catalogue |  |
| FAC-015 | Laboratory Infrastructure Maintenance Record | Record | Catalogue |  |

### 10. Chemical and Reagent Management

| Document ID | Document title | Type | Starter-template priority | Notes |
|---|---|---|---|---|
| CHEM-001 | Chemical Management Procedure | SOP | Catalogue |  |
| CHEM-002 | Chemical Inventory Register | Register | Catalogue |  |
| CHEM-003 | Reagent Preparation Register | Register | Catalogue |  |
| CHEM-004 | Reagent Standardization Record | Record | Catalogue |  |
| CHEM-005 | Standard Solution Preparation Record | Record | Catalogue |  |
| CHEM-006 | Reagent Label Format | Format | Catalogue |  |
| CHEM-007 | Reagent Expiry Monitoring Register | Register | Catalogue |  |
| CHEM-008 | Chemical Receipt Register | Register | Catalogue |  |
| CHEM-009 | Chemical Consumption Register | Register | Catalogue |  |
| CHEM-010 | Stock Solution Register | Register | Catalogue |  |
| CHEM-011 | Working Standard Register | Register | Catalogue |  |
| CHEM-012 | Buffer Solution Preparation Record | Record | Catalogue |  |
| CHEM-013 | Volumetric Solution Standardization Record | Record | Catalogue |  |
| CHEM-014 | Reagent Blank Record | Record | Catalogue |  |
| CHEM-015 | Chemical Disposal Record | Record | Catalogue |  |
| CHEM-016 | SDS / MSDS Register | Register | Catalogue |  |
| CHEM-017 | Chemical Compatibility and Storage Record | Record | Catalogue |  |

### 11. Procurement and External Services

| Document ID | Document title | Type | Starter-template priority | Notes |
|---|---|---|---|---|
| PUR-001 | Purchasing Procedure | SOP | Catalogue |  |
| PUR-002 | Approved Supplier List | Register | Catalogue |  |
| PUR-003 | Supplier Evaluation Form | Form | Catalogue |  |
| PUR-004 | Supplier Re-evaluation Record | Record | Catalogue |  |
| PUR-005 | Purchase Requisition | Form | Catalogue |  |
| PUR-006 | Purchase Order | Document | Catalogue |  |
| PUR-007 | Material Receipt Inspection | Record | Catalogue |  |
| PUR-008 | Supplier Performance Evaluation | Record | Catalogue |  |
| PUR-009 | External Service Provider Evaluation | Form | Catalogue |  |
| PUR-010 | Calibration Service Provider Evaluation | Form | Catalogue |  |
| PUR-011 | Proficiency Testing Provider Evaluation | Form | Catalogue |  |
| PUR-012 | Outsourced Testing Provider Register | Register | Catalogue |  |
| PUR-013 | Consumables Inventory Register | Register | Catalogue |  |
| PUR-014 | Material Rejection Report | Report | Catalogue |  |

### 12. Testing Records and Reporting

| Document ID | Document title | Type | Starter-template priority | Notes |
|---|---|---|---|---|
| TR-001 | Test Request Form | Form | Catalogue |  |
| TR-002 | Test Worksheet | Worksheet | P1 candidate |  |
| TR-003 | Raw Data Recording Sheet | Record | P1 candidate |  |
| TR-004 | Analytical Observation Sheet | Record | Catalogue |  |
| TR-005 | Instrument Reading Sheet | Record | Catalogue |  |
| TR-006 | Calculation Sheet | Worksheet | P1 candidate |  |
| TR-007 | Test Result Review Checklist | Checklist | Catalogue |  |
| TR-008 | Technical Record Review Form | Form | Catalogue |  |
| TR-009 | Laboratory Test Report | Report | P1 candidate |  |
| TR-010 | Amended Test Report | Report | Catalogue |  |
| TR-011 | Test Report Approval Record | Record | Catalogue |  |
| TR-012 | Test Report Dispatch Register | Register | Catalogue |  |
| TR-013 | Test Report Correction Register | Register | Catalogue |  |
| TR-014 | Retest / Repeat Test Authorization | Form | Catalogue |  |
| TR-015 | Test Report Withdrawal Record | Record | Catalogue |  |
| TR-016 | Technical Data Backup Register | Register | Catalogue |  |
| TR-017 | Electronic Data Integrity Record | Record | Catalogue |  |

### 13. Internal Audit, CAPA and Management Review

| Document ID | Document title | Type | Starter-template priority | Notes |
|---|---|---|---|---|
| AUD-001 | Internal Audit Procedure | SOP | Catalogue | See duplicate-title resolution in Section 5 |
| AUD-002 | Annual Internal Audit Programme | Plan | Catalogue |  |
| AUD-003 | Internal Audit Schedule | Schedule | Catalogue |  |
| AUD-004 | Internal Auditor Qualification Record | Record | Catalogue |  |
| AUD-005 | Internal Audit Checklist | Checklist | Catalogue |  |
| AUD-006 | Internal Audit Report | Report | Catalogue |  |
| AUD-007 | Audit Nonconformity Report | Report | Catalogue |  |
| AUD-008 | Corrective Action Request | Form | Catalogue |  |
| AUD-009 | Root Cause Analysis Report | Report | Catalogue |  |
| AUD-010 | Corrective Action Implementation Record | Record | Catalogue |  |
| AUD-011 | Corrective Action Effectiveness Verification | Record | Catalogue |  |
| AUD-012 | Management Review Procedure | SOP | Catalogue | See duplicate-title resolution in Section 5 |
| AUD-013 | Management Review Meeting Agenda | Document | Catalogue |  |
| AUD-014 | Management Review Minutes | Record | Catalogue |  |
| AUD-015 | Management Review Action Tracker | Register | Catalogue |  |
| AUD-016 | Risk and Opportunity Register | Register | Catalogue |  |
| AUD-017 | Risk Assessment Record | Record | Catalogue |  |
| AUD-018 | Continual Improvement Register | Register | Catalogue |  |

### 14. Complaints and Nonconforming Work

| Document ID | Document title | Type | Starter-template priority | Notes |
|---|---|---|---|---|
| CMP-001 | Complaint Handling Procedure | SOP | Catalogue |  |
| CMP-002 | Customer Complaint Form | Form | Catalogue |  |
| CMP-003 | Complaint Register | Register | Catalogue |  |
| CMP-004 | Complaint Investigation Report | Report | Catalogue | See duplicate-title resolution in Section 5 |
| CMP-005 | Complaint Resolution Record | Record | Catalogue |  |
| CMP-006 | Nonconforming Work Report | Report | Catalogue |  |
| CMP-007 | Nonconforming Work Register | Register | Catalogue |  |
| CMP-008 | Impact Assessment of Nonconforming Work | Record | Catalogue |  |
| CMP-009 | Work Suspension / Resumption Authorization | Form | Catalogue |  |
| CMP-010 | Customer Notification Record | Record | Catalogue |  |
| CMP-011 | Customer Feedback Register | Register | Catalogue |  |
| CMP-012 | Customer Satisfaction Analysis | Report | Catalogue | See duplicate-title resolution in Section 5 |

### 15. Measurement Uncertainty and Traceability

| Document ID | Document title | Type | Starter-template priority | Notes |
|---|---|---|---|---|
| MU-001 | Measurement Uncertainty Procedure | SOP | Catalogue |  |
| MU-002 | Measurement Uncertainty Budget | Worksheet | Catalogue | See duplicate-title resolution in Section 5 |
| MU-003 | Uncertainty Calculation Worksheet | Worksheet | Catalogue |  |
| MU-004 | Uncertainty Component Identification | Record | Catalogue |  |
| MU-005 | Repeatability Study | Report | Catalogue |  |
| MU-006 | Reproducibility Study | Report | Catalogue |  |
| MU-007 | Bias Estimation | Report | Catalogue |  |
| MU-008 | Calibration Uncertainty Record | Record | Catalogue |  |
| MU-009 | Reference Material Traceability Record | Record | Catalogue |  |
| MU-010 | Measurement Traceability Matrix | Register | Catalogue |  |
| MU-011 | Uncertainty Review and Approval Record | Record | Catalogue |  |
| MU-012 | Uncertainty Revision History | Register | Catalogue |  |

### 16. Health, Safety and Waste Management

| Document ID | Document title | Type | Starter-template priority | Notes |
|---|---|---|---|---|
| SAF-001 | Laboratory Safety Policy | Policy | Catalogue |  |
| SAF-002 | Laboratory Safety Procedure | SOP | Catalogue |  |
| SAF-003 | Chemical Handling Procedure | SOP | Catalogue |  |
| SAF-004 | PPE Issue Register | Register | Catalogue |  |
| SAF-005 | Laboratory Accident Register | Register | Catalogue |  |
| SAF-006 | Incident / Near-Miss Report | Report | Catalogue |  |
| SAF-007 | Emergency Response Plan | Plan | Catalogue |  |
| SAF-008 | Fire Safety Inspection Record | Record | Catalogue |  |
| SAF-009 | First Aid Register | Register | Catalogue |  |
| SAF-010 | Chemical Spill Response Record | Record | Catalogue |  |
| SAF-011 | Laboratory Waste Management Procedure | SOP | Catalogue |  |
| SAF-012 | Hazardous Waste Disposal Register | Register | Catalogue |  |
| SAF-013 | Waste Segregation Record | Record | Catalogue |  |
| SAF-014 | Emergency Equipment Inspection Record | Record | Catalogue |  |

### 17. Chemical Testing Laboratory

| Document ID | Document title | Type | Starter-template priority | Notes |
|---|---|---|---|---|
| CH-001 | Chemical Analysis SOP | SOP | P1 candidate | Method-specific configuration and technical review required |
| CH-002 | Titration Analysis Worksheet | Worksheet | P1 candidate | Method-specific configuration and technical review required |
| CH-003 | Volumetric Analysis Record | Record | Catalogue | Method-specific configuration and technical review required |
| CH-004 | Gravimetric Analysis Worksheet | Worksheet | Catalogue | Method-specific configuration and technical review required |
| CH-005 | Spectrophotometric Analysis Worksheet | Worksheet | P1 candidate | Method-specific configuration and technical review required |
| CH-006 | pH Analysis Record | Record | Catalogue | Method-specific configuration and technical review required |
| CH-007 | Conductivity Analysis Record | Record | Catalogue | Method-specific configuration and technical review required |
| CH-008 | AAS Analysis Worksheet | Worksheet | Catalogue | Method-specific configuration and technical review required |
| CH-009 | ICP-OES Analysis Worksheet | Worksheet | Catalogue | Method-specific configuration and technical review required |
| CH-010 | ICP-MS Analysis Worksheet | Worksheet | Catalogue | Method-specific configuration and technical review required |
| CH-011 | GC Analysis Worksheet | Worksheet | Catalogue | Method-specific configuration and technical review required |
| CH-012 | HPLC Analysis Worksheet | Worksheet | Catalogue | Method-specific configuration and technical review required |
| CH-013 | Standard Solution Preparation Sheet | Worksheet | P1 candidate | Method-specific configuration and technical review required |
| CH-014 | Chemical Recovery Study | Report | Catalogue | Method-specific configuration and technical review required |
| CH-015 | Analytical Calibration Curve Record | Record | Catalogue | Method-specific configuration and technical review required |

### 18. Environmental Testing Laboratory

| Document ID | Document title | Type | Starter-template priority | Notes |
|---|---|---|---|---|
| ENV-001 | Water Sampling Procedure | SOP | P1 candidate | Method-specific configuration and technical review required |
| ENV-002 | Wastewater Sampling Procedure | SOP | Catalogue | Method-specific configuration and technical review required |
| ENV-003 | Groundwater Sampling Procedure | SOP | Catalogue | Method-specific configuration and technical review required |
| ENV-004 | Surface Water Sampling Procedure | SOP | Catalogue | Method-specific configuration and technical review required |
| ENV-005 | Soil Sampling Procedure | SOP | Catalogue | Method-specific configuration and technical review required |
| ENV-006 | Ambient Air Sampling Procedure | SOP | Catalogue | Method-specific configuration and technical review required |
| ENV-007 | Stack Emission Sampling Procedure | SOP | Catalogue | Method-specific configuration and technical review required |
| ENV-008 | Sampling Plan | Plan | Catalogue | Method-specific configuration and technical review required |
| ENV-009 | Field Sampling Record | Record | Catalogue | Method-specific configuration and technical review required |
| ENV-010 | Sample Preservation Record | Record | Catalogue | Method-specific configuration and technical review required |
| ENV-011 | Chain of Custody Form | Form | Catalogue | Method-specific configuration and technical review required |
| ENV-012 | Water Analysis Worksheet | Worksheet | P1 candidate | Method-specific configuration and technical review required |
| ENV-013 | BOD Analysis Worksheet | Worksheet | P1 candidate | Method-specific configuration and technical review required |
| ENV-014 | COD Analysis Worksheet | Worksheet | P1 candidate | Method-specific configuration and technical review required |
| ENV-015 | TSS Analysis Worksheet | Worksheet | P1 candidate | Method-specific configuration and technical review required |
| ENV-016 | TDS Analysis Worksheet | Worksheet | P1 candidate | Method-specific configuration and technical review required |
| ENV-017 | Total Hardness Analysis Worksheet | Worksheet | Catalogue | Method-specific configuration and technical review required |
| ENV-018 | Chloride Analysis Worksheet | Worksheet | Catalogue | Method-specific configuration and technical review required |
| ENV-019 | Sulphate Analysis Worksheet | Worksheet | Catalogue | Method-specific configuration and technical review required |
| ENV-020 | Fluoride Analysis Worksheet | Worksheet | Catalogue | Method-specific configuration and technical review required |
| ENV-021 | Heavy Metal Analysis Worksheet | Worksheet | Catalogue | Method-specific configuration and technical review required |
| ENV-022 | Soil Analysis Worksheet | Worksheet | Catalogue | Method-specific configuration and technical review required |
| ENV-023 | Ambient Air Monitoring Report | Report | Catalogue | Method-specific configuration and technical review required |
| ENV-024 | Stack Emission Monitoring Report | Report | Catalogue | Method-specific configuration and technical review required |
| ENV-025 | Noise Monitoring Record | Record | Catalogue | Method-specific configuration and technical review required |

### 19. Coal and Coke Testing Laboratory

| Document ID | Document title | Type | Starter-template priority | Notes |
|---|---|---|---|---|
| COAL-001 | Coal Sample Collection Procedure | SOP | Catalogue | Method-specific configuration and technical review required |
| COAL-002 | Coal Sample Preparation Procedure | SOP | Catalogue | Method-specific configuration and technical review required |
| COAL-003 | Coal Sample Crushing Record | Record | Catalogue | Method-specific configuration and technical review required |
| COAL-004 | Coal Sample Pulverization Record | Record | Catalogue | Method-specific configuration and technical review required |
| COAL-005 | Proximate Analysis SOP | SOP | P1 candidate | Method-specific configuration and technical review required |
| COAL-006 | Moisture Determination Worksheet | Worksheet | P1 candidate | Method-specific configuration and technical review required |
| COAL-007 | Ash Content Determination Worksheet | Worksheet | P1 candidate | Method-specific configuration and technical review required |
| COAL-008 | Volatile Matter Determination Worksheet | Worksheet | P1 candidate | Method-specific configuration and technical review required |
| COAL-009 | Fixed Carbon Calculation Sheet | Worksheet | Catalogue | Method-specific configuration and technical review required |
| COAL-010 | Gross Calorific Value Worksheet | Worksheet | P1 candidate | Method-specific configuration and technical review required |
| COAL-011 | Net Calorific Value Calculation | Worksheet | Catalogue | Method-specific configuration and technical review required |
| COAL-012 | Total Sulphur Determination Worksheet | Worksheet | Catalogue | Method-specific configuration and technical review required |
| COAL-013 | Ultimate Analysis Worksheet | Worksheet | Catalogue | Method-specific configuration and technical review required |
| COAL-014 | HGI Determination Worksheet | Worksheet | Catalogue | Method-specific configuration and technical review required |
| COAL-015 | Ash Fusion Temperature Record | Record | Catalogue | Method-specific configuration and technical review required |
| COAL-016 | Coal Analysis Test Report | Report | Catalogue | Method-specific configuration and technical review required |
| COAL-017 | Coal Reference Material QC Record | Record | Catalogue | Method-specific configuration and technical review required |

### 20. NABL Accreditation Documents

| Document ID | Document title | Type | Starter-template priority | Notes |
|---|---|---|---|---|
| NABL-001 | NABL Application Form | Form | Catalogue |  |
| NABL-002 | Proposed Scope of Accreditation | Document | Catalogue |  |
| NABL-003 | Final Scope of Accreditation | Document | Catalogue |  |
| NABL-004 | NABL Assessment Checklist | Checklist | Catalogue |  |
| NABL-005 | ISO 17025 Clause-wise Compliance Matrix | Matrix | Catalogue |  |
| NABL-006 | NABL Pre-assessment Report | Report | Catalogue |  |
| NABL-007 | NABL Assessment Report | Report | Catalogue |  |
| NABL-008 | Assessment Nonconformity Register | Register | Catalogue |  |
| NABL-009 | NABL Corrective Action Report | Report | Catalogue |  |
| NABL-010 | Corrective Action Evidence | Record | Catalogue |  |
| NABL-011 | NABL Surveillance Assessment Records | Record | Catalogue |  |
| NABL-012 | NABL Accreditation Certificate | Certificate | Catalogue |  |
| NABL-013 | Accreditation Scope Amendment Records | Record | Catalogue |  |
| NABL-014 | NABL Symbol Usage Procedure | SOP | Catalogue |  |
| NABL-015 | NABL Surveillance Readiness Checklist | Checklist | Catalogue |  |
| NABL-016 | NABL Renewal Records | Record | Catalogue |  |

## 5. Duplicate-title and shared-template resolution

The source catalogue contains five exact duplicate-title pairs. Both original IDs remain in the catalogue. The proposed treatment is to use one reusable base template where content is genuinely the same, while preserving separate catalogue references and allowing context-specific variants when required.

| Proposed base/canonical entry | Second entry | Duplicate title | Recommended treatment |
|---|---|---|---|
| QMS-015 | AUD-001 | Internal Audit Procedure | One shared base template; retain both catalogue IDs as aliases/context entries unless their required scope differs. |
| QMS-016 | AUD-012 | Management Review Procedure | One shared base template; retain both catalogue IDs as aliases/context entries unless their required scope differs. |
| MU-002 | SOP-009 | Measurement Uncertainty Budget | Use MU-002 as the primary uncertainty-budget catalogue entry; retain SOP-009 as a cross-reference/legacy alias pending owner confirmation. |
| CR-011 | CMP-012 | Customer Satisfaction Analysis | One shared base report structure; allow context-specific content/variants if one is contract-review driven and the other complaint-driven. |
| CMP-004 | CR-012 | Complaint Investigation Report | Use CMP-004 as the primary complaint-investigation entry; retain CR-012 as a cross-reference/legacy alias pending owner confirmation. |

No duplicate IDs are removed. The proposed canonical choice is a starting point for template planning, not a forced merge of existing laboratory documents.

## 6. Proposed first-release starter-template candidates

The following subset is proposed to provide broad coverage across the four confirmed first-release areas. It should be reviewed against the laboratory’s actual needs and method list before implementation tasks are frozen.

| Area | Candidate IDs | Notes |
|---|---|---|
| QMS/general | QMS-001, QMS-002, QMS-008, QMS-009, QMS-010, QMS-011, QMS-012, QMS-013, QMS-014, QMS-015, QMS-016 | Core manuals, policies and general procedures; these are authoring templates, not app-enforced controls. |
| Document/people/equipment foundations | DOC-001, DOC-002, HR-005, HR-006, HR-007, EQ-002, EQ-003, EQ-007 | Useful generic registers and records. |
| General testing records | SOP-001, SOP-002, SOP-003, SOP-008, SMP-002, SMP-003, SMP-013, QC-001, QC-002, TR-002, TR-003, TR-006, TR-009 | Generic SOP, verification/validation, sample receipt, QC and reporting structures. |
| Chemical | CH-001, CH-002, CH-005, CH-013 | Technical formulas/fields require selected methods and expert review. |
| Environmental | ENV-001, ENV-012, ENV-013, ENV-014, ENV-015, ENV-016 | Start with water sampling/analysis and BOD/COD/TSS/TDS examples where supported methods are supplied. |
| Coal and coke | COAL-005, COAL-006, COAL-007, COAL-008, COAL-010 | Proximate analysis and GCV worksheet candidates; formulas/corrections/basis must be method-verified. |

P1 candidate status is not a statement of completed implementation. Templates may be developed in stages and marked ready only after their own acceptance criteria pass.

## 7. Mapping documents to blocks and technical methods

Each developed template should have a companion mapping record containing:
1. Document ID and template UID.
2. Document type and intended discipline/matrix.
3. Required block IDs and block definition versions.
4. Input fields, tables, register references and file attachments.
5. Relevant ISO/NABL references, where verified and applicable.
6. Applicable method, standard identifier and edition for technical documents.
7. Formula IDs/versions, units, correction terms, reporting basis and rounding rules.
8. Validation rules and sample/acceptance test cases.
9. PDF/DOCX/print rendering requirements.
10. Technical reviewer/source evidence for method-specific content.

Do not infer a clause mapping, standard edition, limit, formula or required record solely from a document title. Where no verified mapping is available, record “not yet mapped”.

## 8. Use of document-control, approval and compliance documents

The catalogue retains documents whose subject matter concerns document control, records, approval, audits, nonconformities, CAPA and NABL accreditation. Users may create these documents as ordinary content. DocBuilder will not:
- route documents for review or approval;
- enforce document statuses or lock documents when a status is “Approved”;
- distribute controlled copies or record acknowledgements;
- manage obsolete-document enforcement;
- generate an audit trail of user actions;
- schedule audits, CAPA tasks, calibration alerts or sample workflows;
- maintain a live NABL compliance-management dashboard.

If a user creates an approval register, distribution register, audit report, CAPA form or compliance matrix, it is an editable document/register created with the standard block and record tools.

#### 8.1 Authoring versus externally issued documents

A catalogue entry does not automatically mean DocBuilder should generate the item itself. During template planning, classify each entry by its intended creation/record mode:

- **Authorable document/template:** a laboratory-authored document that DocBuilder can help draft and render.
- **Structured record/register:** a laboratory-maintained collection of records, observations or references.
- **Externally issued/received item:** a certificate, accreditation decision, standard, supplier record or other item issued by an external authority/provider/person. DocBuilder should support recording its metadata and attaching or referencing the original; it must not fabricate or impersonate the issuing body.
- **Mixed use:** the laboratory creates a local record/form that also stores an external certificate, report or evidence attachment.

Examples requiring this distinction include calibration certificates, CRM certificates, personnel education/experience certificates, supplier documents, external standards and the NABL accreditation certificate/final scope. DocBuilder may create a register entry, cover sheet, reference record or attachment wrapper for these items; it must not represent itself as issuing an official certificate or accreditation decision. The template/mode for each item remains to be assigned during detailed template planning.

## 9. Record and register interpretation

- A **register** is a collection of individual local records with grid/table and individual detail views.
- A **record** can be populated from a template and preserved as a completed snapshot.
- An **amendment** creates a separate copy/record linked to the original completed snapshot.
- A document template may be used to render/print one register record or a register extract.
- A register does not imply sample tracking, automatic reminders, task scheduling or operational monitoring.
- A manual review/approval/signature field in a record is ordinary record content, not an application workflow.

## 10. Catalogue integrity and acceptance checks

This file is complete as a catalogue revision when:
- all 324 original document IDs remain present;
- all 20 original categories remain represented;
- the five duplicate-name pairs are explicitly mapped without deleting IDs;
- proposed template priorities are clearly identified as candidates, not completed deliverables;
- technical-method-specific requirements are kept out of generic title-based assumptions;
- block mappings are maintained in a separate traceability matrix or template specification;
- no catalogue entry silently adds document control, approval workflow, AI or operational lab-management functionality.

## 11. Source and status note

This revised catalogue is based on the original `document_master_list.md` retrieved from the repository `main` branch during the 13 November 2026 review. It preserves the original titles, IDs and types while adding priority and product-boundary interpretation. It has not been committed to GitHub.

**End of document master list.**