# Maintenance request tracker

A small Django application for recording maintenance requests. You can list, create, view, edit and delete requests through a simple Bootstrap interface.
 
## Requirements

- Python 3.12 or newer (Django 6.1 does not support older versions)

Check your version with:

```bash
python3 --version
```

On Windows, use `python --version` (or `py --version`).

## Setup

Run these commands from the project folder, the one that contains `manage.py`.

### 1. Create a virtual environment

A virtual environment keeps this project's packages separate from the rest of your system.

```bash
python3 -m venv venv
```

On Windows, use `python -m venv venv`.

### 2. Activate the virtual environment

macOS / Linux:

```bash
source venv/bin/activate
```

Windows (Command Prompt):

```bat
venv\Scripts\activate
```

Windows (PowerShell):

```powershell
venv\Scripts\Activate.ps1
```

Once it is active, your prompt starts with `(venv)`. Activate it again whenever you open a new terminal to work on the project. To leave it, run `deactivate`.

### 3. Install the requirements

```bash
pip install -r requirements.txt
```

### 4. Create the database

```bash
python manage.py migrate
```

This creates a SQLite database file, `db.sqlite3`, in the project folder.

### 5. Add sample data (optional)

```bash
python manage.py seed
```

This adds 200 example requests. Use `--count` for a different number, for example `python manage.py seed --count 20`. To change the default, edit `SEED_REQUEST_COUNT` in `config/settings.py`.

To remove them again:

```bash
python manage.py unseed
```

**Note:** `unseed` deletes *all* requests, including any you created yourself.

### 6. Run the development server

```bash
python manage.py runserver
```

Open http://127.0.0.1:8000/ in your browser. Press `Ctrl+C` in the terminal to stop the server.

### Admin site (optional)

Django's built-in admin site is available at http://127.0.0.1:8000/admin/. To log in, first create an account:

```bash
python manage.py createsuperuser
```

## Running the tests

The tests are in `tracker/tests/`, with one file each for the model, the form, the views and the seed/unseed commands. Run them with:

```bash
python manage.py test
```

Django creates a separate, temporary database for the tests, so they never touch your own data.

### Code coverage

Code coverage shows which lines of code the tests run. It is measured with [coverage.py](https://coverage.readthedocs.io/), which is included in `requirements.txt`. Its settings are in `.coveragerc`.

```bash
coverage run manage.py test   # run the tests and record coverage
coverage report               # print a summary in the terminal
coverage html                 # write a detailed report to htmlcov/
```

Open `htmlcov/index.html` in your browser to see each file with the lines the tests missed highlighted. Coverage data and reports are listed in `.gitignore`, so they are never committed.

## Project structure

```
config/                  Project settings and top-level URLs
tracker/                 The application
├── models.py            The Request model (title and description)
├── forms.py             The form used to create and edit requests
├── views.py             One view function per page
├── urls.py              Maps URLs to view functions
├── tests/               Automated tests for the model, form, views and commands
├── templates/
│   ├── base.html        Shared page layout: navbar and page heading
│   ├── request_list.html
│   ├── request_detail.html
│   ├── request_form.html
│   └── request_confirm_delete.html
└── management/commands/ The seed and unseed commands
manage.py                Django's command-line tool
requirements.txt         Python packages the project needs
.coveragerc              Settings for code coverage
```

## Pages

| URL                       | View function    | Page                         |
|---------------------------|------------------|------------------------------|
| `/`                       | `list_request`   | Table of all requests        |
| `/requests/new/`          | `create_request` | Form to create a request     |
| `/requests/<id>/`         | `view_request`   | A single request             |
| `/requests/<id>/edit/`    | `update_request` | Form to edit a request       |
| `/requests/<id>/delete/`  | `delete_request` | Confirm deleting a request   |

Each URL's name is the same as its view function's name, so templates link to pages with, for example, `{% url 'view_request' service_request.pk %}`.

## Adding a page

Every template extends `base.html` and fills in two blocks:

```django
{% extends "base.html" %}

{% block heading %}Page title{% endblock %}

{% block content %}
  <p>Page content goes here.</p>
  <a class="btn btn-secondary" href="{% url 'list_request' %}">Back to list</a>
{% endblock %}
```

The templates use only a few Bootstrap classes, such as `btn btn-primary` for buttons, `table` for tables and `form-control` for form inputs. Icons come from [Bootstrap Icons](https://icons.getbootstrap.com/), for example `<i class="bi bi-pencil"></i>`.

## Changing the model

After changing `tracker/models.py`, create and apply a migration:

```bash
python manage.py makemigrations
python manage.py migrate
```

## Adding packages

Install the package with `pip install <package>`, then record it in `requirements.txt` so that others get it too:

```bash
pip freeze > requirements.txt
```
