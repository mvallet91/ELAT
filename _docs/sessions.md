---
title: What are Sessions? 
permalink: /docs/sessions/
---
### (work in progress)
 
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

#### Session Types
- Session: This is the general session, it consist only of the *session_id, learner_id, 
start_time, end_time* and *duration*. The session starts when the student opens any element related to 
the course, and it continues throughout the whole interaction, for example, the student may watch a video, 
go to ask a question in the forum and then solve a quiz.
The session, as well as all other types of sessions, ends when the video is closed, or after the mentioned
inactivity period.
- Video Interaction Session: The session starts when a video element is opened, and it comprises *session_id, 
learner_id, start_time, end_time, duration, video_id, times_pause, duration_pause, times_speed_up, 
times_speed_down, times_forward_seek, duration_forward_seek, times_backward_seek* and *duration_backward_seek*.
- Forum Interaction Session: 
- Quiz Session: 
- ...
 
 
