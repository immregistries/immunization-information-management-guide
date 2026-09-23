# Immunization Information Management: Global Functional Guide & Implementation Framework

!!! warning "Planning input, not an authoritative source"
    This document is the original project framework. Its models, mappings, and
    claims are hypotheses to validate against primary sources and implementation
    evidence. See the [project addendum](project-addendum.md) for the current
    editorial direction.

## Executive Summary & Strategic Architecture

### Purpose & Vision
This framework defines a technology-neutral, implementation-neutral functional specification for immunization updates (VXU equivalent) and queries (QBP/RSP equivalent). It bridges the operational gaps between legacy HL7 v2.5.1 messaging, modern HL7 FHIR R4/R5 RESTful APIs, and international standards such as ISO/TS 5384:2024.

The core strategy leverages decades of production-tested U.S. Immunization Information System (IIS) operational experience to construct a universal functional baseline. To ensure global applicability without imposing U.S.-specific administrative rules on other jurisdictions, national-level requirements (such as VFC eligibility, CDC WSDL protocols, and state reporting mandates) are cleanly isolated into **Jurisdictional Addenda**.

```
+-----------------------------------------------------------------------------------+
|                        Core International Functional Guide                        |
|   (Technology-Neutral Workflows: Inbound Event Update & Outbound Query/Forecast)  |
+----------------------------------------+------------------------------------------+
                                         |
     +-----------------------------------+-----------------------------------+
     |                                   |                                   |
     v                                   v                                   v
+-----------------------+     +-----------------------+     +-----------------------+
|  Addendum A: U.S.     |     |  Addendum B: E.U.     |     |  Addendum C: Custom   |
|  Jurisdictional Profile|     |  Jurisdictional Profile|     |  Jurisdictional Profile|
+-----------------------+     +-----------------------+     +-----------------------+
     |                                   |                                   |
     +-----------------------------------+-----------------------------------+
                                         |
                                         v
+-----------------------------------------------------------------------------------+
|                           Technical Realization Layer                             |
|          HL7 v2.5.1  <----->  HL7 FHIR R4/R5  <----->  ISO/TS 5384:2024          |
+-----------------------------------------------------------------------------------+
```

---

## 1. The 5-Layer Conceptual Information Architecture

To resolve the "pancake problem" found in traditional data dictionaries—where raw administration facts, product terminology, clinical evaluations, and recommendations are flattened into a single record—this functional guide enforces a strict 5-layer conceptual separation.

```
+-----------------------------------------------------------------------------------+
| Layer 5: Forecast & Recommendation                                               |
| (Schedule-driven projections, future due dates, series status)                   |
+-----------------------------------------------------------------------------------+
                                         ^
                                         | (Informs)
+-----------------------------------------------------------------------------------+
| Layer 4: Dose Evaluation                                                          |
| (Clinical validation: valid/invalid status, subpotency reasons, schedule rules)   |
+-----------------------------------------------------------------------------------+
                                         ^
                                         | (Evaluates)
+-----------------------------------------------------------------------------------+
| Layer 3: Vaccine Semantics & Interpretation                                      |
| (Terminology mappings, normalized vaccine concepts, functional valences)        |
+-----------------------------------------------------------------------------------+
                                         ^
                                         | (Interprets)
+-----------------------------------------------------------------------------------+
| Layer 2: Product Identity                                                         |
| (Manufactured product, GTIN, NDC, IDMP, package level, lot/batch, expiration)     |
+-----------------------------------------------------------------------------------+
                                         ^
                                         | (Identifies item in)
+-----------------------------------------------------------------------------------+
| Layer 1: Event Record                                                             |
| (Point-of-care administration facts, timestamps, provider, site, provenance)    |
+-----------------------------------------------------------------------------------+
```

