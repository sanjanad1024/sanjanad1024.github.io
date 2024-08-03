---
layout: page
permalink: /papers/
title: Papers
description: 
nav: true
nav_order: 2
topics: [Combinatorics, Number theory]
---
<!-- _pages/publications.md -->

### Research papers 

Here are the research papers that I've written. 

{% assign papers = site.papers | where: 'series', 'research' | sort: 'date' | reverse %}
{% include pub.html papers=papers %}

### Expository writing

Here are some of the expository papers I've written (right now, these are all things that I wrote for class projects at MIT). 

{% assign papers = site.papers | where: 'series', 'expository' | sort: 'date' | reverse %}
{% include pub.html papers=papers %}

<!-- ### Combinatorics
{% assign papers = site.papers | where: 'topic', 'Combinatorics'  | sort: 'date' | reverse %}
{% include pub.html papers=papers %}

### Number theory
{% assign papers = site.papers | where: 'topic', 'Number theory'  | sort: 'date' | reverse %}
{% include pub.html papers=papers %} -->




