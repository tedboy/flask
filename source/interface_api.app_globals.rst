.. module:: flask

Application Globals
-------------------

.. currentmodule:: flask

To share data that is valid for one request only from one function to
another, a global variable is not good enough because it would break in
threaded environments.  Flask provides you with a special object that
ensures it is only valid for the active request and that will return
different values for each request.  In a nutshell: it does the right
thing, like it does for :class:`request` and :class:`session`.

.. data:: g

   Just store on this whatever you want.  For example a database
   connection or the user that is currently logged in.

   Starting with Flask 0.10 this is stored on the application context and
   no longer on the request context which means it becomes available if
   only the application context is bound and not yet a request.  This
   is especially useful when combined with the :ref:`faking-resources`
   pattern for testing.

   Additionally as of 0.10 you can use the :meth:`get` method to
   get an attribute or ``None`` (or the second argument) if it's not set.
   These two usages are now equivalent::

        user = getattr(flask.g, 'user', None)
        user = flask.g.get('user', None)

   It's now also possible to use the ``in`` operator on it to see if an
   attribute is defined and it yields all keys on iteration.

   As of 0.11 you can use :meth:`pop` and :meth:`setdefault` in the same
   way you would use them on a dictionary.

   This is a proxy.  See :ref:`notes-on-proxies` for more information.
