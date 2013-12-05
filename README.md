Django tutorial
===============
Getting started
---------------
1. Log into Windows **on a lab computer**

1. Start Eclipse:
	
        Start > All Programs > eclipse
1. Create a workspace **on the C: drive** for your Python projects:
	
        File > Switch Workspace > Other...
        Workspace: "C:\Users\<your username>\PythonWorkspace"
	
1. Configure the Python interpreter:

        Window > Preferences > PyDev > Interpreter - Python > Auto Config > OK > OK
1. Close the Welcome screen

Slides
------
http://goo.gl/eeXWc8

Notes
-----
* The Django web server is set to ```--noreload``` by default .This means you’ll have to restart it whenever you change your code unless you change the configuration (ask us).
* If you want to keep the code that you write during the practical then you’ll need to copy your workspace onto your network drive at the end

Todo
----
* Move all data modelling to Practical 1 to workaround lack of migrations in Django 1.5
* Move table creation to Pratical 2, to be done at same time as display of patient count
* Remove date filter
* Remove study.id from console
* Update Django, PyDev (and Python?) versions 
