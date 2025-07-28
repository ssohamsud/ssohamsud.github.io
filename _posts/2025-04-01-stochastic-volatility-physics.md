---
title: "Evaluating Diffusing Diffusivity Models in Quantitative Finance - A non-technical summary"
date: 2025-04-01 12:00:00 +0000
author: Soham Sud
categories: [research, physics, stochastic-processes]
tags: [stochastic volatility, physics, research, modeling]
math: false
pinned: true
---

**[Download the full research paper (PDF)](/Stochastic%20Volatility%20Physics.pdf)**

## 1. What does it mean?

Imagine that price moves as you driving. **Volatility** = tyre grip. Most days: dry road; now and then: black ice. Old models say grip never changes. **Stochastic vol** says the road surface changes texture at random. Our physics trick ("diffusing diffusivity") lets the road's conditions change randomly.

## 2. What I tried

* I ran two road‑texture models on VIX:

  * **OU** – dry, predictable tarmac that regains grip quickly after small bumps.
  * **Lévy** – same road but with hidden black‑ice patches that send the car skidding now and then.
* Compared to the usual classics (Heston, Hull‑White) using rolling forecasts.

## 3. What popped out

* **OU**: great for everyday bumps.
* **Lévy**: best for the crazy spikes.
* Heston's OK; Hull‑White did not perform as well as others.

## 4. Use cases

* **Routine hedging:** OU / Heston.
* **Panic planning:** Lévy.

## 5. What I plan to do next 

Flip the models into option‑pricing (risk‑neutral) land.

* Test on more than VIX such as SPX, variance swaps, maybe crypto.

**TL;DR** Volatility is slippery. Choose your model based on how much black ice you expect.

---

*For further details, see the attached PDF or contact the author for discussion.*

{% include social-sharing.html %} 