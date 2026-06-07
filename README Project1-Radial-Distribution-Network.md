# Project 1 — Radial Distribution Network: Complete Load Flow Study

> **Full power systems engineering study covering baseline analysis, step-by-step remediation, power factor analysis, reactive compensation design, and load growth sensitivity — all on the same 33 kV / 22 kV / 11 kV radial network in ETAP 19.0.1.**

---

## Author
**Kalpak Chandrashekhar Nikose — Graduate Electrical Engineer**
Melbourne, Victoria | [LinkedIn](https://linkedin.com/in/kalpaknikose)

---

## Study Overview

| Item | Detail |
|---|---|
| **Software** | ETAP 19.0.1 |
| **Study Type** | AC Load Flow — Newton-Raphson |
| **Network** | 33 kV / 22 kV / 11 kV Radial Distribution |
| **Standard** | AS/NZS 61000.3.100 · AS/NZS 3008.1.1 · BS 6622 |
| **Critical Violations Found** | 3 — all resolved |
| **Studies Completed** | Baseline · Cable Fix · Tap Sensitivity · Cap Bank · Load Growth |
| **Date** | 31 May 2026 – 07 June 2026 |

---

## Network Topology

```
         U1 (33 kV — 1714.73 MVAsc)
                   |
              Bus1 (33 kV)
             /              \
      T1 (10 MVA)        T2 (35 MVA)
      33/11 kV            33/22 kV
      Tap: 0%             Tap: −5% ✓ (remediated)
          |                    |
      Bus2 (11 kV)         Bus9 (22 kV)
          |                    |
       Cable1               Cable2 ✓
      400mm² CU             150mm² CU × 3 parallel (remediated)
          |                    |
      Bus3 (11 kV)         Bus4 (22 kV)
      /         \           /    |    \
  Lump1       Lump2     Lump3  CAP1  Lump4
  3 MVA       2 MVA    10 MVA 6MVAr  20 MVA
  85% PF      80% PF   60% PF        75% PF
```

---

## Complete Results Progression

| Scenario | Bus4 % | Bus9 % | Cable2 % | T2 % | Source PF | Alerts |
|---|---|---|---|---|---|---|
| 1. Baseline | 91.4% ❌ | 91.5% ❌ | 274.6% ❌ | 97.3% | 57.54% | 3 Critical |
| 2. Cable2 fixed (3 cond.) | 91.5% ❌ | 91.5% ❌ | 97.07% ✅ | 97.3% | 57.54% | 2 Critical |
| 3. + T2 tap −2% | 93.6% ❌ | 93.7% ❌ | 97.07% | 97.3% | 57.54% | 2 Critical |
| 4. + T2 tap −5% ✅ | 97.07% ✅ | 97.07% ✅ | 97.07% | 97.07% | 57.54% | **0** |
| 5. + 6 MVAr CAP1 ✅ | 99.32% ✅ | ~99.3% ✅ | 97.07% | ~74.6% ✅ | 77.25% ✅ | **0** |
| 6. + 8 MVAr CAP1 | 100.1% ✅ | ~100.1% ✅ | 97.07% | ~71.4% ✅ | 80.76% ✅ | **0** |
| 7. 110% load growth | 108.4% ❌ | 108.4% ❌ | — | — | — | Over-V |
| 8. 120% load growth | 108.4% ❌ | 108.4% ❌ | — | — | — | Over-V |

---

## Equipment Data

| Device | Type | Rating | Standard | Remediated State |
|---|---|---|---|---|
| U1 | Utility Source | 1714.73 MVAsc, 33 kV | — | — |
| T1 | Transformer | 10 MVA, 33/11 kV | IEC Liquid-Fill | Tap: 0% |
| T2 | Transformer | 35 MVA, 33/22 kV | IEC Liquid-Fill, 65°C | **Tap: −5% ✓** |
| Cable1 | BS6622 XLPE CU | 400 mm², 11 kV, 50 m | BS 6622 | 1 cond./phase |
| Cable2 | BS6622 XLPE CU | 150 mm², 22 kV, 100 m | BS 6622 | **3 cond./phase ✓** |
| CAP1 | Shunt Capacitor | 6 MVAr, 22 kV | — | Added at Bus4 |
| Lump1 | Load | 3 MVA, 11 kV, 85% PF | — | 80% motor / 20% static |
| Lump2 | Load | 2 MVA, 11 kV, 80% PF | — | 80% motor / 20% static |
| Lump3 | Load | 10 MVA, 22 kV, 60% PF | — | 80% motor / 20% static |
| Lump4 | Load | 20 MVA, 22 kV, 75% PF | — | 80% motor / 20% static |

---

## Study 1 — Baseline: Violations Found

### Critical
| Device | Condition | Rating | Operating | % |
|---|---|---|---|---|
| **Cable2** | **Overload** | 301.976 A | **829.2 A** | **274.6%** |
| **Bus4** | **Under-Voltage** | 22 kV | 20.104 kV | **91.4%** |
| **Bus9** | **Under-Voltage** | 22 kV | 20.131 kV | **91.5%** |

### Marginal
| Device | Condition | Rating | Operating | % |
|---|---|---|---|---|
| Bus2 | Under-Voltage | 11 kV | 10.705 kV | 97.3% |
| Bus3 | Under-Voltage | 11 kV | 10.703 kV | 97.3% |

📸 `Fault.png`

---

## Study 2 — Remediation Step 1: Cable2 (3 Conductors/Phase)

**Problem:** 150 mm² cable at 274.6% thermal overload.

**Decision:** 3 × 150 mm² in parallel — same cable type, existing trench, no redesign.
829 A shared across 3 conductors = ~276 A each (within 302 A rating).

**Key insight:** Cable fix cleared the overload but had **zero effect on Bus4/Bus9 voltage** — they stayed at 91.5%. The overload and under-voltage are two independent problems.

| | Before | After |
|---|---|---|
| Cable2 | 274.6% ❌ | 97.07% ✅ |
| Bus4 | 91.4% ❌ | 91.5% ❌ (unchanged) |

📸 `Cable_2_before_change.png` · `Cable_2_after_change.png` · `Reading_after_cable_2_change.png`

---

## Study 3 — Remediation Step 2: T2 Tap Sensitivity

**Problem:** Bus4/Bus9 at 91.4–91.5% — 3.5% below 95% lower limit.

**Principle:** A negative primary tap reduces turns ratio → boosts secondary voltage.

### Sensitivity Results

| T2 % Tap | Primary kV | Turn Ratio | Bus4 % | Bus9 % | Status |
|---|---|---|---|---|---|
| 0% (original) | 33.00 | 1.00 | 91.4% | 91.5% | ❌ Critical |
| −2% | 32.34 | 0.98 | 93.6% | 93.7% | ❌ Critical |
| **−5%** | **31.35** | **0.95** | **97.07%** | **97.07%** | **✅ Pass** |

📸 `T2_tap_before_change.png` · `T2_at_tap_-2.png` · `T2_at_tap_-5.png` · `Reading_after_T2_tap_change_to_-2.png`

---

## Study 4 — Final Remediated State (No Capacitor)

Both fixes applied: Cable2 × 3 conductors + T2 tap −5%.

| Bus | Nominal | Operating | % | Status |
|---|---|---|---|---|
| Bus1 | 33 kV | 33.0 kV | 100.0% | ✅ |
| Bus2 | 11 kV | 10.705 kV | 97.3% | 🟡 Monitor |
| Bus3 | 11 kV | 10.703 kV | 97.3% | 🟡 Monitor |
| Bus4 | 22 kV | ~21.36 kV | 97.07% | ✅ |
| Bus9 | 22 kV | ~21.35 kV | 97.07% | ✅ |

> **ETAP Alert View Critical: empty ✅**

📸 `Reading_after_all_changes.png` · `Readings_with_pf.png`

---

## Study 5 — Power Factor Analysis

With PF annotations enabled on the SLD:

| Point | kVA | PF % | Observation |
|---|---|---|---|
| Source (Bus1) | 36,961 | 57.54% | Very poor — heavily reactive |
| T2 primary | 32,007 | 65.11% | Poor — Lump3 & Lump4 dominate |
| T1 primary | 5,079 | 81.18% | Acceptable |
| Bus4 feeder | 29,516 | 70.33% | Reactive compensation opportunity |
| Lump3 | 9,885 | 60% | Worst PF load |
| Lump4 | 19,769 | 75% | Moderate |

> **Root cause:** All reactive current travels from source through T2 and Cable2 instead of being supplied locally. This causes the voltage drop and increases thermal loading.

---

## Study 6 — Capacitor Bank: CAP1 at Bus4

### Equipment
| Parameter | 6 MVAr (Case A) | 8 MVAr (Case B) |
|---|---|---|
| Bus | Bus4 (22 kV) | Bus4 (22 kV) |
| Capacitance | 39.46 µF | 29.60 µF |
| Rated Current | 157.5 A | 210.0 A |
| Xc | 80.67 Ω | 60.50 Ω |

### Results

| Metric | No Cap | + 6 MVAr | + 8 MVAr |
|---|---|---|---|
| Bus4 voltage | 97.07% | **99.32%** | **100.1%** |
| Source PF | 57.54% | **77.25%** | **80.76%** |
| T2 loading | 97.07% | **~74.6%** | **~71.4%** |
| T2 N-1 margin | ❌ None | ✅ Good | ✅ Good |
| Source kVA | 36,961 | 31,184 | ~30,000 |

> **6 MVAr recommended** — best balance of improvement vs light-load over-voltage risk.
> T2 loading drop from 97.07% → 74.6% creates N-1 contingency margin that did not exist before.

📸 `Adding_Capacitor_Bank_of_6MVAr.png` · `rating_of_capacitor_bank_6MVAr.png` · `Alert_view_at_6MVAr.png` · `Alert_at_8MVAr.png`

---

## Study 7 — Load Growth Sensitivity (110% & 120%)

Loading categories set to 110% (Winter Load) and 120% (Summer Load) for all four lumped loads.
Network in remediated state (3 conductors + T2 tap −5%, no cap bank).

| Loading | Bus4 % | Bus9 % | Status |
|---|---|---|---|
| 100% | 97.07% | 97.07% | ✅ Pass |
| 110% | 108.4% | 108.4% | ❌ Over-voltage |
| 120% | 108.4% | 108.4% | ❌ Over-voltage |

### Key Finding — Fixed Tap Limitation

The −5% tap is optimised for 100% load. At 110–120% load, Bus4/Bus9 rise to 108.4% — exceeding the 105% upper limit.

> **A fixed tap cannot satisfy all load levels. An OLTC on T2 is required for long-term robustness.**

---

## Recommended Actions

| Priority | Action | What It Fixes | Cost |
|---|---|---|---|
| ✅ Done | Cable2: 3 × 150mm² parallel | Thermal overload | Low |
| ✅ Done | T2 tap → −5% | Undervoltage — immediate | Negligible |
| ✅ Done | 6 MVAr cap at Bus4 | Root cause: PF + T2 N-1 margin | Moderate |
| Next | Switchable cap bank (2 × 3 MVAr) | Light-load over-voltage risk | Moderate+ |
| Long term | OLTC on T2 | Auto voltage control all load levels | High |
| Long term | T2 uprate to 40–45 MVA | N-1 contingency + load growth | High |
| Long term | Cable1 parallel/upsize | 11 kV near-limit loading | Moderate |

---

## Key Learnings

1. **Cable overload ≠ voltage drop** — they are independent problems requiring independent fixes. Fixing Cable2 had zero effect on Bus4/Bus9 voltage.

2. **Transformer tap is load-level dependent** — a fixed tap optimised for full load causes over-voltage at higher loading. OLTCs exist for exactly this reason.

3. **Root cause was reactive current** — the 57.54% source PF meant excessive reactive current was flowing all the way from the source. The cap bank fixed this at source.

4. **Capacitor banks improve more than just voltage** — the 6 MVAr bank dropped T2 loading from 97.07% to 74.6%, creating N-1 margin that didn't exist before.

5. **Always run load growth sensitivity** — a network that passes N-0 at 100% load may fail at 110% or create new violations at light load. Planning headroom matters.

---

## Files in This Repository

```
Project1_LoadFlow/
├── README.md                                  ← This file
├── LoadFlow_Complete_Report_Final.pdf         ← Full 14-section report (all studies)
│
└── screenshots/
    ├── SLD.png                                ← Base one-line diagram
    ├── Fault.png                              ← Baseline alert view
    ├── Cable_1.png                            ← Cable1 editor
    ├── Cable_2_before_change.png              ← Cable2 original (1 cond./phase)
    ├── Cable_2_after_change.png               ← Cable2 remediated (3 cond./phase)
    ├── Lump_1.png – Lump_4.png               ← Load editors
    ├── T2_before_change.png                   ← T2 rating tab
    ├── T2_tap_before_change.png               ← T2 tap at 0%
    ├── T2_at_tap_-2.png                       ← T2 tap at −2%
    ├── T2_at_tap_-5.png                       ← T2 tap at −5% (selected)
    ├── Reading_after_cable_2_change.png       ← After cable fix only
    ├── Reading_after_T2_tap_change_to_-2.png  ← After tap −2%
    ├── Reading_after_all_changes.png          ← Final remediated (no cap)
    ├── Readings_with_pf.png                   ← Final with PF annotations
    ├── Adding_Capacitor_Bank_of_6MVAr.png     ← SLD with CAP1
    ├── rating_of_capacitor_bank_6MVAr.png     ← Cap editor
    ├── Alert_view_at_6MVAr.png                ← Result at 6 MVAr
    └── Alert_at_8MVAr.png                     ← Result at 8 MVAr
```

---

## Standards Referenced

- AS/NZS 61000.3.100:2011 — Steady-State Voltage Limits in Public Electricity Systems
- AS/NZS 3008.1.1:2017 — Selection of Cables (Australian conditions)
- BS 6622:1991 — Cables for Distribution Systems up to 33 kV
- IEC 60909-0:2016 — Short-Circuit Currents in Three-Phase AC Systems
- ETAP 19.0.1 User Guide — Operation Technology, Inc.

---

*Prepared by: Kalpak Chandrashekhar Nikose — Graduate Electrical Engineer*
*Date: 07 June 2026 | Tool: ETAP 19.0.1 | Standard: AS/NZS 61000.3.100*
