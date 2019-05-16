---
title: Use Cases - Examples
permalink: /docs/use_cases/
---
### (work in progress)

The main use is to pre-process the events of the logfiles into structured data, to be used in further
analysis with tools ranging from Excel to R or Python. 

#### Basic use:
Alice is a Pedagogy Researcher and her institution has several courses on edX. She is interested in 
the effect that adding short questions during videos may have on the final grade of students. She wants to
do weekly evaluations using the edX log file information, but she is not familiar with data cleaning
techniques and the data generated is simply overwhelming. To avoid this, she can provide this to ELAT and
obtain csv files with the video interactions of the students and easily match them to their grades.

#### Example with R:
Bob is working together with Alice, he is involved in Educational Policy for their institution, and has
a strong background in statistics. He knows his way around R studio, but manipulating dozens of logfiles
and extracting information from JSON records doesn't sound like fun. He can use ELAT to get files in csv
format of several courses to gauge student engagement and decide the best course of action to increase
course completion rates.

Add workflow chart

Add sample R script

#### Example with Python:
