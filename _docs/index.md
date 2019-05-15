---
title: Welcome
permalink: /docs/home/
redirect_from: /docs/index.html
---

### The Project
ELAT is a web-based tool to pre-process the edX metadata and logfiles into a structured database, 
to generate **csv** files for analysis and visualizations.
Given that each edX logfile contain the interactions of **all** learners for **all** the courses of an 
institution throughout a day, filtering the useful information for a researcher or instructor of a particular 
course may be time consuming and challenging.
Here is where ELAT shines, as it handles all the pre-processing of the logfiles and generates structured 
data that can be used for further analysis, such as experiments in R or Python.
The process is fully executed in client side (on the browser) to minimize privacy and security risks. 
It is developed in the Delft University of Technology, based on the [edx-analysis](https://github.com/chauff/edx-analysis) 
approach, read more about it [here](/ELAT/docs/about). 

#### Who is ELAT for?
Researchers, lecturers, educators, and anyone interested in analyzing and experimenting on the edX platform, 
using the information buried in logfiles.
It requires minimal effort, no installation, no programming experience.

#### Important Details/Limitations
- Currently ELAT can only store a **single course** at a time, but the database can be easily deleted 
and then other courses can be analysed  
- Currently ELAT is only **developed and tested** in [Google Chrome](https://www.google.com/chrome/)
- The minimum duration of a [session](/ELAT/docs/sessions) (from first to last event) is **5 seconds**. 
Series of events with less than 5 seconds between start and finish are **discarded**.
(ELAT considers a ["session"](/ELAT/docs/sessions) as a **series of events** of a certain type 
for a single user.)
- [Sessions](/ELAT/docs/sessions) end when the student closes the course element they were interacting with, or after a period 
of inactivity. Currently, this period is **30 minutes**.
- Because of processing limitations, currently ELAT cannot retain [sessions](/ELAT/docs/sessions) that start one day and 
end in the next, since they belong to different files and the files are processed one by one. 
This means that some **overnight sessions are split into two separate sessions** 
- Because of file size limitations (files over ~73MB compressed, ~2GB uncompressed), ELAT has to **chunk** 
some of the bigger into smaller pieces. It does not happen often, but when it does, the sessions that 
start in a chunk and continue to the next are split in two, like overnight sessions.

#### Current Status
- Running everything on client side, no server, no information sent over the internet. 
- Deployed on GitHub Pages 
- Working IndexedDB database, [schema](https://github.com/AngusGLChen/DelftX-Daily-Database#database-schema) adapted from the [MOOCdb Model](http://mooclearnerproject.csail.mit.edu/)
- Decompress and process gzip files in JavaScript
- Take contents from decompressed log file, process in-browser, then store into IndexedDB
- Generate main indicators and graphs
- Prepare csv files for download from the tables in the database

