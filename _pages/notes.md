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

Here are notes from several of the classes I've taken at MIT.  

For fun, I occasionally return to old notes to clean them up and add diagrams. Notes listed in <span style = "color:var(--global-gray-text-color)">gray</span> are ones for which I haven't yet done so. These aren't at the level of polish I'd like &mdash; they have many more errors and typos and may have some lectures in which the LaTeX or sentence structure is messed up (due to me not being able to type fast enough), and many diagrams are incomplete (if a diagram doesn't make sense, you should assume it's incomplete) &mdash; and may get updated in the future. 

<!-- <hr> -->
{% assign notes = site.lecture_notes | sort: 'code' %}
{% include lnotes.html notes=notes %}

I've also contributed to the MIT OCW student notes for <a href="https://ocw.mit.edu/courses/res-18-011-algebra-i-student-notes-fall-2021/" target="_blank">18.701</a> and  <a href="https://ocw.mit.edu/courses/res-18-012-algebra-ii-student-notes-spring-2022/" target="_blank">18.702</a>.

### Reading group seminar

Here are my notes from a reading group seminar at MIT. (Right now, most of the talks from the previous two semesters are missing; hopefully they'll be posted sometime in the future, once I've cleaned them up.)

{% assign notes = site.seminar_notes | where: 'series', 'Reading Group' | sort: 'date' | reverse %}

{% include talks.html talks=notes %}

### Other talks

Here are my notes from a few other talks, either from colloquium talks or the <a href="https://math.mit.edu/combin/" target="_blank">combinatorics seminar</a> at MIT. 

{% assign notes = site.seminar_notes | where: 'series', 'Combinatorics' | sort: 'date' | reverse %}

{% include talks.html talks=notes %}



