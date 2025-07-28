---
title: About
icon: fas fa-user
order: 1
---

<style>
.timeline-content .timeline-description strong {
  color: #d97706 !important;
  font-weight: 700 !important;
}

/* Style links within strong tags */
.timeline-description strong a,
.timeline-content .timeline-description strong a {
  color: #d97706 !important;
  font-weight: 700 !important;
  text-decoration: none !important;
  transition: color 0.3s ease !important;
}

.timeline-description strong a:hover,
.timeline-content .timeline-description strong a:hover {
  color: #f59e0b !important;
  text-decoration: none !important;
}

/* Additional specificity to override theme styles */
#main .timeline-description strong,
.main .timeline-description strong,
.content .timeline-description strong {
  color: #d97706 !important;
  font-weight: 700 !important;
}

/* Style for year labels in education section */
.education-year {
  color: #d97706 !important;
  font-weight: 700 !important;
}

/* Sidebar avatar position adjustment */
#sidebar .profile-wrapper .avatar {
  margin-top: -20px;
}
</style>

# Hello, I'm Soham 👋

I'm a 2nd year Mathematics and Statistics student at Imperial College London, passionate about quantitative finance, machine learning, and mathematical research.

---

## Featured Projects 🚀

<div class="featured-projects">
  <div class="project-grid">
    <div class="project-card featured">
      <div class="project-badge">Featured</div>
      <h4>Cricket Analytics: Batting Pressure Index</h4>
      <p>Advanced statistical analysis of cricket performance using pressure metrics and team dynamics. Features 6 comprehensive visualizations and deep-dive analysis.</p>
      <a href="/posts/under-the-helmet-quantifying-pressure-in-ipl-batting/" class="project-link">Read More →</a>
    </div>
    <div class="project-card">
      <h4>Stochastic Volatility Physics</h4>
      <p>Non-technical exploration of diffusing diffusivity models in quantitative finance, making complex financial mathematics accessible.</p>
      <a href="/posts/evaluating-diffusing-diffusivity-models-in-quantitative-finance-a-non-technical-summary/" class="project-link">Read More →</a>
    </div>
    <div class="project-card">
      <h4>Intestinal Cell Fate Mapping</h4>
      <p>Mathematical modeling of single-cell interactions in intestinal development using advanced statistical techniques.</p>
      <a href="/posts/mapping-the-intestinal-social-network-using-data/" class="project-link">Read More →</a>
    </div>
  </div>
</div>

<style>
.featured-projects {
  margin: 2rem 0;
}

.project-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1.5rem;
  margin-top: 1rem;
}

.project-card {
  background: linear-gradient(135deg, #ffffff 0%, #f8fafc 100%);
  border: 1px solid #e2e8f0;
  border-radius: 12px;
  padding: 1.5rem;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  position: relative;
}

.project-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 15px rgba(0, 0, 0, 0.1);
}

