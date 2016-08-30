What to do if the Server does not Start
---------------------------------------

In case the :command:`python -m flask` fails or :command:`flask` does not exist,
there are multiple reasons this might be the case.  First of all you need
to look at the error message.

Old Version of Flask
````````````````````

Versions of Flask older than 0.11 use to have different ways to start the
application.  In short, the :command:`flask` command did not exist, and
neither did :command:`python -m flask`.  In that case you have two options:
either upgrade to newer Flask versions or have a look at the :ref:`server`
docs to see the alternative method for running a server.

Invalid Import Name
```````````````````

The ``-a`` argument to :command:`flask` is the name of the module to
import.  In case that module is incorrectly named you will get an import
error upon start (or if debug is enabled when you navigate to the
application).  It will tell you what it tried to import and why it failed.

The most common reason is a typo or because you did not actually create an
``app`` object.