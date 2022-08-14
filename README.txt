https://tutorial.djangogirls.org/hu/

---------------------
---------------------
Django Girls Tutorial
---------------------
---------------------



* Tested
--------

 * django 4.0.2
 * django-url-framework 0.5.3




* Linux Fedora Install
----------------------

 $ sudo dnf install python3.4
 $ sudo dnf install python-django
 (pip install django django-url-framework
 python3 -m pip install --upgrade pip)




* Environment
-------------

 $ mkdir djangogirls
 $ cd djangogirls

 $ python3 -m venv myvenv





* DJANGO project
----------------

 $ django-admin startproject mysite .

 djangogirls
 ├───manage.py
 └───mysite
        settings.py
        urls.py
        wsgi.py
        __init__.py


 * mysite/settings.py:

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




* Database (SQLITE)
-------------------

 $ python manage.py makemigrations blog
 $ python manage.py migrate blog
 (python manage.py migrate)




* Web server
------------

 $ python manage.py runserver
 (python manage.py runserver 0:8000)

   Browser: http://127.0.0.1:8000/




* BLOG application
------------------

 $ python manage.py startapp blog

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

 * mysite/settings.py:

    INSTALLED_APPS = [
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',
        'blog',
    ]




* Model
--------

 * blog/models.py:

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


 $ python manage.py makemigrations blog
 $ python manage.py migrate blog
 (python manage.py migrate)




* DJANGO Admin
--------------

 * blog/admin.py:

    from django.contrib import admin
    from .models import Post
    admin.site.register(Post)

 $ python manage.py runserver
 Browser: http://127.0.0.1:8000/admin/


 * Superuser:

 $ python manage.py createsuperuser
 Username: admin
 Email address: admin@admin.com
 Password:
 Password (again):
 Superuser created successfully.




* DEPLOY (Github)
-----------------

 * Register to GitHUB.com
 * Commandline:
 $ git init
 Initialized empty Git repository in ~/djangogirls/.git/
 $ git config --global user.name "Your Name"
 $ git config --global user.email you@example.com

 # make file .gitignore

 $ git status
 On branch master           # On branch main

 $ git add --all .
 $ git commit -m "My Django Girls app, first commit"
 [...]
 13 files changed, 200 insertions(+)
 create mode 100644 .gitignore
 [...]

 $ git remote add origin https://github.com/<your-github-username>/my-first-blog.git
 $ git push -u origin master           # git push -u origin main

 Username for 'https://github.com': hjwp
 Password for 'https://hjwp@github.com':
 Counting objects: 6, done.
 Writing objects: 100% (6/6), 200 bytes | 0 bytes/s, done.
 Total 3 (delta 0), reused 0 (delta 0)
 To https://github.com/hjwp/my-first-blog.git
  * [new branch]      master -> master
 Branch master set up to track remote branch master from origin.




* DJANGO urls
-------------

 * mysite/urls.py:

    from django.contrib import admin
    from django.urls import path
    from django.urls import include
    urlpatterns = [
        path('admin/', admin.site.urls),
        path('', include('blog.urls')),
    ]


 * blog/urls.py

    from django.urls import re_path as url
    from . import views


    urlpatterns = [
        url(r'^$', views.post_list, name='post_list'),
    ]




* DJANGO views
--------------

 * blog/views.py:

    from django.shortcuts import render
    # Create your views here.
    def post_list(request):
        return render(request, 'blog/post_list.html', {})



