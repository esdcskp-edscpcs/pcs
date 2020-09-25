---
title: Événements
layout: no-banner
permalink: /evenements/
---

{% if site.data.evenements.next %}
<div class="well">
    <h2 id="next-event">Our Next Event</h2>
    <p class="lead">The next event is planned for <strong>{{ site.data.evenements.next.date }}</strong> on <a href="https://github.com/esdc-devcop/esdc-devcop.github.io/issues/{{ site.data.evenements.next.id }}">{{ site.data.evenements.next.topic }}</a></p>
    <p>{{ site.data.evenements.next.overview }}</p>
    {% if site.data.evenements.next.part2 %}
    <p class="text-muted">{{ site.data.evenements.next.part2 }}</p>
    {% endif %}
</div>
{% endif %}

<h2>Past Events</h2>

{% for event in site.data.evenements.past %}

<section class="panel panel-default">
    <div class="panel-heading">
        <h3 class="panel-title" id="{{ event.topic | slugify }}">{{ event.topic }}</h3>
    </div>
    <div class="panel-body">
        <div class="pull-right mrgn-rght-lg text-muted small">
            <dl>
                <dt>Presented on:</dt>
                <dd>{{ event.date }}</dd>
    {% if event.id %}
                <dt>GitHub Issue:</dt>
                <dd><a href="https://github.com/esdc-devcop/esdc-devcop.github.io/issues/{{ event.id }}">#{{ event.id }}</a></dd>
    {% endif %}
            </dl>
        </div>
    {% if event.recording %}
        <p><strong><a href="{{ event.recording }}" target="_blank"><span class="glyphicon glyphicon-facetime-video"></span> View the recording</a></strong></p>
    {% endif %}
    {% if event.presentation %}
        <p><a href="{{ event.presentation }}" target="_blank"><span class="glyphicon glyphicon-file"></span> View the presentation slides</a></p>
    {% endif %}
    {% if event.resources %}
        <p>Resources:</p>
        <ul>
        {% for resource in event.resources %}
            <li><a href="{{ resource.link }}" target="_blank">{{ resource.title }}</a></li>
        {% endfor %}
        </ul>
    {% endif %}
    </div>
</section>

{% endfor %}
