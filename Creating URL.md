##	Create URL for Multiple Apps Using Include Function‪

**Step-1:** Create `urls.py` file in every Apps

Question: Why we Create `urls.py` file in every Apps?
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
	
Step-3: 