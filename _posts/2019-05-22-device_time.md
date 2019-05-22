---
layout: post
title:  "Device time lies"
date:   2019-05-22 10:06:43
author: Manuel
---

#### Mobile records strike again

This is a weird one, and it appears once again in video records for the mobile edX app. 
When processing a record, ELAT trusts that the information is correct, and there is no way to verify 
many of the values provided.
Maybe it is possible to check if the referred course element exists, but whatever is written on the record is
assumed as the truth.
The problem here is that some records have a timestamp like they happened days before the current logfile, and
that is not possible, since the logs only record events for one day!
Look at the record below (some info was removed because it was not useful now):
````
{
  "accept_language": "",
  "agent": "Dalvik/2.1.0 (Linux; U; Android 6.0.1; Che1-L04 Build/Che1-L04)",
  "context": {
    "application": {
      "name": "edx.mobileapp.android",
      "version": "2.7.5"
    },
    "client": {
      ...client info...
    },
    "component": "videoplayer",
    "course_id": "course-v1:DelftX+AE1110x+2T2017",
    "org_id": "DelftX",
    "path": "/segmentio/event",
    "received_at": "2017-05-15T02:11:53.196000+00:00",
    "user_id": 9999999
  },
  "event": {
    ...event info...
  },
  "event_source": "mobile",
  "event_type": "seek_video",
  "host": "courses.edx.org",
  "ip": "",
  "name": "edx.video.position.changed",
  "page": "https://courses.edx.org/xblock",
  "referer": "",
  "session": "",
  "time": "2017-05-02T06:59:46.196000+00:00",
  "username": "AAAAAA"
}
````
In the `context` field, it states that the record was `received_ at` the 15 of May, just like all the other records of the
`delftx-edx-events-2017-05-15.log` file.
The `time` field, however, refers to the 2 of May, at a completely different time (for all other records, the values of 
`received_at` and `time` had differences of less than one second).
One explanation could be if edX app takes the time of the device, and this particular user had the time wrong for some reason.
Or maybe the user downloaded, and watched a video the 2nd of May with the app offline, and did not go online until the 15 of 
May on that device.
All records of this user had `time` and `received_at` within a small time frame, so it's not useful to determine which option 
is more likely to be correct.
In addition, the official starting date of the course is May 1st, so both explanations are still plausible. 
Another case of series of video interaction records from the mobile app was found, and in this case it was very clear 
that all records were `received_at` approximately the same time, but the `time` of each record was throughout almost an hour.
We can assume that the mobile app allows users to interact with the video elements and then they get recorded or *received*
when the user goes online, at a different time.

##### Discovery
One of the options for the date range of the graphs is "All uploaded files", perfect if the user wants to visualize sessions
outside of the official course dates, or to verify the range of all the logfiles they have uploaded.
Some courses however, started showing values in dates that were never uploaded to the running instance of ELAT. 
The chart below shows the sessions from the 10 to 15 of May 2017, for the `AE1110x-2T2017` course, 
which runs from May 1st 2017 to March 2018. There is one session from one student the 2nd of May, and that logfile
was never processed by ELAT, which corresponds to the record shown above.
![alt text](/ELAT/img/barChart_badDates.png "Date Error Example")

Once again, the only way to find the source of this issue was to find the "out of range" session in the database, and then go 
file by file searching for the user id on the rouge session until the record appeared, showing different the value of the 
incorrect date in the `time` field, while the `received_at` field was in the correct day.

##### Fix
Given that the error is extremely rare, showing up for one student for one session out of thousands, and we are not sure of
the reason for this time difference, these sessions will be kept, since they belong to the time frame of the course.

