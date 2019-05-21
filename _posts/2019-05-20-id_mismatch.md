---
layout: post
title:  "ID Mismatch"
date:   2019-05-20 13:15:53
author: Manuel
---

#### Old video, New course
The first of many adaptations for ELAT! This was not necessarily a change on the data, but a situation that 
was not foreseen during development: some courses have many life cycles! This means that some course elements 
(like videos or quizzes) will have very similar ids, which can cause a mismatch when processing a course 
that has run in previous editions.
*Discovered and fixed 20/05/2019*

If you remember the [process](/ELAT/docs/process) pipeline, once the metadata is processed, ELAT will check 
every log record for the current **course id**, to avoid unnecessary processing for records of other courses.
The initial implementation, however, was only tested with courses that were first of their series, so there
was never a case where one of the courses had already happened. 
With further evaluation, it was discovered that the course id, for example **FP101x** or **EX103x**, was present
in records from the 2015, 2016 and 2017 editions. Since some courses allow to be self-paced, it can happen that 
students were enrolled in a previous edition, and their interaction records appear in logs together with the records 
of students in the latest edition. 

The easiest way to fix this is by expanding the course id to include the edition, so **FP101x** is now **FP101x-3T2015**
and **EX103x** is **EX103x-2T2016**. All course elements contain this longer identifier and checking for this compared to
 the short version caused no issues in the performance or results of ELAT.
