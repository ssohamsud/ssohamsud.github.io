---
title: "Mean-Field Market Making"
date: 2025-08-01 12:00:00 +0000
author: Soham Sud
categories: [quantitative-finance, research]
tags: [market making, mean field games, stochastic control, HJB, Poisson processes, quantitative finance]
math: true
pinned: false
---

## Introduction
I was reading Terry Tao's blog the recently and stumbled across this pretty cool field of maths: Mean Field Games. It's like if game theory and stochastic differential equations had a kid and its fairly new as well. He has some amazing analogies at the start of his blog which I hope he wont mind me copying (he will never see this blog page) to better explain the ideas of Mean Field Games to sort of set the scene for you.

## Some intuition on Mean Field Games
You've probably been to some sort of stadium in your life (if you haven't then Ctrl+W right now and go book tickets for some random sporting fixture). Mexican Waves are one of those things that most people want to do but a lot of just don't have the guts to start off that chain in the event of horrific failure and consequential embarrasment. At some point in our life, we have all pondered - "how do we determine our *effort* and *response* to the Mexican Wave headed our way." 

Well, ignoring pyschologists and biologists exist, it definitely makes sense to turn to the mathematician and leave it to them to turn a person into a function with parameters that satisfy some (stochastic) differential equation. What this might leave us with is a scenario in which this effort and response to the wave is governed by some differential equations over a "Mean" (hopefully its clicking now), where the mean is a large number of other "players" of this game. What we ask ourselves is: based on our own functions and parameters such as confidence or response of nearby agents, what is our best response in this game?

---
## From Mexican Waves to Market Making
Market Making is something which I was first exposed to at a spring week I did at Optiver. It took some time to get my head around during that spring week but I managed to understand the general idea behind it after being shouted at multiple times - "YOU CANT BUY A BID!"

I'll have a go at explaining market making as best I can with setting the scene of another super common life experience, a bustling fruit market that only sells apples with lots of stalls and lots of customers. In the market place, we consider what the best "ask" of a farmer (seller) is and the best "bid" of an apple buyer is. Intuitively, if these prices lineup/overlap, trades will take place (think someone is selling something for £50 but you are willing to pay £60, you will obviously buy). However, when these prices don't line up and the spread between the two increases more and more, we face a problem called "illiquidity". Trades aren't happening in this marketplace and its neither good for the farmers nor the customers. 

What a market maker (mm) will come and do is tighten that bid-ask spread to provide liquidity to the market to encourage trading and as a result profit on the spread of their bid and ask. A quick example may help to demonstrate this:

No Market Maker Present: Best Ask: £8 a crate, Best Bid: £5 a crate. Result: Super low level of trading

Market Maker Present: MM Ask: £7 a crate, MM Bid: £6 a crate. Result: More trading, market maker profits on £1 spread.

## The Aim of the Project

### Issues with traditional systems

