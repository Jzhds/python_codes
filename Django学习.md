# Django

#### saving ForeignKey and Manytomanyfield fields

```python
>>> from blog.models import Blog, Entry
>>> entry = Entry.objects.get(pk=1)
>>> cheese_blog = Blog.objects.get(name="Cheddar Talk")
>>> entry.blog = cheese_blog
>>> entry.save()
many to many field
>>> from blog.models import Author
>>> joe = Author.objects.create(name="Joe")
>>> entry.authors.add(joe)

>>> john = Author.objects.create(name="John")
>>> paul = Author.objects.create(name="Paul")
>>> george = Author.objects.create(name="George")
>>> ringo = Author.objects.create(name="Ringo")
>>> entry.authors.add(john, paul, george, ringo)

```

#### retrieving objects #检索对象

```python
>>> all_entries = Entry.objects.all()  #retrieving all objects
filter:return a queryset that match
exclude:return a queryset that not match
Entry.objects.filter(pub_date__year=2006) #double underline
#chaining filters from front to behind
__gte >=  __gt >  __lt <  __contains means contain

get:only one result selected
```

#### limiting querysets

negative indexing is not supported

```python
>>> Entry.objects.all()[:10:2] #  return a list of every second object of the first 10
>>> Entry.objects.order_by('headline')[0] # order
>>> Blog.objects.get(name__iexact="beatles blog") # 大小写改变之后也能匹配得到
startwith endwith
```

#### Lookups that span relationships

contain join function in sql.

