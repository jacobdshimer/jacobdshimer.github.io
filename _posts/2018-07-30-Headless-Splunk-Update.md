---
layout: single
title: "Update to: Running a Headless Splunk Search"
date: 2018-07-30
tags: [splunk, python]
excerpt: "Send searches to a Splunk Server and get the results. Now with command line capability."
header:
  image: "/assets/images/Splunk_logo.png"
---

# Updates to the Headless Splunk search script

After a few hours of coding and a bit of coffee, I have created the capability of running this
script in the command line.  The current switch statements that are supported are: search, username,
password, hostname, output mode, output file, and port.  The only switch that is absolutely required
in the command line is search.  In the future, I might code in the ability to send multiple searches,
potentially via a file, and then grab all of their results.  

The username, password, and host are all required fields, kinda.  They are required in the sense that
without them, the code won't work, but they are not required to be entered in the command line.  If
it is not specified in the command line, the program will search within a settings.json file for the
required information.  

Output mode and output file kind of go hand-in-hand.  Output mode changes what the output of the
script looks like, be it XML, JSON, CSV, etc.  Output file flips the switch statement and just
tells the script to output the results to a file, with the SID as the name of the file.  

I've also updated authenticate.py and search.py by adding in two new functions.  The functions I've
added are getResults and deleteSession.  They do exactly what they sound like, getResults within
search.py retrieves the results from the server and deleteSession deletes the session key.  In the
future, I might make it so that the session key is saved and when sending a search again, it checks
to see if the session key is still active (splunk defaults to a one hour TTL for session keys), it will
skip the authentication process, if not then it will retrieve the username/password.  I don't see a
need for this currently seeing as it doesn't take too much computing power to request a session key.

I have not implemented the usage of HTTP Event Collectors within this script.  I know how to add
it in, I just don't know with what context it would be needed with this set of scripts.  Still need to
add the ability to "prettify" the output once it goes to screen or to a file. Currently it just outputs
whatever it receives. Currently the README file is less then stellar, I need to work on documenting
what the script does and how to do it in an easily digestible way.

You can find the repository [HERE](https://github.com/jacobdshimer/Remote-Splunk-Searching).
