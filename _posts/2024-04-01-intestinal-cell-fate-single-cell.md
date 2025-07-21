---
title: "Illuminating Intestinal Cell Fate: A Deep Dive into Single‑Cell Trajectories and Interactions"
date: 2024-04-01 12:00:00 +0000
author: Soham Sud
categories: [biology, machine-learning, statistics]
tags: [single-cell, scRNA-seq, cell-fate, machine-learning, biology, statistics, graph-theory]
math: true
---

> We present a comprehensive exploration of mouse intestinal single‑cell RNA‑seq data, focusing on three pillars of analysis: (1) graph‑based clustering that reveals discrete cell identities, (2) multi‑modal trajectory inference that reconstructs developmental continua, and (3) rich quantification of cell–cell communication (CCC) via LIANA. Throughout, we weave rigorous mathematics with intuitive exposition so that readers grasp “how the algorithms think” while seeing “what the biology means.”

**[Download the full M2R research paper (PDF)](/M2R.pdf)**

## Introduction

Single‑cell RNA sequencing (scRNA‑seq) has transformed tissue biology by offering molecular snapshots of thousands of individual cells. In the murine small intestine, stem cells at crypt bases fuel a conveyor belt of differentiation toward the villus tip. Using the public dataset of Haber *et al.* and our open‑source pipeline ([github.com/ssohamsud/M2R](https://github.com/ssohamsud/M2R)), we dissect this system in three dimensions: identity, trajectory and inter‑cellular dialogue.

## Identity: Graph Community Detection

### Mathematical Pipeline
After log‑normalisation and highly‑variable gene selection, we embed the $n\!\times\!d$ expression matrix $X$ into a $k$‑dimensional principal‑component (PC) space, $X_{\mathrm{PCA}}=(X-\bar{X})W_k$. A $K$‑nearest‑neighbour graph captures local similarity, with edge weights

$$
w_{ij}=\frac{|\mathcal{N}_K(i)\cap\mathcal{N}_K(j)|}{|\mathcal{N}_K(i)\cup\mathcal{N}_K(j)|}.
$$

Leiden clustering maximises modularity

$$
Q=\frac{1}{2m}\sum_{i,j}\left(w_{ij}-\frac{k_i k_j}{2m}\right)\delta(c_i,c_j),
$$

producing seven robust communities: intestinal stem cells (ISCs), transit‑amplifying (TA), absorptive enterocyte, Paneth, goblet, tuft and enteroendocrine (EE) cells.

**Intuition.** Think of each cell as a Twitter user in a friend graph. PCA filters out background noise, leaving the strongest social signals (gene co‑expression). Leiden then discovers densely connected friend groups—our bona‑fide cell types.

## Trajectory: Continuous Differentiation Landscapes

### Diffusion Maps and Pseudotime
A diffusion kernel $K_{ij}=\exp(-\|x_i-x_j\|^2/(\sigma_i\sigma_j))$ defines a Markov chain $P=D^{-1}K$. Eigenvectors $\psi_\ell$ of $P$ yield coordinates $\Phi_t(i)=(\lambda_2^t\psi_2(i),\dots)$. Diffusion pseudotime is the distance from a root ISC along this manifold.

**Intuition.** Imagine dye diffusing from a root cell; the arrival time of dye to another cell is its pseudotime. The resulting trajectory shows a bifurcation into absorptive and secretory branches.

### RNA Velocity and CellRank Fates
Velocity vectors $\nu_i=\beta(u_i-\hat\gamma s_i)$ impose directionality. CellRank converts these velocities into an absorbing Markov chain, computing fate probabilities $F=(I-Q)^{-1}R$ for each transient cell.

**Intuition.** Water flows downhill toward lakes (terminal lineages). CellRank tells us how likely each droplet (cell) is to reach Paneth, goblet, EE or enterocyte lakes. High entropy in TA cells signals their multipotency, which collapses as commitment proceeds.

## Dialogue: Quantifying Cell–Cell Communication

### Why CCC Matters
Cells do not act in isolation; they constantly exchange molecular “SMS messages”—ligands and receptors (LRs)—that coordinate proliferation, immunity and metabolism. Failing to model these interactions reduces scRNA‑seq to a mere census. Our goal is to map who speaks to whom, about what, and with what strength.

### Computational Framework (LIANA)
LIANA wraps five orthogonal scoring engines into a consensus meta‑score:

1. **Connectome:** $\log_2(\text{Expr}_s(L)\,\text{Expr}_r(R)+\varepsilon)$.
2. **NATMI:** raw product $\text{Expr}_s(L)\times\text{Expr}_r(R)$.
3. **CellPhoneDB:** permutation $p$‑value converted to $-\log_{10}p$.
4. **log$_2$FC:** sender versus background differential expression for $L$ plus analogous for $R$.
5. **SignalR:** $\sqrt{\dfrac{\text{Expr}_s(L)\,\text{Expr}_r(R)}{\text{Expr}_s(L)\,\text{Expr}_r(R)+\mu}}$.

Scores are MinMax‑scaled and averaged, producing a dense interaction tensor $T_{s,r}^{(LR)}$. We threshold by the $85^{\text{th}}$ percentile to focus on biologically salient signals.

**Network Construction.** Summing $T_{s,r}^{(LR)}$ over all LR pairs yields an adjacency matrix $A_{sr}$, visualised as a weighted digraph. We compute sender strength $S_s=\sum_r A_{sr}$, receiver strength $R_r=\sum_s A_{sr}$, and betweenness centrality to identify relay hubs.

### Key Findings

1. **Paneth $\to$ ISC Wnt Signalling.** Paneth cells top the sender ranking ($S_{\text{Paneth}}$) despite a narrow LR repertoire. Their outgoing edges concentrate on canonical Wnt ligands (*Wnt3, Wnt4*) binding to *Fzd7, Lrp5/6* on ISCs—a textbook niche support pathway. Perturbation data confirm that ablating Paneth cells collapses $S_{\text{Paneth}}$ and down‑regulates ISC markers.
2. **Enteroendocrine Broadcasting.** EE cells exhibit the broadest LR spectrum (high entropy of outgoing edge weights), secreting peptide hormones (*Gcg, Pyy, Tph1*). Receivers span TA and mature enterocytes, suggesting endocrine modulation of nutrient absorption.
3. **Temporal Waves of Signalling.** Binning cells along diffusion pseudotime and recomputing $A_{sr}^{(\text{bin})}$ reveals a hand‑off: early crypt signalling is Wnt‑heavy (Paneth$\to$ISC), mid‑trajectory signals involve Notch (goblet/inhibitory) and BMP antagonists (Noggin), while late‑villus communication pivots to cytokines (IL‑18) influencing immune crosstalk.
4. **Network Topology Mirrors Differentiation.** Mapping fate probabilities onto the CCC graph shows that cells with high multipotency (entropy$>1.5$ bits) sit at network bottlenecks—they both send and receive diverse signals, acting as information relays. Once a cell commits (entropy < 0.5 bits), its degree shrinks, reflecting specialised communication.

### Mathematical Deep Dive
Let $T \in \mathbb{R}^{S\times R\times L}$ be the LR tensor. Define $g(L,R)=\text{geometric mean}(T_{s,r}^{(L,R)})$ across senders/receivers. A sender score is

$$
S_s=\sum_{r,L,R} T_{s,r}^{(L,R)},\qquad
\text{Var}(S_s)=\sum_{r,L,R}(T_{s,r}^{(L,R)}-\bar S_s)^2.
$$

Bootstrapping ($B=1{,}000$ resamples) yields confidence intervals for $S_s$; Paneth’s 95\% CI does not overlap goblet’s, validating niche specificity. Centrality is computed on $A$ via eigenvector centrality $c=\lambda^{-1}A c$; EE cells score highest ($c_{\text{EE}}=0.41$), echoing their broadcasting role.

**Intuition.** Think of LR pairs as radio frequencies. Paneth cells broadcast a single powerful station (Wnt FM) listened to almost exclusively by ISCs. EE cells run a talk‑radio network with many channels, reaching a wider but shallower audience.

## Putting It All Together
Combining trajectories with CCC gives a 4‑D narrative (state, time, direction, dialogue): ISCs proliferate under Paneth Wnt support, bifurcate, then gradually shut down Wnt reception while ramping lineage‑specific signals. EE hormones surge mid‑villus, coinciding with absorptive gene induction. Tuft and goblet cells contribute IL‑25 and mucin‑related messages, preparing the mucosal barrier.

## Conclusions and Outlook
Our integrative pipeline translates raw scRNA‑seq counts into a cinematic storyboard of intestinal renewal. Beyond confirming canonical pathways, we uncover nuanced temporal shifts and network roles, pointing to unexplored regulators like EE’s *Ghrl*$\to$TA and tuft’s *IL25*$\to$immune cell crosstalk. Next, coupling spatial transcriptomics and perturb‑seq will test these hypotheses *in situ* and under stress models (diet, infection).

**Code and Data:** [https://github.com/ssohamsud/M2R](https://github.com/ssohamsud/M2R)
**[Download the full M2R research paper (PDF)](/M2R.pdf)** 