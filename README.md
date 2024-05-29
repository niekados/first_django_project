# First Django Project

## Installing packages
5. Type the following command in the terminal to install the Django Python package:

`pip3 install Django~=4.2.1`
Note: ~=4.2.1 installs any package version greater than or equal to 4.2.1 but less than 4.3

6. Once the package is installed, add it to the requirements.txt file with the following command:

`pip3 freeze --local > requirements.txt`

7. It's good practice to check what packages have been installed. When you look in the requirements file, you can see that three packages were installed.

```python
asgiref==3.7.2
Django==4.2.7
sqlparse==0.4.4
```

## Creating a Django project 

8. Return to the terminal. In the terminal, create a Django project called my_project in the current directory.

---
**Important:**

Remember that the shortcut to refer to the current directory is a single dot . at the end of the command.

---
`django-admin startproject my_project .`

---
**Note:** 

Check in the explorer tab to see that the my_project project structure has been created.

---

9. Return to the terminal and start the Django server with the following command:

`python3 manage.py runserver`

10. Click on Open Browser.

11. This opens a scary-looking yellow error screen, don't worry! Your server is running properly. This error is telling you that, for security reasons, Django doesn't recognise the hostname - the server name your project is running on.


12. Select and copy the hostname after "Invalid HTTP_HOST header:". In this example, that is `'8000-niekados-firstdjangopro-wwpgflm5p0z.ws-eu114.gitpod.io'` - you can include the quotes.


13. In the `my_project/settings.py file`, paste the hostname between the square brackets of `ALLOWED_HOSTS` and save. For the above example, this would look like:

```python
ALLOWED_HOSTS = ['8000-niekados-firstdjangopro-wwpgflm5p0z.ws-eu114.gitpod.io']
Add to ALLOWED_HOSTS
```

14. Now if you return to the browser tab and refresh you will see a bare-bones Django project.

---
**Note:** 

Return to the terminal and use ctrl-c to kill the server.

---

## Creating a Django app

15. Now we have a Django project created, we need to make an app. To do this, we use the manage.py file. In the terminal, type:

`python3 manage.py startapp hello_world`
---

**Note:**

Check the explorer panel to see the new hello_world app directory has been created.

---

## Creating Views

16. In hello_world/views.py, below from django.shortcuts import render, type:

`from django.http import HttpResponse`

17. Below the # Create your views here. comment, create the following Python function named index. Inside the function, we are returning a simple HTTP response.

18. Within parentheses after `HttpResponse` add the string `"Hello, world!"`

```python
# Create your views here.
def index(request):
    return HttpResponse("Hello, world!")
```

Creating our URLs
19. In my_project/urls.py, You'll see that this urls.py file already has some content in it. That's fine, we will need that in future. Let's include the view we just created.

20. Import the include function by appending it after a comma to the existing django.urls import.


21. Below that, import the views from the hello_world app.

from hello_world import views as index_views
Here we are giving the hello_world/views.py file an alias of index_views. In a one-app project, this would not be strictly necessary. However, in a multiple-app project using descriptive alias names makes your urls.py file much easier to read and maintain.

# Deployment

## Deploying to Heroku

### Create the Heroku app
1. Navigate to your Heroku dashboard and create a new app with a unique name.

2. Click on the Settings tab and reveal the config vars. Add a key of DISABLE_COLLECTSTATIC and a value of 1 and click Add.

This step prevents Heroku from uploading static files, such as CSS and JS, during the build. We will explain this in more detail later when we have written some HTML for the project.

### Update your code for deployment
3. Install a production-ready webserver for Heroku.

`pip3 install gunicorn~=20.1 `
Add gunicorn==20.1.0 to the requirements.txt file with:

`pip3 freeze --local > requirements.txt`

---
**NOTE**
Note: gunicorn is a production equivalent of the manage.py runserver used in development but with speed and security optimisation.
---

4. Create a file named Procfile at the root directory of the project (same directory as requirements.txt).

---
**NOTE**
Note: The Procfile has no file extension.
---

5. In the `Procfile`, declare this is a web process followed by the command to execute your Django project.

`web: gunicorn my_project.wsgi`
This assumes your project is named my_project.

Note the space after the colon.

---
**NOTE**
Note: gunicorn my_project.wsgi is the command heroku will use to start the server. It works similarly to python3 manage.py runserver.
---

6. Open the `my_project/settings.py` file and replace `DEBUG=True` with `DEBUG=False`.

Note the comment regarding security in production.


7. Also, in `settings.py` we need to append the Heroku hostname to the `ALLOWED_HOSTS` list, in addition to the local host we added in the last lesson.

`,'.herokuapp.com'`
Note: Remember the comma and the dot before herokuapp.

```python
# settings.py

# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = False

ALLOWED_HOSTS = ['8000-niekados-firstdjangopro-wwpgflm5p0z.ws-eu114.gitpod.io','.herokuapp.com']
```

8. You can now git add the files you have modified, git commit them and push them to GitHub.

### Deploy on Heroku

9. Now, let's return to the Heroku dashboard, and in your app, click on the Deploy tab.

