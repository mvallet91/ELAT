---
title: Special Cases
permalink: /docs/special_cases/
---

### Edge Cases
As technology keeps changing, edX has to adapt to stay as one of the top online education platforms.
among many other effects, these adaptations will certainly impact the structure of the records in logfiles,
which means that ELAT has to adapt for new types of records and changing structures.
In this section we will show and describe the special cases that we encounter while processing edX log data.

#### Old video, New course
The first of many adaptations for ELAT! This was not necessarily a change on the data, but a situation that 
was not foreseen during development: some courses have many life cycles! This means that some course elements
will have very similar ids, which can cause a mismatch when processing a course that has previous implementations.

*Discovered and fixed 20/05/2019*

[Read More](/ELAT/blog/2019/05/20/id_mismatch/)

#### Mobile Video Watching
This is the second special case that ELAT had to tackle, and it was caused by the introduction of the edX mobile
app, which changed the structure of video related records compared to the browser video records.

*Discovered and fixed 21/05/2019*

[Read More](/ELAT/blog/2019/05/21/mobile_video/)
