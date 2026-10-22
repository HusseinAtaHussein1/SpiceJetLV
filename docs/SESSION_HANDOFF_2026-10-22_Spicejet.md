# Session handoff — Spicejet (Ancillaries), 2026-10-22

Written so the next person can resume without this chat. Read the **Open items** and **Resume** sections first.

## What this session did

1. Added **Spicejet** as a Supplier under the **Ancillaries** Feature (approved by Hussein).
2. Created and ran two runs (below). Run-011 is superseded; **run-012 is the current one**.
3. Fixed several runner problems found while running Spicejet, and replaced the saved-passenger flow with
   Faker-generated passengers for Spicejet.

## Supplier scope

| | |
|---|---|
| Supplier | `ancillaries/spicejet`, `platform_core_data/supplier-features/ancillaries/suppliers/spicejet/` |
| Capabilities | Hold, Pay after Hold, Meal, Baggage, Seat: Supported. Bundle: Not Supported |
| Supplier rules | None |
| Connection name on the Portal | `SpiceJetxxxx` (credential country EGY). The original name "spicejet EGY" did not match anything on screen |
| Portal / Admin | `http://192.168.1.90:9500` / `http://192.168.1.90` |
| Checklist | `suppliers/spicejet/context/SupplierChecklist.md` (condensed Draft, rev 1). **API keys deliberately not stored** |

## Runs

| Run | State |
|---|---|
| `run-011__2026-10-22_08-48-25__Targeted__Ancillaries-Spicejet` | Admin + Portal scope. Used while debugging the runner. **Do not continue it** |
| `run-012__2026-10-22_12-43-32__Targeted__Ancillaries-Spicejet-Portal` | **Current.** Portal only, Headed, frozen scope. Capability Plan: 102 will run, 22 excluded |

Evidence for each run is under `runs/<run>/manual_execution/`: `GroupResults_Group<NN>_<timestamp>.md` (one per
execution, never overwritten) and `artifacts/failures/` (screenshots, one per non-passing case; a re-run adds a
timestamped copy, so use the newest).

## Results so far in run-012 (Portal)

| Group | Result |
|---|---|
| 01 | 36 passed, 8 failed, 7 N/A, 2 not run, 10 excluded |
| 02, 03 | No cases to run for Spicejet (03 also has no women-only seats) |
| 04, 06 | Passed (1 and 3 cases) |
| 05 | 1 passed, 1 failed (TC-BG-001), 2 inconclusive (exit seats all blocked) |
| 07, 08 | 1 failed each (TC-SM-EXR-005, TC-SM-PAX-008) |
| 09 | 2 passed, 3 failed, 2 excluded |
| 10 | 1 passed, 2 failed (TC-SM-MCT-006, TC-SM-MCT-007) |
| 11 | 1 failed (TC-SM-RND-007) |
| 12 | Blocked: needs `nonEgp` settings |
| 13 | Blocked: My Bookings has no rows |
| 14, 15, 16, 17 | Blocked before the Book/Hold gate. **Nothing was booked** |

## Findings, with how sure I am

| Case | Reading |
|---|---|
| TC-SM-CNF-018 | **Likely a real defect.** Going back to step 1 and returning discards the seats silently (22,009 → 20,011 EGP). Reproduced every run |
| TC-SM-STA-005 | **Possible real issue.** 16B and 16E are middle seats labelled Window / Aisle |
| TC-SM-MCT-006 | **Possible real issue.** Only 2 baggage boxes for 3 trips (Trip 1 and Trip 3 share DEL→BLR). Unconfirmed merge theory |
| TC-SM-NAV-001, STA-004, SEL-003, BG-001 | **Probably the check, not the Portal** (expects "…and Bundles" wording; "Overwing seat(s)" not on the known list; a text comparison that may be a space-character mismatch; baggage options worded "Excess Baggage – N Kg"). Not yet fixed or confirmed |
| TC-SM-PAX-007, SEL-009, PAX-006, PAX-008, RND-010, RTP-001, RTP-005, RND-007, MCT-007, EXR-005 | **Need a ruling** on what the case should expect. See the chat history for each |

## What was changed

- **Runner** (`ancillaries/runner/`):
  - `portal.js`: Branded Fares dialog (clicks Proceed); `passengerData: "generated"` mode with Faker names
    (unique first and last, no first name inside another); Title list is Mr/Ms only; Gender is set by the Title;
    `showSeat()` helper for screenshots; per-field error labels.
  - `group01.js` (STA-004/005, SEL-003, PAX-006, CNF-018 screenshots), `group05.js`, `group07.js`, `group08.js`
    (screenshot targets), `group09.js` (skips package picking when the supplier has none), `index.js`
    (settings check relaxed for generated passengers).
- **Settings** (git-ignored): `runner/settings.local.json`: route DEL→BLR, `passengerData: generated`, Admin
  branch/agency/agent, multi-city DEL→BLR (+1d), BLR→DEL (+2d), DEL→BLR (+3d).
- **Dependencies:** `npm ci`, and `@faker-js/faker` added as a dev dependency (`package.json` and the lockfile).
- **Staging Portal data:** 5 dummy travellers (Testadult Alpha/Beta, Testchild Gamma, Testinfant Delta/Epsilon,
  mobile 1012345678) were created early on, and the Portal also saves details typed on the passenger form.
  They remain in the saved list.
- **Not done:** no git commits. Run-011 and run-012 are untouched by any cleanup.

## Open items

1. **Baggage option wording** blocks Groups 14 and 17 and causes the TC-BG-001 false failure. Proposed fix: read
   "Excess Baggage – N Kg" as an option. **Not yet approved.**
2. **Group 13:** treat an empty My Bookings table as zero bookings. **Not yet approved.**
3. **Group 16:** the clash needs a package with checked baggage. Mark Not Applicable for a supplier with none.
4. **Group 15:** stopped because the browser window closed. Cause unknown; re-run without touching the window.
5. **Group 12:** needs `nonEgp` (Portal credential label, currency, rate of exchange from Admin → Master →
   Miscellaneous).
6. **Evidence:** MCT-007, RND-007 and PAX-006 screenshots do not show their failure (taken outside the seat map).
7. **Wording to confirm:** STA-004 (add "Overwing seat(s)" to the known list, or raise it) and SEL-003 (space-character
   comparison).
8. **Admin layer** has not been executed for Spicejet.
9. **Not an open fix, but unresolved:** the supplier's staging and production API keys were pasted into chat. Consider
   rotating them.

## Resume

```bash
cd agentic-ai-testing-platform
node platform_core/tools/supplier_feature_runner/cli.js groups --run run-012
node platform_core/tools/supplier_feature_runner/cli.js run --run run-012 --group 01 --headed --app Portal
```

Always ask Headed or Headless; log in to the Portal when the window opens; the runner never presses Book, Hold or
Pay. Do not modify `run.json` scope fields except through `run_manager/cli.js scope-change`.
