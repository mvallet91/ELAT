---
layout: post
title:  "Mobile Video Interaction"
date:   2019-05-21 11:44:19
author: Manuel
---

#### Mobile records vs Browser records
This is the second special case that ELAT had to tackle, and it was caused by the introduction of the edX mobile
app, which changed the structure of video related records compared to the browser video records.
*Discovered and fixed 21/05/2019*

This is how a `pause_video` record looks like:

````
{
  "accept_language": "en-IN",
  "agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.79 Safari/537.36 Edge/14.14393",
  "context": {
    "course_id": "course-v1:DelftX+EX101x+3T2016",
    "org_id": "DelftX",
    "path": "/event",
    "user_id": 888888
  },
  "event": "{\"code\": \"zppgbNoqBBI\", \"id\": \"ba666985e20d4eb18e593461c28790c0\", \"currentTime\": 303.9990418602173}",
  "event_source": "browser",
  "event_type": "pause_video",
  "host": "courses.edx.org",
  "ip": "N.N.N.N",
  "name": "pause_video",
  "page": "https://courses.edx.org/courses/course-v1:DelftX+EX101x+3T2016/courseware/153c585b5fc643008fafb60c35aaa575/9070b59046dc453998e33b2ccf344ff5/?child=first",
  "referer": "https://courses.edx.org/courses/course-v1:DelftX+EX101x+3T2016/courseware/153c585b5fc643008fafb60c35aaa575/9070b59046dc453998e33b2ccf344ff5/?child=first",
  "session": "8940f3610cfb8825b7a7cd6b85bfa52f",
  "time": "2016-12-16T01:43:57.340314+00:00",
  "username": "AAAAAA"
}
````

This is another `pause_video` record: 

````
{
  "accept_language": "",
  "agent": "Dalvik/2.1.0 (Linux; U; Android 6.0.1; SM-N920C Build/MMB29K)",
  "context": {
    "application": {
      "name": "edx.mobileapp.android",
      "version": "2.7.5"
    },
    "client": {
      "app": {
        "build": 2007005,
        "name": "edX",
        "namespace": "org.edx.mobile",
        "version": "2.7.5"
      },
      "device": {
        "adTrackingEnabled": false,
        "advertisingId": "ad_id",
        "id": "hex_id",
        "manufacturer": "samsung",
        "model": "SM-N920C",
        "name": "noblelte",
        "type": "android"
      },
      "ip": "N.N.N.N",
      "library": {
        "name": "analytics-android",
        "version": "3.4.0"
      },
      "locale": "en-GB",
      "network": {
        "bluetooth": false,
        "carrier": "STC",
        "cellular": false,
        "wifi": true
      },
      "os": {
        "name": "Android",
        "version": "6.0.1"
      },
      "screen": {
        "density": 3.5,
        "height": 2560,
        "width": 1440
      },
      "timezone": "X/X"
    },
    "component": "videoplayer",
    "course_id": "course-v1:DelftX+EX101x+3T2016",
    "org_id": "DelftX",
    "path": "/segmentio/event",
    "received_at": "2017-02-09T07:39:17.857000+00:00",
    "user_id": 999999
  },
  "event": {
    "code": "mobile",
    "currentTime": 33.751,
    "id": "event_id"
  },
  "event_source": "mobile",
  "event_type": "pause_video",
  "host": "courses.edx.org",
  "ip": "",
  "name": "edx.video.paused",
  "page": "https://courses.edx.org/xblock",
  "referer": "",
  "session": "",
  "time": "2017-02-09T07:38:48.857000+00:00",
  "username": "XXXXXXXX"
}
````

The records look pretty different, and that is because the second one is for a student using the edX mobile app,
which apparently can track much more information about the device being used to interact with the video.
The important difference, however, is in the `event` field of both records, since ELAT uses this to find the 
*id of the video* and process the session of a student interacting with that particular video.
This is how it looks in the browser record (which was the type of record used to develop ELAT):
````
 "event": "{\"code\": \"zppgbNoqBBI\", \"id\": \"ba666985e20d4eb18e593461c28790c0\", \"currentTime\": 303.9990418602173}"
````
While for the mobile record, it looks like this:
````
"event": {
  "code": "mobile",
  "currentTime": 33.751,
  "id": "39b0ffc9bfab486fa9ab479f64d730b3"
}
````
In the first one, the whole value for the `event` field is a string, not a JSON object, therefore the  
quotes are being ["escaped"](https://en.wikipedia.org/wiki/Escape_character) by adding a backslash 
like this `\`  in front of them. 
To process this values, ELAT had to read this string and parse it again to create a new "mini-object" 
for the event, and then read the values of each field.
The second one is a normal nested JSON object (check the `context` field of both records, those are nested JSON
objects), so the quotes do not have to be escaped.

##### Discovery
As we tested ELAT on newer courses (read: with mobile viewing records) some course interaction entries were being processed
with no video_id, which raised an error in the database, because it is a required field.
Then it was a matter of diving into log files, searching for the user id and date of those video interactions to find that those
records did have a video id, but in different format.

##### Fix
We may never find out why it was recorded as a string in the browser records, but as an object in the mobile app,
all we know is that ELAT has to identify video records from a mobile application and process them differently than 
the browser records. 
