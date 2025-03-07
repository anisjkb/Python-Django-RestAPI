# Setting Up Django Development Environments

This guide provides two methods to set up Django Virtual Environment (VR) environments, giving flexibility depending on project requirements!
Here, I discuss two favorite ways for setting Up Django Environment:

-	Module 1: Setting Up a Django Environment Using Conda
-	Module 2: Setting Up a Django Environment Using Python's venv

-	Module 3: Comparisons between Conda and venv
-	When to Use Module 1 (Conda) vs. Module 2 (venv)

## Module 1: Setting Up a Django Environment Using Conda
This section covers creating and managing a Django development environment with Conda.

### Why Activate Your Conda Environment?

Activating your Conda environment is recommended for:
- **Dependency Management:** Different projects can have different versions of Python and dependencies.
- **Isolation:** Avoid conflicts between different projects.
- **Reproducibility:** Others can recreate the same environment easily.

---

### Step 1: Install Anaconda
Download Anaconda from [Anaconda Official Site](https://www.anaconda.com) and follow the installation steps for your operating system.

After installation, verify Anaconda by opening a terminal and running:

```bash
conda --version
```
### Step 2: Create and Activate the Conda Environment

#### i) Create the Conda Environment:

-   Open Start> Anaconda PowerShell Prompt
-   Navigate to the directory where you'll manage your project files. For instance:

```bash
cd "E:\Data Science\baab"
```
> **Note:** Conda environments are stored in a central location regardless of the current directory.

Create a new Conda environment named `django310_env` with Python 3.10 (Python 3.10 is the current stable version, date- 05-March-2025):

```bash
conda create -n django310_env python=3.10
```
-	`-n`: Specifies the name of the environment (`django310_env`). The name `django310_env` is not fixed. You can customize it to suit your project or preferences. The main goal is to pick a name that’s meaningful and easy to remember for you.
-	`python=3.10`: Sets the Python version for the environment.
-	Even though your current directory is `E:\Data Science\baab`, that directory is unrelated to where the Conda environments are stored. Conda environments are typically located in the `envs` folder inside the base Conda installation directory `(here, C:\Users\User\anaconda3\envs)`.

#### ii) Activate the Conda environment:

Enter the command

```bash
conda activate django310_env
```
Your command prompt will now show `(django310_env)`, indicating the active environment.
It shows `(django310_env) PS E:\Data Science\baab>` insted of `(base) PS C:\Users\User> cd "E:\Data Science\baab"`

Environment Name in the Prompt: `(django310_env) PS E:\Data Science\baab>` indicates that the `django310_env` environment is currently active.

### Step 3: Install Django
Use Conda to install Django:

```bash
conda install -c anaconda django
```
This command installs Django from the Anaconda repository.

- `-c anaconda`: Ensures that Django is installed from the curated Anaconda repository.

### Step 4: Start a Django Project

1. Navigate to your desired directory where you want to create the project:

   ```bash
   cd path/to/your/project/folder (Example- E:\Data Science\baab directory:)
   ```
While in the `E:\Data Science\baab` directory, create a new Django project:

2. Create a new Django project (replace `myproject` with your desired project name):

   ```bash
   django-admin startproject myproject
   ```
3. Navigate into the project directory/folder:

   ```bash
   cd myproject
   ```
### Step 5: Test the Setup by running the development server

Run the development server:

- Run the development server:
  
  ```bash
  python manage.py runserver
  ```
- Open your browser and go to [http://127.0.0.1:8000](http://127.0.0.1:8000). You should see the Django welcome page.

## 6. Create an App in the Django Project
Django projects are organized into apps. To create an app, follow these steps:

### Example: Create an app named `blog`

```bash
python manage.py startapp blog
```
You’ll see a new folder named `blog` in your project directory.

## 7. Register/Connect the App
To link the `blog` app with your Django project:

1. Open the `settings.py` file in your project folder.
2. Locate the `INSTALLED_APPS` list and add `'blog',` to it:

   ```python
   INSTALLED_APPS = [
       'django.contrib.admin',
       'django.contrib.auth',
       'django.contrib.contenttypes',
       'django.contrib.sessions',
       'django.contrib.messages',
       'django.contrib.staticfiles',
       'blog',
   ]
   ```

## 8. Create a View

Edit `ab_oms/views.py`:

```python
from django.http import HttpResponse

def oms_index(request):
    return HttpResponse("Hello, world. You're at the oms index.")
```

## 9. Create a URL

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

## 10. Migrate the Database
Django uses migrations to handle database schema. Run the following commands:

```bash
python manage.py makemigrations
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

## 11. Test Your Setup
Now it is the time to run the project 

### Create a Superuser to Access the Django Admin Panel

```bash
python manage.py createsuperuser
```
Follow the prompts to set a username, email, and password.

### Start the Server Again

```bash
python manage.py runserver
```
Visit [http://127.0.0.1:8000/admin](http://127.0.0.1:8000/admin) and log in with the superuser credentials.

## 10. Example Directory Structure
After completing the steps, your project directory will look something like this:

```
myproject/
├── blog/
│   ├── migrations/
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── tests.py
│   ├── views.py
├── myproject/
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   ├── wsgi.py
├── manage.py
```

## Additional Notes

### Running Django Shell
You can interact with Django models and execute Python commands within your project using:

```bash
python manage.py shell
```

### Creating a Simple View
To test your `blog` app, modify `views.py` inside the `blog` folder:

```python
from django.http import HttpResponse

def home(request):
    return HttpResponse("Hello, Django with Anaconda!")
```

Then, update `urls.py` in `myproject` to include this view:

```python
from django.contrib import admin
from django.urls import path
from blog.views import home

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', home),
]
```

Now, visiting [http://127.0.0.1:8000](http://127.0.0.1:8000) will display **"Hello, Django with Anaconda!"**.

---

## Module 2: Setting Up a Django Environment Using Python's venv
This section focuses on creating a Django environment using Python's built-in `venv` module.

---

### Step 1: Create a Virtual Environment
Navigate to your project directory, such as `E:\Data Science\baab`:

```bash
cd "E:\Data Science\baab"
```
Create a virtual environment named `django_env`:

```bash
python -m venv django_env
```
- `-m`: Specifies the `venv` module, which creates the virtual environment.
- `django_env`: The name of the virtual environment (a new folder will appear in your directory).

### Step 2: Activate the Virtual Environment
On **Windows**:

```bash
django_env\Scripts\activate
```
On **Mac/Linux**:

```bash
source django_env/bin/activate
```
The command prompt will show `(django_env)`, indicating the environment is active.

### Step 3: Install Django
Install Django with `pip`:

```bash
pip install django
```

### Step 4: Start a Django Project
Create a Django project in the same directory:

```bash
django-admin startproject myproject
```
Navigate into the project folder:

```bash
cd myproject
```

### Step 5: Run the Development Server
Start the server to test the setup:

```bash
python manage.py runserver
```
Visit [http://127.0.0.1:8000](http://127.0.0.1:8000) to see the Django welcome page.

---

## Module 3: Comparisons between Conda and venv
Here are the key differences between using Conda and venv for setting up Django.

### 1. Package Manager
| Aspect  | Conda  | venv  |
|---------|--------|-------|
| **Tool** | Anaconda's package manager | Python's built-in module |
| **Scope** | Manages Python and non-Python dependencies | Manages Python-only dependencies |

### 2. Environment Management
| Feature  | Conda  | venv  |
|----------|--------|-------|
| **Environment Storage** | Centralized in Conda’s directory | Local to the project directory |

---

## When to Use Module 1 (Conda) vs. Module 2 (venv)

### Use Conda (Module 1) if:
- You are working with both Python and non-Python dependencies (e.g., data science, machine learning, scientific computing).
- You want a centralized environment management system.
- You need easy package installation without dependency conflicts.

### Use venv (Module 2) if:
- You need a lightweight and simple Python environment without external tools.
- You are working on a standard Django or Python-based project.
- You prefer built-in Python tools without additional package managers.

---
