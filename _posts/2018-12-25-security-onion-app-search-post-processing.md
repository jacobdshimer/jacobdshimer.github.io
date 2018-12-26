---
layout: single
title: "Security Onion Splunk App - Search Post Processing and Other Updates"
date: 2018-12-25
tags: [splunk, security onion, app development]
header:
  overlay_image: "/assets/images/splunk_seconion.png"
  overlay_color: "#000"
  overlay_filter: 0.6
---

# Search Post Processing

One thing that Splunk can do that I'm pretty sure ELK can't do is search post processing.  What this is taking a simple base search, and then performing additional processing on the results of that search.  This makes it easier to have a dashboard with lots of panels and not constantly sending searches to the indexer.  This allowed me to reduce some dashboards from sending up to 20 concurrent searches, to up to five or six searches.

# Other Updates
## Updated View

I've changed the way the top of the each dashboard looks like and added an additional filtering mechanic.  Instead of having all of the different filtering mechanisms just sitting at the top of the page, I've added them to separate panels based off of the filtering type, all in one row.  So the time based filtering is in one panel, IP address and port number is in another, and finally the new mechanic is in the last panel.

<figure>
   <a href="/assets/images/seconion_app_updated_view.PNG"><img src="assets/images/seconion_app_updated_view.PNG"></a>
   <figcaption>Updated Views</figcaption>
</figure>

## New filtering mechanic

Before, the only filtering that was possible was through IP addresses and Port numbers.  I decided that that was very limiting and created a new mechanic.  This mechanic consist of a dropdown and a multiselect box.  The dropdown corresponds to the available fields within the sourcetype.  The multiselect takes the token from the dropdown and applies its value to both the search that powers the multiselect and to the valuePrefix for the multiselect.  So for example, if the user selected the UID field and selected a UID from the multiselect, then the value from that multiselect would look like this: (uid="example_uid").  If the user selected two UID's to search for, it would look like this: (uid="example_1_uid") OR (uid="example_2_uid").  Unfortunately, the user can only search with one field, but they can search for multiple elements within that field.  I will continue trying to find a way to enable multiple field searching.

<figure class="half">
   <a href="/assets/images/seconion_app_new_filtering-1.PNG"><img src="assets/images/seconion_app_new_filtering-1.PNG"></a>
   <a href="/assets/images/seconion_app_new_filtering-2.PNG"><img src="assets/images/seconion_app_new_filtering-2.PNG"></a>
   <figcaption>Updated Views</figcaption>
</figure>