10. In the Deployment method section enable GitHub integration by clicking on Connect to GitHub.

If you have not deployed a project from GitHub before then, you will be asked to authenticate with GitHub.

11. Start typing your project repo name into the search box and click Search. A list of repositories from your GitHub account should appear. Click on the GitHub repo you want to deploy from.


12. Scroll to the bottom of the page and click Deploy Branch to start a manual deployment of the main branch.

You can view the build output in the application's Activity tab in the dashboard.

13. Click on Open app to view your deployed project.

Note: You will have to append /hello to the browser URL just as you did locally to see your view output.

14. Open the Resources tab and choose an eco dyno. This dyno is a lightweight container to run your project.

15. Open the Resources tab and verify there is no existing Postgres database add-on. If there is one you can destroy it. Hello, World does not use a database and if not destroyed can result in usage costs. If there is a database add-on select Delete Add-on to remove it.

16. Click on Open app to view your deployed project.

---
**NOTE**
Note: You will have to append /hello to the browser URL just as you did locally to see your view output.
---

# CHEATSHEET

## Getting Started Commands

### Installation and Setup

| Terminal Command | Description | Example |
| ---------------- | ----------- | ------- |
| `pip3 install <package_name><optional_version_number>` | Installs packages with an optional version | `pip3 install Django~=4.2.1` |
| `pip3 freeze --local > requirements.txt` | Creates/updates the requirements.txt file | `pip3 freeze --local > requirements.txt` |
| `django-admin startproject <proj_name> .` | Starts a Django project, don't forget the full stop [.] to start the project in the current directory | `django-admin startproject my_project .` |
| `python3 manage.py startapp <app_name>` | Creates a new Django app in the Django project | `python3 manage.py startapp blog` |

### Migration and Database Commands

| Terminal Command | Description | Example |
| ---------------- | ----------- | ------- |
| `python3 manage.py makemigrations` | Create migration files for any new or updated models across the entire project | `python3 manage.py makemigrations` |
| `python3 manage.py makemigrations <app_name>` | Create migration files for any new or updated models within the specified app | `python3 manage.py makemigrations blog` |
| `python3 manage.py migrate` | Migrate your database with any new migrations across the entire project | `python3 manage.py migrate` |
| `python3 manage.py migrate <app_name>` | Migrate your database with any new migrations within the specified app | `python3 manage.py migrate blog` |
| `python3 manage.py migrate <app_name> zero` | Reverts all migrations for a specified app to the initial state, effectively undoing them | `python3 manage.py migrate blog zero` |
| `python3 manage.py makemigrations --dry-run` | Migration preview feature that allows you to see potential model changes without creating actual migration files | `python3 manage.py makemigrations --dry-run` |
| `python3 manage.py makemigrations --check` | Checks if the current model states match the database migrations without actually making new migrations | `python3 manage.py makemigrations --check` |

### General Commands

| Terminal Command | Description | Example |
| ---------------- | ----------- | ------- |
| `python3 manage.py runserver` | Run your Django app in the browser | `python3 manage.py runserver` |
| `python3 manage.py createsuperuser` | Creates an admin user for accessing the Django Admin site | `python3 manage.py createsuperuser` |
| `python3 manage.py collectstatic` | Collects all static files from each of your applications into a single location for production | `python3 manage.py collectstatic` |
| `python3 manage.py loaddata <fixture_name>` | Loads data from a fixture into the database | `python3 manage.py loaddata initial_data` |
| `python3 -V` | Displays your current Python version | `python3 -V` |
| `pip3 show <package_name>` | Gives information on the package including its location in your file structure | `pip3 show django-allauth` |
| `cp -r <from>* <to>` | Copies files from one location to another | `cp -r /home/cistudent/.local/lib/python3.9/site-packages/allauth/templates/* . /templates/` |

### Testing Commands

| Terminal Command | Description | Example |
| ---------------- | ----------- | ------- |
| `python3 manage.py test` | Run all unit tests in files that start with `test_` across the entire project | `python3 manage.py test` |
| `python3 manage.py test <app_name>` | Run all unit tests in files that start with `test_` inside the specified app | `python3 manage.py test blog` |
| `python3 manage.py test <app_name>.<file_name>` | Run all unit tests in a specific test file inside the specified app | `python3 manage.py test about.test_forms` |
| `python3 manage.py test <app_name>.<file_name>.<class_name>` | Run all unit tests in a specific class within the specified location | `python3 manage.py test about.test_forms.TestCollaborateForm` |
| `python3 manage.py test <app_name>.<file_name>.<class_name>.<test_name>` | Run a single unit test within the specified location | `python3 manage.py test about.test_forms.TestCollaborateForm.test_form_is_valid` |

### Other Useful Commands

| Terminal Command | Description | Example |
| ---------------- | ----------- | ------- |
| `python3 manage.py dumpdata <app_name> > <filename>.json` | Creates a fixture (in JSON format) from the current database | `python3 manage.py dumpdata blog > blog_fixtures.json` |
| `python3 manage.py flush` | Removes all data from the database and resets primary key sequences for all models | `python3 manage.py flush` |
