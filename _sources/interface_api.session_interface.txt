.. module:: flask

Session Interface
-----------------

.. versionadded:: 0.8

The session interface provides a simple way to replace the session
implementation that Flask is using.

.. currentmodule:: flask.sessions

.. autoclass:: SessionInterface
   :members:

.. autoclass:: SecureCookieSessionInterface
   :members:

.. autoclass:: SecureCookieSession
   :members:

.. autoclass:: NullSession
   :members:

.. autoclass:: SessionMixin
   :members:

.. autodata:: session_json_serializer

   This object provides dumping and loading methods similar to simplejson
   but it also tags certain builtin Python objects that commonly appear in
   sessions.  Currently the following extended values are supported in
   the JSON it dumps:

   -    :class:`~markupsafe.Markup` objects
   -    :class:`~uuid.UUID` objects
   -    :class:`~datetime.datetime` objects
   -   :class:`tuple`\s

.. admonition:: Notice

   The ``PERMANENT_SESSION_LIFETIME`` config key can also be an integer
   starting with Flask 0.8.  Either catch this down yourself or use
   the :attr:`~flask.Flask.permanent_session_lifetime` attribute on the
   app which converts the result to an integer automatically.