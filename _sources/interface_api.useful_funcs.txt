.. module:: flask

Useful Functions and Classes
----------------------------

.. data:: current_app

   Points to the application handling the request.  This is useful for
   extensions that want to support multiple applications running side
   by side.  This is powered by the application context and not by the
   request context, so you can change the value of this proxy by
   using the :meth:`~flask.Flask.app_context` method.

   This is a proxy.  See :ref:`notes-on-proxies` for more information.

.. autofunction:: has_request_context

.. autofunction:: copy_current_request_context

.. autofunction:: has_app_context

.. autofunction:: url_for

.. function:: abort(code)

   Raises an :exc:`~werkzeug.exceptions.HTTPException` for the given
   status code.  For example to abort request handling with a page not
   found exception, you would call ``abort(404)``.

   :param code: the HTTP error code.

.. autofunction:: redirect

.. autofunction:: make_response

.. autofunction:: after_this_request

.. autofunction:: send_file

.. autofunction:: send_from_directory

.. autofunction:: safe_join

.. autofunction:: escape

.. autoclass:: Markup
   :members: escape, unescape, striptags