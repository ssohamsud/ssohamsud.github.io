---
title: "Mean-Field Market Making"
date: 2025-08-01 12:00:00 +0000
author: Soham Sud
categories: [quantitative-finance, research]
tags: [market making, mean field games, stochastic control, HJB, Poisson processes, quantitative finance]
math: true
pinned: false
---


I was reading Terry Tao's blog and a post on mean-field equations sparked an idea: could we turn mean-field games into a practical market-making model? This article builds that model from first principles, step by step, and keeps a running analogy with a giant **apple fair** (you are a stall-holder, the market is the fair). All formulas are presented in a MathJax-friendly way so they render reliably.

---

## What we’re building

- A **market maker** posts a **bid** (to buy) and **ask** (to sell) around the mid price.
- **Tight** quotes bring more trades but build **inventory**; **wide** quotes slow trades and reduce risk.
- Many similar makers interact through the **average behavior** of the crowd (the *mean field*).

Over a trading day  $[0,T]$, our solver computes:

1. a **value function**  $V(t,x)$  — the best expected objective you can still achieve from time  $t$  with inventory  $x$ ;
2. **optimal quotes**  $\delta^{b*}(t,x)$  and  $\delta^{a*}(t,x)$ ;
3. the evolving **population distribution**  $\rho(t,x)$  of inventories;
4. **actionable metrics** such as average bid/ask, full spread, inventory variance, de-risk probability, and marginal cost.

The interactive dashboard below renders the same objects the solver computes.

---

## Interactive Demo

<div style="text-align: center; margin: 30px 0;">
  <iframe src="/assets/demos/mfg-market-making/index.html"
          width="100%"
          height="800"
          style="border:1px solid #ddd; border-radius:8px; box-shadow:0 4px 8px rgba(0,0,0,0.1);">
  </iframe>
  <p style="margin-top:10px; color:#666; font-size:14px;">
    Interactive Mean-Field Game Market-Making Dashboard
  </p>
</div>

---

## Micro model: quotes, fills, and inventory

Imagine the apple fair. A big board shows the “mid” price. You set two tags relative to that mid:
- **ask markup** $\delta^a_t \ge 0$ (how far above mid you sell one unit),
- **bid markdown** $\delta^b_t \ge 0$ (how far below mid you buy one unit).

Tighter tags (smaller $\delta$ ) attract customers; wider tags discourage them.

We model fills at your quotes with **Poisson processes**. The intensity (rate) falls off exponentially with your distance from mid. This is standard, simple, and empirically reasonable:

$$

\lambda^a(\delta) = A\,e^{-k\delta}, 
\qquad 
\lambda^b(\delta) = A\,e^{-k\delta},
\qquad 
A>0,\ k>0.
$$


- $\lambda$ is a **rate**: expected fills per unit time.
- If you tighten by $\Delta\delta>0$, the relative change in rate is:

$$

\frac{\lambda(\delta-\Delta\delta)}{\lambda(\delta)} = e^{k\,\Delta\delta}.
$$


Inventory $X_t$ changes by single-unit jumps:
- ask fill (you sell) makes $X\to X-1$,
- bid fill (you buy) makes $X\to X+1$.

We use **edge accounting** relative to mid: a sell at $\mathrm{mid}+\delta^a$ earns edge $\delta^a$; a buy at $\mathrm{mid}-\delta^b$ also earns edge $\delta^b$. Mid cancels from the accounting.

---

## Objective: edge vs. inventory risk

During the day you accumulate **edge** when fills occur, pay a **running** cost for carrying inventory, and care about ending flat. For a quoting policy $\pi$:

$$

J^\pi(t,x)
=
\mathbb{E}_{t,x}^{\pi}\!\left[
\int_{t}^{T}
\big(
\delta^a_s\,dN^a_s + \delta^b_s\,dN^b_s - \eta\,X_s^2\,ds
\big)
\;-\;
\gamma\,X_T^2
\right],
$$


