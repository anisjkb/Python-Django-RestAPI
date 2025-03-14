# Installation Django | Creating Project, App, Runserver

## Step-1: Ensure Python and pip are Installed

Open cmd and run:

```
C:\Users\User>where python
C:\Users\User\anaconda3\python.exe
C:\Users\User\AppData\Local\Microsoft\WindowsApps\python.exe

C:\Users\User>python --version
Python 3.12.7

C:\Users\User>pip --version
pip 24.2 from C:\Users\User\anaconda3\Lib\site-packages\pip (python 3.12)
```

### pip is required to install packages
- pip automatically installs with Python.

## Step-2: Check Django Installation

Run the command:

```
django-admin --version
```

If Django is not installed, install it using pip:

```
pip install django
```

Verify the installation:

```
django-admin --version
```

### Why Activate Your Conda Environment?

Activating your Conda environment is recommended for:
- **Dependency Management:** Different projects can have different versions of Python and dependencies.
- **Isolation:** Avoid conflicts between different projects.
- **Reproducibility:** Others can recreate the same environment easily.

However, you can install Django directly using:

```
pip install django
```

To install Django in an Anaconda environment:

```
conda install -c anaconda django
```

Verify the installation:

```
python -m django --version
```

## Creating a Virtual Environment

```
conda create --name myenv
conda activate myenv
conda install django
```

## Create a Django Project

```
mkdir baab
cd baab
django-admin startproject oms
```

## Run the Django Project

1. Open the project folder in Visual Studio Code.
2. Open the terminal in VS Code.
3. Run:

```
python manage.py runserver
```

Copy the server path `http://127.0.0.1:8000/` and open it in a browser.

## Create an App

```
python manage.py startapp ab_oms
```

**Note:** Avoid naming the app with an existing Python module name.

## Connect the App with the Project

1. Open `oms/settings.py`.
2. Add your app in `INSTALLED_APPS`:

```python
INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
    "ab_oms",
]
```

## Create a View

Edit `ab_oms/views.py`:

```python
from django.http import HttpResponse

def oms_index(request):
    return HttpResponse("Hello, world. You're at the oms index.")
```

## Create a URL

Edit `oms/urls.py`:

```python
from django.contrib import admin
from django.urls import path
from ab_oms import views

urlpatterns = [
    path("admin/", admin.site.urls),
    path("", views.oms_index),
]
```

## Apply Migrations

```
python manage.py migrate
```

## Using `python manage.py migrate` in Django

The `python manage.py migrate` command is used to apply database migrations. 

### When to Use `migrate`

1. **Initial Setup:** Creates necessary tables for built-in apps.
2. **After Model Changes:** Apply changes after running `makemigrations`.
3. **Adding/Removing Apps:** Updates related database tables.
4. **Upgrading Django:** Ensures schema matches updated models.

### Workflow for Applying Migrations

1. Make changes to your models.
2. Run `python manage.py makemigrations`.
3. Run `python manage.py migrate`.

This ensures the database schema is in sync with your models.


# Details description
--------------------------------------------------------------------------------
## Create a Project

Step-1: Open cmd and go to your desired directory and create a folder 'like baab here' /* baab meaning Bank Asia Agent Banking*/

E:\Data Science>mkdir baab
E:\Data Science>cd baab

Step-2:run the command "django-admin startproject oms" 'oms' is the project name

E:\Data Science\oms>django-admin startproject oms
E:\Data Science\oms>

### Run a project

1. Open project folder under Visual Studio Code
2. Open terminal in VS
3. Enter command "python manage.py runserver" //manage.py this file help to run the project
(After entering the command the server is open copy the server path (http://127.0.0.1:8000/) and paste it on a browser URL)

### App create

1. Enter the command "python manage.py startapp ab_oms"

	(here 'ab_oms' is the app name. You can provide any name as related your project or any name but the name shall not be existing Python module
	name like 'oms' if the name is same than following error will occured
	
	CommandError: 'oms' conflicts with the name of an existing Python module and cannot be used as an app name. Please try another name

### Connect the App with main project

1. Ater creating App, the app will be connect with the project (django follow MVT structure)

	i) go to main project folder (oms)>settings.py> 'INSTALLED_APPS' section
	ii) add our App "ab_oms" at the last of this section 
	iii) Provide comma ',' at the last of the line and 
	iv) finally save settings.py
	
	Example- 
	
	INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
	"ab_oms",
	]

### Create View

i) Go to App folder (ab_oms)
ii) Open views.py
iii) Create your view 

For Example-

'#Create your views here.
def oms_index(request):
    '#return render(request, 'oms.html')
    return HttpResponse("Hello, world. You're at the oms index.")
	
### Create URL

i) go to main project folder (oms)>urls.py>
ii) create path in urlpatterns = [] section as following example

Example-

from ab_oms import views

urlpatterns = [
    path("admin/", admin.site.urls),
    path("", views.oms_index),
]

## Now it is the time to run the project 