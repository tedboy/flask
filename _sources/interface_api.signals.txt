.. module:: flask

.. _core-signals-list:

Signals
-------

.. versionadded:: 0.6

.. data:: signals.signals_available

   ``True`` if the signaling system is available.  This is the case
   when `blinker`_ is installed.

The following signals exist in Flask:

.. data:: template_rendered

   This signal is sent when a template was successfully rendered.  The
   signal is invoked with the instance of the template as `template`
   and the context as dictionary (named `context`).

   Example subscriber::

        def log_template_renders(sender, template, context, **extra):
            sender.logger.debug('Rendering template "%s" with context %s',
                                template.name or 'string template',
                                context)

        from flask import template_rendered
        template_rendered.connect(log_template_renders, app)

.. data:: flask.before_render_template
   :noindex:

   This signal is sent before template rendering process. The
   signal is invoked with the instance of the template as `template`
   and the context as dictionary (named `context`).

   Example subscriber::

        def log_template_renders(sender, template, context, **extra):
            sender.logger.debug('Rendering template "%s" with context %s',
                                template.name or 'string template',
                                context)

        from flask import before_render_template
        before_render_template.connect(log_template_renders, app)

.. data:: request_started

   This signal is sent when the request context is set up, before
   any request processing happens.  Because the request context is already
   bound, the subscriber can access the request with the standard global
   proxies such as :class:`~flask.request`.

   Example subscriber::

        def log_request(sender, **extra):
            sender.logger.debug('Request context is set up')

        from flask import request_started
        request_started.connect(log_request, app)

.. data:: request_finished

   This signal is sent right before the response is sent to the client.
   It is passed the response to be sent named `response`.

   Example subscriber::

        def log_response(sender, response, **extra):
            sender.logger.debug('Request context is about to close down.  '
                                'Response: %s', response)

        from flask import request_finished
        request_finished.connect(log_response, app)

.. data:: got_request_exception

   This signal is sent when an exception happens during request processing.
   It is sent *before* the standard exception handling kicks in and even
   in debug mode, where no exception handling happens.  The exception
   itself is passed to the subscriber as `exception`.

   Example subscriber::

        def log_exception(sender, exception, **extra):
            sender.logger.debug('Got exception during processing: %s', exception)

        from flask import got_request_exception
        got_request_exception.connect(log_exception, app)

.. data:: request_tearing_down

   This signal is sent when the request is tearing down.  This is always
   called, even if an exception is caused.  Currently functions listening
   to this signal are called after the regular teardown handlers, but this
   is not something you can rely on.

   Example subscriber::

        def close_db_connection(sender, **extra):
            session.close()

        from flask import request_tearing_down
        request_tearing_down.connect(close_db_connection, app)

   As of Flask 0.9, this will also be passed an `exc` keyword argument
   that has a reference to the exception that caused the teardown if
   there was one.

.. data:: appcontext_tearing_down

   This signal is sent when the app context is tearing down.  This is always
   called, even if an exception is caused.  Currently functions listening
   to this signal are called after the regular teardown handlers, but this
   is not something you can rely on.

   Example subscriber::

        def close_db_connection(sender, **extra):
            session.close()

        from flask import appcontext_tearing_down
        appcontext_tearing_down.connect(close_db_connection, app)

   This will also be passed an `exc` keyword argument that has a reference
   to the exception that caused the teardown if there was one.

.. data:: appcontext_pushed

   This signal is sent when an application context is pushed.  The sender
   is the application.  This is usually useful for unittests in order to
   temporarily hook in information.  For instance it can be used to
   set a resource early onto the `g` object.

   Example usage::

        from contextlib import contextmanager
        from flask import appcontext_pushed

        @contextmanager
        def user_set(app, user):
            def handler(sender, **kwargs):
                g.user = user
            with appcontext_pushed.connected_to(handler, app):
                yield

   And in the testcode::

        def test_user_me(self):
            with user_set(app, 'john'):
                c = app.test_client()
                resp = c.get('/users/me')
                assert resp.data == 'username=john'

   .. versionadded:: 0.10

.. data:: appcontext_popped

   This signal is sent when an application context is popped.  The sender
   is the application.  This usually falls in line with the
   :data:`appcontext_tearing_down` signal.

   .. versionadded:: 0.10


.. data:: message_flashed

   This signal is sent when the application is flashing a message.  The
   messages is sent as `message` keyword argument and the category as
   `category`.

   Example subscriber::

        recorded = []
        def record(sender, message, category, **extra):
            recorded.append((message, category))

        from flask import message_flashed
        message_flashed.connect(record, app)

   .. versionadded:: 0.10

.. class:: signals.Namespace

   An alias for :class:`blinker.base.Namespace` if blinker is available,
   otherwise a dummy class that creates fake signals.  This class is
   available for Flask extensions that want to provide the same fallback
   system as Flask itself.

   .. method:: signal(name, doc=None)

      Creates a new signal for this namespace if blinker is available,
      otherwise returns a fake signal that has a send method that will
      do nothing but will fail with a :exc:`RuntimeError` for all other
      operations, including connecting.


.. _blinker: https://pypi.python.org/pypi/blinker