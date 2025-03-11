##	Django and Django REST Framework (DRF) Complete Guideline
Dive into the details of Python Django and Django REST Framework (DRF), their differences, overlap, use cases, and requirements for mastery.
---

### **What are Django and Django REST Framework?**
1. **Django Framework**:
   - Django is a high-level web framework for building web applications efficiently.
   - It is designed for handling server-side logic, managing databases, rendering templates, and creating full-stack web applications.

2. **Django REST Framework (DRF)**:
   - DRF is an extension of Django specifically designed for building RESTful APIs.
   - It simplifies the process of creating APIs by providing tools for serialization, authentication, and viewsets.

---

### **Common Areas Between Django and DRF**
- **Base Framework**: Both are Python-based, and DRF is built on top of Django.
- **ORM (Object-Relational Mapping)**: Both utilize Django’s ORM for database interaction.
- **Middleware**: They share Django's middleware capabilities.
- **Shared Components**: Both rely on Django’s models, views, and templates (in part).

---

### **When to Use Django vs. Django REST Framework?**
| **Scenario**                                      | **Use Django**              | **Use DRF**                         |
|---------------------------------------------------|-----------------------------|-------------------------------------|
| Building a full-stack web application with HTML templates. | ✅                          | ❌                                  |
| Building a RESTful API for backend communication with other systems or frontend frameworks (React, Angular, etc.). | ❌                          | ✅                                  |
| Handling forms and server-side rendering.        | ✅                          | ❌                                  |
| Providing APIs for external consumption.         | ❌                          | ✅                                  |

---

### **Requirements to Learn**
1. **Django**:
   - Basic Python knowledge.
   - Familiarity with web concepts: HTTP, URLs, etc.
   - Understanding of databases (SQL basics).
   - Django ORM for interacting with databases.
   - Template rendering for building frontends.

2. **Django REST Framework**:
   - All Django prerequisites.
   - RESTful API concepts: HTTP methods (GET, POST, PUT, DELETE), status codes.
   - JSON serialization/deserialization.
   - Authentication and permissions in APIs.

---

### **Step-by-Step Live Example**
Let’s build a project where we combine Django and DRF. We'll create an **Employee Management System** where:
- Django handles the admin panel and rendering of an employee list in HTML.
- DRF provides an API to access employee data.

---

#### **1. Setting Up the Environment**

**Process-A**

Install necessary tools:
```bash
pip install django djangorestframework
```

**Process-B** If you plan to set up a Django Virtual Environment (VR) using **Conda** follow below steps

### **Setting Up the Conda Environment**

- **Step**: Create a Conda environment and install Django and Django REST Framework.
```bash
# (i) Create a new Conda environment
conda create --name django_env python=3.10 -y

# (ii) Activate the Conda environment
conda activate django_env

# (iii) Install Django and DRF
pip install django djangorestframework
```

- **Scenario**: This is **common to both Django and DRF**, as both require setting up the Python environment.

#### **2. Create a Django Project**
```bash
django-admin startproject employee_mgmt
cd employee_mgmt
```

- **Note**: If you want to create within the Conda environment, make sure your Conda environment is active while running the command.
- **Scenario**: This is a **Django** step, as it involves setting up the core structure of the Django project.

#### **3. Create an App**
```bash
python manage.py startapp employees
```
Add `employees` and `rest_framework` to `INSTALLED_APPS` in `settings.py`.

#####	**Scenario:**	This is a Django step because apps are core units of a Django project.

#### **4. Define a Model**
Create a `models.py` file in the `employees` app:
```python
from django.db import models

class Employee(models.Model):
    name = models.CharField(max_length=100)
    position = models.CharField(max_length=50)
    salary = models.DecimalField(max_digits=10, decimal_places=2)

    def __str__(self):
        return self.name
```
-	Here's the SQL query to create a table equivalent to your Django model Employee (This is just for compression btween Model and SQL):
```sql
CREATE TABLE Employee (
    id SERIAL PRIMARY KEY, -- Auto-incrementing primary key
    name VARCHAR(100) NOT NULL, -- Name with a maximum length of 100 characters
    position VARCHAR(50) NOT NULL, -- Position with a maximum length of 50 characters
    salary DECIMAL(10, 2) NOT NULL -- Salary with up to 10 digits, including 2 decimal places
);
```

In this schema:

- `id` is an auto-incrementing primary key added by Django automatically to every model unless you specify otherwise.
- `name` and `position` are non-null character fields with constraints on maximum lengths (`100` and `50`, respectively).
- `salary` is a non-null decimal field, with a precision of up to `10` digits, including `2` decimal places.
```

Run migrations:
```bash
python manage.py makemigrations
python manage.py migrate
```

#####	**Scenario:**	This is a Django step because models are part of Django's ORM (Object-Relational Mapping).

#### **5. Create Admin Interface**
Register the model in `admin.py`:
```python
from django.contrib import admin
from .models import Employee

