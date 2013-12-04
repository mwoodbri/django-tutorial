Practical 1
===========
In this first practical we'll create a basic but fully functional Django app to manage studies run by a ficticious Clinical Trials Unit. You will create an application resembling [this example](http://ld-cisbic1.bc.ic.ac.uk:8000/).

We'll include a single model representing a study:

![](https://raw.github.com/mwoodbri/django-tutorial/master/Practical-1/Study.png)

1. Creating an empty project
----------------------------
First we'll create a new project, then add and enable a main app

1. Create a new project

        File > New > Other...
        PyDev > PyDev Django Project > Next
        Project name: CTU
        Next > Next > Finish
        Yes
        
1. Create a main app

        CTU (right-click) > Django > Create application
        "main" > OK
        
1. Enable it in ```settings.py```

        Add 'main' to INSTALLED_APPS

2. Adding a study model
-----------------------
Now we'll create a model representing a clinical study, which corresponds to database table

1. Add a class to ```models.py```:

    ```python
    class Study(models.Model):
        name = models.CharField(max_length=200)
        start_date = models.DateField()
    ```
1. Sync the new model to the database

        CTU > Django > Sync DB
            "no"

1. Create an example study in the database

        CTU > Django > Shell with django environment
            "OK"
            >>> from main.models import Study
            >>> from django.utils import timezone
            >>> study = Study(name="my study", start_date=timezone.now())
            >>> study.id
            >>> study.save()
            >>> study.id
            >>> Study.objects.all()

3. Creating a template for a study
----------------------------------
We'll now create an HTML page for displaying a study. Add a ```templates``` folder under the ```main``` folder and create a new file named ```index.html```:

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

4. Creating a view for a study
------------------------------
The template receives a list of studies that must first be retrieved from the database. Add the following to ```views.py```:

```python
from models import Study
from django.shortcuts import render

def index(request):
    studies = Study.objects.all().order_by('-start_date')[:5]
    return render(request, 'index.html', {'studies': studies})
```

5. Adding our view to the URL dispatcher
----------------------------------------
Finally we have to tell our application to route HTTP requests from the root URL ("/") to our new view. Add the following to the header of ```urls.py```:

```python
from main import views
```

and the following to the ```urlpatterns``` section:

```python
url(r'^$', views.index, name='index')
```

6. Run the application
----------------------
Now we can run our application and check that it works

1. ```CTU > Run As > PyDev Django```
2. Open web browser and visit the address shown in PyDev Console: http://127.0.0.1:8000/

Exercises
---------
1. Create one or more extra studies using the Django shell and ensure that they display in your web browser
1. Display the start date of each study in the template. Optional: use a [SHORT_DATE_FORMAT filter](https://docs.djangoproject.com/en/dev/ref/templates/builtins/#date) to make the date more readable.
1. Modify the template to display the studies in a two-column HTML table, rather than as a list. Optional: apply [some CSS](http://www.w3.org/Style/Examples/007/evenodd.en.html) to the head of your HTML page to distinguish alternate rows in your table.
