Practical 2
===========
In this practical we'll add more features to our clinical trials management web application. You should finish with an application resembling [this example](http://ld-cisbic1.bc.ic.ac.uk:8000).
1. Create a data entry form
---------------------------
To enable users of our system to add new studies we need an HTML form and some Python code to modify the database:

1. Add a form (as shown in the "Forms" section of the lecture notes) to the ```<body>``` of your existing ```index.html```
1. Add a corresponding function to ```views.py```, similar to that shown in the lecture notes, but with a couple of extra lines of code to a) create a Study with the relevant name (and today's date) and b) store it in the database. This code is similar to that used in the Django shell in Practical 1. After creating the Study you'll need to redirect the browser back to the main page like this:

        return HttpResponseRedirect(reverse('index'))
        
    You'll need some extra imports:
    
        from django.utils import timezone
        from django.http import HttpResponseRedirect
        from django.core.urlresolvers import reverse

1. Add an entry to ```urlpatterns``` in ```urls.py```:

        url(r'^create$', views.create, name='create'),
        
1. Try creating some new studies in your web browser

2. Add a new model
------------------
To represent a real study we also need to record the patients who are enrolled. We'll extend our schema:

![](https://raw.github.com/mwoodbri/django-tutorial/master/Practical-2/StudyPatient.png)

We do this by creating a Patient model:

1. Add a new class to ```models.py``` called Patient, similar to Study but with two ```CharField``` attributes called ```first_name``` and ```last_name``` and a ```ForeignKey``` attribute called ```study``` (see the lectures notes for the correct syntax).
1. Sync your database (as in Practical 1)
1. Create some patients, each linked to an existing study, using the Django shell (see Practical 1):

        >>> study = Study.objects.all()[0]
        >>> patient = Patient(first_name="foo", last_name="bar", study=study)
        >>> patient.save()

1. Add an extra column to the HTML table on the main page of our web application that shows the number of patients for each study. Hint 1: the syntax to return the collection of patients for a study is ```study.patient_set.all```. Hint 2: the filter to display the length of a collection in a template is ```{{ collection|length }}```

1. Check that the patient counts are displayed directly in the web browser

3. Add a second view
--------------------
Now we'll add a second page to our web application: a detailed view of a study listing its patients.

1. Create a new file called ```study.html``` in ```templates```, similar to ```index.html``` but the ```<body>``` element should just contain:

        <h1>Study: {{ study.name }}</h1>
        <p>Patients:</p>
        <ul>
          {% for patient in study.patient_set.all %}
          <li>{{ patient.first_name }} {{ patient.last_name}}</li>
          {% endfor %}
        </ul>

1. Add another function to ```views.py``` named ```study```, similar to ```index``` but with two arguments: ```request``` and ```study_id```. The first line should retrieve the required ```Study``` from the database:

        study = Study.objects.get(pk=study_id)

    The second line should call ```render```, like ```index``` but for ```study.html``` and passing ```study``` rather than ```studies```.

1. Add an extra line to ```urls.py``` to dispatch requests to your new view:

        url(r'^study/(?P<study_id>\d+)$', views.study, name='study')
        
1. Test your new page by visiting http://127.0.0.1:8000/study/1
        
1. Modify ```index.html``` to add hyperlinks to the details page for each row of your HTML table. Hint: replace ```{{ study.name }}``` with ```<a href="/study/{{ study.pk }}">{{ study.name }}</a>```

4. Template inheritance
-----------------------
The ```index.html``` and ```study.html``` now have now duplicated HTML code. Extract the common elements into a file named ```base.html``` (as shown in the notes) and edit ```index.html``` and ```study.html``` to specify ```title``` and ```content``` blocks.

Optional exercises
------------------
1. Add an [HTML5 date entry field](http://www.w3schools.com/html/tryit.asp?filename=tryhtml5_input_type_date) to the form on the front page of the application to allow the start date of the study to be specified
1. Add a [chart](http://www.chartjs.org/) to the front page of the application showing the number of patients for each study


