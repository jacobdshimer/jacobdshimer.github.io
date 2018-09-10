---
layout: single
title: "Security Onion Splunk App - Finished CapMe Functionality"
date: 2018-09-05
tags: [splunk, security onion, app development, PHP]
header:
  overlay_image: "/assets/images/splunk_seconion.png"
  overlay_color: "#000"
  overlay_filter: 0.6
---

# Finished CapMe Functionality

I have finished building out the CapMe functionality.  It only took me all weekend to analyze and convert the original elastic.php, elastic.js, and callback-elastic.php to Splunk specific files.  I copied these three files, replacing elastic with Splunk of course, and gutted them, keeping the bones of the files. 

# Index

1. Creating the basic skeleton of the app [here]({{site.baseurl}}{% post_url 2018-08-22-security-onion-app-first-post %})
2. Getting base CapMe functionality [here]({{site.baseurl}}{% post_url 2018-08-31-security-onion-app-capme %})
3. Finalizing CapMe functionality [THIS POST]

## splunk.php and splunk.js

Splunk.php and splunk.js are just copies of their elastic and capme brothers.  I used the ESID fields from the elastic version to create the SPID and stype fields and I used the stime field from the original capme files to create my stime field.  The splunk.php is the page that the user will see.  Splunk.js handles retrieving the data from functions.php, processing the data before pushing it to callback-splunk.php, and then retrieving data frkom callback-splunk.php and sending it back to splunk.php. 

## functions.php

Functions.php handles the string to header and header to string functions and also pulling the information from the URL.  I had to edit this to handle pulling the stype (sourcetype) and SPID fields (explained later).  This was pretty easy, I just added in my two new fields where the others are declared and copied one of the fields and changed their variables to my new variables. 

## callback-splunk.php

One problem I encountered initially is the ESID field.  The ESID field is an ELK specific field that points to a specific ELK document.  Splunk doesn't actually have an ID for individual events, though they do have two internal fields though, \_cd and \_bkt, that are unique(ish) between events.  The \_cd field provides an address for an event within the index and \_bkt field contains the ID of the bucket that an event is stored in.  I created a calculated field that concatenates these two fields together before MD5 hashing them. I aptly called this new field the SPID, for Splunk ID.

Next problem I encountered was sending searches to the Splunk REST API.  In order to do this, the user sending the searches need to authenticate with Splunk.  To combat this, I used the PHP auth package to get the username and password that is authenticated with the Security Onion Web App.  I then created a user in Splunk with this same credentials.  In the future I might use Security Onion to handle logging onto Splunk, but that is for a future date.

### Splunk Authentication

Next I went line by line, copying and pasting code from callback-elastic.php and interpreting what was being accomplished in that block of code.  This was a little difficult seeing as I've never programmed in PHP before. To help out, I created a test.php with some HTML, echoing out the output of whatever code I copied.  I had to create five functions that deal specifically with Splunk.  The first is the authentication function, that handles sending the username and password to Splunk and returning a Session Key.  

### Splunk Querying 

The next one that I created handled creating the initial Splunk search.  This search retrieves the sourcetype, SPID, and epochTime, which is just a calculated field equaling the \_time field.  Initally I was just setting the earliest_time to thirty days, but upon generating over 2.8 million events over a few minutes, I realized that this was looking way too far back.  The searches were timing out before even finishing because of how many events it had to look through.  So I added in the earliest = epochTime to filter the events down.  This brought my search down from never finishing, to taking about fourty to sixty seconds to finish.  I feel like this is was still too long.  So I added in the sourcetype field to my Splunk Dashboard table and added it to my Splunk query.  This brought it down to about .047 seconds to complete.  Much better.

The fourth function handles the second Splunk query looking for the actual connnection.  This is so that we can retrieve the sensorname field from and make sure that this was an actual connection.  This also makes it so that we can pull the PCAP for files.log, pe.log, and x509.log.

### Getting Results from Splunk

The next two functions go together, the first checks if the search job is done.  I did this by putting my CURL command within a do while loop, waiting for the isDone field to turn true.  If isDone field is true and isFailed is true, it returns the error back to the main program.  If not, it returns the fact that it is finished to the main program.  The next function gets the results from Splunk and returning just the results back to the main program.

The rest of the code just replaces Elastic specific code with Splunk code.  One other difference is that Elastic sends the date back in "Y-m-d H:M:S" format while I'm sending the time back in epoch time.  Where they first convert their timestamps to epoch time, I leave mine alone and where they format their timestamp, I convert and format in one line. Unfortunately I can not publish the code on GitHub yet, but once I do, I will provide a link down below.  
