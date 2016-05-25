---
layout: chapters
id: chapters
permalink: /chapters/
title: "Содержание"
---

{% assign prefaceChapters = site.chapters | where:'section', 'Preface' %}
{% assign backgroundChapters = site.chapters | where:'section', 'Background' %}
{% assign coreChapters = site.chapters | where:'section', 'Core' %}
{% assign extraChapters = site.chapters | where:'section', 'Extras' %}

# Содержание

## Предисловие

<ol>
  {% for chapter in prefaceChapters %}
    <li><a href="{{ chapter.url }}">{{ chapter.title }}</a></li>
  {% endfor %}
</ol>

## Основа

{% assign backgroundStart = prefaceChapters.size | plus: 1 %}

<ol start="{{backgroundStart}}">
  {% for chapter in backgroundChapters %}
    <li><a href="{{ chapter.url }}">{{ chapter.title }}</a></li>
  {% endfor %}
</ol>

## Суть

{% assign coreStart = backgroundStart | plus: backgroundChapters.size %}

<ol start="{{coreStart}}">
	{% for chapter in coreChapters %}
		<li><a href="{{ chapter.url }}">{{ chapter.title }}</a></li>
	{% endfor %}
</ol>

## Дополнительно

{% assign extrasStart = coreStart | plus: coreChapters.size %}

<ol start="{{extrasStart}}">
	{% for chapter in extraChapters %}
		<li><a href="{{ chapter.url }}">{{ chapter.title }}</a></li>
	{% endfor %}
</ol>
