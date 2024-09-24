# What is Django ?

# <hr>

-> Free and open-source framework for building web apps with Python.

# Frameworks for Python

# <hr>

-> Django
-> Flask
-> Tornado
-> Bottle
-> Falcon
-> Hug

# Companies that Django using:

# <hr>

-> YouTube
-> Instagram
-> Spotify
-> Dropbox

# Django Features

# <hr>

-> Admin Panel
-> Object-relational mapper (ORM)
-> Authentication
-> Caching

# Why people choose?

# <hr>

-> Maturity
-> Difficulty
-> Stability
-> Community

# <hr>

# Before diving into Django

# <hr>

# Python

-   basic
-   OOP
-   classes
-   inheritance

# Relational Databases

-   tables
-   columns
-   keys

# <hr>

#### Environment Development Setup

# <hr>

## 1. Install Python

    -> Download python from https://www.python.org/

## 2. Check Python Version

    -> python3 --version - mac
    -> python --version

## 3. Install pipenv to config pipenv

    -> pip3 install pipenv - mac
    -> pip install pipenv // pip is package manager tool, install globally

## 4. Create Folder

    -> cd Desktop
    -> mkdir storefront
    -> cd storefront

## 5. Install Django

    -> pipenv install Django

## 6. Activate the Virtual Environment

    -> pipenv shell

## 7. Deactivate the Virtual Environment

    -> deactivate

## 8. Create Project with Django

    -> django-admin startproject <project_name>
    -> django-admin startproject <project_name> .

## 9. Run Project

    -> cd <project_name>
    -> python manage.py runserver 9000

## 10. Run Project

    -> pipenv --venv

# <hr>

#### settings.py : Default Apps

'django.contrib.admin', : manage data for storate
'django.contrib.auth', : user authentication
'django.contrib.contenttypes',
'django.contrib.sessions', : login session
'django.contrib.messages', : One Time Notification to user
'django.contrib.staticfiles', : serving static files images/css

## 11. Create a Django App

    -> python manage.py startapp myapp

## 12. Register App inside main app

    # add inside settings.py
    INSTALLED_APPS = [
    'playground',
    ]

## <hr>

## 13. views.py

## <hr>

-   Create your views here
-   request -> response
-   request handler
-   actions

from django.shortcuts import render
from django.http import HttpResponse

# Create your views here.

def say_hello (request):
return HttpResponse("Hello");

## 14 Create urls.py file inside app folder

# urls.py

from django.urls import path
from . import views

urlpatterns = [
path('hello/', views.say_hello)
]

## 14. import urls.py file inside main app urls.py

    from django.contrib import admin
    from django.urls import path, include

urlpatterns = [
path('admin/', admin.site.urls),
path('playground/', include('playground.urls'))
]

## <hr>

Templates

## <hr>

## 15. Creae folder called template inside app

# templates

-   hello.html

# Instead return HttpResponse();

# Render Templete

-   syntax
-   def say_hello (request):
    return render(request, 'home.html')

================================================
Dynamic display views content inside template
================================================

# views.py

def say_hello (request):
return render(request, 'home.html',{ 'name' : "binod"})

# home.html

  <h1>Hello {{ name }}</h1>

# Conditionally display

{% if name %}

  <h1>Hello {{ name }}</h1>
  {% else %}
  <h1>Hello World</h1>
  {% endif %}

# Loop

==============
def say_hello (request):
names = [{'name': "Binod"}, {'name': "Manisha"}, {'name': "Nikita"}]
return render(request, 'home.html', { 'datas' : names})

<ul>
  {% for person in datas %}
    <li>{{ person.name }}</li>
  {% endfor %}
</ul>

================================================
Dynamic Images
================================================

# Create static folder under main root folder

Exampple:
/static/images
/static/css
/static/js

# views.py

======================
def say_hello (request):
watches = [
{'name': "Gold", 'image': "images/AuraGold.webp"},
{'name': "Silver", 'image': "images/AuraSilver.webp"},
{'name': "Black", 'image': "images/AuraBlack.webp"},
]
return render(request, 'home.html', { 'datas' : watches})

# home.html

{% load static %}

<h1>Watche Images</h1>
<ul>
	{% for watch in datas %}
	<li>
		<h3>{{ watch.name }}</h3>
		<img src="{% static watch.image %}" alt="{{ watch.name }}" />
	</li>
	{% endfor %}
</ul>

================================================================================================
Base Template Layouts
================================================================================================

# templates/base.html

=========================
{% load static %}

<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8" />
		<meta name="viewport" content="width=device-width, initial-scale=1.0" />
		<title>{% block title %} Beauty Parlor {% endblock %}</title>
		<link rel="stylesheet" href="{% static 'css/styles.css' %}" />
	</head>
	<body>
		<!-- Header -->
		<header>{% include 'includes/navbar.html' %}</header>

    	<!-- Main Content -->
    	<div class="content">
    		{% block content %}
    		<!-- Default content can go here -->
    		{% endblock %}
    	</div>

    	<!-- Footer -->
    	<footer>
    		<p>&copy; 2024 Beauty Parlor</p>
    	</footer>
    </body>

</html>

# Create CSS folder with styles.css

=========================

-   styles.css
    body {
    margin: 0;
    padding: 0;
    width: 100%;
    background: green;
    }

# Extending the Base Template

=========================
templates/home.html
templates/services.html
templates/appointments.html
templates/contact.html

{% extends 'base.html' %} {% block content %}

<p>This is my page content.</p>
{% endblock %} {% block title %} home - Beauty Parlor {% endblock %}

# Including Additional Templates (Optional)

=========================
templates/includes/navbar.html

<nav>
	<ul>
		<li><a href="{% url 'home' %}">Home</a></li>
		<li><a href="{% url 'services' %}">Services</a></li>
		<li><a href="{% url 'appointments' %}">Book Appointment</a></li>
		<li><a href="{% url 'contact' %}">Contact Us</a></li>
	</ul>
</nav>

# import in base.html template

=========================
{% include 'includes/navbar.html' %}

# Static Files (CSS/JS)

=========================
from pathlib import Path
import os

-   settings.py
    STATIC_URL = 'static/'
    STATICFILES_DIRS = [
    BASE_DIR / "static",
    ]

TEMPLATES = [
{
'DIRS': [os.path.join(BASE_DIR, 'templates')],
}
]

# URL Configuration

=========================
urls.py : Project

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
path('admin/', admin.site.urls),
path('playground/', include('playground.urls'))
]

urls.py : App

from django.urls import path
from . import views

urlpatterns = [
path('', views.home, name='home'),
path('services/', views.services, name='services'),
path('appointments/', views.appointments, name='appointments'),
path('contact/', views.contact, name='contact'),
]

# View Configuration

=========================
from django.shortcuts import render

def home(request):
return render(request, 'home.html')

def services(request):
return render(request, 'services.html')

def appointments(request):
return render(request, 'appointments.html')

def contact(request):
return render(request, 'contact.html')

===========================================================================
Summery Flow in Django
===========================================================================

# Create Project : One Project

# Create App : App can one or more

# settings.py

-   configures templates and static files/images
-   INSTALLED_APPS = [register_app]

# urls.py : project

-   default 'admin'
-   include other app urls

# urls: app

-   defind path/url name
-   point class

# views

-   defined class
-   return render templates

# templates

-   its responsible to display html content
