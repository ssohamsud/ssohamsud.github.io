---
title: "Modelling Stochastic Volatility with Statistical Physics"
date: 2024-03-30 12:00:00 +0000
author: Soham Sud
categories: [cool-stuff, research]
tags: [stochastic-processes, volatility, finance, physics, math]
math: true
---

![Research Screenshot](/Screenshot%202025-07-20%20at%2013.04.08.png)

[Download the full research paper (PDF)](/1743167385141%20(1).pdf)

## 1. Model Formulations

**Intuition.** We want volatility ($V_t$) to behave like a spring—always pulling back toward its average—but with a “mood” that changes over time. Some days it jiggles wildly, other days it barely moves.

### 1.1 Ornstein–Uhlenbeck with Diffusing Diffusivity

I start from a classic OU variance process, but let its instantaneous volatility of volatility $\xi_t$ itself follow a stochastic diffusion:

$$
\begin{aligned}
dV_t &= \kappa\bigl(\theta - V_t\bigr)\,dt + \xi_t\,dW_t^{(1)},\\
d\xi_t &= \gamma\bigl(\mu_\xi - \xi_t\bigr)\,dt + \sigma_\xi\,dW_t^{(2)},
\end{aligned}
$$

where:
- $V_t$ is the current variance, pulled back toward $\theta$ at speed $\kappa$.
- $\xi_t$ controls how “jumpy” $V_t$ is, itself reverting to $\mu_\xi$ at speed $\gamma$.
- $W^{(1)}$ and $W^{(2)}$ are independent Brownian motions, introducing randomness.

**Intuition.**  This two-layer system means volatility can switch between calm (small $\xi_t$) and stormy (large $\xi_t$) phases, much like weather patterns.

### 1.2 Lévy‑Stable Noise Extension

To capture abrupt spikes that Gaussians miss, we replace the Brownian shock in $V_t$ with an $\alpha$‑stable Lévy process:

$$
\begin{aligned}
dV_t &= \kappa\bigl(\theta - V_t\bigr)\,dt + \xi_t\,dL_t^{\alpha},\\
d\xi_t &= \gamma\bigl(\mu_\xi - \xi_t\bigr)\,dt + \sigma_\xi\,dW_t^{(2)},
\end{aligned}
$$

and

$$
\mathbb{E}\bigl[e^{iuL_t^\alpha}\bigr]
= \exp\Bigl\{-t\,c\,|u|^\alpha\bigl(1 - i\beta\,\mathrm{sgn}(u)\tan(\tfrac{\pi\alpha}{2})\bigr)\Bigr\}.
$$

**Intuition.** Lévy noise lets variance take occasional huge leaps—think of sudden earthquakes—while Gaussian noise only whispers small tremors.

## 2. From Continuous SDE to Discrete Likelihood

**Intuition.** We observe daily VIX data, so we need the probability of moving from today’s variance $V_t$ to tomorrow’s $V_{t+1}$ under each model.

### 2.1 OU–Gaussian

Over one day ($\Delta t=1$):

$$
V_{t+1} = V_t e^{-\kappa} + \theta(1 - e^{-\kappa})
+ \xi_t\sqrt{\frac{1 - e^{-2\kappa}}{2\kappa}}\,Z_t,\quad Z_t\sim N(0,1).
$$

Thus the conditional density is

$$
f(V_{t+1}\mid V_t,\xi_t)
= \frac{1}{\sqrt{2\pi\sigma_V^2}}
\exp\biggl(-\frac{(V_{t+1}-m_V)^2}{2\sigma_V^2}\biggr),
$$

with

$$
m_V = V_t e^{-\kappa} + \theta(1 - e^{-\kappa}),\quad
\sigma_V^2 = \xi_t^2\frac{1-e^{-2\kappa}}{2\kappa}.
$$

**Intuition.** We get a familiar bell curve for tomorrow’s variance: centered at $m_V$, with width determined by $\xi_t$ and $\kappa$.

### 2.2 Lévy–Stable

No closed‑form density exists, so we work with the characteristic function and invert it numerically:

$$
\varphi_{\Delta V}(u)
= \exp\bigl\{-|u\,\xi_t|^\alpha\,C(\alpha,\kappa)\bigr\},
\quad
C(\alpha,\kappa)=\int_0^1 e^{-\kappa(1-s)\alpha}\,ds.
$$

**Intuition.** Rather than a neat bell, the distribution now has fat tails—meaning a small but real chance of giant jumps.

## 3. Parameter Estimation

### 3.1 Maximum Likelihood

We find parameters by maximizing the log‑likelihood over $N$ observations:

$$
\ell(\Theta)=\sum_{t=1}^N\log f(V_{t+1}\mid V_t;\Theta).
$$

SciPy’s BFGS optimizer efficiently searches for $\Theta=(\kappa,\theta,\gamma,\mu_\xi,\sigma_\xi)$.

**Intuition.** We tweak our model knobs until the predicted probabilities best match the real data we’ve seen.

### 3.2 Fitting the $\alpha$-Stable Law

We fit $\alpha$ and scale $c$ via SciPy’s `levy_stable.fit` on increments $\Delta V_t$, but first seed $\alpha$ using the Hill estimator:

$$
\widehat\alpha
= \Bigl(\tfrac{1}{k}\sum_{i=1}^k\log\tfrac{X_{(i)}}{X_{(k+1)}}\Bigr)^{-1}.
$$

**Intuition.** The Hill estimator gives a quick, rough sense of tail heaviness, which then guides the more delicate MLE fit.

## 4. Forecasting & Rolling Windows

Each day $t$ from 2018 to 2024:

1. Re‑estimate parameters $\hat\Theta_t$ on the past 252 days.
2. Forecast $\hat V_t = \mathbb{E}[V_t\mid\mathcal{F}_{t-1};\hat\Theta_t]$.
3. Simulate 10,000 paths to study distributional outcomes.

**Intuition.** This “moving window” mimics a real forecaster who uses only past data and updates every day.

## 5. Statistical & Risk Metrics

### 5.1 Point Accuracy

$$
\mathrm{RMSE} = \sqrt{\tfrac1T\sum(V_t-\hat V_t)^2},\quad
\mathrm{MAE} = \tfrac1T\sum|V_t-\hat V_t|.
$$

**Intuition.** RMSE penalizes big misses heavily, while MAE treats all errors equally.

### 5.2 Distributional Fit

$$
D_{\mathrm{KL}}(p\|q)
= \int p(x)\log\frac{p(x)}{q(x)}\,dx,
$$

plus Q–Q plots comparing empirical vs. simulated quantiles.

**Intuition.** KL divergence measures how “surprised” the true data would be if we used our model distribution.

### 5.3 Tail Risk

$$
\mathrm{ES}_{0.95}
= \frac{1}{0.05}\int_{0.95}^1 q_p\,dp,
$$

and the Hill estimator to verify $\alpha$.

**Intuition.** Expected Shortfall looks at the worst 5% of outcomes—what happens when things go really wrong?

## 6. Mathematical Insights & Trade‑Offs

**Key Takeaways.**
- **Gaussian OU/Heston**: Quick to fit, excel at average forecasts (RMSE≈2.4).
- **Lévy‑Stable**: Computationally heavier but nail fat tails ($\alpha\approx1.3$, KL↓75%, ES≈14.8%).
- **Hill Seeding**: Using a semi‑parametric tail estimate speeds up and stabilizes the full MLE for $\alpha$.

**Overall Intuition.** We trade a bit of everyday accuracy for a model that truly understands rare, extreme moves—critical for any serious risk manager. 