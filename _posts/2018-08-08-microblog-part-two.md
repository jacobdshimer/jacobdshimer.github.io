---
layout: single
title: "Part Two: Building a Microblog using Flask"
date: 2018-08-08
tags: [flask, python]
excerpt: "Creating the template pages that will be used."
header:
#  overlay_image: "/assets/images/Splunk_logo.png"
#  overlay_color: "#000"
#  overlay_filter: 0.6
---

# Introduction

This post will encompass the second chapter from the tutorial that I followed.  I will group the different chapters as I go through them and explain what I did within the chapter.

## Chapter Two - Creating Templates

This part of the tutorial covers templating in HTML.  Say you wanted each user to have a personalized heading, maybe something that gives them a greeting like "Hello, Jacob" every time they log in?  Instead of making a new HTML document for each user, a server side application would create the heading based off of a template.  This is the idea behind dynamic websites.  

Within the app directory, create another directory titled templates.  This folder will contain all of your websites template webpages that will be used to create your dynamic web pages.  The first template we're going to create is the going to be index.html:

{% highlight html %}{% raw %}
  <html>
      <head>
          <title>{{ title }} - Microblog</title>
      </head>
      <body>
          <h1>Hello, {{ user.username }}!</h1>
      </body>
  </html>
{% endraw %}{% endhighlight %}

See the part in {{ ... }}?  These are placeholders for our dynamic content.  The title part is pretty self explanatory, but the user.username part might need some explanation.  This is referring to a variable user that has a key value pair where the key is username.  Now we need to edit our routes.py script to point to the new content.

{% highlight python linenos %}
  from flask import render_template
  from app import app

  @app.route('/')
  @app.route('/index')
  def index():
      user = {'username': 'Miguel'}
      return render_template('index.html', title='Home', user=user)
{% endhighlight %}

Going over the changes we made, we are importing the render_template function from flask in order to use flasks (Jinja2)[http://jinja.pocoo.org/] template engine and we created a mock user for testing called Miguel.  Miguel is a placeholder until we develop the concept of users with our website.  Jinja2 is a very powerful rendering engine and can easily handle conditional statements and loops.  The following shows what we will do to utilize these two concepts within our index.html template and routes.py script.

The index.html:
{% highlight html %}{% raw %}
  <html>
      <head>
          {% if title %}
          <title>{{ title }} - Microblog</title>
          {% else %}
          <title>Welcome to Microblog</title>
          {% endif %}
      </head>
      <body>
          <h1>Hi, {{ user.username }}!</h1>
          {% for post in posts %}
          <div><p>{{ post.author.username }} says: <b>{{ post.body }}</b></p></div>
          {% endfor %}
      </body>
  </html>
{% endraw %}{% endhighlight %}

Within index.html, you can see we implement an if condition and a for loop.  If the variable title exists, then it will change the title of the webpage to what is contained in the variable title - Microblog.  If it the title variable doesn't exist, then it will use a default title.  The for loop loops through the a list called posts and then create a div for each post's author's username and the body of the post.

The routes.py:
{% highlight python linenos %}
  from flask import render_template
  from app import app

  @app.route('/')
  @app.route('/index')
  def index():
      user = {'username': 'Miguel'}
      posts = [
          {
              'author': {'username': 'John'},
              'body': 'Beautiful day in Portland!'
          },
          {
              'author': {'username': 'Susan'},
              'body': 'The Avengers movie was so cool!'
          }
      ]
      return render_template('index.html', title='Home', user=user, posts=posts)
{% endhighlight %}

Here you can see how this is done with the render_template.  The template page is pushed into the function along with the title, the user variable, and the posts variable.

So now we have an index page, what now? Well most websites like to have a navigation bar at the top and for statically generated website, the navigation bar has to be typed in for each page.  We can easily do that by changing our index.html to point to a base.html that will handle the header elements.  The base.html will look like this:

{% highlight html %}{% raw %}
  <html>
      <head>
        {% if title %}
        <title>{{ title }} - Microblog</title>
        {% else %}
        <title>Welcome to Microblog</title>
        {% endif %}
      </head>
      <body>
          <div>Microblog: <a href="/index">Home</a></div>
          <hr>
          {% block content %}{% endblock %}
      </body>
  </html>
{% endraw %}{% endhighlight %}

Notice the div block that has an a tag pointing to our index.html?  This is the start of our navigation bar.  See the {% block content %} tag? This will be where our templates will be inserted.

Our index.html document will look like this now:

{% highlight html %}{% raw %}
  {% extends "base.html" %}

  {% block content %}
      <h1>Hi, {{ user.username }}!</h1>
      {% for post in posts %}
      <div><p>{{ post.author.username }} says: <b>{{ post.body }}</b></p></div>
      {% endfor %}
  {% endblock %}
{% endraw %}{% endhighlight %}

The area surrounded by { % block content %} tells flask to insert this into the document that is being extended in the {% extends %} tag.

(HERE)[https://github.com/jacobdshimer/microblog] is a link to my GitHub repository that contains my working version of the microblog.
(HERE)[https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-ii-templates] is a link to the tutorial page for this part.
