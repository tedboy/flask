.. module:: flask

Incoming Request Data
---------------------

.. autoclass:: Request
   :members:

   .. attribute:: form

      A :class:`~werkzeug.datastructures.MultiDict` with the parsed form data from ``POST``
      or ``PUT`` requests.  Please keep in mind that file uploads will not
      end up here,  but instead in the :attr:`files` attribute.

   .. attribute:: args

      A :class:`~werkzeug.datastructures.MultiDict` with the parsed contents of the query
      string.  (The part in the URL after the question mark).

   .. attribute:: values

      A :class:`~werkzeug.datastructures.CombinedMultiDict` with the contents of both
      :attr:`form` and :attr:`args`.

   .. attribute:: cookies

      A :class:`dict` with the contents of all cookies transmitted with
      the request.

   .. attribute:: stream

      If the incoming form data was not encoded with a known mimetype
      the data is stored unmodified in this stream for consumption.  Most
      of the time it is a better idea to use :attr:`data` which will give
      you that data as a string.  The stream only returns the data once.

   .. attribute:: headers

      The incoming request headers as a dictionary like object.

   .. attribute:: data

      Contains the incoming request data as string in case it came with
      a mimetype Flask does not handle.

   .. attribute:: files

      A :class:`~werkzeug.datastructures.MultiDict` with files uploaded as part of a
      ``POST`` or ``PUT`` request.  Each file is stored as
      :class:`~werkzeug.datastructures.FileStorage` object.  It basically behaves like a
      standard file object you know from Python, with the difference that
      it also has a :meth:`~werkzeug.datastructures.FileStorage.save` function that can
      store the file on the filesystem.

   .. attribute:: environ

      The underlying WSGI environment.

   .. attribute:: method

      The current request method (``POST``, ``GET`` etc.)

   .. attribute:: path
   .. attribute:: full_path
   .. attribute:: script_root
   .. attribute:: url
   .. attribute:: base_url
   .. attribute:: url_root

      Provides different ways to look at the current `IRI
      <http://tools.ietf.org/html/rfc3987>`_.  Imagine your application is
      listening on the following application root::

          http://www.example.com/myapplication

      And a user requests the following URI::

          http://www.example.com/myapplication/%CF%80/page.html?x=y

      In this case the values of the above mentioned attributes would be
      the following:

      ============= ======================================================
      `path`        ``u'/π/page.html'``
      `full_path`   ``u'/π/page.html?x=y'``
      `script_root` ``u'/myapplication'``
      `base_url`    ``u'http://www.example.com/myapplication/π/page.html'``
      `url`         ``u'http://www.example.com/myapplication/π/page.html?x=y'``
      `url_root`    ``u'http://www.example.com/myapplication/'``
      ============= ======================================================

   .. attribute:: is_xhr

      ``True`` if the request was triggered via a JavaScript
      `XMLHttpRequest`. This only works with libraries that support the
      ``X-Requested-With`` header and set it to `XMLHttpRequest`.
      Libraries that do that are prototype, jQuery and Mochikit and
      probably some more.

.. class:: request

   To access incoming request data, you can use the global `request`
   object.  Flask parses incoming request data for you and gives you
   access to it through that global object.  Internally Flask makes
   sure that you always get the correct data for the active thread if you
   are in a multithreaded environment.

   This is a proxy.  See :ref:`notes-on-proxies` for more information.

   The request object is an instance of a :class:`~werkzeug.wrappers.Request`
   subclass and provides all of the attributes Werkzeug defines.  This
   just shows a quick overview of the most important ones.
