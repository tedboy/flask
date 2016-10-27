.. module:: flask

Command Line Interface
----------------------

.. currentmodule:: flask.cli

.. autoclass:: FlaskGroup
   :members:

.. autoclass:: AppGroup
   :members:

.. autoclass:: ScriptInfo
   :members:

.. autofunction:: with_appcontext

.. autofunction:: pass_script_info

   Marks a function so that an instance of :class:`ScriptInfo` is passed
   as first argument to the click callback.

.. autodata:: run_command

.. autodata:: shell_command
