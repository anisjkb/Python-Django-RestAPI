# Setting Up Django Development Environments

Here I discuss two favorite ways for setting Up Django Environment

-	Module 1: Setting Up a Django Environment Using Conda
-	Module 2: Setting Up a Django Environment Using Python's venv
-	Module 3: Comparisons between Conda and venv
-	When to Use Module 1 (Conda) vs. Module 2 (venv)

## Module 1: Setting Up a Django Environment Using Conda
This section covers creating and managing a Django development environment with Conda.

---

### Step 1: Install Anaconda
Download Anaconda from [Anaconda Official Site](https://www.anaconda.com) and follow the installation steps for your operating system.

After installation, verify Anaconda by opening a terminal and running:

```bash
conda --version
```

### Step 2: Create a Conda Environment
Navigate to the directory where you'll manage your project files. For instance:

```bash
cd "E:\Data Science\baab"
```
> **Note:** Conda environments are stored in a central location regardless of the current directory.

Create a new Conda environment named `django_env` with Python 3.10 (Python 3.10 is the current stable version, date- 05-March-2025):

```bash
conda create -n django_env python=3.10
```
- `-n`: Specifies the name of the environment (`django_env`).
- `python=3.10`: Sets the Python version for the environment.

Activate the Conda environment:

```bash
conda activate django_env
```
Your command prompt will now show `(django_env)`, indicating the environment is active.

### Step 3: Install Django
Use Conda to install Django:

```bash
conda install -c anaconda django
```
- `-c anaconda`: Ensures that Django is installed from the curated Anaconda repository.

### Step 4: Start a Django Project
While in the `E:\Data Science\baab` directory, create a Django project:

```bash
django-admin startproject myproject
```
Navigate into the project folder:

```bash
cd myproject
```

### Step 5: Test the Setup
Run the development server:

```bash
python manage.py runserver
```
Open a browser and visit [http://127.0.0.1:8000](http://127.0.0.1:8000). You should see the Django welcome page.

### Step 6: Additional Commands for Managing the Project
Create a Django app (e.g., a blog app):

```bash
python manage.py startapp blog
```
Apply migrations:

```bash
python manage.py makemigrations
python manage.py migrate
```

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

This guide provides two methods to set up Django environments, giving you flexibility depending on your project requirements!