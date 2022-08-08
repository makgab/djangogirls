https://tutorial.djangogirls.org/hu/

---------------------
Django Girls Tutorial
---------------------


* Linux Fedora Install
 # sudo dnf install python3.4
 # sudo dnf install python-django
 (pip install django
 python3 -m pip install --upgrade pip)


* Environment

 # mkdir djangogirls
 # cd djangogirls

 # python3 -m venv myvenv


* DJANGO projekt

 # django-admin startproject mysite .

 djangogirls
 ├───manage.py
 └───mysite
        settings.py
        urls.py
        wsgi.py
        __init__.py


* mysite/settings.py

 TIME_ZONE = 'Europe/Budapest'
 STATIC_URL = '/static/'
 STATIC_ROOT = os.path.join(BASE_DIR, 'static')
 ALLOWED_HOSTS = ['127.0.0.1', '.pythonanywhere.com']

 DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.sqlite3',
            'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
        }
    }

* Adatbázis (SQLITE)

 # python manage.py migrate


* Webszerver

 # python manage.py runserver
 (python manage.py runserver 0:8000)

   Browser: http://127.0.0.1:8000/


* BLOG application

 # python manage.py startapp blog

 djangogirls
 ├── mysite
 |       __init__.py
 |       settings.py
 |       urls.py
 |       wsgi.py
 ├── manage.py
 └── blog
     ├── migrations
     |       __init__.py
     ├── __init__.py
     ├── admin.py
     ├── models.py
     ├── tests.py
     └── views.py

* mysite/settings.py
 INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog',
 ]


* Modell:
 blog/models.py:

 from django.conf import settings
 from django.db import models
 from django.utils import timezone

 class Post(models.Model):
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    title = models.CharField(max_length=200)
    text = models.TextField()
    created_date = models.DateTimeField(
            default=timezone.now)
    published_date = models.DateTimeField(
            blank=True, null=True)

    def publish(self):
        self.published_date = timezone.now()
        self.save()

    def __str__(self):
        return self.title


 # python manage.py makemigrations blog
 # python manage.py migrate blog
 (python manage.py migrate)


* DJANGO Admin

 blog/admin.py:

 from django.contrib import admin
 from .models import Post
 admin.site.register(Post)

 # python manage.py runserver
 Browser: http://127.0.0.1:8000/admin/


 * Superuser:

 $ python manage.py createsuperuser
 Username: admin
 Email address: admin@admin.com
 Password:
 Password (again):
 Superuser created successfully.