1. **Layer 1: Source Event Record:** Captures the facts asserted at the point of care or reported by historical sources (who administered what, to whom, when, where, and source reliability).
2. **Layer 2: Product Identity:** Identifies the regulated manufactured item (GTIN, NDC, IDMP identifiers, lot/batch number, expiration date).
3. **Layer 3: Vaccine Semantics & Interpretation:** Interprets the product code into normalized vaccine concepts and functional valences via maintained terminology services (e.g., CVX, SNOMED CT, NUVA). Valences travel as derived metadata, preserving original source facts.
4. **Layer 4: Dose Evaluation:** Evaluates whether a recorded dose meets clinical schedule rules (valid vs. invalid, dose potency/subpotency reasons, target disease/series credit).
5. **Layer 5: Forecast & Recommendation:** Calculates future immunization requirements, recommended date windows, and series completion status based on jurisdictional clinical decision support (CDS) protocols.

---

## 2. Core International Functional Specification (Technology-Neutral)

### 2.1 Workflow A: Immunization Event Update & Ingestion (Inbound / VXU Equivalent)
* **Functional Scope:** Ingestion of new administration records, historical reports, and record modifications from clinical EHRs, pharmacies, or public health portals into an IIS/registry.
* **Key Functional Capabilities:**
  1. **Source Event Assertion Capture:** Preserves exact original coding, text descriptions, administration timestamps, route, site, and reporting source type (e.g., clinic-administered vs. historical patient report).
  2. **Event Business Identification & Lifecycle:** Assigns immutable business identifiers to the event; supports record lifecycle operations including `New`, `Update/Correction`, `Entered-in-Error/Retraction`, and `Record Deletion`.
  3. **Product & Lot Resolution:** Validates product identifiers against national/international registries; records batch/lot identifiers and expiration dates; logs cold-chain breaks or subpotent dose assertions.
  4. **Provenance & Audit Logging:** Captures author, recorder, administering provider, organization, and timestamp of receipt to establish historical trust and source reliability.

### 2.2 Workflow B: History Retrieval & Forecast Query (Inbound QBP / Outbound RSP Equivalent)
* **Functional Scope:** Querying an IIS or central repository to retrieve a consolidated, deduplicated lifetime immunization history alongside active clinical decision support (CDS) evaluations and recommendations.
* **Key Functional Capabilities:**
  1. **Patient Match & Identity Boundary:** Accepts demographic query parameters; executes deterministic/probabilistic identity matching; returns matched status, single match history, or multiple candidate list.
  2. **Consolidated Lifetime History Compilation:** Aggregates administration records across multiple reporting sources, resolving duplicates while maintaining source traceability.
  3. **Automated Clinical Decision Support (CDS) Triggering:** Submits compiled history to a rules engine evaluating against applicable jurisdictional schedules.
  4. **Explicit Dose Evaluation Generation:** Delivers structured evaluations for each dose in the history (validity status, reason for invalidity if applicable, target disease/valence credited).
  5. **Forecast & Recommendation Output:** Generates future due dates (Earliest, Recommended, Overdue, Latest) and series status (Complete, In Progress, Not Started) for all relevant target diseases.

### 2.3 Workflow C: System & Administrative Registry Functions
* **Functional Scope:** Operational management of client registries and data quality maintenance.
* **Key Functional Capabilities:**
  1. **Deduplication & Record Consolidation:** Identifies duplicate event submissions across different providers and merges records without destroying underlying source provenance.
  2. **Data Quality & Exception Management:** Flags missing required metadata, invalid lot numbers, or dates exceeding plausible boundaries; routes exceptions to administrative queues or error response payloads.

---

## 3. Standards Realization & Technical Crosswalks

### 3.1 Mapping Functional Workflows to Technical Exchange Standards

