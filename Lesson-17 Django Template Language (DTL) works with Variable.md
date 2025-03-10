##	Django Template Language (DTL) work with Variable

Django Template Language (DTL) is a templating system provided by Django to dynamically render HTML by injecting variables, logic, and control structures. It separates presentation logic (HTML) from business logic (views), allowing data passed from views to be displayed in templates efficiently.

---

### **How DTL Works with Variables**

1. **Passing Variables from Views to Templates:**
   Variables are passed to templates using the `render()` function in views. The `render()` function takes three arguments:
   - The request object.
   - The template name.
   - A dictionary containing variables and their values (referred to as the **context**).

   **Example in a View:**
   ```python
   from django.shortcuts import render

   def home(request):
       context = {
           'username': 'Md',
           'age': 25
       }
       return render(request, 'home.html', context)
   ```

2. **Accessing Variables in Templates:**
   Variables in the context dictionary can be accessed in templates using double curly braces (`{{ }}`).

   **Example in a Template:**
   ```html
   <h1>Welcome, {{ username }}</h1>
   <p>You are {{ age }} years old.</p>
   ```

   **Rendered Output:**
   ```html
   <h1>Welcome, Md</h1>
   <p>You are 25 years old.</p>
   ```

3. **Default Behavior for Missing Variables:**
   If a variable is not provided in the context, it is treated as an empty string in the template, and no error is thrown. For example:
   ```html
   <p>Hello, {{ non_existent_var }}</p>
   ```
   **Output:**
   ```html
   <p>Hello, </p>
   ```

4. **Using Filters with Variables:**
   Filters modify variables in the template. They are applied using a pipe (`|`) symbol.

   **Example of Filters:**
   ```html
   <p>{{ username|upper }}</p> <!-- Converts username to uppercase -->
   <p>{{ age|add:"5" }}</p>   <!-- Adds 5 to the age -->
   ```

   **Rendered Output:**
   ```html
   <p>MD</p>
   <p>30</p>
   ```

5. **Accessing Dictionary Keys and Looping Through Variables:**
   Variables can be complex data structures like dictionaries and lists. Templates allow you to iterate through lists and access dictionary keys.

   **Example in a View:**
   ```python
   def home(request):
       context = {
           'user_data': {
               'name': 'Md',
               'email': 'md@example.com',
           },
           'items': ['Item 1', 'Item 2', 'Item 3']
       }
       return render(request, 'home.html', context)
   ```

   **Template Example:**
   ```html
   <h1>User Info</h1>
   <p>Name: {{ user_data.name }}</p>
   <p>Email: {{ user_data.email }}</p>

   <h2>Items:</h2>
   <ul>
       {% for item in items %}
           <li>{{ item }}</li>
       {% endfor %}
   </ul>
   ```

   **Rendered Output:**
   ```html
   <h1>User Info</h1>
   <p>Name: Md</p>
   <p>Email: md@example.com</p>

   <h2>Items:</h2>
   <ul>
       <li>Item 1</li>
       <li>Item 2</li>
       <li>Item 3</li>
   </ul>
   ```

---

### **Did We Use DTL in the Above Discussion?**
Yes, we did use DTL in the previous discussion when creating the templates for the **Blog** and **Store** apps. Here’s how DTL was applied:

1. **Rendering Variables:**
   While we didn't explicitly pass variables in the example views, DTL placeholders like `{% url %}` were used for dynamic URL generation in templates.

   **Example:**
   ```html
   <a href="{% url 'blog:about' %}">Go to About Page</a>
   ```
   This dynamically generates the URL for the `about` page of the `blog` app using Django's URL resolver.

2. **Contextual Usage of Templates:**
   If variables were passed from views (e.g., blog post titles, store products), DTL would have been used to inject the variable values dynamically.

3. **Expandable Example (DTL with Variables):**
   To demonstrate DTL further, let’s modify the earlier `Blog` app example to pass variables:
   ```python
   # views.py
   from django.shortcuts import render

   def home(request):
       context = {
           'posts': [
               {'title': 'Post 1', 'author': 'Md'},
               {'title': 'Post 2', 'author': 'John'}
           ]
       }
       return render(request, 'blog/home.html', context)
   ```

   **Template (`blog/home.html`):**
   ```html
   <h1>Blog Posts</h1>
   <ul>
       {% for post in posts %}
           <li>{{ post.title }} by {{ post.author }}</li>
       {% endfor %}
   </ul>
   ```

   **Rendered Output:**
   ```html
   <h1>Blog Posts</h1>
   <ul>
       <li>Post 1 by Md</li>
       <li>Post 2 by John</li>
   </ul>
   ```

---

### **Conclusion**
- **How DTL Works:** DTL allows you to render dynamic content from views into templates using variables, filters, loops, and conditionals.
- **Usage in the Example:** While we used `{% url %}` to demonstrate DTL for URL resolution, variables could further enhance the templates by dynamically injecting data like blog posts or product lists.

Let me know if you'd like me to expand on advanced DTL features like custom filters, template inheritance, or conditional logic!