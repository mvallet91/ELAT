---
title: Process - Under the Hood
permalink: /docs/process/
---

### Process Pipeline

##### 1. User enters the website:
As soon as the interested user visits the ELAT workbench, the 
[schema](https://github.com/AngusGLChen/DelftX-Daily-Database#database-schema) for the database is generated
in the local IndexedDB.

##### 2. User uploads metadata files to browser:
By uploading the metadata files, ELAT obtains all the information from the course itself:
- Basic course information such as identifier, name, start and finish date
- List of students with their characteristics: demographics, registration, status, etc. 
- Course structure: lists of all the videos, quizzes, questions, assessments, etc
- Forum information: Posts and comments with date, author, etc.
- And more

##### 3. User upload log files to browser: 
The user must upload all files in the time period between start and end of the course.
The logfiles are named like this: _delftx-edx-events-2014-10-25.log.gz_. 
This is the institution name, followed by _-edx-events-_ and the date of the file. 
The format of the file is _.log_ and it is compressed using **gzip**, that's why it ends with _.gz_. 
ELAT will decompress the files, so don't worry about this.

##### 4. Log file(s) are processed in browser 
Each log file contains every interaction of every student for all active courses of an institution. 
For example, for [DelftX](https://www.edx.org/school/delftx) (the TU Delft MOOC profile in edX) there can be 
dozens of courses active simultaneously, and the activity of a single day can have hundreds of thousands records. 
Once decompressed, the logfile is just a long list of JSON format records ordered by time:
![alt text](/ELAT/img/logfile_1.PNG "Just a bunch of text...")

A single record looks like this: 
````
{"username": "SAMPLE_USER", "event_source": "browser", "name": "play_video", "accept_language": "en-US,en;q=0.8,pl;q=0.6", "time": "2015-10-15T16:15:36.283878+00:00", "event": "{\"code\": \"8PkkRiTgtFY\", \"id\": \"6b3be05bc6734592acd8bea94f1e7424\", \"currentTime\": 1}", "agent": "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/45.0.2454.85 Safari/537.36", "label": "course-v1:DelftX+FP101x+3T2015", "host": "courses.edx.org", "session": "038a12d0fb08197db0f3fc7aeefd0f18", "referer": "https://courses.edx.org/courses/course-v1:DelftX+FP101x+3T2015/courseware/79633017711e44c9878df4f337014766/65e74e8c3eff465496e0cbedae8eab45/", "context": {"user_id": 999999, "org_id": "DelftX", "course_id": "course-v1:DelftX+FP101x+3T2015", "path": "/event"}, "ip": "N.N.N.N", "page": "https://courses.edx.org/courses/course-v1:DelftX+FP101x+3T2015/courseware/79633017711e44c9878df4f337014766/65e74e8c3eff465496e0cbedae8eab45/", "nonInteraction": 1, "event_type": "play_video"}
````
We can organize it for easier human reading:
````
{
  "username": "SAMPLE_USER",
  "event_source": "browser",
  "name": "play_video",
  "accept_language": "en-US,en;q=0.8,pl;q=0.6",
  "time": "2015-10-15T16:15:36.283878+00:00",
  "event": "{\"code\": \"8PkkRiTgtFY\", \"id\": \"6b3be05bc6734592acd8bea94f1e7424\", \"currentTime\": 1}",
  "agent": "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/45.0.2454.85 Safari/537.36",
  "label": "course-v1:DelftX+FP101x+3T2015",
  "host": "courses.edx.org",
  "session": "038a12d0fb08197db0f3fc7aeefd0f18",
  "referer": "https://courses.edx.org/courses/course-v1:DelftX+FP101x+3T2015/courseware/79633017711e44c9878df4f337014766/65e74e8c3eff465496e0cbedae8eab45/",
  "context": {
    "user_id": 999999,
    "org_id": "DelftX",
    "course_id": "course-v1:DelftX+FP101x+3T2015",
    "path": "/event"
  },
  "ip": "N.N.N.N",
  "page": "https://courses.edx.org/courses/course-v1:DelftX+FP101x+3T2015/courseware/79633017711e44c9878df4f337014766/65e74e8c3eff465496e0cbedae8eab45/",
  "nonInteraction": 1,
  "event_type": "play_video"
}
````
This particular event indicates that the learner **999999** of the course **DelftX+FP101x** started watching 
video **79633017711e44c9878df4f337014766** at **16:15:36** of October 15, 2015.

ELAT will process the files by session type, for instance, the steps for *video* sessions are the following: 

 1. Read each record in the file and filter the ones that correspond to the course it's currently processing, that refer to a video component
 
 2. Group all these video records by the learner id, and order them by timestamp
 
 3. From the set of video records for a learner, for instance **999999**, ELAT will process ordered records one by one and count pauses, forward and backward seeks, and other video-related activities to form a [session](/ELAT/docs/sessions)
 
 4. If the time between video records for a learner exceeds 30 minutes, the [session](/ELAT/docs/sessions) is finished and a new one is started
 
 This process is executed for General Sessions (i.e. any activity in the course, including the rest of the types), Forum Sessions, Problem Sessions (Quiz/Assessment) and Video Sessions.
   
##### 5. Info is stored in browser 
Once processed, the sessions are stored into the [database](https://github.com/AngusGLChen/DelftX-Daily-Database#database-schema).

##### 6. System prepares indicators, graphs and files for download 


### Architecture
![alt text](/ELAT/img/elat_architecture.png "ELAT Modules")

