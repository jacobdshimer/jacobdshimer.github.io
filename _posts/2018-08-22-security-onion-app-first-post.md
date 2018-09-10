---
layout: single
title: "Security Onion Splunk App - First Post"
date: 2018-08-22
tags: [splunk, security onion, app development]
excerpt: "Creating the basic skeleton for a Splunk App"
header:
  overlay_image: "/assets/images/splunk_seconion.png"
  overlay_color: "#000"
  overlay_filter: 0.6
---

# Creating the basic skeleton for a Splunk App

## Index

1. Creating the basic skeleton of the app [THIS POST]
2. Getting base CapMe functionality [here]({{site.baseurl}}{% post_url 2018-08-31-security-onion-app-capme %})
3. Finalizing CapMe functionality [here]({{site.baseurl}}{% post_url 2018-09-05-security-onion-app-capme-finished %})

There are mutliple ways to start creating an app.  I followed the initial tutorial on [Splunk Dev](http://dev.splunk.com/view/quickstart/SP-CAAAFDC "Splunk Dev") to get started. This app will follow the Common Information Module, though it will not utilize the add-on developed by Splunk.  The Common Information Module is an open standard that defines how managed elements in an IT environment are represented as a common set of objects and relationships between them.  What this means for the app: without the CIM and if you wanted to output the data within the sguild sourcetype and the bro_conn sourcetype, you would have to run the command:

    sourcetype=sguild sourcetype=bro_conn id.orig_h=1.1.1.1 src_ip=1.1.1.1 | do stuff...

With using the CIM this command is shortened:

    sourcetype=sguild sourcetype=bro_conn src=1.1.1.1 | do stuff...

After setting this up, I then created four basic dashboards: the BASE DASHBOARD, the Host Dashboard, the Port Dashboard, and the UID Dashboard.  These are the basis for all of my other dashboards.  The BASE DASHBOARD I use to make sure that the head of my dashboards are all similar.  What this means is that I created placeholders in all of my searches once I got the information I needed.  

I have five fields setup at the top of each dashboard: a timepicker, a radio button selector that in the future will setup auto-refresh capability, and four drop downs.  The four drop downs correspond to the four base fields when network traffic hunting: source IP, source port, destination IP, and destination port.  These drop downs are dynamically assigned, meaning they get their information from searches.  They are also linked to each other so that if you specifiy the source IP, only the the source port, destination IP and destination port that belongs with that source IP is shown.  You can do this with any of the other drop downs.

Below my fields is a custom HTML panel that just outputs the data from the fields to the screen.  This is to help the analyst looking at the dashboard know what they are drilling down on, if they forgot.

Below my custom HTML is two basic panels, the log count and log count over time.  The log count panel is just a simple single number panel that is the count of all logs for that log type.  The log count over time panel, on the other hand, is a timechart.  Whats cool about the log count over time is that it is the only panel that gets its time selector from the timepicker.  Everything else gets their timepicker from the selection made on the timepicker. If there is no selection, then the selection defaults to everything covered by the timepicker's time range.

