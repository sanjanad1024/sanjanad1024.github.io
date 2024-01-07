---
layout: page
permalink: /research/
title: research
description: My research by topic.
nav: true
nav_order: 2
topics: [Combinatorics, Number theory]
---
<!-- _pages/publications.md -->


### Combinatorics
{% assign papers = site.papers | where: 'topic', 'Combinatorics'  | sort: 'date' | reverse %}
{% include pub.html papers=papers %}

### Number theory
{% assign papers = site.papers | where: 'topic', 'Number theory'  | sort: 'date' | reverse %}
{% include pub.html papers=papers %}