- $dN^a_s$ and $dN^b_s$ are Poisson increments,
- $\eta>0$ penalizes **intra-day** inventory,
- $\gamma>0$ penalizes **terminal** inventory (overnight risk, closing dumps, etc.).

The **value function** is the best you can do from $(t,x)$:

$$

V(t,x) = \sup_{\pi} J^\pi(t,x).
$$


Intuition: “From now until close, what’s the maximum expected ‘edge minus risk’ I can still achieve if I currently hold $x$ baskets?”

---

## From dynamic programming to the HJB

Look ahead a short time $h>0$. Three outcomes can happen over $[t,t+h]$:
- **ask fill** with probability $\lambda^a(\delta^a_t)h + o(h)$: reward $\delta^a_t$, inventory $x\to x-1$;
- **bid fill** with probability $\lambda^b(\delta^b_t)h + o(h)$: reward $\delta^b_t$, inventory $x\to x+1$;
- **no fill** with probability $1-(\lambda^a+\lambda^b)h + o(h)$.

Running cost over that interval is approximately $-\eta\,x^2 h$. The **Dynamic Programming Principle** gives:

$$

V(t,x) =
\sup_{\delta^a,\delta^b}
\mathbb{E}\!\left[
\int_t^{t+h} \big(\delta^a_s\,dN^a_s + \delta^b_s\,dN^b_s - \eta\,x^2\,ds\big)
+ V(t+h, X_{t+h})
\right]
+ o(h).
$$


Expand $V(t+h,\cdot) = V(t,\cdot) + \partial_t V(t,\cdot)\,h + o(h)$. Take expectations, subtract $V(t,x)$, divide by $h$, and let $h\to 0$. The **Hamilton–Jacobi–Bellman (HJB)** equation is:

$$

-\partial_t V(t,x)
=
\sup_{\delta^a,\delta^b}
\Big\{
\lambda^a(\delta^a)\,[\delta^a + V(t,x-1) - V(t,x)]
+
\lambda^b(\delta^b)\,[\delta^b + V(t,x+1) - V(t,x)]
-
\eta\,x^2
\Big\},
$$

$$

V(T,x) = -\gamma\,x^2.
$$


Read each bracket as “instant edge plus change in future value, weighted by how likely that fill is right now.”

---

## Optimal quotes in closed form

Inside the HJB, each side can be optimized separately. For the ask side, let $\Delta V^a(t,x)=V(t,x-1)-V(t,x)$. We maximize:

$$

f(\delta) = \lambda(\delta)\,[\Delta V^a + \delta] = A e^{-k\delta}(\Delta V^a + \delta).
$$


Differentiate and set to $0$:

$$

f'(\delta) = A e^{-k\delta}\,[-k(\Delta V^a+\delta) + 1] = 0
\quad\Rightarrow\quad
\delta^{a*}(t,x) = \frac{1}{k} - \big[V(t,x-1) - V(t,x)\big].
$$


Bid is analogous:

$$

\delta^{b*}(t,x) = \frac{1}{k} - \big[V(t,x+1) - V(t,x)\big].
$$


Intuition: if selling is urgent (you are long near close), $V(t,x-1)-V(t,x)$ is **very negative**, so $\delta^{a*}$ becomes **small** — you **tighten** to get hit. If buying back is urgent (you are short), $\delta^{b*}$ shrinks too. In code we clip $\delta$ to practical bounds.

---

## The crowd: the master equation (Fokker–Planck for jumps)

Let $\rho(t,x)$ be the fraction of makers at inventory $x$ and time $t$. Under the optimal quotes:

$$

\partial_t \rho(t,x)
=
\rho(t,x+1)\,\lambda^a(\delta^{a*}(t,x+1))
+
\rho(t,x-1)\,\lambda^b(\delta^{b*}(t,x-1))
-
\rho(t,x)\,[\lambda^a(\delta^{a*}(t,x)) + \lambda^b(\delta^{b*}(t,x))].
$$


