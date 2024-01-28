---
layout: page
permalink: /notes/
title: Notes
description: Lecture notes from classes I have taken and notes from seminars I have attended.
nav: true
nav_order: 1
series: [Combinatorics, Reading Group]
---


{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}


### Lecture Notes
Notes are published with the permission of the course staff, but please note that all course content is owned by MIT and the course instructors. All mistakes should be attributed to me (in transcription or interpretation -- they were not checked by instructors).
<!-- <hr> -->
{% assign notes = site.lecture_notes | sort: 'date' | reverse %}
{% include lnotes.html notes=notes %}

### Combinatorics Seminar
{% assign notes = site.seminar_notes | where: 'series', 'Combinatorics' | sort: 'date' | reverse %}

{% include talks.html talks=notes %}




### Reading Group Seminar
{% assign notes = site.seminar_notes | where: 'series', 'Reading Group' | sort: 'date' | reverse %}

{% include talks.html talks=notes %}



