Django REST Framework (DRF) comes with many advanced features that make it robust, flexible, and suitable for building modern APIs. Below are some key advanced features, along with examples, to help you understand and apply them effectively:

1. Token-Based Authentication
Token-based authentication allows users to authenticate API requests using a token instead of credentials like a username and password. This is useful for stateless APIs.

Steps to Implement:
Install DRF's token authentication module:

```bash
pip install djangorestframework
```

Add rest_framework.authtoken to INSTALLED_APPS in settings.py.

Run migrations to create a table for tokens:

```bash
python manage.py migrate
```

Add token authentication to your API views.

Example: Token Authentication Setup

Generate tokens for users:

```python
from rest_framework.authtoken.models import Token
from django.contrib.auth.models import User

user = User.objects.get(username='example_user')
token, created = Token.objects.get_or_create(user=user)
print(token.key)
```

Use the token in API requests (e.g., with Authorization: Token <your-token> in headers).

2. Viewsets and Routers
ViewSets simplify the combination of logic for multiple HTTP methods (GET, POST, PUT, etc.). Routers automatically generate URLs for these ViewSets.

Steps to Implement:
Create a ViewSet for a model.

Register the ViewSet with a Router.

Example: Using ViewSets and Routers

Create a PostViewSet in blog/views.py:

```python
from rest_framework import viewsets
from .models import Post
from .serializers import PostSerializer

class PostViewSet(viewsets.ModelViewSet):
    queryset = Post.objects.all()
    serializer_class = PostSerializer
```

Register it in urls.py using a Router:

```python
from rest_framework.routers import DefaultRouter
from blog.views import PostViewSet

router = DefaultRouter()
router.register(r'posts', PostViewSet, basename='post')

urlpatterns = router.urls
```

Now, your API supports all CRUD operations on /posts/ without manually defining URLs for each operation.

3. Filtering and Search
DRF supports powerful filtering and searching mechanisms using django-filter.

Steps to Implement:
Install django-filter:

```bash
pip install django-filter
```

Add rest_framework.filters in settings.py.

Example: Filtering and Search

Add filtering in a ViewSet:

```python
from rest_framework import viewsets, filters
from .models import Product
from .serializers import ProductSerializer

class ProductViewSet(viewsets.ModelViewSet):
    queryset = Product.objects.all()
    serializer_class = ProductSerializer
    filter_backends = [filters.SearchFilter]
    search_fields = ['name', 'description']
```

Access endpoints with search queries, e.g., /products/?search=laptop.

4. Pagination
Pagination is essential for APIs that return large datasets.

Steps to Implement:
Add default pagination settings in settings.py:

```python
REST_FRAMEWORK = {
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
    'PAGE_SIZE': 10,
}
```

Example: Pagination

Create a paginated response for a ViewSet:

```python
from rest_framework.pagination import PageNumberPagination

class CustomPagination(PageNumberPagination):
    page_size = 5
```

Now, results are automatically paginated with links to next and previous pages.

5. Custom Permissions
Custom permissions allow you to enforce business rules on APIs.

Example: Allow Only Authors to Edit Their Posts
Create a custom permission in blog/permissions.py:

```python
from rest_framework.permissions import BasePermission

class IsAuthorOrReadOnly(BasePermission):
    def has_object_permission(self, request, view, obj):
        if request.method in ['GET', 'HEAD', 'OPTIONS']:
            return True
        return obj.author == request.user
```

Apply the permission in a ViewSet:

```python
from rest_framework.permissions import IsAuthenticated
from .permissions import IsAuthorOrReadOnly

class PostViewSet(viewsets.ModelViewSet):
    queryset = Post.objects.all()
    serializer_class = PostSerializer
    permission_classes = [IsAuthenticated, IsAuthorOrReadOnly]
```

Now, only authors of a post can modify it.

6. Throttling
Throttling limits the rate of API requests to protect against misuse or abuse.

Steps to Implement:
Add throttling in settings.py:

```python
REST_FRAMEWORK = {
    'DEFAULT_THROTTLE_CLASSES': [
        'rest_framework.throttling.AnonRateThrottle',
        'rest_framework.throttling.UserRateThrottle',
    ],
    'DEFAULT_THROTTLE_RATES': {
        'anon': '5/day',
        'user': '1000/day'
    }
}
```

7. Django REST Framework with Swagger or Redoc
You can auto-generate API documentation using tools like Swagger or Redoc.

Steps to Implement Swagger:
Install drf-yasg:

```bash
pip install drf-yasg
```

Add Swagger documentation in urls.py:

```python
from rest_framework import permissions
from drf_yasg.views import get_schema_view
from drf_yasg import openapi

schema_view = get_schema_view(
    openapi.Info(
        title="API Documentation",
        default_version="v1",
    ),
    public=True,
    permission_classes=(permissions.AllowAny,),
)

urlpatterns = [
    path('swagger/', schema_view.with_ui('swagger', cache_timeout=0), name='schema-swagger-ui'),
]
```

Visit /swagger/ to view the API documentation.

8. Caching API Responses
Caching can improve the performance of APIs by storing responses for frequently requested data.

Example: Using Django’s Cache Framework
Configure caching in settings.py:

```python
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.locmem.LocMemCache',
    }
}
```

Apply caching in views using DRF's cache_response decorator:

```python
from rest_framework.decorators import api_view
from rest_framework.response import Response
from rest_framework.decorators import cache_response

@api_view(['GET'])
@cache_response(timeout=60*15)
def cached_post_list(request):
    posts = Post.objects.all()
    serializer = PostSerializer(posts, many=True)
    return Response(serializer.data)
```

These advanced features make Django REST Framework a powerful choice for modern APIs.