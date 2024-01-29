---
layout: page
permalink: /notes/
title: Notes
description: 
nav: true
nav_order: 1
series: [Combinatorics, Reading Group]
---

I like to LaTeX notes; some of these notes are posted here. 

{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

### Lecture Notes

Here are notes from several of the classes I've taken at MIT. Please note that all course content is owned by MIT and its professors, while all errors should be attributed to me (either in transcription or interpretation). 

For fun, I occasionally return to old notes to clean them up and add diagrams. Notes listed in <span style = "color:darkgray">gray</span> are ones for which I haven't yet done so. These aren't at the level of polish I'd like &mdash; they have many more errors and typos and may have several lectures in which the LaTeX or sentence structure is messed up (typically due to me not being able to type fast enough), and most diagrams are incomplete &mdash; and may get updated in the future. 

<!-- <hr> -->
{% assign notes = site.lecture_notes | sort: 'code' %}
{% include lnotes.html notes=notes %}

I've also contributed to the MIT OCW student notes for <a href="https://ocw.mit.edu/courses/res-18-011-algebra-i-student-notes-fall-2021/" target="_blank">18.701</a> and  <a href="https://ocw.mit.edu/courses/res-18-012-algebra-ii-student-notes-spring-2022/" target="_blank">18.702</a>.

<!-- ### Combinatorics Seminar
{% assign notes = site.seminar_notes | where: 'series', 'Combinatorics' | sort: 'date' | reverse %}

{% include talks.html talks=notes %}
 -->



### Reading Group Seminar
{% assign notes = site.seminar_notes | where: 'series', 'Reading Group' | sort: 'date' | reverse %}

{% include talks.html talks=notes %}



