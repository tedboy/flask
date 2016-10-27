.. module:: flask

Extensions
----------

.. data:: flask.ext

   This module acts as redirect import module to Flask extensions.  It was
   added in 0.8 as the canonical way to import Flask extensions and makes
   it possible for us to have more flexibility in how we distribute
   extensions.

   If you want to use an extension named “Flask-Foo” you would import it
   from :data:`~flask.ext` as follows::

        from flask.ext import foo

   .. versionadded:: 0.8