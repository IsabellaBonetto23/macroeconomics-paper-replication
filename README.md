# macroeconomics-paper-replication
# Monetary Policy Shocks and Macroeconomic Effects  
### Replication of Bauer & Swanson (2021) using proxy-SVAR and high-frequency monetary policy shocks in Python.

## 📌 Project Overview
This project replicates and extends the empirical analysis of high-frequency monetary policy shocks in:

> Bauer, M. & Swanson, E. (2021)

The objective is to reassess the causal effects of monetary policy on the macroeconomy by addressing key limitations in standard high-frequency identification strategies.

---

## 🎯 Research Question
Are high-frequency monetary policy surprises truly exogenous?

If not, how does correcting for endogeneity affect estimated macroeconomic responses?

---

## ⚠️ Key Problem
Standard high-frequency monetary policy surprises are partially **contaminated**:

- They are predictable using macroeconomic information available before FOMC announcements.
- This violates the exogeneity assumption required for valid instrumental variables.

As a result, conventional estimates may understate the true macroeconomic effects of monetary policy.

---

## 💡 Methodology

### 1. High-Frequency Identification
- Monetary policy shocks measured using financial market reactions around FOMC announcements.
- Principal Component (PC1) used to summarize policy surprises.

### 2. Orthogonalization
We remove predictable components by regressing surprises on:
- Macroeconomic variables
- Financial indicators available prior to announcements

This produces a cleaned, exogenous shock measure (`MPS_ORTH`).

### 3. VAR-IV Framework

We estimate a reduced-form VAR:

Y_t = A₁Y_{t−1} + ... + A_pY_{t−p} + u_t

Structural shocks are identified using an external instrument:

- Instrument: z_t = MPS_ORTH  
- Target: residual of the 2-year Treasury yield

The goal is to isolate the monetary-policy component of financial market movements.

### 4. Impulse Response Functions (IRFs)

IRFs are computed using the companion form of the VAR:

State_{t+1} = A_comp · State_t

The identified impact vector b₁ initializes the system, and responses are computed recursively.

### 5. Bootstrap Inference

A wild bootstrap procedure is implemented to account for heteroskedasticity:

- Residuals are resampled using random sign flips.
- The VAR is re-estimated in each iteration.
- IRFs are recomputed and normalized to a 25-basis-point impact on the 2-year yield.

Confidence bands correspond to the 5th–95th percentiles.

---

## 📊 Main Findings

- Orthogonalized shocks generate larger and more persistent declines in industrial production.
- Inflation responds with a delay.
- Financial conditions tighten significantly (increase in excess bond premium).

These results suggest that previous literature underestimated macroeconomic effects due to shock contamination.

---
## 🔁 Reproducibility

Python version: 3.10.11

Install required packages:

```bash
pip install -r requirements_bauer_swanson.txt
