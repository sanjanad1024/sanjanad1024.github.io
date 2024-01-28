---
layout: page
permalink: /papers/
title: Papers
description: A list of research papers I've written. 
nav: true
nav_order: 2
topics: [Combinatorics, Number theory]
---
<!-- _pages/publications.md -->

{% assign papers = site.papers | sort: 'date' | reverse %}
{% include pub.html papers=papers %}

<!-- ### Combinatorics
{% assign papers = site.papers | where: 'topic', 'Combinatorics'  | sort: 'date' | reverse %}
{% include pub.html papers=papers %}

### Number theory
{% assign papers = site.papers | where: 'topic', 'Number theory'  | sort: 'date' | reverse %}
{% include pub.html papers=papers %} -->