admin.site.register(Employee)
```

#####	**Scenario:**	This is a Django step because the admin panel is a feature of Django.


Access the Django Admin panel for CRUD operations:
```bash
python manage.py createsuperuser
python manage.py runserver
```

#### **6. Create an API Using DRF**
- **Serializer**: Define a serializer for the `Employee` model in `serializers.py`.
```python
from rest_framework import serializers
from .models import Employee

class EmployeeSerializer(serializers.ModelSerializer):
    class Meta:
        model = Employee
        fields = '__all__'
```
- **View**: Create an API view using DRF’s `viewsets` in `views.py`.
```python
from rest_framework import viewsets
from .models import Employee
from .serializers import EmployeeSerializer

class EmployeeViewSet(viewsets.ModelViewSet):
    queryset = Employee.objects.all()
    serializer_class = EmployeeSerializer
```
- **Router**: Use DRF’s routing system in `urls.py`.
```python
from rest_framework.routers import DefaultRouter
from .views import EmployeeViewSet

router = DefaultRouter()
router.register(r'employees', EmployeeViewSet, basename='employee')

urlpatterns = router.urls
```

-	Include the `employees` URLs in `employee_mgmt/urls.py`:
```python
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/', include('employees.urls')),
]
```

**Scenario**: This is a Django REST Framework step because it involves creating RESTful API endpoints.

#### **7. Test the API**
- **Step**: Use tools like Postman, cURL, or DRF’s built-in API browser to test the endpoints:
- List all employees: `GET /api/employees/`
- Create a new employee: `POST /api/employees/`
- Update an employee: `PUT /api/employees/<id>/`
- Delete an employee: `DELETE /api/employees/<id>/`

- **Scenario**: This is a **Django REST Framework** step because it focuses on testing RESTful API functionality.

#### **8. Add a Template for HTML Rendering**

-	Create a view to render an employee list in HTML in views.py:

```python
from django.shortcuts import render
from .models import Employee

def employee_list(request):
    employees = Employee.objects.all()
    return render(request, 'employees/employee_list.html', {'employees': employees})
```
-	Create the template `employees/templates/employees/employee_list.html`:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Employee List</title>
</head>
<body>
    <h1>Employees</h1>
    <ul>
        {% for employee in employees %}
        <li>{{ employee.name }} - {{ employee.position }} - ${{ employee.salary }}</li>
        {% endfor %}
    </ul>
</body>
</html>
```

-	Add the URL pattern for the HTML view in `employees/urls.py`:

```python
from django.urls import path
from .views import employee_list

urlpatterns += [
    path('', employee_list, name='employee_list'),
]
```

##### Scenario: This is a Django step because it involves server-side rendering using Django templates.

### **9. Add HTML and API Routes**
Include the `employees` app URLs in the main project’s `urls.py`:
```python
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/', include('employees.urls')),  # DRF-related
    path('', include('employees.urls')),     # Django-related
]
```

##### Scenario: Both Django (HTML rendering) and Django REST Framework (API routes) are covered here.
---
### **10. Save Environment for Future Use (Conda-Specific Step)**
- **Step**: Save the Conda environment configuration for reproducibility.
```bash
conda env export > environment.yml
```
- Later, you can recreate the environment using:
```bash
conda env create -f environment.yml
```
- **Scenario**: This step is **common to both Django and DRF** as it ensures a consistent development environment.
---

### **Project Workflow Summary**
1. Django handles the admin panel and front-end rendering.
2. DRF provides a RESTful API to interact with employee data.
3. The database, managed by Django ORM, is shared between both.

### **Updated Workflow Summarycfor the Project**
| Step                             | Scenario                        |
|----------------------------------|---------------------------------|
| Setting Up the Conda Environment | Common to Django and DRF        |
| Create a Django Project          | Django                          |
| Create an App                    | Django                          |
| Define a Model                   | Django                          |
| Create Admin Interface           | Django                          |
| Create an API Using DRF          | Django REST Framework           |
| Test the API                     | Django REST Framework           |
| Add a Template for HTML Rendering| Django                          |
| Add HTML and API Routes          | Both Django and DRF             |
| Save Environment (Conda-specific)| Common to Django and DRF        |

-	By integrating Django and DRF, this project showcases how they complement each other
-	Django for server-side rendering and admin panels, and DRF for building robust APIs.
-	This step-by-step example demonstrates how Django and Django REST Framework complement each other.
---
