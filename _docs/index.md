---
title: Welcome
permalink: /docs/home/
redirect_from: /docs/index.html
---

### Quick Guide
1. Go to the ELAT [homepage](https://mvallet91.github.io/untitled/) on Google Chrome *(it's highly recommended that you close all other tabs on your Chrome browser, as well as other applications on your computer)* 
2. Click on **Upload Files**
3. Upload all metadata files for a **single course** (unzipped) and reload the page when prompted
4. Upload all **log files** between the starting and ending dates of the course and reload the page when prompted
5. Some main indicators and plots will be generated in the page
6. With the **download** buttons, the processed information can be obtained in csv format for further analysis

### Project
Based on the [edx-analysis](https://github.com/chauff/edx-analysis) tool, this is a web-based solution to parse the edX log and metadata files, into different possible outputs, ranging from csv files to visualizations. 
edX log files contain all learners' activity for all the courses of an institution, and filtering the useful information for a researcher or instructor of a particular course may be time consuming and challenging, especially for users with less computer-related experience.
The process is fully executed in client side (on the browser) to minimize privacy and security risks. 

#### Processing Details/Limitations
- Currently ELAT can only store a **single course** at a time, but the database can be easily deleted and then other courses can be analysed  
- Currently ELAT is only developed and tested in **[Google Chrome](https://www.google.com/chrome/)**
- ELAT considers a "session" as a **series of events** of a certain type for a single user. For example a *video interaction session* consists of a series of events where a student interacts with a video: start the video, pause, rewind, and any other events until closing the video. The minimum duration of any type of session (from first to last event) is **5 seconds**. Series of events with less than **5 seconds between them are discarded**.
- Because of processing limitations, currently ELAT cannot retain sessions that start one day and end in the next one since they belong to different files and the files are processed one by one automatically. This means that some **overnight sessions are lost** 
-Because of file size limitations, ELAT has to **chunk** some of the bigger into 2 smaller pieces. It does not happen often, but when it does, the sessions that start in a chunk and continue to the next are lost, like overnight sessions 

#### Current Status
- Running everything on client side, no server, no information sent over the internet. Deployed on GitHubPage 
- Working IndexedDB database (automatically generated schema) 
- Decompress and process gzip files in JavaScript
- Take contents from decompressed log file, ~~pass to Python script~~, process in-browser, then store into IndexedDB
- Generate main indicators and graphs
- Generate and Download csv files for the tables in the database

