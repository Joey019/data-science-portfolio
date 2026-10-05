---
layout: page
title: Projects
---

# Projects
A showcase of my work, from ideas to implementation.

---
{% for project in site.data.projects %}
## Project {{ forloop.index }}

<div class="card-grid">
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
</div>

{% endfor %}

<!-- Original link preserved for reference: [Posted Speed Limits and Severe Crash Rates in Mecklenburg County](project/project1/project1.md) -->
