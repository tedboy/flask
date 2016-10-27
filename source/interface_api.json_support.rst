.. module:: flask

JSON Support
------------

.. module:: flask.json

Flask uses ``simplejson`` for the JSON implementation.  Since simplejson
is provided by both the standard library as well as extension, Flask will
try simplejson first and then fall back to the stdlib json module.  On top
of that it will delegate access to the current application's JSON encoders
and decoders for easier customization.

So for starters instead of doing::

    try:
        import simplejson as json
    except ImportError:
        import json

You can instead just do this::

    from flask import json

For usage examples, read the :mod:`json` documentation in the standard
library.  The following extensions are by default applied to the stdlib's
JSON module:

1.  ``datetime`` objects are serialized as :rfc:`822` strings.
2.  Any object with an ``__html__`` method (like :class:`~flask.Markup`)
    will have that method called and then the return value is serialized
    as string.

The :func:`~htmlsafe_dumps` function of this json module is also available
as filter called ``|tojson`` in Jinja2.  Note that inside ``script``
tags no escaping must take place, so make sure to disable escaping
with ``|safe`` if you intend to use it inside ``script`` tags unless
you are using Flask 0.10 which implies that:

.. sourcecode:: html+jinja

    <script type=text/javascript>
        doSomethingWith({{ user.username|tojson|safe }});
    </script>

.. admonition:: Auto-Sort JSON Keys

    The configuration variable ``JSON_SORT_KEYS`` (:ref:`config`) can be
    set to false to stop Flask from auto-sorting keys.  By default sorting
    is enabled and outside of the app context sorting is turned on.

    Notice that disabling key sorting can cause issues when using content
    based HTTP caches and Python's hash randomization feature.

.. autofunction:: jsonify

.. autofunction:: dumps

.. autofunction:: dump

.. autofunction:: loads

.. autofunction:: load

.. autoclass:: JSONEncoder
   :members:

.. autoclass:: JSONDecoder
   :members: