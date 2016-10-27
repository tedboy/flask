.. module:: flask


Response Objects
----------------

.. autoclass:: flask.Response
   :members: set_cookie, data, mimetype

   .. attribute:: headers

      A :class:`~werkzeug.datastructures.Headers` object representing the response headers.

   .. attribute:: status

      A string with a response status.

   .. attribute:: status_code

      The response status as integer.
