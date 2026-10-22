# Supplier Checklist — Spicejet

**Scope family:** Supplier Features → Feature → Supplier
**Feature:** Ancillaries (ancillaries)
**Integration Layer:** Unknown / not declared
**Supplier:** Spicejet (spicejet)
**Status:** Draft
**Revision:** 1
**Last Updated:** 2026-10-22
**Produced by run:** run-011__2026-10-22_08-48-25__Targeted__Ancillaries-Spicejet
**Inherits (by reference, never copied):** `platform_core_data/supplier-features/ancillaries/context/ExecutionFlow_HappyPath.md`, `FeatureChecklistProfile.md`, shared scope `ancillaries-baseline`

### Revision History

| Rev | Date | Changed By | What Changed | Why / Evidence |
|---|---|---|---|---|
| 1 | 2026-10-22 | Claude, for Hussein | Initial intake from the supplier's integration checklist (Sup001–Sup018) pasted by the human | Supplier Spicejet approved 2026-10-22 by Hussein. Supplier API keys deliberately **not** copied here |

---

> Scope: Application UI → Application → Supplier. Ancillaries declares no `integrationLayer`, so
> there is no NDC Core Service tier. Anything not stated below is `Unknown` or `To Be Discovered`,
> never invented.

## 1. Supplier Identity

| Field | Value |
|---|---|
| Supplier name | SpiceJet |
| Registered name / slug | Spicejet / `spicejet` |
| Credential (connection name on the Portal) used by run-011 | SpiceJetxxxx (credential country EGY) |
| Credential selector name (credential country) | EGY (Sup013, Sup018) |
| Country of contract | EGY (Sup016) |
| Credentials, current and future | EGY only; working status not stated — `Pending Confirmation` (Sup017) |

## 2. Supplier Scope

| Item | Value |
|---|---|
| Supported features (Sup008) | Search, FareConfirm, Book/Hold, BookAfterHold, FareConfirmAfterHold, Retrieve, GetAncillary, SeatMap |
| Routes — staging (Sup006) | DEL, BOM, BLR, DXB, LHR |
| Routes — production (Sup007) | DEL, BOM, BLR, DXB, LHR |
| Currency (Sup005) | Staging: INR · Production: AED |

## 2b. Testing Layer Scope

| Layer | State |
|---|---|
| Admin | In Scope |
| Portal | In Scope |
| API / Contract / Integration | To Be Discovered — no Postman collection supplied (Sup010) |

## 2c. Supplier Capabilities

Recorded by `supplier capabilities` on 2026-10-22 (Hussein). Source of truth is `supplier.json`.

| Capability | Value |
|---|---|
| Hold | Supported |
| Pay after Hold | Supported |
| Meal | Supported |
| Baggage | Supported |
| Seat | Supported |
| Bundle | Not Supported |

Note: the supplier's own feature list (Sup008) names `GetAncillary` and `SeatMap` but does not itemise
meals or baggage. The human confirmed Meal and Baggage as Supported; to be verified in Discovery.

## 3. Environment & Access

### 3a. UI

| Item | Value |
|---|---|
| Admin URL | http://192.168.1.90/auth/login |
| Portal URL | http://192.168.1.90:9500/auth/login |
| Login | Human logs in; the runner waits for it. Credentials never stored here |

### 3b. API / Integration

| Item | Value |
|---|---|
| Staging base URL (Sup001) | https://ndc-supplier-integration.azurewebsites.net/ |
| Production base URL (Sup002) | https://ndc-prd-ne-integration.proudtree-71b8270c.northeurope.azurecontainerapps.io/ |
| API keys (Sup001, Sup002, Sup004) | **Redacted.** Supplied by the human at intake; kept out of this file. Sup004 was left blank by the supplier |
| Postman collection (Sup010) | Not provided — `Pending Confirmation` |

Production must not be mutated: staging only unless the human says otherwise
(`execution_safety_policy.json`).

## 6. Supplier Flags (Sup011–Sup015)

| Flag | Value |
|---|---|
| Supplier provides SUPPORT_TICKET_NO | NO |
| SOTO_ENABLED | NO |
| Business alias (e.g. BK-1, BK-CT-ELQ) | NO |
| BSP_SIGN | NO |

## 7c. Supplier-Specific Rules

**None.** The human confirmed on 2026-10-22 that Spicejet has no rules of its own beyond the Feature's.
The Ladies-only seats and Business cabin rules do not apply.

## 11. Ownership

| Role | Name |
|---|---|
| Approver / run owner | Hussein |

## 12. Open Questions

| ID | Question | State |
|---|---|---|
| SJ-OQ-01 | Which of the credentials are working? Only EGY is listed | Pending Confirmation |
| SJ-OQ-02 | Postman collection for API-level checks | Pending Confirmation |
| SJ-OQ-03 | Does the supplier sell meals and baggage, given Sup008 does not itemise them? | To Be Discovered |
| SJ-OQ-04 | The Staging API key (Sup001) and Production key (Sup002) were pasted in chat; rotate if that is a concern | Pending Confirmation |

## 13. Intake Decision

Draft. Not yet reviewed for approval.
