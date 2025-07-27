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
    <a href="mailto:?subject=Check out Soham Sud's Portfolio&body=Hi! I found this amazing portfolio: {{ site.url }}{{ page.url }}" class="share-btn email">
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

---

## Societies & Hobbies

- Head of Quant Research at Cauchy Capital (Imperial Maths Quant Fund)
- Societies: Badminton/Cricket Team, Indian, Punjabi and Hindu Society

- Guitar for over 10 years, performed in over 15 concerts - Youtube Channel Coming Soon!
- Black Belt Karate (2018)
- Poker Player
