---

# 🌸 Production of Aniline by Vapour-Phase Catalytic Hydrogenation of Nitrobenzene

<p align="center">
  <b>ChE453 – Capstone Project (Group 14)</b><br>
  Department of Chemical Engineering, Indian Institute of Technology Kanpur
</p>

---

## 📌 Project Overview

✨ This project presents the **design, simulation, and optimisation of a continuous industrial process** for the production of **Aniline** via the vapour-phase catalytic hydrogenation of **Nitrobenzene**.

Aniline (C₆H₅NH₂) is a bulk aromatic amine and a key intermediate of the organic chemical industry — about **85%** of world production feeds into MDI (methylene diphenyl diisocyanate) for rigid polyurethane foam, insulation panels, and adhesives, with the remainder going into rubber processing chemicals, agrochemicals, dyes, and pharmaceutical intermediates.

🔁 The project evolves systematically from **process/route selection and material balance** to **thermodynamic model validation and regression**, then to **flowsheet development and convergence in Aspen Plus**, with material and energy balances closing tightly against the hand calculations.

---

## 🧪 Reaction Chemistry

**Main Reaction:**

> C₆H₅NO₂ + 3 H₂ → C₆H₅NH₂ + 2 H₂O  (ΔHr ≈ −443 kJ/mol)

**Side Reactions (both proceed through aniline):**

> C₆H₅NH₂ + 3 H₂ → C₆H₁₃N  (over-hydrogenation to cyclohexylamine)
> C₆H₅NH₂ + H₂ → C₆H₆ + NH₃  (hydrogenolysis of the C–N bond)

🔬 Key characteristics:

* Vapour-phase, catalytic (Cu or Ni supported catalyst), continuous operation
* Strongly exothermic — reaction heat release of **~17.6 MW** at design capacity, driving both a steam-recovery opportunity and a hot-spot/runaway risk
* Selectivity split (moles nitrobenzene reacted basis): **95% aniline : 1.5% cyclohexylamine : 3.5% benzene+ammonia**
* Battery limits start from purchased nitrobenzene — nitration of benzene is outside project scope

---

## 🎯 Project Objectives

✅ Identify and justify a suitable **industrial process route** for aniline production
✅ Specify feed/product streams and close the **overall material balance**
✅ Perform an **input–output cost analysis** as a preliminary economic screen
✅ Select and validate an appropriate **thermodynamic model** (including in-house parameter regression)
✅ Develop the **process flow diagram** and synthesis logic (recycle structure, separation sequencing)
✅ Build, specify, and **converge the flowsheet in Aspen Plus**
✅ Verify **material and energy balance closure** against hand calculations

---

## 📦 Feed & Product Specifications

| Stream | Specification | Temp. | Pressure |
|--------|---------------|-------|----------|
| Aniline product | ≥ 99.9 wt%; water < 0.1 wt%; nitrobenzene < 5 ppm | 40 °C | 1.0 bar |
| Nitrobenzene feed | ≥ 99.5 wt%, technical grade | Ambient | 1.0 bar |
| Hydrogen make-up | 99.9 mol%, balance N₂ | Ambient | 5 bar |
| Treated wastewater | Aniline < 1 ppm before discharge | 40 °C | 1.0 bar |

**Design basis:**
- Plant capacity: **100,000 t/y** aniline, **8,000 h/y** operation (12.5 t/h)
- Nitrobenzene conversion per pass: **99%**
- Hydrogen-to-nitrobenzene molar ratio at reactor inlet: **10:1**

Market prices used in the economic screen (August 2026):
- Aniline: **Rs. 155,000/t** (≈ USD 1,631/t)
- Nitrobenzene: **Rs. 85,500/t** (bulk/term procurement target)
- Hydrogen: **Rs. 185,000/t** (on-site SMR production cost basis)

---

## 🧱 Process Development Summary

### 🧩 1. Process Identification, Material Balance & Thermodynamics (Report 1)

* **Route selected:** vapour-phase catalytic hydrogenation of nitrobenzene (Cu/Ni catalyst), chosen over liquid-phase hydrogenation, the Béchamp process, and phenol amination for its continuous operation, high selectivity (95–98%), clean by-product (water only), and recoverable reaction heat
* Overall material balance closed: **134.22 kmol/h** aniline product from **141.28 kmol/h** nitrobenzene reacted, with hydrogen loop (make-up, recycle, purge) balanced explicitly — purge ≈ 2% of make-up, carrying away ~1.9% of purchased hydrogen
* Reaction heat duty computed rigorously from actual reaction extents: **17.6 MW total** (main reaction 17,385 kW + side reactions 182 kW)
* Input–output economic screen: **gross contribution of Rs. 23,041/t aniline (14.9% margin)** — raw materials (mainly nitrobenzene, ~77% of revenue) dominate cost structure; screen passed
* **Thermodynamic model:** NRTL-RK selected (NRTL for liquid-phase activity coefficients, Redlich–Kwong for vapour); H₂ and N₂ declared as Henry components (supercritical at operating conditions)
* Default Aspen databank parameters for the ANILINE–WATER pair were found unreliable (sourced from a VLE bank despite being an LLE-splitting pair, and valid only over 99–168 °C versus the ~40 °C decanter operating point)
* **In-house NRTL regression performed** using NIST TDE binary LLE data (three-parameter fit, bⱼᵢ fixed at zero after diagnosing over-parameterisation in the initial four-parameter attempt)
* Regression validated independently against a held-out azeotropic data set: predicted azeotrope at **99.34 °C, x_aniline = 0.0368** vs. experimental **98.97 °C, x_aniline = 0.04** (agreement within 0.37 K)
* Confirmed a **heterogeneous aniline–water azeotrope**, meaning a decanter (not a distillation column) is required to break this pair

