---
title: Troubleshooting
permalink: /docs/troubleshooting/
---

Since ELAT is only working on the browser, it is directly affected by any restrictions or errors Chrome (or Firefox) might suffer. 
Another issue caused by this client-side configuration, is that there is no way for developers to know if a user is having an issue!
There is no server, so it's all up to the person in front of the keyboard to figure it out.

Some things to try are:

##### If you're only trying Sample Data, and ELAT is not showing anything
- Clear the database

    1. Click on `Clear Database`
    
    2. `Accept`
    
    3. Reload the page and try again

- Check the error messages

    1. Open the *Console:* in Windows and Chrome, press the *F12* key or *Ctrl + Alt + J*. In Mac, press *Cmd + Option + J*
    
    2. The problem may be in your configuration, if it looks like an issue in ELAT, let us know [here](https://github.com/mvallet91/ELAT-Workbench/issues)

##### If you're only trying Sample Data, and ELAT is stuck in "loading"
Work in progress

##### If you have uploaded metadata and logfiles, and ELAT seems to be stuck "loading"
Work in progress

##### If you've already seen your graphs and it's suddenly stuck in "loading" 
- Process the graphs again
 
    1. In `Graph Options`, press `Delete and Update`
     
    2. Reload the page to force ELAT to re-process all the indicators and graphs.