.project-card.featured {
  border: 2px solid #d97706;
  background: linear-gradient(135deg, #fff7ed 0%, #fed7aa 100%);
}

.project-badge {
  position: absolute;
  top: -8px;
  right: 1rem;
  background: #d97706;
  color: white;
  padding: 0.25rem 0.75rem;
  border-radius: 12px;
  font-size: 0.75rem;
  font-weight: 600;
  text-transform: uppercase;
}

.project-card h4 {
  color: #1e293b;
  margin-bottom: 0.75rem;
  font-size: 1.1rem;
  font-weight: 600;
}

.project-card p {
  color: #475569;
  line-height: 1.6;
  margin-bottom: 1rem;
}

.project-link {
  color: #d97706;
  text-decoration: none;
  font-weight: 600;
  transition: color 0.2s ease;
}

.project-link:hover {
  color: #f59e0b;
  text-decoration: underline;
}

@media (max-width: 768px) {
  .project-grid {
    grid-template-columns: 1fr;
  }
}
</style>

---

## Experience Timeline

{% include timeline.html experiences=site.data.experiences %}

---

## Education

### Imperial College London
**Integrated Masters in Mathematics and Statistics** *(2023 - 2027)*

**Year 2 Modules:**
- **Core:** Real Analysis, Complex Analysis, Multivariable Calculus, Differential Equations, Linear Algebra, Numerical Analysis (Julia)
- **Optional:** Probability for Statistics, Principles of Programming (Python), Statistical Modelling (R), Lebesgue Measure and Integration
- **Self-Study:** Network Science
- **Research:** M2R - Modelling Differentiation Trajectories and Cell-Cell Interactions of scRNA data.

**Year 1 Modules:**
- **Core:** Intro to Uni Maths, Analysis 1, Linear Algebra and Groups, Calculus and Applications, Probability and Statistics (R)
- **Computing:** Intro to Computing (Python), Intro to Applied Maths (MatLab)
- **Research:** M1R Individual Research Project - Merton Jump Diffusion Model

### Sutton Grammar School
**A-Levels & GCSEs** *(2016 - 2023)*

- **Academic Excellence:** GCSE Prize, A-Level Prize for best examination grades
- **Leadership:** Founded Mathematics Society, Mathematics Magazine, Maths Mentor, Hans Woyda team captain
- **Competitions:** Gold/Medals/Distinction in all UKMT challenges and Olympiads
- **Extracurricular:** Physics Challenge Gold, Salter's Chemistry Festival, Longitude Explorer Prize, Chess Team, Cricket Team, DofE (Bronze/Silver/Gold)

---

## Teaching

### Private Tutoring *(2020 - Present)*
- **Experience:** 350+ hours of teaching
- **Specialization:** Mathematics and admission tests (STEP, MAT, TMUA)
- **Levels:** GCSEs, A-Levels, University Admission Exams
- **Success Rate:** All students ended up at Imperial, Oxford, Cambridge, LSE, Warwick

---

* Feel free to [get in touch](/contact/) for tutoring inquiries or other project proposals/ideas I'd be interested in!*

---

## Competition Highlights 🏆

<div class="competition-highlights">
  <div class="competition-grid">
    <div class="competition-card quant">
      <h4>Quantitative Finance</h4>
      <ul>
        <li><strong>QRT Data Challenge</strong> - Advanced quantitative analysis</li>
        <li><strong>G-research Quant Challenge</strong> - Algorithmic trading strategies</li>
        <li><strong>IMC Prosperity Challenge</strong> - Market making and risk management</li>
        <li><strong>QRT Quant Challenge</strong> - Mathematical modeling excellence</li>
      </ul>
    </div>
    <div class="competition-card ai">
      <h4>AI & Machine Learning</h4>
      <ul>
        <li><strong>Kaggle AI Competition</strong> - "LLMs You Can't Please Them All"</li>
        <li>Advanced natural language processing and model optimization</li>
      </ul>
    </div>
    <div class="competition-card spring">
      <h4>Spring Week Victories</h4>
      <ul>
        <li><strong>Multiple Competition Wins</strong> during spring week programs</li>
        <li>Demonstrated leadership and problem-solving skills</li>
      </ul>
    </div>
  </div>
</div>

<style>
.competition-highlights {
  margin: 2rem 0;
}

.competition-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 1.5rem;
  margin-top: 1rem;
}

.competition-card {
  background: linear-gradient(135deg, #f8fafc 0%, #e2e8f0 100%);
  border: 1px solid #e2e8f0;
  border-radius: 12px;
  padding: 1.5rem;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.competition-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 15px rgba(0, 0, 0, 0.1);
}

.competition-card h4 {
  color: #1e293b;
  margin-bottom: 1rem;
  font-size: 1.1rem;
  font-weight: 600;
  border-bottom: 2px solid #d97706;
  padding-bottom: 0.5rem;
}

.competition-card.quant h4 {
  border-bottom-color: #3b82f6;
}

.competition-card.ai h4 {
  border-bottom-color: #8b5cf6;
}

.competition-card.spring h4 {
  border-bottom-color: #10b981;
}

.competition-card ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.competition-card li {
  margin-bottom: 0.75rem;
  color: #475569;
  line-height: 1.5;
}

.competition-card strong {
  color: #1e293b;
  font-weight: 600;
}

@media (max-width: 768px) {
  .competition-grid {
    grid-template-columns: 1fr;
  }
}
</style>

## Societies & Hobbies

- Head of Quant Research at Cauchy Capital (Imperial Maths Quant Fund)
- Societies: Badminton/Cricket Team, Indian, Punjabi and Hindu Society

- Guitar for over 10 years, performed in over 15 concerts - Youtube Channel Coming Soon!
- Black Belt Karate (2018)
- Poker Player

---

## GitHub Activity

<div class="github-activity">
  <h3>My Coding Activity</h3>
  <div class="github-stats">
    <img src="https://github-readme-stats.vercel.app/api?username=ssohamsud&show_icons=true&theme=radical&hide_border=true" alt="GitHub Stats" />
    <img src="https://github-readme-streak-stats.herokuapp.com/?user=ssohamsud&theme=radical&hide_border=true" alt="GitHub Streak" />
  </div>
  <div class="github-contributions">
    <h4>Recent Contributions</h4>
    <img src="https://ghchart.rshah.org/0366d6/ssohamsud" alt="GitHub Contributions" />
  </div>
  <div class="github-top-languages">
    <h4>Top Languages</h4>
    <img src="https://github-readme-stats.vercel.app/api/top-langs/?username=ssohamsud&layout=compact&theme=radical&hide_border=true" alt="Top Languages" />
  </div>
</div>

<style>
.github-activity {
  margin: 2rem 0;
  padding: 1.5rem;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 12px;
  color: white;
}

.github-activity h3 {
  color: white;
  margin-bottom: 1rem;
  text-align: center;
}

.github-stats {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  justify-content: center;
  margin-bottom: 1.5rem;
}

.github-stats img {
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0,0,0,0.1);
}