This is a **mass balance**: inflow from neighbors that trade into $x$ minus outflow from $x$ that trade away. Probabilities sum to one at each time: $\sum_x \rho(t,x)=1$. The **de-risk probability** used in the charts is $P(|X_t|<\varepsilon) = \sum_{|x|<\varepsilon} \rho(t,x)$.

---

## Mean-field coupling: competition

Customers route to tighter quotes. A simple, tractable way to encode competition is to make intensities depend on **relative** tightness:

$$

\lambda^a(\delta^a;\,\bar\delta^a_t) = A\,e^{-k(\delta^a-\bar\delta^a_t)},
\qquad
\bar\delta^a_t = \sum_x \delta^{a*}(t,x)\,\rho(t,x),
$$

$$

\lambda^b(\delta^b;\,\bar\delta^b_t) = A\,e^{-k(\delta^b-\bar\delta^b_t)},
\qquad
\bar\delta^b_t = \sum_x \delta^{b*}(t,x)\,\rho(t,x).
$$


Algorithmically we solve a **fixed point**:
1. Guess $\bar\delta^a(t)$ and $\bar\delta^b(t)$.
2. Solve HJB backward to get $V$ and $\delta^{*}$.
3. Solve the master equation forward to update $\rho$ and recompute $\bar\delta$.
4. Repeat 2–3 until $\bar\delta$ stabilizes.

---

## Discretization (how the code mirrors the math)

We use a grid: times $t_n=n\Delta t$ and inventories $x_i\in\{-X_{\max},\dots,X_{\max}\}$.

**Backward (HJB)** — start from $V(T,x)=-\gamma x^2$. For each step $t_{n+1}\to t_n$, compute $\delta^{*}$ from the closed forms (with finite differences for $V(t,x\pm1)-V(t,x)$), then update $V$ with a stable backward step.

 **Forward (master equation)** 

$$

\rho_{n+1,i} = \rho_{n,i}
+ \Delta t\left(
\rho_{n,i+1}\lambda^a_{n,i+1}
+ \rho_{n,i-1}\lambda^b_{n,i-1}
- \rho_{n,i}(\lambda^a_{n,i}+\lambda^b_{n,i})
\right),
$$


ignoring terms that would step outside the inventory grid (reflective/absorbing behavior as chosen). This conserves probability up to numerical error.

 **Metrics** 

$$

\bar\delta^{b}(t_n) = \sum_i \delta^{b*}_{n,i}\,\rho_{n,i},
\qquad
\bar\delta^{a}(t_n) = \sum_i \delta^{a*}_{n,i}\,\rho_{n,i},
$$

$$

\text{FullSpread}(t_n) = \bar\delta^{a}(t_n) - \bar\delta^{b}(t_n),
\qquad
\text{Var}[X](t_n) = \sum_i (x_i - \bar x_n)^2\,\rho_{n,i},
$$

$$

P(|X_{t_n}|<\varepsilon) = \sum_{|x_i|<\varepsilon} \rho_{n,i},
\qquad
\partial_x V(t_n,0) \approx \frac{V(t_n,\Delta x) - V(t_n,-\Delta x)}{2\,\Delta x}.
$$


---

## Interpreting the dashboard (through the fair)

- **Value surface** $V(t,x)$: deepest near $x=0$ (safe when flat); rises as $|x|$ grows; steepens near $T$ (less time to fix mistakes).
- **Control surface** $\delta^{*}(t,x)$: small (tight) where trading is urgent (e.g., long inventory near close); larger (wide) when you want to slow down.
- **Distribution** $\rho(t,x)$: the ridge of the crowd funneling toward zero inventory over time—some finish early, others lag.
- **Spreads over time**: average bid and ask and their difference (full spread) reveal competition’s compression during the session.
- **Variance**: shows where crowding peaks (often mid-session).
- **De-risk probability**: fraction of makers inside $|x|<\varepsilon$; if it misses a policy target by $T$, increase $\eta$ or $\gamma$.
- **Marginal cost** $\partial_x V(t,0)$: a shadow price for one more unit at flat inventory—useful as a real-time risk budget.

