# HMM 2008 Recession Detection

Interactive visualization of a 2-state Hidden Markov Model detecting the 2008 financial crisis in near real-time.

## Live Demo

**[https://hmm-2008-recession.vercel.app](https://hmm-2008-recession.vercel.app)**

## What it shows

| Algorithm | Description |
|-----------|-------------|
| Forward (causal) | P(Crisis \| O₁:t) — updates month-by-month using only past observations |
| Smoothed / Forward-Backward | P(Crisis \| O₁:T) — uses full sequence (hindsight) |
| Viterbi path | Most probable hidden state sequence (log-domain) |

## Data Sources

- **WTI Crude Oil**: FRED series  (monthly average, Jan 2004–Dec 2010)
- **Mortgage Delinquency**: FRED series  (90+ days, quarterly → monthly linear interpolation)
- **NBER Recession**: December 2007 (peak) – June 2009 (trough)

## HMM Parameters



## Usage

Open  directly in any browser — fully self-contained, no build step, no dependencies except Chart.js (CDN).
