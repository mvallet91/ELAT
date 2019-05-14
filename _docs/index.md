---
title: Welcome
permalink: /docs/home/
redirect_from: /docs/index.html
---

### The Project
Based on the [edx-analysis](https://github.com/chauff/edx-analysis) approach, ELAT is a web-based tool to 
parse the edX metadata and logfiles into a database that can generate different outputs, mainly **csv** 
files and visualizations. 
Given that each edX logfile contain the interactions of **all** learners for **all** the courses of an 
institution throughout a day, filtering the useful information for a researcher or instructor of a particular 
course may be time consuming and challenging.
Here is where ELAT shines, as it handles all the pre-processing of the logfiles and generates structured 
data that can be used for further analysis, such as learning experiments in R or Python.
The process is fully executed in client side (on the browser) to minimize privacy and security risks. 

#### Who is ELAT for?
- Researchers and educators interested in analyzing and experimenting on the edX platform, using 
the information generated through log files (especially users with less programming experience).

#### Processing Details/Limitations
- Currently ELAT can only store a **single course** at a time, but the database can be easily deleted and then other courses can be analysed  
- Currently ELAT is only developed and tested in **[Google Chrome](https://www.google.com/chrome/)**
- ELAT considers a "session" as a **series of events** of a certain type for a single user. For example a *video interaction session* consists of a series of events where a student interacts with a video: start the video, pause, rewind, and any other events until closing the video. The minimum duration of any type of session (from first to last event) is **5 seconds**. Series of events with less than **5 seconds between them are discarded**.
- Because of processing limitations, currently ELAT cannot retain sessions that start one day and end in the next one since they belong to different files and the files are processed one by one automatically. This means that some **overnight sessions are lost** 
-Because of file size limitations, ELAT has to **chunk** some of the bigger into 2 smaller pieces. It does not happen often, but when it does, the sessions that start in a chunk and continue to the next are lost, like overnight sessions 

#### Current Status
- Running everything on client side, no server, no information sent over the internet. Deployed on GitHub Pages 
- Working IndexedDB database, [schema](https://github.com/AngusGLChen/DelftX-Daily-Database#database-schema) adapted from the [MOOCdb Model](http://mooclearnerproject.csail.mit.edu/)
- Decompress and process gzip files in JavaScript
- Take contents from decompressed log file, ~~pass to Python script~~, process in-browser, then store into IndexedDB
- Generate main indicators and graphs
- Generate and Download csv files for the tables in the database

