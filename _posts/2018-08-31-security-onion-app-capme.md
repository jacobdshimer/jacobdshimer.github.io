---
layout: single
title: "Security Onion Splunk App - CapMe Functionality and Pulling PCAPs"
date: 2018-08-31
tags: [splunk, security onion, app development]
header:
  overlay_image: "/assets/images/splunk_seconion.png"
  overlay_color: "#000"
  overlay_filter: 0.6
---
# Index

1. Creating the basic skeleton of the app [here]({{site.baseurl}}{% post_url 2018-08-22-security-onion-app-first-post %})
2. Getting base CapMe functionality [THIS POST]
3. Finalizing CapMe functionality [here]({{site.baseurl}}{% post_url 2018-09-05-security-onion-app-capme-finished %})

# CapMe Functionality and Pulling PCAPs

Something I've been banging my head on trying to figure out the last week was how to get the CapMe functionality that the ELK stack within Security Onion enjoys. The way I thought ELK was using CapMe was by querying the sguild database which for security onion was renamed the securityonion_db within MySQL. Within this database are lots of tables, but the most important one is the event table. Every sguild event is stored within this table.

Now the problem I had was taking the Bro Conn log data and querying the events table. Sending the data to CapMe was quite easy, they tell you right in security onion's GitHub for CapMe:

    https://[security_onion_master]/capme/index.php?sip=[src_ip]&spt=[src_port]&dip=[dest]&dpt=[dest_port]&stime=[start]&etime=[end]&usr=[username]&pwd=[password]

In order to send this data, I created a custom drilldown from a table, creating a match condition looking for when the user clicks the time field. What took me over a week to figure out is how to get the proper data. I had to make sure that the connection triggered an alert and existed in the sguild sourcetype. The query I ended up with follows:

    sourcetype=bro_conn
    | join src,src_port,dest,dest_port [search sourcetype=sguild]
    | eval startTime = floor(_time), endTime = round((startTime + duration),0)
    | table _time, startTime, endTime, src, src_port, dest, dest_port, uid
    | sort -_time

This returns only the data where the Source IP, Source Port, Destination IP, and Destination Port exists in both sourcetypes. Like I said, CapMe's index.php just takes the inputted data and queries the Securtiy Onion database. This is the way the database is queried to see if the data event exists:

{% highlight mysql linenos %}
  SELECT event.timestamp AS start_time, s2.sid, s2.hostname
         FROM event
         LEFT JOIN sensor ON event.sid = sensor.sid
         LEFT JOIN sensor AS s2 ON sensor.net_name = s2.net_name
         WHERE timestamp BETWEEN '[start]' AND '[end]'
         AND ((src_ip = INET_ATON('[src_ip]') AND src_port = [src_port]
         AND dst_ip = INET_ATON('[dest]') AND dst_port = [dest_port] )
         OR (src_ip = INET_ATON('[dest]') AND src_port = [dest_port]
         AND dst_ip = INET_ATON('[src_ip]') AND dst_port = [src_port] ))
         AND s2.agent_type = 'pcap' LIMIT 1";

{% endhighlight %}

This is pretty ingenious because it checks for mismatches between the source and destinations.

Now, I said that this is the way I thought ELK was sending their data which made me confused because they are able to pull none event connection's PCAPs. Everything that I learned about CapMe's functionality came from their callback.php. I noticed another file, callback-elastic.php. Going through this, I realized that they weren't querying the database at all. They utilize cliscript.tcl or cliscriptbro.tcl, depending on the type of data. To find the PCAP, first they request it from the dailylogs folder, then they request it again, saving the requested data into a new PCAP within the archive folder.

I'm still figuring this out but with my query above, I'm able to get the PCAP information for data that have sguild alerts. I might write my own script that does the same, querying Splunk on the backend through REST for input validation like they do with callback-elastic.php.
