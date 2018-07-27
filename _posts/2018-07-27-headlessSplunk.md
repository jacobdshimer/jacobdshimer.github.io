---
layout: single
title: "Running a Headless Splunk Search"
date: 2018-07-27
tags: [splunk, python]
excerpt: "Sending searches to a Splunk Server and get the results."
header:
  image: "/images/Splunk_logo.png"
---

# Creating a Python program that Runs Splunk Headless

I am currently working on a python program that will reach out to a Splunk server
through it management port (typically port 8089) and send a search to that server.
Future additions to the program will include getting the results and formatting the
results and potentially an ability to send check if a Splunk server was running
an HTTP Event Collector (HEC), what available tokens are, and the ability to send
data to that HEC.

The modules that are currently in the works is an authenticate.py script that will
take the Splunk hostname, username, password, and, if running the Splunk daemon
on a different port, the Splunk daemon port. This module will return a session key
to the calling program as a string, or if running by itself it outputs it to the screen.
Potential changes to this would be to implement a switch statement functionality
that changes the output mode.

Another module in the works is the search.py module.  This module contains two
functions so far, one that sends searches to the Splunk backend and another that
retrieves the status of the search.  Future additions will be actually receiving
the search and formatting it.
