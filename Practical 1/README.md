Practical 1
===========
In this first practical we'll create a basic but fully functional Django app to manage studies run by a ficticious Clinical Trials Unit. We'll include a single model representing a study:

![](https://raw.github.com/mwoodbri/django-tutorial/master/Practical%201/Study.png)

Creating an empty project
-------------------------
First we'll create a new project, then add and enable a main app

1. Create a new project

        File > New > Other...
        PyDev > PyDev Django Project
        Project name: CTU
        Next > Next > Finish
        Yes
1. Create a main app

        CTU > Django > Create application
        "main" > OK
1. Enable it in settings.py

        Add 'main' to INSTALLED_APPS

Adding a study model
--------------------
Now we'll create a model representing a clinical study, which corresponds to database table

1. Add a class to models.py

    ```python
    class Study(models.Model):
        name = models.CharField(max_length=200)
        start_date = models.DateTimeField()
    ```

1. Sync the new model to the database

        CTU > Django > Sync DB
            "yes"
            (blank)
            (blank)
            "password"
            "password"

1. Create an example study in the database

        CTU > Django > Shell with django environment
            >>> from main.models import Study
            >>> from django.utils import timezone
            >>> study = Study(name="my study", start_date=timezone.now())
            >>> study.id
            >>> study.save()
            >>> study.id
            >>> Study.objects.all()

Creating a template for a study
--------------------------------------
We'll now create an HTML page for displaying a study. Add a ```templates``` folder under the ```main``` folder and create a new file named ```index.html``` containing:

```html
<!DOCTYPE html>
<html><head><title>CTU</title></head><body>
  <h1>Studies</h1>
  <ul>
    {% for study in studies %}
    <li>{{ study.name }}</li>
    {% endfor %}
  </ul>
</body></html>
```

Creating a view for a study
---------------------------
The template receives a list of studies that must first be retrieved from the database. Add the following to ```views.py```:

```python
from models import Study
from django.shortcuts import render

def index(request):
    studies = Study.objects.all().order_by('-start_date')[:5]
    return render(request, 'index.html', {'studies': studies})
```

Adding our view to the URL dispatcher
-------------------------------------
Finally we have to tell our application to route HTTP requests from the root URL ("/") to our new view. Add the following to the header of ```urls.py```:

```python
from main import views
```

and the following to the ```urlpatterns``` section:

```python
url(r'^$', views.index)
```

Run the application
-------------------
Now we can run our application and check that it works

1. ```CTU > Run As > PyDev Django```
2. Open web browser and visit the address shown in PyDev Console

TODO enable reload
