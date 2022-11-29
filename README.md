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


  At this point, you will see an instance of a Django application running successfully. Once you are finished, you can stop the server (CONTROL+C or CTRL+C).









































