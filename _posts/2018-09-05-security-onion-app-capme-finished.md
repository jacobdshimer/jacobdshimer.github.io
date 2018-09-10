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
# Index

1. Creating the basic skeleton of the app [here]({{site.baseurl}}{% post_url 2018-08-22-security-onion-app-first-post %})
2. Getting base CapMe functionality [here]({{site.baseurl}}{% post_url 2018-08-31-security-onion-app-capme %})
3. Finalizing CapMe functionality [THIS POST]

# Finished CapMe Functionality

I have finished building out the CapMe functionality.  It only took me all weekend to analyze and convert the original elastic.php, elastic.js, and callback-elastic.php to Splunk specific files.  I copied these three files, replacing elastic with Splunk of course, and gutted them, keeping the bones of the files.  

One problem I encountered initially is the ESID field.  The ESID field is an ELK specific field that points to a specific ELK document.  Splunk doesn't actually have an ID for individual events, though they do have two internal fields though, \_cd and \_bkt, that are unique(ish) between events.  The \_cd field provides an address for an event within the index and \_bkt field contains the ID of the bucket that an event is stored in.  I created a calculated field that concatenates these two fields together before MD5 hashing them. I aptly called this new field the SPID, for Splunk ID.

Next problem I encountered was sending searches to the Splunk REST API.  In order to do this, the user sending the searches need to authenticate with Splunk.  To combat this, I used the PHP auth package to get the username and password that is authenticated with the Security Onion Web App.  I then created a user in Splunk with this same credentials.  In the future I might use Security Onion to handle logging onto Splunk, but that is for a future date.

Next I went line by line, copying and pasting code from callback-elastic.php and interpreting what was being accomplished in that block of code.  This was a little difficult seeing as I've never programmed in PHP before. To help out, I created a test.php with some HTML, echoing out the output of whatever code I copied.  I had to create five functions that deal specifically with Splunk.  The first is the authentication function, that handles sending the username and password to Splunk and returning a Session Key.  The next one that I created handled creating the initial Splunk search.  This search looks for the SPID and returns a table of all possible fields.  For efficiency, I might change this search to return less data.  This search looks back thirty days, to ensure that users are able to find the data they are looking for.  I chose thirty days because this is how long sguild is configured to keep data.  I then created a more specific search function that is purely looking for the connection UID's for the specific connection in the conn.log.  

The next two functions go together, the first checks if the search job is done.  I did this by putting my CURL command within a do while loop, waiting for the isDone field to turn true.  If isDone field is true and isFailed is true, it returns the error back to the main program.  If not, it returns the fact that it is finished to the main program.  The next function gets the results from Splunk and returning just the results back to the main program.

The rest of the code just replaces Elastic specific code with Splunk code.  One other difference is that Elastic sends the date back in "Y-m-d H:M:S" format while I'm sending the time back in epoch time.  Where they first convert their timestamps to epoch time, I leave mine alone and where they format their timestamp, I convert and format in one line. Unfortunately I can not publish the code on GitHub yet, but once I do, I will provide a link down below.  
