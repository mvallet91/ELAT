---
title: Quick Start Guide
permalink: /docs/quickstart/
---

### Quick-Start Guide
1. Go to the ELAT [workbench](https://mvallet91.github.io/ELAT-Workbench/) on Google Chrome *(it's highly 
recommended that you close other tabs on your Chrome browser, as well as other applications on your computer)* 
2. Click on **Upload Files** to show 2 upload fields, one for Metadata and one for Logfiles
3. Upload **all** metadata files for a **single course**, this should take a minute or less.
Reload the page when prompted
4. Upload all the **logfiles** (named something like _university-edx-events-2014-10-25.log.gz_) between the 
starting and ending dates of the course, this will take around **2 to 3 hours**. Once the process starts,
 tou can leave it working. Reload the page when prompted
5. Some main indicators and plots will be generated in the page, it will take a few minutes the first time
6. The processed [tables](https://github.com/AngusGLChen/DelftX-Daily-Database#database-schema) 
can be obtained in csv format for further analysis with the **download** buttons
7. (Optional) After obtaining the csv files, it is possible to delete the database by pressing the
 *Clear Database* button at the bottom. This allows to repeat the process for another course
 
### Step-by-Step Detail: 
(images may change as prototype is reviewed)

#### 1. Workbench Home (Describe Course Details and Database Details)

On the first visit to ELAT, a database will be created in the browser's IndexedDB, and a message will appear (as shown in the picture below) 
![alt text](/ELAT/img/S01.png "Workbench Welcome")

#### 2. Upload Files

By clicking the button 'Upload Files', two file managers will appear, one for metadata and one for logs. Metadata goes first, since it contains all the course information: course id, component ids, students, etc.
![alt text](/ELAT/img/S02.png "Upload Files")

#### 3. Metadata

ELAT needs **ALL** the metadata files to work correctly: `auth_user-prod-analytics.sql`, `auth_userprofile-prod-analytics.sql`, `certificates_generatedcertificate-prod-analytics.sql`, `course_structure-prod-analytics.json`, etc. Don't worry about formats or names, just upload all the metadata files for a single course.
![alt text](/ELAT/img/S03.png "Upload ALL Metadata")
![alt text](/ELAT/img/S04.png "Processing Metadata")
Once ELAT is finished with the metadata processing, reload the page.
![alt text](/ELAT/img/S05.png "Done with Metadata")

#### 3a. Metadata Indicators

The first indicators will appear in the dashboard, in the `Course Indicators` table. *Course Name, Start and End Time, Course Elements* (all the components of the course: videos, quizzes, assessments, etc.), *Quiz Questions* (all individiual questions existing in the couse) and *Forum Interactions* (the count of all posts and comments in the Forum of the cousrse).
The values that can be obtained from metadata will appear in the `Main Indicators` table, *Completion Rate* (students that finish the course), *Average Grade* (for students that completed the course) *Number of Students per Enrolment Mode* (only with completed course), and their *Average Grades per EM*. The *Time Spent on Video* and *Students that Watched at Least one Video* require the logfile data, so they will not be filled yet.
![alt text](/ELAT/img/S06.png "Metadata Indicators")

#### 4. Logfiles 

Similar to metadata, press the 'Upload Files' button, then under 'Select log files', and select all log files corresponding to the course. For example `FP101x` runs from October 15, 2015 to January 5, 2016. In the image below you can see that **83 files** were uploaded. 
![alt text](/ELAT/img/S07.png "Processing Metadata")
This step can take a few hours. It is recommended to close other tabs on the browser and other applications.

Once the log processing is done, ELAT will prompt again to reload the page.
![alt text](/ELAT/img/S08.png "Done with logfiles")

#### 5. Dashboard: Indicators (Describe Indicators and Graphs)
When reloaded, it will automatically start processing indicators and graph data, updating with every step.
![alt text](/ELAT/img/S09.png "Processing indicatos and graph")

 The values for *Time Spent on Video* and *Students that Watched at Least one Video* will be prepared, as well as the graphs.
 ![alt text](/ELAT/img/S10.png "More indicators")

 ![alt text](/ELAT/img/S11.png "Sessions and Students per Day")
 ![alt text](/ELAT/img/S12.png "Average Session Duration, with Video, Quiz and Forum sessions per Day")
 ![alt text](/ELAT/img/S13.png "Zoom-able Sessions per Day")
 ![alt text](/ELAT/img/S14.png "Weekly Forum Analysis")

#### 6. Download Files
By pressing the corresponding button, ELAT will prepare the csv file(s) for download. The fields of each one are described in [sessions](/ELAT/docs/sessions)
 ![alt text](/ELAT/img/S15.PNG "Download files")
 
#### 7. Delete
The button to delete everything is located under the `Course Details` table. If confirmed, it will delete all the information, tables and schema from your machine. This allows to start the process again, with a new course for example.
 ![alt text](/ELAT/img/S16.PNG "To Delete or not to Delete")
