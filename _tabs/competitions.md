---
title: Competitions
icon: fas fa-trophy
order: 2
---

<style>
.comp-list {
  margin: 1rem 0 2rem;
}

.comp {
  border-top: 1px solid var(--main-border-color);
  padding: 0.85rem 0;
}

.comp:last-child {
  border-bottom: 1px solid var(--main-border-color);
}

.comp-head {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  flex-wrap: wrap;
  gap: 0.25rem 1rem;
}

.comp-name {
  font-weight: 600;
  color: var(--heading-color, var(--text-color));
}

.comp-result {
  font-size: 0.78em;
  color: var(--text-muted-color);
  border: 1px solid var(--main-border-color);
  border-radius: 999px;
  padding: 1px 10px;
  font-variant-numeric: tabular-nums;
  white-space: nowrap;
}

.comp-note {
  margin-top: 0.3rem;
  font-size: 0.95em;
  line-height: 1.55;
}

.gallery {
  display: flex;
  gap: 1rem;
  overflow-x: auto;
  scroll-snap-type: x mandatory;
  padding: 0.5rem 0 1rem;
  margin-top: 1rem;
}

.gallery figure {
  flex: 0 0 auto;
  width: 320px;
  margin: 0;
  scroll-snap-align: start;
  background: var(--card-bg);
  border: 1px solid var(--main-border-color);
  border-radius: 10px;
  overflow: hidden;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.06);
  transition: transform 0.25s ease, box-shadow 0.25s ease;
}

.gallery figure:hover {
  transform: translateY(-3px);
  box-shadow: 0 10px 28px rgba(0, 0, 0, 0.14);
}

.gallery img {
  width: 100%;
  height: 220px;
  object-fit: cover;
  display: block;
}

.gallery figcaption {
  padding: 0.75rem 1rem;
  font-size: 0.9em;
}

.gallery figcaption strong {
  display: block;
  color: var(--heading-color, var(--text-color));
}

.gallery figcaption span {
  color: var(--text-muted-color);
}

@media (max-width: 576px) {
  .gallery figure {
    width: 260px;
  }

  .gallery img {
    height: 180px;
  }
}
</style>

Trading, data and machine learning competitions I've taken part in.

<div class="comp-list">
  <div class="comp">
    <div class="comp-head">
      <span class="comp-name">QRT Data Challenge (Algothon 2023)</span>
      <span class="comp-result">3rd place</span>
    </div>
    <div class="comp-note">Electricity price prediction using time series analysis and machine learning.</div>
  </div>
  <div class="comp">
    <div class="comp-head">
      <span class="comp-name">IMC Prosperity</span>
      <span class="comp-result">167th of ~10,000 teams</span>
    </div>
    <div class="comp-note">Top 10 in the UK, using a mix of mean reversion, options volatility strategies, statistical arbitrage and spatial arbitrage.</div>
  </div>
  <div class="comp">
    <div class="comp-head">
      <span class="comp-name">G-Research Quant Challenge</span>
      <span class="comp-result">Top 5%</span>
    </div>
    <div class="comp-note">Problems on optimal transport and robot bidding algorithms.</div>
  </div>
  <div class="comp">
    <div class="comp-head">
      <span class="comp-name">QRT Quant Challenge</span>
      <span class="comp-result">Organiser</span>
    </div>
    <div class="comp-note">Helped QRT run their challenge for 200 participants, marking solutions and handling logistics.</div>
  </div>
  <div class="comp">
    <div class="comp-head">
      <span class="comp-name">Kaggle AI Competition</span>
      <span class="comp-result">Participant</span>
    </div>
    <div class="comp-note">Adversarial attacks on large language models.</div>
  </div>
</div>

## Gallery

<div class="gallery">
  <figure>
    <img src="/assets/img/qrtdatawin.png" alt="Algothon 2023 prize" loading="lazy" />
    <figcaption><strong>Algothon 2023</strong><span>3rd place prize</span></figcaption>
  </figure>
  <figure>
    <img src="/assets/img/imcprosperity.png" alt="IMC Prosperity result" loading="lazy" />
    <figcaption><strong>IMC Prosperity</strong><span>167th of ~10,000 teams</span></figcaption>
  </figure>
  <figure>
    <img src="/assets/img/qrtquantchallenge.png" alt="QRT Quant Challenge winners" loading="lazy" />
    <figcaption><strong>QRT Quant Challenge</strong><span>Winners photo</span></figcaption>
  </figure>
  <figure>
    <img src="/assets/img/qrtchallengeleaderboard.png" alt="QRT Quant Challenge leaderboard" loading="lazy" />
    <figcaption><strong>QRT Quant Challenge</strong><span>Final scores of the top teams</span></figcaption>
  </figure>
</div>
