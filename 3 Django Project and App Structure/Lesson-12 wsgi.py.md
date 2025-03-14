# WSGI Configuration in Django

wsgi.py is 

i.	The interphase of Web Server or Web Application.
ii.	We use this file when we work with Web Server. For example, if work with Apace Server (Hosting the project on Apache) than we use wsgi.py. 
Generally, no code modification is required here, it remains as it is by default.
iii.	It provides Synchronous Python Apps standard
(More Information - https://docs.djangoproject.com/en/5.1/howto/deployment/wsgi/)


## Overview
The `wsgi.py` file in a Django project is responsible for configuring the WSGI (Web Server Gateway Interface) application. It acts as the entry point for WSGI-compatible web servers, enabling Django to handle HTTP requests in a synchronous manner.

## Code Breakdown

### 1. Module Docstring
```python
"""
WSGI config for oms project.

It exposes the WSGI callable as a module-level variable named ``application``.

For more information on this file, see
https://docs.djangoproject.com/en/5.1/howto/deployment/wsgi/
"""
```
**Purpose:**
- Provides documentation on the purpose of the file.
- Links to Django’s official WSGI deployment guide for further details.

### 2. Importing Required Modules
```python
import os
from django.core.wsgi import get_wsgi_application
```
**Purpose:**
- `os`: Used to set and retrieve environment variables.
- `get_wsgi_application`: Imports Django’s WSGI application function, which initializes the Django project for synchronous request handling.

### 3. Setting the Default Django Settings Module
```python
os.environ.setdefault("DJANGO_SETTINGS_MODULE", "oms.settings")
```
**Purpose:**
- Defines which settings file Django should use.
- `os.environ.setdefault()` ensures that `DJANGO_SETTINGS_MODULE` is set to `oms.settings` before running the WSGI application.
- This step is crucial to ensure the application runs with the correct configuration settings.

### 4. Creating the WSGI Application
```python
application = get_wsgi_application()
```
**Purpose:**
- Calls `get_wsgi_application()` to initialize Django’s WSGI application.
- The `application` variable is exposed at the module level, allowing WSGI servers like `Gunicorn` or `uWSGI` to interact with Django.

## Why is `wsgi.py` Needed?
1. **WSGI Server Compatibility:**
   - Required when deploying Django with WSGI servers like `Gunicorn`, `uWSGI`, or Apache’s `mod_wsgi`.
   - Allows Django applications to serve HTTP requests in a synchronous manner.

2. **Production Deployment:**
   - Acts as an interface between Django and the web server.
   - Enables handling of requests in a structured way.

3. **Standardized Request Handling:**
   - WSGI is a widely accepted standard for Python web applications.
   - Ensures compatibility with multiple web servers.

## Conclusion
The `wsgi.py` file is a crucial component in Django projects for synchronous request handling. It initializes the Django WSGI application, ensuring compatibility with WSGI servers and enabling deployment in a production environment.