Many of the current models used in market making such as Avellaneda–Stoikov (I'll write a blog about this soon!) assume you are the only agent, you end up optimising your quotes based on exogenous flow (in the apple analogy this translates to "the number of shoppers walking past each minute is fixed by the weather,regardless of how stalls set prices"). In reality, markets witness more endogenous flow: "if all apple stalls cut prices, shoppers run to the cheapest vendors, changing your foot traffic."

Another issue is scalability, simulating hundreds of interacting makers as an N-player stochastic game is intractable; single-agent models can’t capture the feedback loop and N-player models don’t scale reasonably.

### Why is adding Mean Field Games so good?

Mean Field Games (MFG) treats a continuum of small players and treats everyone else with a mean field - this is the distribution of inventories and average quotes.

MFG uses two coupled equations: Hamilton-Jacobi-Bellman (HJB) and Fokker Plank (FP) to capture the loop between making new quotes based on the evolution of other makers distribution.

From a broad angle, the mechanics of the two equations are as follows:

- **HJB** (backward): each maker’s optimal quotes given the crowd.
- **Fokker–Planck** (forward): how the crowd distribution evolves when everyone follows those quotes.

Solving these equations for their fixed point gives you:

- Endogenous intensities: fill rates depend on your relative tightness to the crowd average—exactly the competitive mechanism.
- Equilibrium spreads: a time-path for the population’s bid/ask behavior (when spreads naturally compress or widen).
- Crowd inventory dynamics: when de-risking waves happen, how concentrated inventory becomes, who is likely stuck at the bell.


In a world where algorithmic tradings tumps manual trading, ignoring feedback from interacting strategies creates systematically biased expectations for trades, PnL and risk. 

The aim of this project is to implement this theory in the world of market making to hopefully create a system which provides optimal quotes based on the market's trading itself - without us actually knowing what the market is trading and is about.

Below is a quick overview of what we found - mainly for those who don't want to get their hands dirty in the maths and only in results: 

Over a trading day  $[0,T]$, our solver computes:

1. a **value function**  $V(t,x)$  — the best expected objective you can still achieve from time  $t$  with inventory  $x$ 
2. **optimal bid**  $\delta^{b*}(t,x)$
3. **optimal ask**  $\delta^{a*}(t,x)$
4. the evolving **population distribution**  $\rho(t,x)$  of inventories;
5. **actionable metrics** such as average bid/ask, full spread, inventory variance, de-risk probability, and marginal cost.

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

## Modelling the Bid-Ask Spread

Lets go back to the apple market. Imagine that a big board shows the current mid price of the apples. As a market maker, you want to set two prices relative to that mid:

- **ask markup** $\delta^a_t \ge 0$ (how far above mid you sell one unit),
- **bid markdown** $\delta^b_t \ge 0$ (how far below mid you buy one unit).

Intuitively: tighter prices (smaller $\delta$ ) attract customers; wider prices discourage them from trading.

We can model trades at our quotes with **Poisson processes**. The intensity (rate) falls off exponentially with the distance from mid-price. This is standard, simple, and empirically reasonable:

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

## Edge vs Inventory Risk
If you have apples at the end of the day, they might go bad overnight or maybe the orange market sees literal overnight success, reducing demand significantly for apples.

Similarly, market makers may try and finish close to a neutral position by market close: this is definitely not always true but this strategy certainly helps reduce risk.


During the day you accumulate an **edge** when trades occur, pay a **running** cost for carrying inventory, and care about ending flat. Assume we have some policy $\pi$ such that:

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


You can think of this as: “from now until close, what’s the maximum expected ‘edge minus risk’ I can still achieve if I currently hold $x$ crates?”

---

## From dynamic programming to the HJB


Consider a small shift in time $h>0$. Three outcomes can happen over $[t,t+h]$:
- **ask fill** with probability $\lambda^a(\delta^a_t)h + o(h)$: reward $\delta^a_t$, inventory $x\to x-1$;
- **bid fill** with probability $\lambda^b(\delta^b_t)h + o(h)$: reward $\delta^b_t$, inventory $x\to x+1$;
- **no fill** with probability $1-(\lambda^a+\lambda^b)h + o(h)$.

Running cost over that interval is approximately $-\eta\,x^2 h$. We utilise the **Dynamic Programming Principle** to give:

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


We can interpret each bracket as the “instant edge plus change in future value, weighted by how likely that trade is right now.”

---

## Optimal quotes in closed form

Inside the HJB, each side can be optimized separately. For the ask side, let $\Delta V^a(t,x)=V(t,x-1)-V(t,x)$. We maximize:

$$

f(\delta) = \lambda(\delta)\,[\Delta V^a + \delta] = A e^{-k\delta}(\Delta V^a + \delta).
$$


Differentiate and set equal to $0$:

$$

f'(\delta) = A e^{-k\delta}\,[-k(\Delta V^a+\delta) + 1] = 0
\quad\Rightarrow\quad
\delta^{a*}(t,x) = \frac{1}{k} - \big[V(t,x-1) - V(t,x)\big].
$$


Our bid is analogous:

$$

\delta^{b*}(t,x) = \frac{1}{k} - \big[V(t,x+1) - V(t,x)\big].
$$


If selling is urgent (you are long near close), $V(t,x-1)-V(t,x)$ is **very negative**, so $\delta^{a \*}$ becomes **small** — you **tighten** to get hit. If buying back is urgent (you are short), $\delta^{b \*}$ shrinks too. In our code we clipped $\delta$ to practical bounds.

$$
\delta^{a*} \;=\; \frac{1}{k}\;-\;\bigl[V(t,x-1)-V(t,x)\bigr].
$$


---

## Everyone Else: Fokker–Planck

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

This is a mass balance inflow from neighbors that trade into $x$ minus outflow from $x$ that trade away.

Probabilities sum to one at each time: $\sum_x \rho(t,x)=1$.

The de-risk probability used in the charts is:

$$

P(|X_t|<\varepsilon) = \sum_{|x|<\varepsilon} \rho(t,x)
$$ 

---

## Mean-field coupling: competition

Naturally, customers route to tighter quotes. A simple, tractable way to encode competition is to make intensities depend on relative tightness:

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

## Discretization

To solve this system we use a grid: times $t_n=n\Delta t$ and inventories $x_i\in\{-X_{\max},\dots,X_{\max}\}$.

**Backward (HJB)** — start from $V(T,x)=-\gamma x^2$. For each step $t_{n+1}\to t_n$, compute $\delta^{*}$ from the closed forms (with finite differences for $V(t,x\pm1)-V(t,x)$), then update $V$ with a stable backward step.

 **Forward (Fokker-Planck)** 

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


rises as |x| grows; steepens near T (less time to fix mistakes).
---

## Interpreting the dashboard

- **Value surface** $V(t,x)$: deepest near $x=0$ ,safe when flat, rises as $abs(x)$ grows and steepens near $T$ as there is less time fix heavy position

- **Value surface** $V(t,x)$: deepest near $x=0$ (safe when flat),

- **Control surface** $\delta^{*}(t,x)$: small (tight) where trading is urgent (e.g., long inventory near close); larger (wide) when you want to slow down.

- **Distribution** $\rho(t,x)$: the ridge of the crowd funneling toward zero inventory over time—some finish early, others lag.

- **Spreads over time**: average bid and ask and their difference (full spread) reveal competition’s compression during the session.

- **Variance**: shows where crowding peaks (often mid-session).

- **De risk probability**: fraction of makers inside $x<\varepsilon$; if it misses a policy target by $T$, increase $\eta$ or $\gamma$.

- **Marginal cost** $\partial_x V(t,0)$: a shadow price for one more unit at flat inventory—useful as a real-time risk budget.

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

## Conclusion

The fair opens. You choose prices (quotes). Tight prices attract buyers but may pile up baskets; wide prices keep you safe but slow you down. The crowd’s average behavior feeds back into your fortunes: if everyone tightens, bargains abound and your edge shrinks; if the crowd widens, your caution looks wise.

The mathematics: Poisson intensities, a clean HJB with closed-form optimal quotes, and a forward equation for the population—turns this story into a solvable loop that yields policy maps and risk-aware schedules a desk can actually use.

