---
title: Formations sur la sécurité des applications
layout: no-banner
permalink: /formation-securite-app/
---

<p>Une liste de cours sur la sécurité des applications. Des prix de groupe sont disponibles pour certains cours.</p>
<p>
	<ul class="list-unstyled">
	{% assign list_of_courses = site.data.app-security-training.training | sort_natural: "title" %}
	{% for course in list_of_courses %}
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
</p>