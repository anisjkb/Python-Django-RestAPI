#	Modular and template-driven approach- Create Template Folder and Rendering Template Files

Let’s now focus on creating a `templates` folder, rendering HTML template files for the views we built earlier, and explain this step-by-step.
We'll finish with a clear project structure display.

---

## **Step 1: Setting Up the Templates Folder**

1. **Create a `templates` folder in your project root directory:**
   - Inside your project (`myproject`) directory, create a new folder called `templates`. This will hold all the HTML files for your app views.

     **Folder Structure:**
     ```
     myproject/
         ├── templates/
     ```

2. **Organize by App:**
   - Create subfolders within the `templates` folder, one for each app (e.g., `blog/` and `store/`).

     **Updated Structure:**
     ```
     myproject/
         ├── templates/
             ├── blog/
             ├── store/
     ```

3. **Update `settings.py` to Add Template Directory:**
   Tell Django where to find the `templates` directory by updating `TEMPLATES` in `settings.py`.

   ```python
   TEMPLATES = [
       {
           'BACKEND': 'django.template.backends.django.DjangoTemplates',
           'DIRS': [BASE_DIR / 'templates'],  # Path to templates folder
           'APP_DIRS': True,
           'OPTIONS': {
               'context_processors': [
                   'django.template.context_processors.debug',
                   'django.template.context_processors.request',
                   'django.contrib.auth.context_processors.auth',
                   'django.contrib.messages.context_processors.messages',
               ],
           },
       },
   ]
   ```

---

## **Step 2: Creating Template Files**

### **For the Blog App**
1. **Create a Home Template:**
   File: `templates/blog/home.html`
   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>Blog Home</title>
   </head>
   <body>
       <h1>Welcome to the Blog</h1>
       <p>This is the home page for our blog application.</p>
       <a href="{% url 'blog:about' %}">Go to About Page</a>
   </body>
   </html>
   ```

2. **Create an About Template:**
   File: `templates/blog/about.html`
   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>About the Blog</title>
   </head>
   <body>
       <h1>About the Blog</h1>
       <p>This page provides information about our blog application.</p>
       <a href="{% url 'blog:home' %}">Back to Home</a>
   </body>
   </html>
   ```

### **For the Store App**
1. **Create a Home Template:**
   File: `templates/store/home.html`
   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>Store Home</title>
   </head>
   <body>
       <h1>Welcome to the Store</h1>
       <p>This is the home page for our store application.</p>
       <a href="{% url 'store:products' %}">View Products</a>
   </body>
   </html>
   ```

2. **Create a Products Template:**
   File: `templates/store/products.html`
   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>Product List</title>
   </head>
   <body>
       <h1>Available Products</h1>
       <ul>
           <li>Product 1</li>
           <li>Product 2</li>
           <li>Product 3</li>
       </ul>
       <a href="{% url 'store:home' %}">Back to Home</a>
   </body>
   </html>
   ```

---

## **Step 3: Updating Views to Render Templates**

### **Blog App Views**
Update the `blog/views.py` file to render templates:
```python
from django.shortcuts import render

def home(request):
    return render(request, 'blog/home.html')

def about(request):
    return render(request, 'blog/about.html')
```

### **Store App Views**
Update the `store/views.py` file to render templates:
```python
from django.shortcuts import render

def home(request):
    return render(request, 'store/home.html')

def products(request):
    return render(request, 'store/products.html')
```

---

## **Step 4: Testing the Application**

1. **Run the Server:**
   ```bash
   python manage.py runserver
   ```

2. **Visit the Following URLs:**
   - Blog Home: `http://127.0.0.1:8000/blog/`
   - Blog About: `http://127.0.0.1:8000/blog/about/`
   - Store Home: `http://127.0.0.1:8000/store/`
   - Store Products: `http://127.0.0.1:8000/store/products/`

---

## **Final Project Structure**

Here’s what the project structure will look like after all steps:

```
myproject/
    ├── blog/
    │   ├── views.py
    │   ├── urls.py
    │   ├── models.py
    │   ├── migrations/
    │   └── __init__.py
    ├── store/
    │   ├── views.py
    │   ├── urls.py
    │   ├── models.py
    │   ├── migrations/
    │   └── __init__.py
    ├── templates/
    │   ├── blog/
    │   │   ├── home.html
    │   │   └── about.html
    │   ├── store/
    │   │   ├── home.html
    │   │   └── products.html
    ├── myproject/
    │   ├── settings.py
    │   ├── urls.py
    │   ├── wsgi.py
    │   ├── asgi.py
    │   └── __init__.py
    ├── db.sqlite3
    └── manage.py
```

---

### **Explanation of Key Features**

1. **Templates Folder:**
   The `templates` folder is used for HTML files to separate presentation logic (HTML) from business logic (views).

2. **Modular Templates for Each App:**
   Each app (`blog` and `store`) has its own folder within `templates` for better organization and scalability.

3. **Template Rendering:**
   The `render()` function dynamically sends data from views to templates, allowing you to display or manipulate data in the browser.

4. **URLs and Namespaces:**
   App-specific `urls.py` files and namespaces ensure clarity and avoid conflicts when linking templates or handling navigation.

---

This modular and template-driven approach keeps the project clean, organized, and highly scalable, making it suitable for modern web development!

### In Lesson-17 we will discuss how Django Template Language (DTL) work with Variable.