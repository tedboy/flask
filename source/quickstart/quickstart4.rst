Routing
-------

Modern web applications have beautiful URLs.  This helps people remember
the URLs, which is especially handy for applications that are used from
mobile devices with slower network connections.  If the user can directly
go to the desired page without having to hit the index page it is more
likely they will like the page and come back next time.

As you have seen above, the :meth:`~flask.Flask.route` decorator is used to
bind a function to a URL.  Here are some basic examples::

    @app.route('/')
    def index():
        return 'Index Page'

    @app.route('/hello')
    def hello():
        return 'Hello, World'

But there is more to it!  You can make certain parts of the URL dynamic and
attach multiple rules to a function.

Variable Rules
``````````````

To add variable parts to a URL you can mark these special sections as
``<variable_name>``.  Such a part is then passed as a keyword argument to your
function.  Optionally a converter can be used by specifying a rule with
``<converter:variable_name>``.  Here are some nice examples::

    @app.route('/user/<username>')
    def show_user_profile(username):
        # show the user profile for that user
        return 'User %s' % username

    @app.route('/post/<int:post_id>')
    def show_post(post_id):
        # show the post with the given id, the id is an integer
        return 'Post %d' % post_id

The following converters exist:

=========== ===============================================
`string`    accepts any text without a slash (the default)
`int`       accepts integers
`float`     like ``int`` but for floating point values
`path`      like the default but also accepts slashes
`any`       matches one of the items provided
`uuid`      accepts UUID strings
=========== ===============================================

.. note:: 

   **Unique URLs / Redirection Behavior**

   Flask's URL rules are based on Werkzeug's routing module.  The idea
   behind that module is to ensure beautiful and unique URLs based on
   precedents laid down by Apache and earlier HTTP servers.

   Take these two rules::

        @app.route('/projects/')
        def projects():
            return 'The project page'

        @app.route('/about')
        def about():
            return 'The about page'

   Though they look rather similar, they differ in their use of the trailing
   slash in the URL *definition*.  In the first case, the canonical URL for the
   ``projects`` endpoint has a trailing slash.  In that sense, it is similar to
   a folder on a filesystem.  Accessing it without a trailing slash will cause
   Flask to redirect to the canonical URL with the trailing slash.

   In the second case, however, the URL is defined without a trailing slash,
   rather like the pathname of a file on UNIX-like systems. Accessing the URL
   with a trailing slash will produce a 404 "Not Found" error.

   This behavior allows relative URLs to continue working even if the trailing
   slash is omitted, consistent with how Apache and other servers work.  Also,
   the URLs will stay unique, which helps search engines avoid indexing the
   same page twice.


.. _url-building:

URL Building
````````````

If it can match URLs, can Flask also generate them?  Of course it can.  To
build a URL to a specific function you can use the :func:`~flask.url_for`
function.  It accepts the name of the function as first argument and a number
of keyword arguments, each corresponding to the variable part of the URL rule.
Unknown variable parts are appended to the URL as query parameters.  Here are
some examples::

    >>> from flask import Flask, url_for
    >>> app = Flask(__name__)
    >>> @app.route('/')
    ... def index(): pass
    ...
    >>> @app.route('/login')
    ... def login(): pass
    ...
    >>> @app.route('/user/<username>')
    ... def profile(username): pass
    ...
    >>> with app.test_request_context():
    ...  print url_for('index')
    ...  print url_for('login')
    ...  print url_for('login', next='/')
    ...  print url_for('profile', username='John Doe')
    ...
    /
    /login
    /login?next=/
    /user/John%20Doe

(This also uses the :meth:`~flask.Flask.test_request_context` method, explained
below.  It tells Flask to behave as though it is handling a request, even
though we are interacting with it through a Python shell.  Have a look at the
explanation below. :ref:`context-locals`).

Why would you want to build URLs using the URL reversing function
:func:`~flask.url_for` instead of hard-coding them into your templates?
There are three good reasons for this:

1. Reversing is often more descriptive than hard-coding the URLs.  More
   importantly, it allows you to change URLs in one go, without having to
   remember to change URLs all over the place.
2. URL building will handle escaping of special characters and Unicode
   data transparently for you, so you don't have to deal with them.
3. If your application is placed outside the URL root - say, in
   ``/myapplication`` instead of ``/`` - :func:`~flask.url_for` will handle
   that properly for you.


HTTP Methods
````````````

HTTP (the protocol web applications are speaking) knows different methods for
accessing URLs.  By default, a route only answers to ``GET`` requests, but that
can be changed by providing the ``methods`` argument to the
:meth:`~flask.Flask.route` decorator.  Here are some examples::

    from flask import request

    @app.route('/login', methods=['GET', 'POST'])
    def login():
        if request.method == 'POST':
            do_the_login()
        else:
            show_the_login_form()

If ``GET`` is present, ``HEAD`` will be added automatically for you.  You
don't have to deal with that.  It will also make sure that ``HEAD`` requests
are handled as the `HTTP RFC`_ (the document describing the HTTP
protocol) demands, so you can completely ignore that part of the HTTP
specification.  Likewise, as of Flask 0.6, ``OPTIONS`` is implemented for you
automatically as well.

You have no idea what an HTTP method is?  Worry not, here is a quick
introduction to HTTP methods and why they matter:

The HTTP method (also often called "the verb") tells the server what the
client wants to *do* with the requested page.  The following methods are
very common:

``GET``
    The browser tells the server to just *get* the information stored on
    that page and send it.  This is probably the most common method.

``HEAD``
    The browser tells the server to get the information, but it is only
    interested in the *headers*, not the content of the page.  An
    application is supposed to handle that as if a ``GET`` request was
    received but to not deliver the actual content.  In Flask you don't
    have to deal with that at all, the underlying Werkzeug library handles
    that for you.

``POST``
    The browser tells the server that it wants to *post* some new
    information to that URL and that the server must ensure the data is
    stored and only stored once.  This is how HTML forms usually
    transmit data to the server.

``PUT``
    Similar to ``POST`` but the server might trigger the store procedure
    multiple times by overwriting the old values more than once.  Now you
    might be asking why this is useful, but there are some good reasons
    to do it this way.  Consider that the connection is lost during
    transmission: in this situation a system between the browser and the
    server might receive the request safely a second time without breaking
    things.  With ``POST`` that would not be possible because it must only
    be triggered once.

``DELETE``
    Remove the information at the given location.

``OPTIONS``
    Provides a quick way for a client to figure out which methods are
    supported by this URL.  Starting with Flask 0.6, this is implemented
    for you automatically.

Now the interesting part is that in HTML4 and XHTML1, the only methods a
form can submit to the server are ``GET`` and ``POST``.  But with JavaScript
and future HTML standards you can use the other methods as well.  Furthermore
HTTP has become quite popular lately and browsers are no longer the only
clients that are using HTTP. For instance, many revision control systems
use it.

.. _HTTP RFC: http://www.ietf.org/rfc/rfc2068.txt