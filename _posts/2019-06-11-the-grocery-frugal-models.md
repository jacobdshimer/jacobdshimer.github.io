---
layout: single
title: "The Grocery Frugal - Models"
date: 2019-06-11
tags: [java, hibernate, postgresql]
header:
  overlay_image: "/assets/images/the-grocery-frugal/chantal-garnier-1464696-unsplash.jpg"
  caption: "Photo by Chantal Garnier on [**Unsplash**](https://unsplash.com)"
  overlay_color: "#000"
  overlay_filter: 0.6
---

# Database Description

The database that I chose to use for this project is a Heroku PostgreSQL database.  The choice for using Heroku was quite simple, especially with their PostgreSQL being free up to 10,000 rows which I doubt we will ever reach.  Before creating any tables though I needed to create an Entity Relationship Diagram (ERD).  Instead of using pen and paper to model it out or using [draw.io](https://draw.io), I chose to use [dbdiagram.io](https://dbdiagram.io).  This is a pretty cool, free software for creating out an ERD.

<figure>
	<img src="/assets/images/database_design.png">
	<figcaption>"Database ERD"</figcaption>
</figure>
