# Python ToDo App Developed in Django

1. Create a Django Project:

- Install Django using pip: `pip install django`.
- Create a project directory: `mkdir todo_app`.
- Initialize the Django project: `django-admin startproject todo_app`.
- Navigate into the project directory: `cd todo_app`.

2. Create a Django App:

- Create an app for the to-do list: `python manage.py startapp todo`.

3. Define Models:

- Open `todo/models.py`.
- Define a model named `TodoItem` with fields for `task`, `completed`, and `priority`.

```
from django.db import models

class TodoItem(models.Model):
    task = models.CharField(max_length=200)
    completed = models.BooleanField(default=False)
    priority = models.IntegerField(default=0)
```

4. Configure Django Settings:

- Open `todo_app/settings.py`.
- Add the `todo` app to the `INSTALLED_APPS` list.
- Configure the database settings.

5. Design Views:

- Open `todo/views.py`.
- Create a view function to display the to-do list:

from django.shortcuts import render
from .models import TodoItem

def index(request):
todo_items = TodoItem.objects.all()
return render(request, 'todo/index.html', {'todo_items': todo_items})

```
- Create a view function to add a new to-do item:

```python
from django.shortcuts import redirect
from .models import TodoItem

def add_todo(request):
    if request.method == 'POST':
        task = request.POST['task']
        TodoItem.objects.create(task=task, completed=False, priority=0)
        return redirect('index')
    else:
        return render(request, 'todo/add_todo.html')
```

6. Create Templates:

- Create a directory named `templates` inside the `todo` app.
- Create an HTML file named `index.html` inside the `templates/todo` directory.
- Add the following code to display the to-do list:

```
<!DOCTYPE html>
<html>
<head>
    <title>To-do List</title>
</head>
<body>
    <h1>To-do List</h1>
    <ul>
        {% for item in todo_items %}
            <li{% if item.completed %} class="completed"{% endif %}>
                {{ item.task }}
            </li>
        {% endfor %}
    </ul>
    <a href="{% url 'add_todo' %}">Add To-do Item</a>
</body>
</html>
```

- Create an HTML file named `add_todo.html` inside the `templates/todo` directory.
- Add the following code to form for adding a new to-do item:

```
<!DOCTYPE html>
<html>
<head>
    <title>Add To-do Item</title>
</head>
<body>
    <h1>Add To-do Item</h1>
    <form method="post">
        {% csrf_token %}
        <label for="task">Task:</label>
        <input type="text" id="task" name="task" required>
        <button type="submit">Add To-do Item</button>
    </form>
</body>
</html>
```

7. Configure URLs:

- Open `todo_app/urls.py`.
- Add URL patterns for the views:

```
from django.urls import path
from todo import views

urlpatterns = [
    path('', views.index, name='index'),
    path('add_todo/', views.add_todo, name='add_todo'),
]
```

8. Run the Django Development Server:

- Run the Django development server: `python manage.py runserver`.
- Open the following URL in your web browser: http://127.0.0.1