.github-contributions, .github-top-languages {
  margin: 1.5rem 0;
}

.github-contributions h4, .github-top-languages h4 {
  color: white;
  margin-bottom: 0.5rem;
  text-align: center;
}

.github-contributions img {
  width: 100%;
  max-width: 800px;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0,0,0,0.1);
}

.github-top-languages img {
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0,0,0,0.1);
}

@media (max-width: 768px) {
  .github-stats {
    flex-direction: column;
    align-items: center;
  }
  
  .github-stats img {
    width: 100%;
    max-width: 400px;
  }
}
</style>

---

## Social Sharing

<div class="social-sharing">
  <h3>Share This Page</h3>
  <div class="share-buttons">
    <a href="https://twitter.com/intent/tweet?text=Check out Soham Sud's amazing portfolio!&url={{ site.url }}{{ page.url }}" target="_blank" class="share-btn twitter">
      <i class="fab fa-twitter"></i> Twitter
    </a>
    <a href="https://www.linkedin.com/sharing/share-offsite/?url={{ site.url }}{{ page.url }}" target="_blank" class="share-btn linkedin">
      <i class="fab fa-linkedin"></i> LinkedIn
    </a>
    <a href="https://www.facebook.com/sharer/sharer.php?u={{ site.url }}{{ page.url }}" target="_blank" class="share-btn facebook">
      <i class="fab fa-facebook"></i> Facebook
    </a>
    <a href="https://wa.me/?text=Check out Soham Sud's portfolio: {{ site.url }}{{ page.url }}" target="_blank" class="share-btn whatsapp">
      <i class="fab fa-whatsapp"></i> WhatsApp
    </a>
    <a href="mailto:ss8323@ic.ac.uk?subject=Check out Soham Sud's Portfolio&body=Hi! I found this amazing portfolio: {{ site.url }}{{ page.url }}" class="share-btn email">
      <i class="fas fa-envelope"></i> Email
    </a>
  </div>
</div>

<style>
.social-sharing {
  margin: 2rem 0;
  padding: 1.5rem;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 12px;
  color: white;
}

.social-sharing h3 {
  color: white;
  margin-bottom: 1rem;
  text-align: center;
}

.share-buttons {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  justify-content: center;
}

.share-btn {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.75rem 1.5rem;
  background: rgba(255, 255, 255, 0.1);
  color: white;
  text-decoration: none;
  border-radius: 25px;
  transition: all 0.3s ease;
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.share-btn:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: translateY(-2px);
  color: white;
  text-decoration: none;
}

.share-btn.twitter:hover {
  background: #1da1f2;
}

.share-btn.linkedin:hover {
  background: #0077b5;
}

.share-btn.facebook:hover {
  background: #4267b2;
}

.share-btn.whatsapp:hover {
  background: #25d366;
}

.share-btn.email:hover {
  background: #ea4335;
}

@media (max-width: 768px) {
  .share-buttons {
    flex-direction: column;
    align-items: center;
  }
  
  .share-btn {
    width: 200px;
    justify-content: center;
  }
}
</style>
