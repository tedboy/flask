Flask, WerkzeugAPI
==================
Flask and Werkzeug API I created using the rst source codes and ``autosummary``.

- ``|today|`` = |today|
- Flask Version 0.11
- Werkzeug Version 0.11
- All modules for which code is available (`link <./_modules/index.html>`_)

.. important:: Content directly from official documentation page from (`Flask <http://flask.pocoo.org/docs/0.11/>`__ and `Werkzeug <http://werkzeug.pocoo.org/docs/0.11/>`__. I just created this page for my own liking since I love the readthedoc Sphinx theme.

- https://github.com/pallets/werkzeug/tree/master/docs
- https://github.com/pallets/flask/tree/master/docs

.. toctree::
   :maxdepth: 1
   :caption: Table of Contents

   tutorial/index.rst
   quickstart/flask_doc.quickstart.rst
   patterns/index1.rst
   patterns/index2.rst
   deploying/index.rst
   flask_doc.templating.rst
   flask_doc.config.rst
   flask_doc.testing.rst
   flask_doc.rst
   flask_doc2.rst
   werk_doc.rst


.. now include api part

.. toctree::
    :maxdepth: 1
    :caption: Auto-generated API

    generated/flask
    generated/werkzeug

.. autosummary::
   :toctree:generated/
   :template:module_custom.rst

    flask
    werkzeug