## Building a Django Project with REST API and Best Practices

### This guide assumes:
- You have Django installed in your Conda environment.
- You are familiar with the basics of Django (models, views, URLs).

### 1. Setting Up the Project and Conda Environment

#### Activate Your Conda Environment:
If you're using Conda, activate your environment and ensure Django and DRF are installed.

```bash
conda activate myenv
pip install django djangorestframework
```

You may also install additional tools for deployment later:

```bash
pip install gunicorn psycopg2-binary whitenoise
```

#### Start Your Django Project:
Create a Django project named `multiproject` and navigate to it.

```bash
django-admin startproject multiproject .
```

### 2. Creating and Managing Apps
Let’s create two apps: `blog` (for blog management) and `store` (for e-commerce).

```bash
python manage.py startapp blog
python manage.py startapp store
```

Add these apps to the `INSTALLED_APPS` section in `multiproject/settings.py`.

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework',
    'blog',
    'store',
]
```

### 3. Defining Models for Each App

#### Blog App Models:
In `blog/models.py`, create a `Post` model.

```python
from django.db import models
from django.contrib.auth.models import User

class Post(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    author = models.ForeignKey(User, on_delete=models.CASCADE)
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.title
```

#### Store App Models:
In `store/models.py`, create a `Product` model.

```python
from django.db import models

class Product(models.Model):
    name = models.CharField(max_length=200)
    description = models.TextField()
    price = models.DecimalField(max_digits=10, decimal_places=2)
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.name
```

Run migrations:

```bash
python manage.py makemigrations
python manage.py migrate
```

### 4. Setting Up Django REST Framework (DRF)

#### Serializers:
Create serializers for the models to convert them into JSON format.

##### Blog Serializer (`blog/serializers.py`):

```python
from rest_framework import serializers
from .models import Post

class PostSerializer(serializers.ModelSerializer):
    class Meta:
        model = Post
        fields = '__all__'
```

##### Store Serializer (`store/serializers.py`):

```python
from rest_framework import serializers
from .models import Product

class ProductSerializer(serializers.ModelSerializer):
    class Meta:
        model = Product
        fields = '__all__'
```

#### Views:
Create API views for each app.

##### Blog Views (`blog/views.py`):

```python
from rest_framework.decorators import api_view
from rest_framework.response import Response
from .models import Post
from .serializers import PostSerializer

@api_view(['GET'])
def post_list(request):
    posts = Post.objects.all()
    serializer = PostSerializer(posts, many=True)
    return Response(serializer.data)
```

##### Store Views (`store/views.py`):

```python
from rest_framework.decorators import api_view
from rest_framework.response import Response
from .models import Product
from .serializers import ProductSerializer

@api_view(['GET'])
def product_list(request):
    products = Product.objects.all()
    serializer = ProductSerializer(products, many=True)
    return Response(serializer.data)
```

#### URLs:
Define API endpoints in the `urls.py` for each app.

##### Blog URLs (`blog/urls.py`):

```python
from django.urls import path
from . import views

urlpatterns = [
    path('posts/', views.post_list, name='post-list'),
]
```

##### Store URLs (`store/urls.py`):

```python
from django.urls import path
from . import views

urlpatterns = [
    path('products/', views.product_list, name='product-list'),
]
```

### 5. Adding User Authentication
Use Django’s built-in User model for authentication.

#### Register Users via REST API:
Create a new app called `accounts`.

```bash
python manage.py startapp accounts
```

In `accounts/views.py`, create a registration view.

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from django.contrib.auth.models import User
from rest_framework import status

class RegisterView(APIView):
    def post(self, request):
        username = request.data.get('username')
        password = request.data.get('password')

        if User.objects.filter(username=username).exists():
            return Response({'error': 'Username already exists'}, status=status.HTTP_400_BAD_REQUEST)

        user = User.objects.create_user(username=username, password=password)
        return Response({'message': 'User created successfully'}, status=status.HTTP_201_CREATED)
```

Add `accounts/urls.py`:

```python
from django.urls import path
from .views import RegisterView

urlpatterns = [
    path('register/', RegisterView.as_view(), name='register'),
]
```

### 6. Deployment Best Practices

#### Static Files with Whitenoise:
Update `settings.py` to handle static files in production.

```python
STATIC_URL = '/static/'
STATIC_ROOT = BASE_DIR / 'staticfiles'

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'whitenoise.middleware.WhiteNoiseMiddleware',
    # other middlewares...
]

STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'
```

#### Database:
Use PostgreSQL for production. Install and configure `psycopg2`.

```bash
pip install psycopg2-binary
```

Update `settings.py`:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'dbname',
        'USER': 'dbuser',
        'PASSWORD': 'dbpassword',
        'HOST': 'localhost',
        'PORT': '5432',
    }
}
```

#### Gunicorn for WSGI:
Install Gunicorn:

```bash
pip install gunicorn
```

Run the application:

```bash
gunicorn multiproject.wsgi
```

#### Deploy to Heroku or Render:
Create a `Procfile`:

```bash
web: gunicorn multiproject.wsgi
```

Use tools like Docker for containerized deployments.

### Testing Your Setup
Test API endpoints via Postman or cURL:

```bash
curl http://127.0.0.1:8000/blog/posts/
curl http://127.0.0.1:8000/store/products/
```
---
### Lesson-15 Django REST Framework (DRF) advanced features