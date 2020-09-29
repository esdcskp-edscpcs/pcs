---
title: Formations
layout: no-banner
permalink: /formation-securite-app/
---

## Développement Infonuagique

<p>
	<ul class="list-unstyled">
	{% assign list_of_courses = site.data.formation-securite-app.cloud | sort_natural: "title" %}
	{% for course in list_of_courses %}
	  <li>
	  <details>
		<summary>
		  <h3 id="{{ course.title | slugify }}">{{ course.title }}</h3>
		</summary>
		{{ course.details }}
	  </details>
	  </li>
	{% endfor %}
	</ul>
</p>

## Applications sécurisées

<p>
	<ul class="list-unstyled">
	{% assign list_of_courses = site.data.formation-securite-app.secureapplications | sort_natural: "title" %}
	{% for course in list_of_courses %}
	  <li>
	  <details>
		<summary>
		  <h3 id="{{ course.title | slugify }}">{{ course.title }}</h3>
		</summary>
		{{ course.details }}
	  </details>
	  </li>
	{% endfor %}
	</ul>
</p>