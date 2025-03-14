##	Create URL for Multiple Apps Using Include Function‪

**Step-1:** Create `urls.py` file in every Apps

###	Question: Why we Create `urls.py` file in every Apps?
Ans: We create `urls.py` file in `employees` App because Into this (`urls.py`) file we define url for the function of `views.py`

```
from django.urls import path
from . import views # '.' means this imports the views.py file from the same directory.

urlpatterns = [
    path('', views.employee_list), # Empty String ('') means it is root address or default.
    path('emp/', views.employee),# This is the employee page.
]
```
**Step-2:** Connect the `urls.py` file of app with main project (here inner project- oms_project) `urls.py`

Example: Connect the `urls.py` file of `employees` app with main project `urls.py`

-	(i) Go to main project (oms_project) `urls.py` file
-	(ii)add `include` after `from django.urls import path`
		after the adding, the line will be `from django.urls import path, include`
-	(iii)Add the path `path('',include('employees.urls'))` under `urlpatterns = []` section. 
		If more function exist in the App `urls.py` than add all using similer way like `path('emp/', include('employees.urls'))`.
	
	**note-** 
		(i)in the path `path('',include('employees.urls'))`, the Empty String ('') means this path is root address. When anyone hit the root URL (172.0.0.1:8080)
			system will call the Empty String ('') path- `path('',include('employees.urls'))`
		(ii) to call the specific URL must hit using the specific URL as- `172.0.0.1:8080/emp`
		
	Finally the code will be like
	
	```
	from django.contrib import admin
	from django.urls import path, include

	urlpatterns = [
		path('admin/', admin.site.urls),
		path('',include('employees.urls')), # go to employees/urls.py/ and import employee_list function as default.
		# This is the root address. 
		# Empty String ('') means the default page. When hit the root address, it will call the employee_list function from views.py.
		path('emp/', include('employees.urls')),# This is the employee page.
	]
	```
	
** Explain the URL: http://127.0.0.1:8000/ml/rf/**

Here is the url patterns of App `Machine Learning` under `urls.py` file
```
urlpatterns = [
    path('', views.machine_learning_index), # Empty String ('') means it is root address or default.
    path('dt/', views.dt), # This is the address for Decision Tree page.
    path('knn/', views.knn), # This is the address for K-Nearest Neighbors page.
    path('rf/', views.rf), # This is the address for Random Forest page.
]
```

`urls.py` file of main project

```
urlpatterns = [
    path('admin/', admin.site.urls),
    path('',include('employees.urls')), # go to employees/urls.py/ and import employee_list function as default.
    # This is the root address. 
    # Empty String ('') means the default page. When hit the root address, it will call the employee_list function from views.py.
    path('emp/', include('employees.urls')),# This is the employee page.
    path('ml/', include('machine_learning.urls')),# This is the machine learning page.
]
```

###	Explanation:

Let's break down how the URL `http://127.0.0.1:8000/ml/rf/` gets resolved based on the provided `urls.py` configurations:

1.  **Base URL:**
    * `http://127.0.0.1:8000` indicates the local server address and port.

2.  **Project-Level Routing:**
    * The first part of the path, `/ml/`, matches this line in the main project's `urls.py`:
        ```python
        path('ml/', include('machine_learning.urls')),
        ```
    * This tells Django: "If the URL starts with `/ml/`, delegate the rest of the URL to the `machine_learning.urls` module."

3.  **App-Level Routing:**
    * Now, Django looks at the `machine_learning/urls.py` file.
    * The remaining part of the path is `/rf/`.
    * This matches this line:
        ```python
        path('rf/', views.rf),
        ```
    * This tells Django: "If the remaining URL is `/rf/`, call the `rf` function from the `views.py` file within the `machine_learning` app."

**Therefore, the URL `http://127.0.0.1:8000/ml/rf/` will:**

* Route to the `machine_learning` app.
* Execute the `rf` function within the `views.py` file of the `machine_learning` app.