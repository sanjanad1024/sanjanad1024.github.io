---
layout: page
permalink: /notes/
title: Notes
description: 
nav: true
nav_order: 1
series: [Combinatorics, Reading Group]
---

I like to LaTeX notes; some of these notes are posted here. For all of these notes, all content is due to the professor (for lecture notes) or speaker (for talk notes), while all errors are due to me. 

{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

### Lecture notes

Here are notes from some subset of the classes I've taken at MIT.  

For fun, I occasionally return to old notes to clean them up and add diagrams. Notes listed in <span style = "color:var(--global-gray-text-color)">gray</span> are ones for which I haven't yet done so. These aren't at the level of polish I'd like, and may get updated in the future. 

<!-- <hr> -->
{% assign notes = site.lecture_notes | sort: 'code' %}
{% include lnotes.html notes=notes %}

I've also contributed to the MIT OCW student notes for <a href="https://ocw.mit.edu/courses/res-18-011-algebra-i-student-notes-fall-2021/" target="_blank">18.701</a> and  <a href="https://ocw.mit.edu/courses/res-18-012-algebra-ii-student-notes-spring-2022/" target="_blank">18.702</a>.

### Reading group seminar

Here are my notes from some of the talks at a reading group seminar at MIT.

{% assign notes = site.seminar_notes | where: 'series', 'Reading Group' | sort: 'date' | reverse %}

{% include talks.html talks=notes %}

### Other talks

Here are my notes from a few other talks I've attended; most are from the <a href="https://math.mit.edu/combin/" target="_blank">Richard P. Stanley Seminar in Combinatorics</a>, and a few are from colloquium talks at MIT. (Note that these have many errors or holes.)

{% assign notes = site.seminar_notes | where: 'series', 'Combinatorics' | sort: 'date' | reverse %}

{% include talks.html talks=notes %}



