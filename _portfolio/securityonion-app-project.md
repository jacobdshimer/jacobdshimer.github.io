---
title: Security Onion App for Splunk
excerpt: "Creating a new Security Onion 16.04 Compliant App for Splunk"
header:
  show_overlay_excerpt: False
  overlay_image: "/assets/images/securityonion-portfolio-page.jpg"
  overlay_color: "#000"
  overlay_filter: 0.6
  teaser: assets/images/securityonion-portfolio-page-teaser.jpg
  caption: "Photo by Carlos Muza on [**Unsplash**](https://unsplash.com)"
---

# Description of the Project

This will probably become a multipart project.  The last time the Security Onion app for Splunk was updated was when Security Onion was on 12.04. A lot has changed since then, implementation of ELK as the main hunting method and dropping ELSA, changing the Bro outputs from TSV to JSON, and many more.

My goal for this project is to take the dashboards developed in Kibana by the Security Onion team and implement them in Splunk.  I will also be looking over the old Security Onion app and see what I can implement from their dashboards into this updated app.

I will be writing posts and will link them here, providing a brief description with each post.  Once the app is finished I will package it up and put it onto [splunkbase](https://splunkbase.splunk.com/ "Splunkbase").  If I need to update the app, I will post them here.  Future goals will be be to add a setup page for the app to auto-edit some of the configuration files.

1. Creating the basic skeleton of the app [here]({{site.baseurl}}{% post_url 2018-08-22-security-onion-app-first-post %})

Security Onion's website can be found [here](https://securityonion.net/ "Security Onion")

Security Onion's GitHub can be found [here](https://github.com/security-onion-solutions/security-onion/wiki/IntroductionToSecurityOnion "Security Onion GitHub")
