.. _debug-mode:

Debug Mode
----------

(Want to just log errors and stack traces? See :ref:`application-errors`)

The :command:`flask` script is nice to start a local development server, but
you would have to restart it manually after each change to your code.
That is not very nice and Flask can do better.  If you enable debug
support the server will reload itself on code changes, and it will also
provide you with a helpful debugger if things go wrong.

To enable debug mode you can export the ``FLASK_DEBUG`` environment variable
before running the server::

    $ export FLASK_DEBUG=1
    $ flask run

(On Windows you need to use ``set`` instead of ``export``).

This does the following things:

1.  it activates the debugger
2.  it activates the automatic reloader
3.  it enables the debug mode on the Flask application.

There are more parameters that are explained in the :ref:`server` docs.

.. attention::

   Even though the interactive debugger does not work in forking environments
   (which makes it nearly impossible to use on production servers), it still
   allows the execution of arbitrary code. This makes it a major security risk
   and therefore it **must never be used on production machines**.

Screenshot of the debugger in action:

.. image:: ../_static/debugger.png
   :align: center
   :class: screenshot
   :alt: screenshot of debugger in action

Have another debugger in mind? See :ref:`working-with-debuggers`.