#	Create Template Folder, Create Template Files (.html), Integrate the Template Files with Views file
-	Create `Template Folder` and Rendering `Template Files`
-	Create `Folder` Inside `Template Folder` for App
-	Create Template Files (.html)
-	Integrate the Template Files with Views file

###	Step-1: Create 'templates' folder in the outer project folder 'oms_project'

-	Templet folder can't create inner project folder it shall always be created in the outer project folder

###	Step-2: Add 'templates' folder in the main project
	i) Go to inner project folder 'oms_project' > 'settings.py' file 
	
-	To define template directory store the directory path in a variable
-	At the below of the project directory 'BASE_DIR' define the 'templates' directory as 
	`TEMPLATE_DIR = os.path.join(BASE_DIR,'templates')`, that means joint the 'templates' directory with the project main directory 'BASE_DIR'
-	Importing 'os' as  `import os`
(Project directory is the "BASE_DIR")

**We have fininshed successfully defie directory**

###	Step-3: Define the directory under templates section `"DIRS": [],` in 'settings.py' file as `"DIRS": [TEMPLATE_DIR]`

```
TEMPLATES = [
    {
        "BACKEND": "django.template.backends.django.DjangoTemplates",
        "DIRS": [TEMPLATE_DIR],
        "APP_DIRS": True,
        "OPTIONS": {
            "context_processors": [
                "django.template.context_processors.debug",
                "django.template.context_processors.request",
                "django.contrib.auth.context_processors.auth",
                "django.contrib.messages.context_processors.messages",
            ],
        },
    },
]
```
**We have successfully connect the `TEMPLATE_DIR` with main `BASE_DIR` directory**

###	Step-4: Create separate forder for separate App into 'templates' folder
-	Create separate forder for separate App into `templates` folder. **Example** `employees` for `employees` App; `audit` for `audit` app 

###	Step-5:	Now we create some `HTML` file for `App` in `templetes folder` and `connects` the templets with `views` file

-	Example- Create `employee.html` file in the `employees` folder

###	Step-5:	Now we create `views` file for the App `employees` in `templetes folder`

-	**Path:** oms_project/templates/employees/employee.html

```
<! DOCTYPE html>
<html lang="en">

<head>
<meta charset="UTF-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Document</title>
</head>

<body>
<h1>Bank Asia offering lots of befefits for employees</h1>
</body>

</htm1>

```

### Step-6: Integrate the `employee.html` file with `views.py` file of the App project

-	go to App `employees>views.py`
-	Create function in the `views.py` file as following way

``` bash

from django.shortcuts import render

# Create your views here.
def employee(request):
return render(request, 'employee.html')

```