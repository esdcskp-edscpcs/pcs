---
title: Événements
layout: no-banner
permalink: /evenements/
---

{% if site.data.evenements.next %}
<div class="well">
    <h2 id="next-event">Événements qui pourraient vous intéresser</h2>
{% for event in site.data.evenements.next %}
    <h3>{{ event.topic }}</h3>
	<p><strong>{{ event.date }}</strong><br />
	Présentateur: {{ event.presenter }}</p>
	<p>{{ event.overview }}</p>
	<p><a href="{{ event.link }}" class="btn btn-primary">S'inscrire</a></p>
{% endfor %}
</div>
{% endif %}

<h2>Événements précédents</h2>

{% for event in site.data.evenements.past %}

<section class="panel panel-default">
    <div class="panel-heading">
        <h3 class="panel-title" id="{{ event.topic | slugify }}">{{ event.topic }}</h3>
    </div>
    <div class="panel-body">
        <div class="pull-right mrgn-rght-lg text-muted small">
            <dl>
                <dt>Présenté le:</dt>
                <dd>{{ event.date }}</dd>
            </dl>
        </div>
    {% if event.recording %}
        <p><strong><a href="{{ event.recording }}" target="_blank"><span class="glyphicon glyphicon-facetime-video"></span> Voir l'enregistrement</a></strong></p>
    {% endif %}
    {% if event.presentation %}
        <p><a href="{{ event.presentation }}" target="_blank"><span class="glyphicon glyphicon-file"></span> Voir les diapositives de la présentation</a></p>
    {% endif %}
    {% if event.resources %}
        <p>Ressources:</p>
        <ul>
        {% for resource in event.resources %}
            <li><a href="{{ resource.link }}" target="_blank">{{ resource.title }}</a></li>
        {% endfor %}
        </ul>
    {% endif %}
    </div>
</section>

{% endfor %}
