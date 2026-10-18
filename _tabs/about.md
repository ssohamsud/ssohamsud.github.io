---
title: About
icon: fas fa-user
order: 1
---

<style>
/* Sidebar avatar position adjustment */
#sidebar .profile-wrapper .avatar {
  margin-top: -20px;
}

.school {
  margin-top: 1.25rem;
}

.school-head {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  flex-wrap: wrap;
  gap: 0 1rem;
}

.school-name {
  font-weight: 600;
  font-size: 1.05em;
  color: var(--heading-color, var(--text-color));
}

.school-dates {
  color: var(--text-muted-color);
  font-size: 0.9em;
  font-variant-numeric: tabular-nums;
}

.school-degree {
  color: var(--text-muted-color);
  margin-bottom: 0.75rem;
}

.drop {
  border-top: 1px solid var(--main-border-color);
}

.drop:last-of-type {
  border-bottom: 1px solid var(--main-border-color);
}

.drop summary {
  list-style: none;
  cursor: pointer;
  padding: 0.6rem 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.drop summary::-webkit-details-marker {
  display: none;
}

.drop summary::after {
  content: '+';
  color: var(--text-muted-color);
  font-size: 1.1em;
}

.drop[open] summary::after {
  content: '\2212';
}

.drop ul,
.drop p {
  margin: 0 0 0.75rem;
}

.module-grid {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 0.25rem 1.5rem;
}

.module-year {
  font-size: 0.78em;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: var(--text-muted-color);
  margin: 0.25rem 0 0.35rem;
}

.module-grid ul {
  padding-left: 1.1rem;
}

@media (max-width: 576px) {
  .module-grid {
    grid-template-columns: 1fr;
  }
}

.soc {
  margin: 0.15rem 0 0.9rem;
}

.soc-name {
  font-weight: 600;
  color: var(--heading-color, var(--text-color));
}

.soc-role {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  gap: 1.5rem;
  font-size: 0.95em;
  line-height: 1.5;
  margin-top: 0.15rem;
}

.soc-year {
  flex-shrink: 0;
  color: var(--text-muted-color);
  font-size: 0.85em;
  font-variant-numeric: tabular-nums;
}

.drop summary:hover {
  color: var(--heading-color, var(--text-color));
}

.drop[open] > *:not(summary) {
  animation: drop-in 0.25s ease;
}

@keyframes drop-in {
  from { opacity: 0; transform: translateY(-4px); }
  to { opacity: 1; transform: none; }
}
</style>

Hey, I'm Soham, a 4th year MSci Maths student at Imperial. I interned at D. E. Shaw this summer and I'll be joining full-time as a Trader in fall 2027.

I use this site to share things I find interesting, the projects I'm working on or collaborating on, and the occasional blog post on whatever has caught my attention.

## Experience

<details class="drop">
<summary>Timeline</summary>

{% include timeline.html experiences=site.data.experiences %}

</details>

## Education

<div class="school">
  <div class="school-head">
    <span class="school-name">Imperial College London</span>
    <span class="school-dates">Oct 2023 – Jun 2027</span>
  </div>
  <div class="school-degree">MSci Mathematics</div>

  <details class="drop">
    <summary>Modules</summary>
    <div class="module-grid">
      <div>
        <div class="module-year">Year 4</div>
        <ul>
          <li>Quantum Mechanics</li>
          <li>Survival Models</li>
          <li>Computational Neurodynamics</li>
          <li>Applied Probability</li>
          <li>Stochastic Calculus and Non-linear Filtering</li>
          <li>Probabilistic Generative Modelling</li>
          <li>Deep Graph Based Learning</li>
          <li>Contemporary Philosophy in the Age of AI</li>
        </ul>
      </div>
      <div>
        <div class="module-year">Year 3</div>
        <ul>
          <li>Probability Theory 1</li>
          <li>Probability Theory 2 (Stochastic Analysis)</li>
          <li>Stochastic Differential Equations and Diffusion Processes</li>
          <li>Stochastic Differential Equations in Financial Modelling</li>
          <li>Dynamics of Games and Iterated Learning</li>
          <li>Time Series Analysis</li>
          <li>Statistical Learning</li>
          <li>Methods for Data Science</li>
        </ul>
      </div>
      <div>
        <div class="module-year">Year 2</div>
        <ul>
          <li>Real Analysis</li>
          <li>Complex Analysis</li>
          <li>Multivariable Calculus</li>
          <li>Differential Equations</li>
          <li>Linear Algebra</li>
          <li>Numerical Analysis (Julia)</li>
          <li>Probability for Statistics</li>
          <li>Principles of Programming (Python)</li>
          <li>Statistical Modelling (R)</li>
          <li>Lebesgue Measure and Integration</li>
          <li>Mind, Self and the Social World: Philosophy and the Human Sciences</li>
        </ul>
      </div>
      <div>
        <div class="module-year">Year 1</div>
        <ul>
          <li>Introduction to University Mathematics</li>
          <li>Analysis 1</li>
          <li>Linear Algebra and Groups</li>
          <li>Calculus and Applications</li>
          <li>Probability and Statistics (R)</li>
          <li>Introduction to Computing (Python)</li>
          <li>Introduction to Applied Mathematics (MATLAB)</li>
        </ul>
      </div>
    </div>
  </details>
  <details class="drop">
    <summary>Academic projects</summary>
    <ul>
      <li>Year 4: Master's thesis (TBD)</li>
      <li>Year 3: Mean Field Reinforcement Learning in Simplified Poker Games</li>
      <li>Year 2: <a href="/posts/intestinal-cell-fate-single-cell/">Modelling Differentiation Trajectories and Cell-Cell Interactions in scRNA-seq Data</a></li>
      <li>Year 1: Merton Jump Diffusion Model</li>
    </ul>
  </details>
  <details class="drop">
    <summary>Societies</summary>
    <div class="soc">
      <div class="soc-name">Algorithmic Trading Society</div>
      <div class="soc-role"><span>Vice President of a 1,800+ member society</span><span class="soc-year">2026/27</span></div>
    </div>
    <div class="soc">
      <div class="soc-name">Indian Society</div>
      <div class="soc-role"><span>Music Act Director</span><span class="soc-year">2026/27</span></div>
      <div class="soc-role"><span>Music Choreographer for the EMW performance</span><span class="soc-year">2025/26</span></div>
    </div>
    <div class="soc">
      <div class="soc-name">MathSoc</div>
      <div class="soc-role"><span>Designed and taught a six-lecture course on sports betting with machine learning to around 150 students, finishing with a prediction competition</span><span class="soc-year">2025/26</span></div>
      <div class="soc-role"><span>Sponsorship Officer</span><span class="soc-year">2024/25</span></div>
    </div>
    <div class="soc">
      <div class="soc-name">Sport</div>
      <div class="soc-role"><span>Badminton/Cricket</span></div>
    </div>
  </details>
</div>

<div class="school">
  <div class="school-head">
    <span class="school-name">Sutton Grammar School</span>
    <span class="school-dates">Sep 2016 – Jul 2023</span>
  </div>
  <div class="school-degree">A-Levels and GCSEs</div>

  <details class="drop">
    <summary>Supercurricular</summary>
    <ul>
      <li>GCSE Prize and A-Level Prize for best examination grades</li>
      <li>Gold, medals or distinction in all UKMT challenges and olympiads</li>
      <li>Hans Woyda team captain</li>
      <li>Founded the Mathematics Society and Mathematics Magazine</li>
      <li>Maths Mentor</li>
      <li>Physics Challenge Gold</li>
      <li>Salters' Chemistry Festival</li>
      <li>Longitude Explorer Prize</li>
    </ul>
  </details>
  <details class="drop">
    <summary>Extracurricular</summary>
    <ul>
      <li>Cricket Team</li>
      <li>Chess Team</li>
      <li>Duke of Edinburgh (Bronze and Silver)</li>
    </ul>
  </details>
</div>

## Other interests

<div class="school">
  <details class="drop">
    <summary>Guitar</summary>
    <p>I've been playing for 12 years. I started on flamenco and fingerstyle, performing at school concerts and external venues, then picked up the electric at 16, where I'm mostly influenced by Matteo Mancuso and Ichika Nito. My favourite thing to play is open tunings, somewhere between Alan Gogoll's tunings and Sean Angus Watson's loops. Otherwise I listen to a lot of blues and rock. I've played around 25 concerts and events so far.</p>
  </details>
  <details class="drop">
    <summary>Poker</summary>
    <p>I'm more into poker theory than actually playing, though I did win the D. E. Shaw annual poker tournament while I was interning.</p>
  </details>
  <details class="drop">
    <summary>Misc</summary>
    <div class="module-year">Trivia</div>
    <p>Nothing serious, I just enjoy it. Mostly geography and Indian history.</p>
    <div class="module-year">Philosophy</div>
    <p>I don't know a huge amount yet, but I'm interested, especially in philosophy of language.</p>
  </details>
</div>

Feel free to [get in touch](/contact/).