* HTML
------

 * mysite/settings.py

    TEMPLATES = [
        {
            'BACKEND': 'django.template.backends.django.DjangoTemplates',
            'DIRS': [os.path.join(BASE_DIR, 'templates')],

    * make dir 'templates'

 * templates/post_list.html file:

    <html>
        <p>Hi there!</p>
        <p>It works!</p>
    </html>




* DJANGO ORM QuerySet
---------------------

 $ python manage.py shell

 # https://tutorial.djangogirls.org/hu/django_orm/





* Dinamic data in template
--------------------------

 * blog/models.py:

    from django.conf import settings
    from django.db import models
    from django.utils import timezone


 * blog/views.py:

    from django.shortcuts import render
    from django.utils import timezone
    from .models import Post

    def post_list(request):
        posts = Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')
        return render(request, 'post_list.html', {'posts': posts})




* DJANGO template
-----------------

 * templates/post_list.html file:

    <div>
    <h1><a href="/">Django Girls Blog</a></h1>
    </div>

    <hr>
    {% for post in posts %}
    <div>
        <p>published: {{ post.published_date }}</p>
        <h1><a href="">{{ post.title }}</a></h1>
        <p>{{ post.text|linebreaksbr }}</p>
    </div>
    {% endfor %}
    <hr>





* CSS Bootstrap
---------------

 * templates/post_list.html file:

  {% load static %}
  <html>
    <head>
        <title>Django Girls blog</title>
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css">
        <link rel="stylesheet" href="{% static 'css/blog.css' %}">
    </head>
    <body>
        <div>
            <h1><a href="/">Django Girls Blog</a></h1>
        </div>

        {% for post in posts %}
            <div>
                <p>published: {{ post.published_date }}</p>
                <h1><a href="">{{ post.title }}</a></h1>
                <p>{{ post.text|linebreaksbr }}</p>
            </div>
        {% endfor %}
    </body>
  </html>





* DJANGO forms
--------------

 * blog/forms.py:

    from django import forms
    from .models import Post

    class PostForm(forms.ModelForm):

    class Meta:
        model = Post
        fields = ('title', 'text',)


 * templates/base.html file:

    {% load static %}
    <html>
        <head>
            <title>Django Girls blog</title>
            <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
            <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css">
            <link href='//fonts.googleapis.com/css?family=Lobster&subset=latin,latin-ext' rel='stylesheet' type='text/css'>
            <link rel="stylesheet" href="{% static 'css/blog.css' %}">
        </head>
        <body>
            <div class="page-header">
                    {% if user.is_authenticated %}
                    <a href="{% url 'post_new' %}" class="top-menu"><span class="glyphicon glyphicon-plus"></span></a>
                    {% endif %}
                <h1><a href="/">Django Girls Blog</a></h1>
            </div>
            <div class="content container">
                <div class="row">
                    <div class="col-md-8">
                        {% block content %}
                        {% endblock %}
                    </div>
                </div>
            </div>
        </body>
    </html>


 * templates/post_edit.html file:

    {% extends 'base.html' %}

    {% block content %}
        <h1>New post</h1>
        <form method="POST" class="post-form">{% csrf_token %}
            {{ form.as_p }}
            <button type="submit" class="save btn btn-default">Save</button>
        </form>
    {% endblock %}


 * templates/post_detail.html file:

    {% extends 'base.html' %}

    {% block content %}
        <div class="post">
            {% if post.published_date %}
                <div class="date">
                    {{ post.published_date }}
                </div>
            {% endif %}
            <a class="btn btn-default" href="{% url 'post_edit' pk=post.pk %}"><span class="glyphicon glyphicon-pencil"></span></a>
            <h1>{{ post.title }}</h1>
            <p>{{ post.text|linebreaksbr }}</p>
        </div>
    {% endblock %}


 * templates/post_list.html file:

    {% load static %}
    <html>
        <head>
            <title>Django Girls blog</title>
            <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
            <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css">
            <link rel="stylesheet" href="{% static 'css/blog.css' %}">
        </head>
        <body>
            <div class="page-header">
                <h1><a href="/">Django Girls Blog</a></h1>
            </div>

            <div class="content container">
                <div class="row">
                    <div class="col-md-8">
                        {% for post in posts %}
                            <div class="post">
                                <div class="date">
                                    {{ post.published_date }}
                                </div>
                                <h1><a href="">{{ post.title }}</a></h1>
                                <p>{{ post.text|linebreaksbr }}</p>
                            </div>
                        {% endfor %}
                    </div>
                </div>
            </div>

        </body>
    </html>


 * blog/urls.py:

    from django.urls import re_path as url
    from . import views


    urlpatterns = [
        url(r'^$', views.post_list, name='post_list'),
        url(r'^post/(?P<pk>[0-9]+)/$', views.post_detail, name='post_detail'),
        url(r'^post/(?P<pk>[0-9]+)/edit/$', views.post_edit, name='post_edit'),
        url(r'^post/new/$', views.post_new, name='post_new'),
    ]


 * blog/views.py:

    from .forms import PostForm

    def post_new(request):
    if request.method == "POST":
        form = PostForm(request.POST)
        if form.is_valid():
            post = form.save(commit=False)
            post.author = request.user
            post.published_date = timezone.now()
            post.save()
            return redirect('post_detail', pk=post.pk)
    else:
        form = PostForm()
    return render(request, 'post_edit.html', {'form': form})


    def post_detail(request, pk):
    post = get_object_or_404(Post, pk=pk)
    return render(request, 'post_detail.html', {'post': post})


    def post_edit(request, pk):
    post = get_object_or_404(Post, pk=pk)
    if request.method == "POST":
        form = PostForm(request.POST, instance=post)
        if form.is_valid():
            post = form.save(commit=False)
            post.author = request.user
            post.published_date = timezone.now()
            post.save()
            return redirect('post_detail', pk=post.pk)
    else:
        form = PostForm(instance=post)
    return render(request, 'post_edit.html', {'form': form})





