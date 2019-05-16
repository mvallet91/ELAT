---
title: Output Data: Sessions Information
permalink: /docs/sessions/
---
### Learner Sessions
ELAT considers a **session** as an ordered series of events where a student or learner interacts with the
edX learning system, from the moment the student opens the course homepage, every interaction such as 
a mouse click to open a lesson, then a video or a quiz, with every pause or every answer, respectively. 
A session ends when the student closes the element they were interacting with, or after a long period
of inactivity, this is currently **30 minutes**.

#### Why use sessions?
The use of sessions is common for log tracking analysis, since the records in logfiles are only
isolated events, a real notion of student's engagement can be obtained from their behavior during a period
of interaction: a session.

#### Database Details: Session Types
Once ELAT has [processed](/ELAT/docs/process) all the logfiles, the sessions are stored in the database
according to the [schema](https://github.com/AngusGLChen/DelftX-Daily-Database#database-schema) adapted 
from the [MOOCdb Model](http://mooclearnerproject.csail.mit.edu/). Each of the following session types 
is also a table in the database, and can be downloaded for further analysis.
- Session: This is the general session, it consist only of the *session_id, course_learner_id, 
start_time, end_time* and *duration*. The session starts when the student opens any element related to 
the course, and it continues throughout the whole interaction, for example, the student may watch a video, 
go to ask a question in the forum and then solve a quiz.
The session, as well as all other types of sessions, ends when the video is closed, or after the mentioned
inactivity period.
- Video Interaction: The video session starts when a video element is opened, and it comprises *session_id, 
course_learner_id, start_time, end_time, duration, video_id, times_pause, duration_pause, times_speed_up, 
times_speed_down, times_forward_seek, duration_forward_seek, times_backward_seek* and *duration_backward_seek*.
- Forum Session:  These sessions involve the time spent in forums, reading and searching for posts, 
 but not actually posting anything. The stored values are *session_id, course_learner_id, start_time, end_time, 
 duration, times_search* and *relevant_item_id*
- Forum Interaction: These are different to Forum Sessions, because they are not a series
of events over time, but the single action of a user posting something in a forum *course_learner_id, post_id, 
post_title, post_content, post_type, post_timestamp, post_thread_id* and *post_parent_id*
- Quiz Questions: The list of questions in a course, can be used together with Submissions *question_id, 
question_type, question_weight* and *question_due*
- Quiz Session: Simply the time spent in a quiz element of the course *session_id, course_learner_id, 
start_time, end_time* and *duration*
- Submission: Isolated action of a learner submitting an answer to a question *course_learner_id, 
submission_id, question_id* and *submission_timestamp*
- Assessment: The result of an assessment *course_learner_id, assessment_id, grade* and *max_grade*
 
 