---

### 🔀 2. Flowsheet Development & Convergence in Aspen Plus (Report 2)

* Operating conditions fixed across all units: reactor at **300 °C, 3 bar** (catalyst operating window ~250–350 °C); flash/decanter at **40 °C** (bracketed by the regressed LLE data range of 39–70 °C)
* **Process synthesis logic:** vapour–liquid split taken first (flash), then liquid–liquid split (decanter) to break the aniline–water azeotrope for free, before handing the residue to distillation; three recycles identified — hydrogen gas recycle, unconverted nitrobenzene recycle, and recovered aniline recycle
* **Separation sequence:** DEC (decanter) → COL-1 (removes water-rich azeotrope overhead, three-phase RadFrac) → COL-2 (aniline overhead as product, nitrobenzene to bottoms/recycle, vacuum at 0.2–0.3 bar to avoid aniline darkening) → STRIP (atmospheric stripping of the aqueous phase)
* Full flowsheet built in Aspen Plus (RStoic reactor, Flash2, FSplit, Compr, Decanter, 3× RadFrac columns) and **converged with all three recycle loops closed** (Wegstein tear-stream convergence, max relative error 8.23×10⁻⁴)
* **Mass balance closure:** 0.7 kg/h in 18,300 kg/h (**0.004%**) | **Energy balance closure: 0.003%**
* Converged results matched the Report 1 hand balance closely (e.g., fresh nitrobenzene feed and hydrogen make-up agree exactly; reactor H₂:NB ratio settles at 9.12:1 due to recycle-gas nitrogen buildup, vs. 10:1 assumed)
* Product stream at this stage: **97.04 wt% aniline** (limited by residual water, cyclohexylamine, and nitrobenzene — still above the 5 ppm nitrobenzene spec), flagged for sharper column design in the next report
* Total heating **22.48 MW**, total cooling **45.16 MW**, against a **17.57 MW** reaction exotherm — heat integration beyond the single feed–effluent exchanger identified as future work

---

## ⚙️ Key Technical Features (as of Report 2)

* Vapour-phase catalytic hydrogenation reactor (RStoic, 300 °C / 3 bar), 17.6 MW exotherm
* Hydrogen recycle loop with compressor and controlled purge (0.86% split) to manage nitrogen build-up
* Decanter-first separation strategy exploiting the heterogeneous aniline–water azeotrope
* Four-unit separation train: Decanter → COL-1 → COL-2 (vacuum) → Stripper
* In-house regressed NRTL-RK thermodynamic parameters (ANILINE–WATER pair), validated against independent azeotropic data
* Fully converged Aspen Plus flowsheet with three closed recycle loops

---

## 💰 Economic Results (Preliminary Screen — Report 1)

| Metric | Value |
|--------|-------|
| Aniline revenue | Rs. 155,000/t |
| Total raw materials cost | Rs. 131,959/t |
| **Gross contribution** | **Rs. 23,041/t** |
| **Gross margin** | **14.9%** |

This input–output screen considers raw materials only (nitrobenzene, hydrogen); utilities, labour, maintenance, and capital charges are excluded. Full profitability metrics (ROCE, NPV, IRR, payback) are deferred to a later report once equipment costs and working capital are estimated.

---

## ⚠️ Limitations & Objectives Carried Forward

**From Report 1:**
* Reactor performance (conversion/selectivity) is a specified assumption (RStoic block), not yet a kinetics-derived result
* Aniline–nitrobenzene NRTL pair has not yet been regressed (still on narrow-validity databank values, 83–99 °C)
* Economic screen rests on three externally sourced prices, pending confirmation via delivered quotations
* A third (unidentified) azeotrope appeared in the ternary aniline/water/nitrobenzene diagram after regression

**From Report 2:**
* Aniline–nitrobenzene pair regression still pending — directly affects COL-2, which runs at 144 °C, outside current databank validity
* Columns are converged but not yet designed to meet product specification (99.9 wt% aniline, 5 ppm nitrobenzene not yet achieved — currently 97.04 wt% / 660 ppm)
* COL-1 currently uses a total condenser despite non-condensable gases (H₂, N₂, NH₃) in its feed — a partial condenser with vapour draw is flagged as the needed fix
* No pinch/heat-integration analysis or utility costing performed yet (only the single feed–effluent exchanger is in place)
* A pump is missing at the MIX-1 inlet (currently defaults to the lowest inlet pressure)

**Future work (per both reports):**
* Kinetic parameter estimation for the reaction network, replacing the RStoic assumption
* Aniline–nitrobenzene NRTL regression
* Rigorous distillation column design and sequencing to meet final product purity
* Pinch analysis, heat exchanger network design, and utility costing
* Sensitivity analysis and full techno-economic evaluation (capital cost, NPV, IRR, payback)

---

## 👥 Team Members (Group 14)

| Member | Roll No. |
|--------|----------|
| Nithin T M | 230709 |
| Aditya Nitin Patil | 230074 |
| Nithin D H | 230708 |
| Shubham Singh | 230999 |
| Shivam Dongare | 230971 |
| Sajag Masane | 230896 |

---

✨ *This repository documents the ongoing academic capstone journey — from process/route selection and thermodynamic validation through to a converged Aspen Plus flowsheet, with further reports to follow on kinetics, column design, heat integration, and final techno-economic evaluation.*

---
