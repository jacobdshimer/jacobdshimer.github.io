---
layout: single
title: "Part One: Building a Microblog using Flask"
date: 2018-08-06
tags: [flask, python]
excerpt: "Initializing the flask app and building out the directory structure."
header:
#  overlay_image: "/assets/images/Splunk_logo.png"
#  overlay_color: "#000"
#  overlay_filter: 0.6
---

# Introduction

This post will encompass the first chapter from the tutorial that I followed.  I will group the different chapters as I go through them and explain what I did within the chapter.

## Chapter One - Initialize the Flask App

The first thing covered in this chapter is installing python and the dependencies required to create this microblog.  I already had python 3.6 installed, but I didn't have flask installed so I ran 'pip install flask'. Once flask has been successfully installed, you need to make the directory tree that your project will live in.  The basic folder structure should look like this:

microblog
  app/

If running this within a virtual environment, you will have an additional folder called venv.  The app directory will contain all of the python scripts that make up the application and the HTML templates.  The first python script that we will create is the __init__.py file.  When the app directory is imported, it will execute the __init__.py and defines what symbols the package exposes to the outside world.  

{% highlight python linenos %}
  from flask import Flask
  app = Flask(__name__)
  from app import routes
{% endhighlight %}

What this basically does is creates the application object as an instance of the Flask class imported from the flask package (notice the case of the word flask? This is important).  The 'app = Flask(__name__)' portion of the code creates an instance of the Flask class for the web app.  __name__ is used to get the import name of the place the app is defined.  Flask uses this to know where to look up resources, templates, static files, instance folders, etc.  The 'from app import routes' is used to define the different URLs that the application uses.  In Flask, the application routes are written as Python functions, called view functions.  One or more route URLs can be mapped to one view functions.

Next we need to create the routes.py that we just imported:

{% highlight python linenos %}
  from app import app
  @app.route('/')
  @app.route('/index')
  def index():
    return "Hello, World!"
{% endhighlight %}

This is a very simple route, but notice that we are using what was mentioned before about using more then one route for one view function?  The '/' URL and '/index' URL are utilize the same function index().  This is a very simple function that currently just outputs "Hello, World!" to the screen.

To complete the application, we need to create the top-level script that defines the Flask application instance.  We will call this file microblog.py and within it we write:

{% highlight python linenos %}
  from app import app
{% endhighlight %}

Now this is might be a little confusing.  We are importing the app variable which is the Flask application instance from the app package.  Now in order to run this you need to add the FLASK_APP environmental variable.  This can be done one of two ways depending on your operating system.

For Linux users, you will run the following command:

{% highlight bash %}
  export FLASK_APP=microblog.py
{% endhighlight %}

Or if a Windows user:

{% highlight batch %}
  set FLASK_APP=microblog.py
{% endhighlight %}

In order to run your app, run the following command in your prompt: flask run.  You should see some information about your Flask server, specifically what app its running, what IP address and what port it is using. Take note of the port and navigate to http://localhost:5000 to see your application. Pretty cool, right?

(HERE)[https://github.com/jacobdshimer/microblog] is a link to my GitHub repository that contains my working version of the microblog.
(HERE)[https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world] is a link to the tutorial page for this part.