```python
>>> Entry.objects.filter(blog__name='Beatles Blog')
# This example retrieves all Entry objects with a Blog whose name is 'Beatles Blog':
>>> Blog.objects.filter(entry__headline__contains='Lennon')
# This example retrieves all Blog objects which have at least one Entry whose headline contains 'Lennon':  double underlines should be paid attention to

Blog.objects.filter(entry__headline__contains='Lennon', entry__pub_date__year=2008)
and
Blog.objects.filter(entry__headline__contains='Lennon').filter(entry__pub_date__year=2008)
are different

```

 UUIDField is a special field to store **universally unique identifiers**. It uses Python’s [UUID](https://docs.python.org/3/library/uuid.html#uuid.UUID) class. UUID, Universal Unique Identifier, is a python library that helps in generating random objects of 128 bits as ids. It provides the uniqueness as it generates ids on the basis of time, Computer hardware (MAC etc.). Universally unique identifiers are a good alternative to AutoField for `primary_key`. The database will not generate the UUID for you, so it is recommended to use `default`. 

#### Tutorial

```python
py -m pip install Django
django-damin startproject mysite
cd mysite
py manage.py runserver
py manage.py startapp polls
# create polls/urls.py  
from django.urls import path

from . import views

urlpatterns = [
    path('', views.index, name='index'),
]

# update mysite/urls.py
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('polls/', include('polls.urls')),
    path('admin/', admin.site.urls),
]

py manage.py migrate

# in polls/models create database you need
# add pollsconfig in mysite/settings-installed_apps
'polls.apps.PollsConfig'
# By running makemigrations, you’re telling Django that you’ve made some changes to your models (in this case, you’ve made new ones) and that you’d like the changes to be stored as a migration.
py manage.py makemigrations polls
# Migrations are how Django stores changes to your models (and thus your database schema)
# run migrate again to create those model tables in your database:
py manage.py migrate

py manage.py shell
# django 数据库里面用get和filter得到的对象类型是不一样的
# creating an admin user
py manage.py createsuperuser

# create templates in the polls directory(按照惯例展示时会自动搜寻在installed_apps下的templates文件夹)  and create polls directory in templates directory wihin inner polls create a file called index.html


```



filemeaning:

- The outer `mysite/` root directory is a container for your project. Its name doesn’t matter to Django; you can rename it to anything you like.
- `manage.py`: A command-line utility that lets you interact with this Django project in various ways. You can read all the details about `manage.py` in [django-admin and manage.py](https://docs.djangoproject.com/en/3.0/ref/django-admin/).
- The inner `mysite/` directory is the actual Python package for your project. Its name is the Python package name you’ll need to use to import anything inside it (e.g. `mysite.urls`).
- `mysite/__init__.py`: An empty file that tells Python that this directory should be considered a Python package. If you’re a Python beginner, read [more about packages](https://docs.python.org/3/tutorial/modules.html#tut-packages) in the official Python docs.
- `mysite/settings.py`: Settings/configuration for this Django project. [Django settings](https://docs.djangoproject.com/en/3.0/topics/settings/) will tell you all about how settings work.
- `mysite/urls.py`: The URL declarations for this Django project; a “table of contents” of your Django-powered site. You can read more about URLs in [URL dispatcher](https://docs.djangoproject.com/en/3.0/topics/http/urls/).
- `mysite/asgi.py`: An entry-point for ASGI-compatible web servers to serve your project. See [How to deploy with ASGI](https://docs.djangoproject.com/en/3.0/howto/deployment/asgi/) for more details.
- `mysite/wsgi.py`: An entry-point for WSGI-compatible web servers to serve your project. See [How to deploy with WSGI](https://docs.djangoproject.com/en/3.0/howto/deployment/wsgi/) for more details.



#### django-rest-framework tutorial

1. serialization

加djangorestframework包

createing a serializer class:provide a way of serializing and deserializing the instances into representations such as json.

create a file in the polls directory named serilalizers.py

```python
from rest_framework import serializers
from .models import Question, Choice


class QuestionSerializer(serializers.Serializer):
    question_text = serializers.CharField(max_length=200)
    pub_date = serializers.DateTimeField('date published')

    def create(self, validated_data):
        return Question.objects.create(**validated_data)

    def update(self, instance, validated_data):
        instance.question_text = validated_data.get('question_text', instance.question_text)
        instance.pub_date = validated_data.get('pub_date', instance.pub_date)
        instance.save()
        return instance

# 就是把models换成serializers
# 外键引用没搞懂，所以还没加Choice序列器
# run_in_python
from poll.models import Question
from poll.serializers import QuestionSerializer
from rest_framework.renderers import JSONRenderer
from rest_framework.parsers import JSONParser
serializer = QuestionSerializer(m) # m 为Question实体化对象
content = JSONRenderer().render(serializer.data)
content
# deserialization:
import io

stream = io.BytesIO(content)
data = JSONParser().parse(stream)

serializer = QuestionSerializer(data=data)
serializer.is_valid()

serializer.validated_data
serializer.save()

# serialize querysets instead of model instances. To do so we simply add a many=True flag to the serializer arguments.

serializer = QuestionSerializer(Question.objects.all(), many=True)
serializer.data

# using modelserializer:
class QuestionSerializer(serializers.ModelSerializer):
    class Meta:
        model = Question
        fields = ['question_text', 'pub_date']

# Writing regular Django views using our Serializer

# edit poll/views.py
# add  attention the before views should be abandoned
from django.http import HttpResponse, JsonResponse
from django.views.decorators.csrf import csrf_exempt
from rest_framework.parsers import JSONParser
from .serializers import QuestionSerializer

@csrf_exempt
def question_list(request):
    """
    List all code snippets, or create a new snippet.
    """
    if request.method == 'GET':
        questions = Question.objects.all()
        serializer = QuestionSerializer(questions, many=True)
        return JsonResponse(serializer.data, safe=False)

    elif request.method == 'POST':
        data = JSONParser().parse(request)
        serializer = QuestionSerializer(data=data)
        if serializer.is_valid():
            serializer.save()
            return JsonResponse(serializer.data, status=201)
        return JsonResponse(serializer.errors, status=400)

@csrf_exempt
def question_detail(request, pk):
    """
    Retrieve, update or delete a code snippet.
    """
    try:
        question = Question.objects.get(pk=pk)
    except Question.DoesNotExist:
        return HttpResponse(status=404)

    if request.method == 'GET':
        serializer = QuestionSerializer(question)
        return JsonResponse(serializer.data)

    elif request.method == 'PUT':
        data = JSONParser().parse(request)
        serializer = QuestionSerializer(question, data=data)
        if serializer.is_valid():
            serializer.save()
            return JsonResponse(serializer.data)
        return JsonResponse(serializer.errors, status=400)

    elif request.method == 'DELETE':
        question.delete()
        return HttpResponse(status=204)

    
# then edit poll/urls corresponding functions
```



