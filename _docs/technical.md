### Technical Sheet

#### Core Use Cases - End Users
- Researchers and educators interested in analyzing and experimenting on the edX platform, using the information generated through log files (especially users with less programming experience).

#### Requirements

- Web application (fully client-side - platform agnostic)

- Persistent database (stored in the client machine, via IndexedDB, such as _%LocalAppData%\Google\Chrome\User Data\Default\Local Storage_)

- Input: edX metadata files, edX daily log files in gzip format, as obtained from the institution's [_data czar_](https://edx.readthedocs.io/projects/devdata/en/stable/internal_data_formats/data_czar.html), who in turn obtains everything from edX

- Outputs: csv files (one for each table: sessions, video_interactions, quiz_sessions, etc.), reports with aggregated information (metrics and indicators TBD) and with temporal values (also TBD)

#### Process Pipeline

1.- User accesses website

2.- User uploads metadata files to browser 

3.- User specifies configuration options

    a.- Select course identifier 
    
    b.- Select modes: Learner, Video, Forum, Quiz... 
    
    c.- Select data output: full csv, reports, metrics
    
    d.- Select visualizations types: Video Interactions, Behavior Pattern, from reports...  and granularity: total, by module, week...

4.- User upload log file(s) to browser 

5.- Log file(s) are processed in browser 

7.- Info is stored in browser 

8.- System returns the selected files for download

### Development Choices/Challenges

- Website deployment

    - Currently github pages

- Uploading files to browser using [FileReader](https://developer.mozilla.org/en-US/docs/Web/API/FileReader) 

    - In gzip format, as obtained from Data Czar/edX
        
- Local processing

    - **efficient?** Good enough!
    
    - **JS vs Python = translate or transpile** Definitely translate to JS (Python-in-the-browser was more than 200 times slower!)

- Translation Script: Python vs JavaScript

    - **Winner** Use [Transcrypt](https://www.transcrypt.org/) or [Javascripthon](https://pypi.org/project/javascripthon/) to transform the *translation* module from Python to JavaScript, correct, adapt and add I/O modules
        
    - **Runner-up** Use [Brython](https://www.brython.info/index.html) to run the *translation* module in Python in the browser and handle I/O with JavaScript
    
    - **Not necessary** Write from scratch in JavaScript 

- Database

    - [JsStore](https://github.com/ujjwalguptaofficial/JsStore) + [SQLWeb](https://github.com/ujjwalguptaofficial/sqlweb) + [IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API) SQLWeb parses regular SQL queries into api calls for JsStore, which is an SQL-focused wrapper for IndexedDB, a local key-value store in most modern browsers. Might be problematic when adding new rows from logs, and for the metrics queries (especially JOINS) already created in SQL.  
    
    - [WebStorage](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API) **(worst option)** to store JS serialized objects: localStorage (persist in browser) or sessionStorage (cleared when browser is closed), Max 5 MB Chrome, 10 MB Firefox, per origin

#### Existing Tools

- edX [Analytics](https://github.com/edx/edx-analytics-pipeline) and Insights](https://edx.readthedocs.io/projects/edx-insights/en/latest/): Proprietary pipeline for the edX [analytics dashboard](https://github.com/edx/edx-analytics-dashboard) and [edX Insights](https://edx.readthedocs.io/projects/edx-insights/en/latest/index.html#)

- Event log parser [pyedx](https://github.com/epfl-cede/pyedx): Written in Python, it parses the edX log file into CEDE (Center for Digital Education) format JSON, used by the Ecole Polytecnique Federale de Lausanne (EPFL)

- edX Student and Course Analytics and Visualization [Pipeline](https://github.com/cns-iu/edx-learnertrajectorynetpipeline): Set of R scripts to process edX Data Package files to create tables and learning trajectory visualizations, from Cyberinfrastructure for Network Science Center (CNS) at Indiana University

- [edX Tools](https://github.com/edx/edx-tools/wiki): Set of (easy, medium and advanced) tools created by various iniversities and research institutions for different purposes, from RSS generators to data parsers into sql or mongo (some look old or obsolete).