| Functional Concept | HL7 v2.5.1 Realization (U.S. IIS) | HL7 FHIR R4/R5 Realization | ISO/TS 5384:2024 Alignment |
| :--- | :--- | :--- | :--- |
| **Inbound Event Update** | `VXU^V04^VXU_V04` message trigger | `POST /Immunization` or `Bundle` transaction | "Populate registry" / "Record current event" use cases |
| **Query for History/Forecast** | `QBP^Q11^QBP_Q21` query message | `GET /Immunization?patient=X` or `$everything` | "Create immunization history" / "Schedule event" use cases |
| **Query Response (History & CDS)** | `RSP^K11^RSP_K11` response message | `Bundle` containing `Immunization`, `ImmunizationEvaluation`, `ImmunizationRecommendation` | "Create immunization history" & "Forecast" group |
| **Event Identifier** | `ORC-2` / `ORC-3` Placer/Filler ID | `Immunization.identifier` | *Missing in ISO 5384* (Proposed Core Addition) |
| **Administration Record Fact** | `RXA` segment (`RXA-3`, `RXA-5`, `RXA-11`) | `Immunization` resource | "Immunization event" data element group |
| **Product Identity** | `RXA-5` Administered Code, `RXA-15` Lot | `Immunization.vaccineCode`, `lotNumber` | Common Name, Medicinal Product ID/Name/Batch |
| **Dose Evaluation** | `OBX` segment linked to `RXA` | `ImmunizationEvaluation` resource | *Missing in ISO 5384* (Proposed Core Addition) |
| **Forecast & Recommendations** | `OBX` segment linked to `QAK` / `RXA` | `ImmunizationRecommendation` resource | "Immunization forecast" data element group |

---

## 4. ISO/TS 5384:2024 Gap Analysis & Feedback Strategy

This functional guide identifies key operational gaps in ISO/TS 5384:2024 and provides a roadmap for national body comments (via ANSI/AFNOR) and HL7 Liaison contributions:

1. **Establish Explicit 5-Layer Separation:** Request that ISO convert its flat 22-page data dictionary into an explicit entity-relationship conceptual model separating Event Record, Product Identity, Vaccine Semantics, Dose Evaluation, and Forecast.
2. **Add Event Identity & Lifecycle Semantics:** Introduce business identifiers, event creation/update timestamps, versioning, and status codes (`entered-in-error`, `corrected`, `retracted`) to support registry deduplication.
3. **Introduce Explicit Dose Evaluation Entity:** Bridge the gap between event history and forecast by adding a dose evaluation concept aligned with FHIR `ImmunizationEvaluation`.
4. **Treat Valences as Terminology Metadata:** Position valences (e.g., NUVA) within Layer 3 (Vaccine Semantics) as derived terminology properties rather than required primary fields on point-of-care event records.
5. **Refine Subject Identity & Contextual Conditions:** Remove the blanket requirement for mandatory gender identity across all registry use cases; establish structured representations for patient clinical context (pregnancy, immunocompromised status) that influence forecasting.

---

## Addendum A: Jurisdictional Specification – United States

### A.1 Regulatory & Administrative Requirements
* **Vaccine for Children (VFC) Eligibility:** Inbound updates must capture financial eligibility at the dose level (`OBX` segment in v2; `Immunization.programEligibility` extension in FHIR).
* **Jurisdictional Reporting Mandates:** Mandatory reporting requirements governed by state/local statutes, including opt-in/opt-out consent tracking.

### A.2 Technical Implementation Profiles
* **HL7 v2 Realization:** CDC HL7 v2.5.1 Implementation Guide for Immunization Messaging (Release 1.5).
* **Transport Protocol:** CDC WSDL SOAP Web Services Specification (CDC Transport Layer).
* **FHIR Realization:** US Core Implementation Guide (`US Core Immunization Profile`) and SMART on FHIR authorization.

### A.3 Clinical Decision Support (CDS)
* **ACIP Schedule Compliance:** Forecasting and dose evaluation must execute against the Advisory Committee on Immunization Practices (ACIP) recommendations as codified in the CDC Clinical Decision Support for Immunization (CDSi) logic.

---

## Addendum B: Template for Custom Jurisdictional Addenda

National health authorities adopting this Core Functional Guide should populate Addendum B with:
1. **National Regulatory Mandates:** Patient consent policies, legal reporting timelines, and funding program tracking.
2. **National Terminology Bindings:** Mapping national drug codes (e.g., DM+D in the UK, PPN in Germany) to Layer 2/3 concepts.
3. **Regional Transport & Security:** OAuth2, SAML2, or national health information network transport protocols.
4. **National Schedule Logic:** Alignment with national technical advisory groups on immunization (NITAGs).
