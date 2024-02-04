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

Here are notes from some of the classes I've taken at MIT. (I type notes during class; for fun, I occasionally return to old notes to clean them up and add diagrams. The notes posted here are ones for which I have already done so; it's likely that notes from more of my classes will appear at some indefinite point in the future.) All course content is owned by MIT and its professors, while all errors should be attributed to me (either in transcription or interpretation). 

<!-- <hr> -->
{% assign notes = site.lecture_notes | sort: 'code' %}
{% include lnotes.html notes=notes %}

I've also contributed to the MIT OCW student notes for <a href="https://ocw.mit.edu/courses/res-18-011-algebra-i-student-notes-fall-2021/" target="_blank">18.701</a> and  <a href="https://ocw.mit.edu/courses/res-18-012-algebra-ii-student-notes-spring-2022/" target="_blank">18.702</a>.

<!-- ### Combinatorics Seminar
{% assign notes = site.seminar_notes | where: 'series', 'Combinatorics' | sort: 'date' | reverse %}

{% include talks.html talks=notes %}
 -->



### Reading Group Seminar

Here are notes from a reading group seminar at MIT. As before, all errors are due to me, not the presenter. 

{% assign notes = site.seminar_notes | where: 'series', 'Reading Group' | sort: 'date' | reverse %}

{% include talks.html talks=notes %}



