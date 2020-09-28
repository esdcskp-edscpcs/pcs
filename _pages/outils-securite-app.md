---
title: Outils pour la sécurité des applications
layout: default
permalink: /outils-securite-app/
---

## Outils de sécurité standard d'EDSC

La sécurité informatique nécessite que les équipes de développement intègrent dans leur pipeline CI / CD au moins les types de tests de sécurité suivants à l'aide des outils recommandés. Les outils suivants sont standards au sein du ministère et s'intègrent à l'outil de gestion des vulnérabilités des applications (GVA) d'entreprise [** Threadfix **] (https://threadfix.it/) afin de consolider les résultats de l'analyse de tous les outils de sécurité de l'ensemble du ministère.

Les guides de mise en œuvre de nos outils de sécurité standards se trouvent à côté du produit dès qu'ils sont disponibles pour la distribution.

Pour demander une licence pour l'un des outils de sécurité suivants, veuillez [nous contacter via le Réseau des Champions de la Sécurité sur MS Teams](https://teams.microsoft.com/l/channel/19%3a7fb48ff71f584a309817c64b3d599a77%40thread.tacv2/Licenses?groupId=bea80905-7f0f-432d-9a83-60561c1efcd2&tenantId=9ed55846-8a81-4246-acd8-b1a01abfc0d1).

REMARQUE: Les développeurs peuvent utiliser tout autre outil de sécurité pris en charge pour répondre à leurs besoins, mais la sécurité informatique exigera que le ou les outils sélectionnés s'intègrent à la solution GVA d'entreprise Threadfix. Voir [Outils de sécurité additonnels recommandés](#outils-de-sécurité-additonnels-recommandés) pour plus de détails.

<ul class="list-unstyled">
{% for type in site.data.outils-securite-app.standard %}
  <li>
  <details>
    <summary>
      <h2 class="h3" id="{{ type.focus | slugify }}">{{ type.focus }}</h2>
    </summary>
    {% if type.definition %}
      {{ type.definition %}}
    {% endif %}
    {% if type.tools %}
		<p><strong>Standard(s) corporatif(s):</strong></p>
		<ul class="list-group list-inline row mrgn-lft-0 mrgn-rght-0">
		  {% assign list_of_tools = type.tools | sort_natural: "name" %}
		  {% for tool in list_of_tools %}
			<li class="list-group-item col-md-4 brdr-rds-0">
			  <h3 class="list-group-item-heading" id="{{ tool.name | slugify }}">{{ tool.name }}</h3>
			  <ul class="list-group-item-text list-inline">
				{% if tool.availability %}
				  <li>{{ tool.availability }}</li>
				{% endif %}
				{% if tool.details %}
				  <li><a href="{{ tool.details }}" target="_blank">Détails</a></li>
				{% endif %}
				{% if tool.guide %}
				  <li><a href="{{ tool.guide }}" target="_blank">Guide</a></li>
				{% endif %}
			  </ul>
			</li>
		  {% endfor %}
		</ul>
	{% else %}
		<p><strong>EDSC n'a pas acheté d'outil de ce type jusqu'à présent.</strong></p>
	{% endif %}
  </details>
  </li>
{% endfor %}
</ul>

## Outils de sécurité additionnels recommandés

Les outils suivants s'intègrent également à l'outil AVM d'entreprise Threadfix et peuvent être utilisés à la place des outils standard listés ci-dessus. Cependant, la sécurité informatique ne s'appropriera pas ces outils et ne pourra peut-être pas en assurer le support.

<ul class="list-unstyled">
{% for type in site.data.outils-securite-app.supported %}
  <li>
  <details>
    <summary>
      <h2 class="h3" id="{{ type.focus | slugify }}">{{ type.focus }}</h2>
    </summary>
    {% if type.tools %}
		<p><strong>Outil(s) de sécurité additionnel(s) recommandé(s):</strong></p>
		<ul class="list-group list-inline row mrgn-lft-0 mrgn-rght-0">
		  {% assign list_of_tools = type.tools | sort_natural: "name" %}
		  {% for tool in list_of_tools %}
			<li class="list-group-item col-md-4 brdr-rds-0">
			  <h3 class="list-group-item-heading">{{ tool.name }}<br />{{ tool.product }}</h3>
			  <ul class="list-group-item-text list-inline">
				{% if tool.pricing %}
				  <li>{{ tool.pricing }}</li>
				{% endif %}
				{% if tool.details %}
				  <li><a href="{{ tool.details }}" target="_blank">Détails</a></li>
				{% endif %}
				{% if tool.guide %}
				  <li><a href="{{ tool.guide }}" target="_blank">Guide</a></li>
				{% endif %}
			  </ul>
			</li>
		  {% endfor %}
		</ul>
	{% else %}
		<p><strong>EDSC n'a pas acheté d'outil de ce type jusqu'à présent.</strong></p>
	{% endif %}
  </details>
  </li>
{% endfor %}
</ul>
