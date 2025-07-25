---
title: "Under the Helmet: Quantifying Pressure in IPL Batting"
date: 2025-07-20 12:00:00 +0000
author: Soham Sud
categories: [sports-analytics, competitions, misc-maths]
tags: [cricket, statistics, analytics, pressure-index, batting]
math: true
---

# Introduction

*Pressure.*  
We’ve all felt it—hands a little colder, heart a little louder.  
Professional athletes train for years to tame that chemical chaos, yet every fan senses the moments when even the greats look mortal:  
the last ball before tea with eight wickets down, *60 off 24* in a T20 chase, or simply knowing the tail won’t finish the job after you.

Turning such a visceral, in‑the‑moment feeling into a *number* sounds borderline heretical.  
But that’s the quest of this blog series.  
I’m a dyed‑in‑the‑wool math nerd who also happens to play village‑standard leg‑spin.  
Across these posts I’ll use hard data to peek behind cricket’s curtain—starting with a metric I call the **Batting Pressure Handling Index (BPHI)**.

<br/>

**TL;DR**  
- We model the *pressure* on every ball in IPL history.  
- We weight each run, boundary, and dismissal by that pressure.  
- We shrink the result so one‑match wonders don’t outrank Virat.  
- [Jump to the leaderboards &rarr;](#bphi-leaderboards)

If you just want the leaderboards, jump to [the tables below](#bphi-leaderboards).  
If you relish equations, read on.

---

# Why Quantify Pressure?

> “If you can’t measure it, you can’t improve it.”  
> —a line often pinned on Peter Drucker but probably coined by a bored analyst at Lord’s.

Strike‑rate, dot‑ball %, false‑shot %—coaches already track the lot.  
Trouble is, *none* of those care whether the ask was 4 an over or 14, whether five wickets were still in the shed or the No. 10 was duct‑taping his ankle.  
Pressure is a **latent variable**: unseen but felt.

A worthwhile metric therefore must

1. **Scale smoothly** from “Sunday‑friendly” to “heartbeat in your eardrums”.  
2. **Reward** batters who add runs *precisely when* runs are priceless.  
3. **Punish** brain‑fade wickets harder in clutch chases than in 200‑for‑2 strolls.  

And crucially, it should do all that without being a black box—because if Ravi Shastri can’t explain it in ten seconds on air, it’s dead on arrival.

---

# Data Journey

## Raw Material

* **Ball‑by‑ball feed** — `all_ipl_matches.csv`, seasons 2008‑2025, ≈ 200 000 deliveries.  
* **Venue par scores** — `venue_characteristics.csv` (because 160 in Chepauk ≠ 160 in the Chinnaswamy).  
* **Hygiene pass** —  
  * Levenshtein smushing (“Rohit Sharma” ↔ “RG Sharma”).  
  * Impute missing extras as 0 — looking at you, multi‑page scorecards.  
  * Drop super‑overs because they break every normal distribution known to humankind.

## Feature Kitchen – How the potion is brewed 🧪

For every ball \(j\) we cook up

$$
\begin{aligned}
\text{ball-number}_j &= 6 \times \text{over} + \text{ball-in-over}, \\[6pt]
\text{balls-left}_j  &= 120 - \text{ball-number}_j, \\[6pt]
\text{overs-left}_j  &= \frac{\text{balls-left}_j}{6}, \\[10pt]
\text{wkts-left}_j   &= 10 - \text{wkts-fallen}_j.
\end{aligned}
$$

If it’s the second innings we also track \(\text{runs\_left}_j\) to the target;  
otherwise we chase a venue‑adjusted par.  
Think of it as Google Maps for run‑chases—always rerouting.

---

# From Context to Pressure

## The Five Drivers

1. **$\displaystyle \frac{\text{RRR}}{\text{CRR}}$** — required run rate over current run rate.  
2. **$\text{Wkts}_N$** — wickets fallen, min–max scaled to $[0,1]$.  
3. **$\text{TBF}_N$** — total balls bowled so far (i.e.\ game clock).  
4. **$\text{IBF}_N$** — balls the striker has already faced (are they set?).  
5. **$\text{MI}_N$** — match importance: 0.7 league, 0.9 play‑off, 1.0 final.  

These five are the Avengers of match context—together they explain ≈ 90 % of win‑probability swings in T20 cricket.

## Pressure Function

$$
P_j \;=\; \sigma\!\Bigl(
  \alpha_1 \bigl(\tfrac{\text{RRR}}{\text{CRR}}\bigr)_N
 + \alpha_2\,\text{Wkts}_N
 + \alpha_3\,\text{TBF}_N
 + \alpha_4\,\text{IBF}_N
 + \alpha_5\,\text{MI}_N
\Bigr),
\qquad
\sigma(x)=\frac{1}{1+e^{-x}}.
$$

Why a sigmoid?  
Because pressure feels linear at the low end (“meh, 5 an over”) but saturates near panic (“need 38 off 12!”).  
The $\alpha$’s are our DJ deck—slide them to remix what “pressure” means.

| **Preset** | $\alpha_1$ | $\alpha_2$ | $\alpha_3$ | $\alpha_4$ | $\alpha_5$ | Narrative |
|:----------:|:-----------:|:-----------:|:-----------:|:-----------:|:-----------:|-----------|
| **rrr**    | 1 | 0 | 0 | 0 | 0 | Pure “ask” rate. |
| **wkts**   | 0 | 1 | 0 | 0 | 0 | Batting‑collapse nightmare. |
| **tbf**    | 0 | 0 | 1 | 0 | 0 | End‑game timer. |
| **ibf**    | 0 | 0 | 0 | –1 | 0 | Cold start vs set batter. |
| **mi**     | 0 | 0 | 0 | 0 | 1 | Big‑match bias. |

*(Positive $\alpha_4$ means pressure eases once you’re “in”; negative means it ramps up.)*

---

---

## Turning Events Into a Score – the Original NCBPI

I kept the recipe ultra‑simple—three ingredients every scorecard already shows:

| Symbol | What it measures | Intuition |
|--------|------------------|-----------|
| $\text{RPB}_{ij}$ | Runs‑per‑ball for ball $j$ faced by player $i$ | Raw scoring speed |
| $\text{BoundaryRate}_{ij}$ | 1 if ball $j$ went for 4 or 6, else 0 | Explosive shot bonus |
| $\text{DismissalRate}_{ij}$ | 1 if the striker was dismissed on ball $j$ | Pain penalty |

With weight vector $\mathbf{w}=(w_1,w_2,w_3)$ the per‑ball performance is

$$
\bigl(w_1\,\text{RPB}_{ij}
      + w_2\,\text{BoundaryRate}_{ij}
      - w_3\,\text{DismissalRate}_{ij}\bigr)
\;P_{ij},
$$

where $P_{ij}$ is the pressure weight we just defined.

Putting them together you get the **No‑Confidence Batting Pressure Index**:

$$
\text{NCBPI}_i \;=\;
\frac{1}{N_i}
\sum_{j=1}^{N_i}
\Bigl(
  w_1\,\text{RPB}_{ij}
  + w_2\,\text{BoundaryRate}_{ij}
  - w_3\,\text{DismissalRate}_{ij}
\Bigr)\,
P_{ij}
$$


* $N_i$ = balls faced by player $i$  
* Default weights: $w_1=1$, $w_2=4$, $w_3=10$  
  *A four ≈ four singles; getting out ≈ ten singles of damage.*

> **Read‑out:** “How many weighted runs per ball did you add once we scale every event by the fear‑factor of the moment?”

Later we wrap this with Bayesian shrinkage to get **CWBPI**, so a two‑ball cameo doesn’t outrank AB de Villiers’ decade of carnage—but the core engine is the formula above.


# Case Study – 2024 IPL Leaderboards
![Leaderboard (rrr flavour)](/assets/img/leaderboard_rrr.png)

## Observations 🎯

* **RRR only** — Jos Buttler, Nicholas Pooran, Andre Russell: pure chasers.  
* **Wickets only** — MS Dhoni & Kieron Pollard are your goats for IPL finishers
* **IBF only** — Sunil Narine crashes the top 5: ten‑ball chaos is his love language.  
* **MI heavy** — Big‑match knights (Watson 117*, Bisla 89*, Sudharsan 96) finally get algorithmic respect.

### Does the eye‑test match?

Ask any neutral for “greatest clutch knocks” and you’ll hear Watson 117*, Pollard 60*(32), Russell 48*(13).  
Every one of those names lights up at least one BPHI preset.  
When math and folklore shake hands, you know you’re onto something.

---

# How Good Is This Metric?



* **Explainability** — CWBPI boils down to

  > “(runs + boundary bonus – dismissals × 10) per pressure‑weighted ball.”  

  Even granny gets it by the second ad break.

---

## <a id="bphi-leaderboards"></a>BPHI Leaderboards: Top 10 by Each Alpha

Below are the top 10 IPL batters for each pressure weighting (alpha). The BPHI (CWBPI) column is the final, shrunk pressure index for each player.

### High Run Rate Pressure
<table style="width:100%; border-collapse: collapse; margin: 20px 0; background: #23272f; border-radius: 8px; overflow: hidden;">
<thead>
<tr style="background: #374151;">
<th style="padding: 12px; text-align: left; color: #fbbf24; border-bottom: 2px solid #4b5563;">Rank</th>
<th style="padding: 12px; text-align: left; color: #fbbf24; border-bottom: 2px solid #4b5563;">Player</th>
<th style="padding: 12px; text-align: left; color: #fbbf24; border-bottom: 2px solid #4b5563;">BPHI (CWBPI)</th>
</tr>
</thead>
<tbody>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">1</td><td style="padding: 12px; color: #f9fafb;">JC Buttler</td><td style="padding: 12px; color: #f9fafb;">1.214</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">2</td><td style="padding: 12px; color: #f9fafb;">N Pooran</td><td style="padding: 12px; color: #f9fafb;">1.212</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">3</td><td style="padding: 12px; color: #f9fafb;">AD Russell</td><td style="padding: 12px; color: #f9fafb;">1.205</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">4</td><td style="padding: 12px; color: #f9fafb;">SA Yadav</td><td style="padding: 12px; color: #f9fafb;">1.199</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">5</td><td style="padding: 12px; color: #f9fafb;">AB de Villiers</td><td style="padding: 12px; color: #f9fafb;">1.177</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">6</td><td style="padding: 12px; color: #f9fafb;">KL Rahul</td><td style="padding: 12px; color: #f9fafb;">1.173</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">7</td><td style="padding: 12px; color: #f9fafb;">YBK Jaiswal</td><td style="padding: 12px; color: #f9fafb;">1.166</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">8</td><td style="padding: 12px; color: #f9fafb;">Shubman Gill</td><td style="padding: 12px; color: #f9fafb;">1.165</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">9</td><td style="padding: 12px; color: #f9fafb;">H Klaasen</td><td style="padding: 12px; color: #f9fafb;">1.159</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">10</td><td style="padding: 12px; color: #f9fafb;">Abhishek Sharma</td><td style="padding: 12px; color: #f9fafb;">1.158</td></tr>
</tbody>
</table>

### Many Wickets Lost Pressure
<table style="width:100%; border-collapse: collapse; margin: 20px 0; background: #23272f; border-radius: 8px; overflow: hidden;">
<thead>
<tr style="background: #374151;">
<th style="padding: 12px; text-align: left; color: #fbbf24; border-bottom: 2px solid #4b5563;">Rank</th>
<th style="padding: 12px; text-align: left; color: #fbbf24; border-bottom: 2px solid #4b5563;">Player</th>
<th style="padding: 12px; text-align: left; color: #fbbf24; border-bottom: 2px solid #4b5563;">BPHI (CWBPI)</th>
</tr>
</thead>
<tbody>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">1</td><td style="padding: 12px; color: #f9fafb;">AD Russell</td><td style="padding: 12px; color: #f9fafb;">0.975</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">2</td><td style="padding: 12px; color: #f9fafb;">AB de Villiers</td><td style="padding: 12px; color: #f9fafb;">0.950</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">3</td><td style="padding: 12px; color: #f9fafb;">KA Pollard</td><td style="padding: 12px; color: #f9fafb;">0.924</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">4</td><td style="padding: 12px; color: #f9fafb;">SA Yadav</td><td style="padding: 12px; color: #f9fafb;">0.923</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">5</td><td style="padding: 12px; color: #f9fafb;">MS Dhoni</td><td style="padding: 12px; color: #f9fafb;">0.920</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">6</td><td style="padding: 12px; color: #f9fafb;">N Pooran</td><td style="padding: 12px; color: #f9fafb;">0.917</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">7</td><td style="padding: 12px; color: #f9fafb;">RR Pant</td><td style="padding: 12px; color: #f9fafb;">0.905</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">8</td><td style="padding: 12px; color: #f9fafb;">KD Karthik</td><td style="padding: 12px; color: #f9fafb;">0.897</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">9</td><td style="padding: 12px; color: #f9fafb;">HH Pandya</td><td style="padding: 12px; color: #f9fafb;">0.896</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">10</td><td style="padding: 12px; color: #f9fafb;">GJ Maxwell</td><td style="padding: 12px; color: #f9fafb;">0.891</td></tr>
</tbody>
</table>

### Time-Based Pressure
<table style="width:100%; border-collapse: collapse; margin: 20px 0; background: #23272f; border-radius: 8px; overflow: hidden;">
<thead>
<tr style="background: #374151;">
<th style="padding: 12px; text-align: left; color: #fbbf24; border-bottom: 2px solid #4b5563;">Rank</th>
<th style="padding: 12px; text-align: left; color: #fbbf24; border-bottom: 2px solid #4b5563;">Player</th>
<th style="padding: 12px; text-align: left; color: #fbbf24; border-bottom: 2px solid #4b5563;">BPHI (CWBPI)</th>
</tr>
</thead>
<tbody>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">1</td><td style="padding: 12px; color: #f9fafb;">AB de Villiers</td><td style="padding: 12px; color: #f9fafb;">0.398</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">2</td><td style="padding: 12px; color: #f9fafb;">MS Dhoni</td><td style="padding: 12px; color: #f9fafb;">0.378</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">3</td><td style="padding: 12px; color: #f9fafb;">AD Russell</td><td style="padding: 12px; color: #f9fafb;">0.372</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">4</td><td style="padding: 12px; color: #f9fafb;">SA Yadav</td><td style="padding: 12px; color: #f9fafb;">0.369</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">5</td><td style="padding: 12px; color: #f9fafb;">CH Gayle</td><td style="padding: 12px; color: #f9fafb;">0.358</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">6</td><td style="padding: 12px; color: #f9fafb;">RR Pant</td><td style="padding: 12px; color: #f9fafb;">0.358</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">7</td><td style="padding: 12px; color: #f9fafb;">V Kohli</td><td style="padding: 12px; color: #f9fafb;">0.357</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">8</td><td style="padding: 12px; color: #f9fafb;">KA Pollard</td><td style="padding: 12px; color: #f9fafb;">0.355</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">9</td><td style="padding: 12px; color: #f9fafb;">JC Buttler</td><td style="padding: 12px; color: #f9fafb;">0.354</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">10</td><td style="padding: 12px; color: #f9fafb;">DA Warner</td><td style="padding: 12px; color: #f9fafb;">0.354</td></tr>
</tbody>
</table>

### No Time To Settle Pressure
<table style="width:100%; border-collapse: collapse; margin: 20px 0; background: #23272f; border-radius: 8px; overflow: hidden;">
<thead>
<tr style="background: #374151;">
<th style="padding: 12px; text-align: left; color: #fbbf24; border-bottom: 2px solid #4b5563;">Rank</th>
<th style="padding: 12px; text-align: left; color: #fbbf24; border-bottom: 2px solid #4b5563;">Player</th>
<th style="padding: 12px; text-align: left; color: #fbbf24; border-bottom: 2px solid #4b5563;">BPHI (CWBPI)</th>
</tr>
</thead>
<tbody>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">1</td><td style="padding: 12px; color: #f9fafb;">AD Russell</td><td style="padding: 12px; color: #f9fafb;">0.402</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">2</td><td style="padding: 12px; color: #f9fafb;">SA Yadav</td><td style="padding: 12px; color: #f9fafb;">0.393</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">3</td><td style="padding: 12px; color: #f9fafb;">CH Gayle</td><td style="padding: 12px; color: #f9fafb;">0.390</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">4</td><td style="padding: 12px; color: #f9fafb;">V Sehwag</td><td style="padding: 12px; color: #f9fafb;">0.389</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">5</td><td style="padding: 12px; color: #f9fafb;">AB de Villiers</td><td style="padding: 12px; color: #f9fafb;">0.387</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">6</td><td style="padding: 12px; color: #f9fafb;">N Pooran</td><td style="padding: 12px; color: #f9fafb;">0.387</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">7</td><td style="padding: 12px; color: #f9fafb;">JC Buttler</td><td style="padding: 12px; color: #f9fafb;">0.385</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">8</td><td style="padding: 12px; color: #f9fafb;">SP Narine</td><td style="padding: 12px; color: #f9fafb;">0.384</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">9</td><td style="padding: 12px; color: #f9fafb;">GJ Maxwell</td><td style="padding: 12px; color: #f9fafb;">0.382</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">10</td><td style="padding: 12px; color: #f9fafb;">YBK Jaiswal</td><td style="padding: 12px; color: #f9fafb;">0.379</td></tr>
</tbody>
</table>

### Important Match Pressure
<table style="width:100%; border-collapse: collapse; margin: 20px 0; background: #23272f; border-radius: 8px; overflow: hidden;">
<thead>
<tr style="background: #374151;">
<th style="padding: 12px; text-align: left; color: #fbbf24; border-bottom: 2px solid #4b5563;">Rank</th>
<th style="padding: 12px; text-align: left; color: #fbbf24; border-bottom: 2px solid #4b5563;">Player</th>
<th style="padding: 12px; text-align: left; color: #fbbf24; border-bottom: 2px solid #4b5563;">BPHI (CWBPI)</th>
</tr>
</thead>
<tbody>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">1</td><td style="padding: 12px; color: #f9fafb;">KA Pollard</td><td style="padding: 12px; color: #f9fafb;">1.051</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">2</td><td style="padding: 12px; color: #f9fafb;">SR Watson</td><td style="padding: 12px; color: #f9fafb;">1.030</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">3</td><td style="padding: 12px; color: #f9fafb;">B Sai Sudharsan</td><td style="padding: 12px; color: #f9fafb;">0.975</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">4</td><td style="padding: 12px; color: #f9fafb;">WP Saha</td><td style="padding: 12px; color: #f9fafb;">0.974</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">5</td><td style="padding: 12px; color: #f9fafb;">MS Bisla</td><td style="padding: 12px; color: #f9fafb;">0.972</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">6</td><td style="padding: 12px; color: #f9fafb;">M Vijay</td><td style="padding: 12px; color: #f9fafb;">0.961</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">7</td><td style="padding: 12px; color: #f9fafb;">SK Raina</td><td style="padding: 12px; color: #f9fafb;">0.959</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">8</td><td style="padding: 12px; color: #f9fafb;">VR Iyer</td><td style="padding: 12px; color: #f9fafb;">0.945</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">9</td><td style="padding: 12px; color: #f9fafb;">YK Pathan</td><td style="padding: 12px; color: #f9fafb;">0.945</td></tr>
<tr style="border-bottom: 1px solid #374151;"><td style="padding: 12px; color: #f9fafb;">10</td><td style="padding: 12px; color: #f9fafb;">Shashank Singh</td><td style="padding: 12px; color: #f9fafb;">0.942</td></tr>
</tbody>
</table>

*Scroll down for interpretation and discussion.*

---

# Feedback & Freebies

Spotted a bug? Want a custom dashboard for your club?  
Drop me an email.  
If BPHI wins you a Dream11 mini‑grand, I accept chai, samosas, or a lifetime supply of bat tape.

*Happy number‑crunching, and may your pressure never spike!*
