# HMM 2008 Recession Detection

> **An interactive, animated visualization of a 2-state Hidden Markov Model detecting the 2008 financial crisis in near real-time.**
> Built for a Calculus 2 / probability course — all mathematics is rigorous and PhD-reviewed.

## Live Demo

**[hmm-2008-recession.vercel.app](https://hmm-2008-recession.vercel.app)**

---

## What Is This?

This project uses a **Hidden Markov Model (HMM)** to answer a real question:

> *Could a mathematically principled algorithm have flagged the 2008 recession as it was unfolding — using only data available at the time?*

The answer, demonstrated month-by-month, is **yes**.

Two observable time series from FRED are fed into the model:
- **WTI Crude Oil prices** — monthly average (MCOILWTICO)
- **Residential mortgage delinquency rate** — 90+ days past due (DRSFRMACBS), quarterly, linearly interpolated to monthly

Each month these are converted into a single **discrete observation symbol**:

| Symbol | Condition | Meaning |
|--------|-----------|---------|
| 0 | Oil ↓, Delinquency ↓ | Benign |
| 1 | Oil ↓, Delinquency ↑ | Financial Stress |
| 2 | Oil ↑, Delinquency ↓ | Supply Shock |
| 3 | Oil ↑, Delinquency ↑ | Dual Stress ⚠ |

The HMM then infers the probability of being in a hidden **Crisis** vs **Stable** economic regime at each point in time.

---

## Three Algorithms, Three Questions

| Algorithm | Question answered | Causal? |
|-----------|-------------------|---------|
| **Forward (Filtering)** | P(Crisis \| observations so far) | ✅ Yes — valid in real time |
| **Forward-Backward (Smoothing)** | P(Crisis \| all observations) | ❌ No — requires full sequence |
| **Viterbi** | What is the single most probable hidden state path? | ❌ No — requires full sequence |

The animation steps month-by-month through the **Forward Algorithm**, showing the exact recursion being computed at each step in a live math trace panel.

---

## Model Parameters

The HMM is manually specified with economic justification — **not estimated via Baum-Welch EM**.



---

## Key Mathematical Concepts

- **Markov property** — P(Xt | X1...Xt-1) = P(Xt | Xt-1)
- **Forward variable** — αt(j) = P(O1...Ot, Xt=Sj | λ)
- **Normalization** — γt(j) = αt(j) / Σ αt(j) = P(Xt=Sj | O1:t, λ)
- **Backward variable** — βt(j) = P(Ot+1...OT | Xt=Sj, λ)
- **Smoothed posterior** — γt(j) ∝ αt(j) · βt(j)
- **Viterbi (log domain)** — δt(j) = max path log-prob to state j at time t

Log-returns r_t = ln(P_t / P_{t-1}) are used for oil (not simple returns) — they are time-additive and consistent with continuous compounding.

---

## Usage

No build step. No framework. No dependencies except Chart.js (loaded via CDN).



Or visit the live Vercel deployment: **[hmm-2008-recession.vercel.app](https://hmm-2008-recession.vercel.app)**

---

## Data Sources

| Series | Source | Frequency | Period |
|--------|--------|-----------|--------|
| WTI Crude Oil | [FRED MCOILWTICO](https://fred.stlouisfed.org/series/MCOILWTICO) | Monthly | Jan 2004 – Dec 2010 |
| Mortgage Delinquency | [FRED DRSFRMACBS](https://fred.stlouisfed.org/series/DRSFRMACBS) | Quarterly → Monthly (interpolated) | Q1 2004 – Q4 2010 |
| NBER Recession Dates | [NBER Business Cycle Dating](https://www.nber.org/research/data/us-business-cycle-expansions-and-contractions) | — | Peak: Dec 2007, Trough: Jun 2009 |

---

## Tech Stack

- **Vanilla HTML/CSS/JS** — single self-contained file
- **Chart.js 4.4** — synchronized line charts with custom NBER shading plugin
- **Inline SVG** — animated HMM state diagram
- **Python** — used for data preprocessing and Vercel/GitHub deployment via REST API
