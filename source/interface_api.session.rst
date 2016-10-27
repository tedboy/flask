.. module:: flask


Sessions
--------

If you have the :attr:`Flask.secret_key` set you can use sessions in Flask
applications.  A session basically makes it possible to remember
information from one request to another.  The way Flask does this is by
using a signed cookie.  So the user can look at the session contents, but
not modify it unless they know the secret key, so make sure to set that
to something complex and unguessable.

To access the current session you can use the :class:`session` object:

.. class:: session

   The session object works pretty much like an ordinary dict, with the
   difference that it keeps track on modifications.

   This is a proxy.  See :ref:`notes-on-proxies` for more information.

   The following attributes are interesting:

   .. attribute:: new

      ``True`` if the session is new, ``False`` otherwise.

   .. attribute:: modified

      ``True`` if the session object detected a modification.  Be advised
      that modifications on mutable structures are not picked up
      automatically, in that situation you have to explicitly set the
      attribute to ``True`` yourself.  Here an example::

          # this change is not picked up because a mutable object (here
          # a list) is changed.
          session['objects'].append(42)
          # so mark it as modified yourself
          session.modified = True

   .. attribute:: permanent

      If set to ``True`` the session lives for
      :attr:`~flask.Flask.permanent_session_lifetime` seconds.  The
      default is 31 days.  If set to ``False`` (which is the default) the
      session will be deleted when the user closes the browser.