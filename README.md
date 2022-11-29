  #### FRONTENT AS REACT BACKEND AS DJANGO 


  CONTENTS
  
  Prerequisites
  
  Step 1 — Setting Up the Backend
  
  Step 2 — Setting Up the APIs
  
  Step 3 — Setting Up the Frontend


  #### Step 1 — Setting Up the Backend


     Install the dependencies to run the Django app.

    If you are using Django with Python 3, type:

  - Firstly, we need to update local apt package index using

     ```sh
     sudo apt-get update
     ```


   - This will install pip, the Python development files needed to build Gunicorn later, the Postgres database system and the libraries needed to              interact  with it, and the Nginx web server.

  ```sh
  sudo apt-get install python3-pip python3-dev libpq-dev postgresql postgresql-contrib
  ```

  - Log into an interactive Postgres session by typing:

    ```sh
    sudo -u postgres psql
    ```

  - First, create a database for your project:

     ```sh
     CREATE DATABASE myproject
     ```

  - Next, create a database user for our project. Make sure to select a secure password

     ```sh
     CREATE USER postgres WITH PASSWORD '123456'
     ```

  - Now, we can give our new user access to administer our new database

    ```sh
    GRANT ALL PRIVILEGES ON DATABASE myproject TO myprojectuser
    ```

  - To check postgreSQL version use 

    ```sh
    psql -V
    ```

  - Upgrade pip and install the package by typing:


     ```sh
     sudo -H pip3 install --upgrade pip
     ```

  Run the following command to create a new project directory:


    ```sh
    mkdir django-todo-react
    ```

   Next, navigate into the directory:


   ```sh
   cd django-todo-react
   ```
  - Insatll a Python Virtual Environment for My Project  
   
   ```sh
   sudo -H pip3 install virtualenv
   ```

 create a Python virtual environment by typing

  ```sh
  virtualenv myprojectenv
  ```
Before we install our project’s Python requirements, we need to activate the virtual environment. You can do that by typing:


   ```sh
   source myprojectenv/bin/activate
   ```

  Install Django using Pipenv:

   ```sh
   pip install django
   ```

  Then create a new project called backend:

   ```sh
   django-admin startproject backend
   ```

  Next, navigate into the newly created backend directory:


  ```sh
  cd backend
  ```

  Start a new application called todo:

  ```sh
  python manage.py startapp todo
  ```

  Run migrations:

  ```sh
  python manage.py migrate
  ```

  And start up the server

  ```sh
  http://localhost:8000
  ```
  ![ django ](https://user-images.githubusercontent.com/106643382/204489190-058be87b-85a4-4ea7-b7f5-146b9616bba6.png "django")

  At this point, you will see an instance of a Django application running successfully. 
  Once you are finished, you can stop the server (CONTROL+C orCTRL+C).

Registering the todo Application
Now that you have completed the setup for the backend, you can begin registering 
the todo application as an installed app so that Django can recognize it.

Open the backend/settings.py file in your code editor and add todo to the INSTALLED_APPS:


# Application definition


```sh
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'todo',
]
```

Then, save your changes.


Defining the Todo Model
Let’s create a model to define how the Todo items should be stored in the database.

Open the todo/models.py file in your code editor and add the following lines of code:

```sh
from django.db import models

# Create your models here.

class Todo(models.Model):
    title = models.CharField(max_length=120)
    description = models.TextField()
    completed = models.BooleanField(default=False)

    def _str_(self):
        return self.title
```        
        
The code snippet above describes three properties on the Todo model:

-title
-description
-completed


The completed property is the status of a task. A task will either be completed or not completed at any time. 
Because you have created a Todo model, you will need to create a migration file:

    
    
  ```sh
  python manage.py makemigrations todo
  ```

And apply the changes to the database:

  ```sh
  python manage.py migrate todo
  ```

You can test to see that CRUD operations work on the Todo model 
you created by using the admin interface that Django provides by default.

Open the todo/admin.py file with your code editor and add the following lines of code:


  
```sh
from django.contrib import admin
from .models import Todo

class TodoAdmin(admin.ModelAdmin):
    list_display = ('title', 'description', 'completed')

# Register your models here.

admin.site.register(Todo, TodoAdmin)
```
  
---  
  
#### Step 2 — Setting Up the APIs


In this section, you will create an API using the Django REST framework.

Install the djangorestframework and django-cors-headers using Pipenv:


```sh
pipenv install djangorestframework django-cors-headers
```

You need to add rest_framework and corsheaders to the list of installed applications. 
Open the backend/settings.py file in your code editor and update the INSTALLED_APPS and MIDDLEWARE sections:


```sh
# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'corsheaders',
    'rest_framework',
    'todo',
]

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    'corsheaders.middleware.CorsMiddleware',
]

```

Then, add these lines of code to the bottom of the backend/settings.py file:


```sh
CORS_ORIGIN_WHITELIST = [
     'http://localhost:3000'
]
```
django-cors-headers is a Python library that will prevent the errors that you would normally get due to CORS rules. 
In the CORS_ORIGIN_WHITELIST code, you whitelisted localhost:3000 because you want the frontend 
(which will be served on that port) of the application to interact with the API.


Creating serializers
You will need serializers to convert model instances to JSON so that the frontend can work with the received data.

Create a todo/serializers.py file with your code editor.
Open the serializers.py file and update it with the following lines of code:

```sh
from rest_framework import serializers
from .models import Todo

class TodoSerializer(serializers.ModelSerializer):
    class Meta:
        model = Todo
        fields = ('id', 'title', 'description', 'completed')
 
```

This code specifies the model to work with and the fields to be converted to JSON.


#### Creating the View

You will need to create a TodoView class in the todo/views.py file.

Open the todo/views.py file with your code editor and add the following lines of code:

```sh
from django.shortcuts import render
from rest_framework import viewsets
from .serializers import TodoSerializer
from .models import Todo

# Create your views here.

class TodoView(viewsets.ModelViewSet):
    serializer_class = TodoSerializer
    queryset = Todo.objects.all()

```

The viewsets base class provides the implementation for CRUD operations by default. 
This code specifies the serializer_class and the queryset.

Open the backend/urls.py file with your code editor and replace the contents with the following lines of code:

```sh
from django.contrib import admin
from django.urls import path, include
from rest_framework import routers
from todo import views

router = routers.DefaultRouter()
router.register(r'todos', views.TodoView, 'todo')

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/', include(router.urls)),
]
```




































































































