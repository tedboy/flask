.. module:: flask

Useful Internals
----------------

.. autoclass:: flask.ctx.RequestContext
   :members:

.. data:: _request_ctx_stack

   The internal :class:`~werkzeug.local.LocalStack` that is used to implement
   all the context local objects used in Flask.  This is a documented
   instance and can be used by extensions and application code but the
   use is discouraged in general.

   The following attributes are always present on each layer of the
   stack:

   `app`
      the active Flask application.

   `url_adapter`
      the URL adapter that was used to match the request.

   `request`
      the current request object.

   `session`
      the active session object.

   `g`
      an object with all the attributes of the :data:`flask.g` object.

   `flashes`
      an internal cache for the flashed messages.

   Example usage::

      from flask import _request_ctx_stack

      def get_session():
          ctx = _request_ctx_stack.top
          if ctx is not None:
              return ctx.session

.. autoclass:: flask.ctx.AppContext
   :members:

.. data:: _app_ctx_stack

   Works similar to the request context but only binds the application.
   This is mainly there for extensions to store data.

   .. versionadded:: 0.9

.. autoclass:: flask.blueprints.BlueprintSetupState
   :members: