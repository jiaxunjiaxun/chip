# Django Girl Tutorial

[Django Girls](https://tutorial.djangogirls.org)

[Django Girls Extensions](https://tutorial-extensions.djangogirls.org)

## 0. Install python and virtualenv

```shell script
mkdir djgirls
cd djgirls
python3 -m venv myvenv
```

## 1. Working with virtualenv

### 1.1 Activate virtualenv

```shell script
source myvenv/bin/activate

myvenv/bin/deactivate
```

May be python 2.x or different version of python 3.x

### 1.2 Install Django

```shell script
python -m pip install --upgrade pip
vim requirements.txt
```
requirements.txt
```text
Django~=2.0.0
```
install Django
```shell script
pip install -r requirements.txt
```

### 1.3 Create Django project

```shell script
# Note: the dot . at the end
django-admin startproject mysite .
```

```shell script
djgirls
|--- manage.py
|--- mysite
|    |--- __init__.py
|    |--- settings.py
|    |--- urls.py
|    |--- wsgi.py
|--- myvenv
|--- requirements.txt
```

change settings

```python
# mysite/settings.py
TIME_ZONE = 'Aisa/Shanghai'

LANGUAGE_CODE = 'en-us'

STATIC_URL = '/static/'

STATIC_ROOT = os.path.join(BASE_DIR, 'static')

DEBUG = True

ALLOWED_HOSTS = [ 'localhost', '127.0.0.1', '[::1]', ]

DATABASES = {
    'default': {
        #--- SQLite
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),

        #--- MySQL
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'django_girls_tutorial',
        'USER': 'root',
        'PASSWORD': '123456',
        'HOST': 'localhost',
        'PORT': '3306',
    }
}
```
migrate project
```shell script
python manage.py migrate
```
run project
```shell script
python manage.py runserver
python manage.py runserver 0.0.0.0:8080
```

## 2. Develop with Django

### 2.1 Create application
```shell script
python manage.py startapp blog
```
change settings
```python
# mysite/settings.py
INSTALLED_APPS = [
    # ...
    'blog.apps.BlogConfig',
]
```

### 2.2 Create model
```python
# blog/models.py
```
migrate application
```shell script
python manage.py makemigrations blog
python manage.py migrate blog
```

### 2.3 Add to admin
```python
# blog/admin.py
admin.site.register(Post)
```
create super user
```shell script
python manage.py createsuperuser
```

### 2.4 Route, view and controller
route
```python
# mysite/urls.py
urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('blog.urls')),
]

# blog/urls.py
urlpatterns = [
    path('', views.post_list, name='post_list'),
]
```

controller
```python
# blog/views.py
def post_list(request):
    return render(request, 'blog/post_list.html', {})
```

template
```html
<!-- blog/templates/blog/post_list.html -->
<!-- Mustache template -->
```

### 2.5 Form and security
```python
# blog/forms.py
```

```html
{% if user.is_authenticated %}
```
```python
@login_required
def authenticated_method():
    pass
```
