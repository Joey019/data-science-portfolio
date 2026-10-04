---
layout: home
title: Home
---

<div class="hero" markdown="1">
<div class="hero-text" markdown="1">
<div class="hero-eyebrow">Data Science · Machine Learning · Analytics</div>

# Johan Athial
Data Science Student  
UNC Charlotte

<div class="hero-actions">
<a class="btn btn-primary" href="{{ '/Johan_Athial_Resume.pdf' | relative_url }}" target="_blank" rel="noopener">Download Resume</a>
<a class="btn btn-secondary" href="{{ '/projects.html' | relative_url }}">View Projects</a>
<a class="btn btn-secondary" href="{{ site.author.github }}" target="_blank" rel="noopener">GitHub</a>
</div>
</div>
</div>

<div class="panel section" markdown="1">

## About Me
I am a Data Science student with a passion for AI and automation. I love finding inefficiencies in workflows and implementing automation to streamline the process. With the advent of AI, automation has been able to reach another level. I believe that with enough data, we will be able to automate all work, leaving us with the time to pursue any interest imaginable. In my free time I enjoy playing sports such as pickleball, soccer, and ping pong.
</div>

<div class="section" markdown="1">

## Portfolio
<div class="quick-links" markdown="1">
- [Blog](blog.md)
- [Projects](projects.md)
- [LinkedIn](https://www.linkedin.com/in/johan-athial/)
- [Resume](resume.md)
</div>
</div>

<div class="section" markdown="1">

## Featured Project

<div class="card-grid">
{% for project in site.data.projects limit:2 %}
<div class="card project-card">
  {% if project.image %}
  <div class="project-card-img">
    <img src="{{ project.image | relative_url }}" alt="{{ project.title }}">
  </div>
  {% endif %}
  <div class="project-card-body">
    <h3><a href="{{ project.url | relative_url }}">{{ project.title }}</a></h3>
    {% if project.teaser %}<p>{{ project.teaser }}</p>{% endif %}
    {% if project.tags %}
    <div class="tag-list">
      {% for t in project.tags %}<span class="tag">{{ t }}</span>{% endfor %}
    </div>
    {% endif %}
    <div class="project-card-links">
      <a href="{{ project.url | relative_url }}">Read case study &rarr;</a>
      {% if project.repo %}<a href="{{ project.repo }}" target="_blank" rel="noopener">Code</a>{% endif %}
    </div>
  </div>
</div>
{% endfor %}
</div>

<p><a href="{{ '/projects.html' | relative_url }}">See all projects &rarr;</a></p>
</div>
