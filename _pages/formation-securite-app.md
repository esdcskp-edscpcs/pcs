---
title: Formations sur la sécurité des applications
layout: no-banner
permalink: /formation-securite-app/
---

<ul class="list-unstyled">
{% for course in site.data.app-security-training.training %}
  <li>
  <details>
    <summary>
      <h2 class="h3" id="{{ course.title | slugify }}">{{ course.title }}</h2>
    </summary>
	{{ course.details }}
  </details>
  </li>
{% endfor %}
</ul>