---

## Choosing and calibrating $A$ and $k$ 

- **Data route:** Poisson regression of fill counts on distance from mid (plus covariates): $\log \lambda = \log A - k\delta + \text{controls}$ .
- **Operational route:** back-solve from desk targets (e.g., “one tick tighter doubles flow” gives $k=\ln 2$; “at mid we expect 20 fills/min” gives $A=20/\text{min}$ ).
- **Toy route:** pick $k\in[0.3,1.0]$ per tick and choose $A$ so $\lambda\Delta t$ is modest around typical $\delta$ (keeps the chain active but stable).

---

## Sanity checks

- **Symmetry ⇒ net spread zero:** with symmetric parameters and centered $\rho_0$, the average of bid and ask cancels. Plot **full spread** $\bar\delta^a-\bar\delta^b$ .
- **De-risk time:** use a Gaussian $\rho_0$ and a meaningful $\varepsilon$ (e.g., $0.5$ ). Larger $\eta,\gamma$ push $P(|X_t|<\varepsilon)$ higher sooner.
- **Monotone value slices:** for fixed $t$, expect $V(t,0)\ge V(t,\pm1)\ge V(t,\pm2)\cdots$ .
- **Probability conservation:** check $\sum_i \rho_{n,i}\approx 1$ and no edge mass accumulation.

---

## Key formulas at a glance



 **Poisson intensities** 

$$

\lambda(\delta)=A e^{-k\delta},
\qquad
\lambda(\delta-\Delta\delta) = e^{k\Delta\delta}\,\lambda(\delta).
$$


 **Objective** 

$$

J^\pi(t,x)
=
\mathbb{E}_{t,x}^{\pi}\!\left[
\int_t^{T} \big(\delta^a\,dN^a + \delta^b\,dN^b - \eta\,X^2\,dt\big)
- \gamma\,X_T^2
\right].
$$


 **Value function** 

$$

V(t,x)=\sup_{\pi} J^\pi(t,x).
$$


 **HJB** 

$$

-\partial_t V
=
\sup_{\delta^a,\delta^b}
\left\{
\lambda^a(\delta^a)\,[\delta^a+V(t,x-1)-V(t,x)]
+
\lambda^b(\delta^b)\,[\delta^b+V(t,x+1)-V(t,x)]
-
\eta x^2
\right\},
\qquad
V(T,x)=-\gamma x^2.
$$


 **Optimal quotes** 

$$

\delta^{a*}=\frac{1}{k}-[V(t,x-1)-V(t,x)],
\qquad
\delta^{b*}=\frac{1}{k}-[V(t,x+1)-V(t,x)].
$$


 **Population (master equation)** 

$$

\partial_t \rho
=
\rho(t,x+1)\lambda^a(\delta^{a*}(t,x+1))
+
\rho(t,x-1)\lambda^b(\delta^{b*}(t,x-1))
-
\rho(t,x)\big(\lambda^a(\delta^{a*}(t,x))+\lambda^b(\delta^{b*}(t,x))\big).
$$


 **Mean field** 

$$

\bar\delta^a_t=\sum_x \delta^{a*}(t,x)\rho(t,x),
\qquad
\bar\delta^b_t=\sum_x \delta^{b*}(t,x)\rho(t,x).
$$



---

## Closing picture

The fair opens. You choose price tags (quotes). Tight tags attract buyers but may pile up baskets; wide tags keep you safe but slow you down. The crowd’s average behavior feeds back into your fortunes: if everyone tightens, bargains abound and your edge shrinks; if the crowd widens, your caution looks wise. The mathematics—Poisson intensities, a clean HJB with closed-form optimal quotes, and a forward equation for the population—turns this story into a solvable loop that yields policy maps and risk-aware schedules a desk can actually use.

Explore the demo, play with the knobs, and watch the fair breathe. The pictures you see—value bowls, control dials, and the crowd’s ridge drifting to zero—are just these equations, painted in color.
