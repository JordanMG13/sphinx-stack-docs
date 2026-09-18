.. meta::
   :description: Troubleshooting guidance for issues related to running a local development environment for Sphinx Stack documentation.

.. _local_deployment_troubleshooting:

Local deployment issues
=======================

Port or address already in use
------------------------------

Possible cause
~~~~~~~~~~~~~~

When you run ``make html`` or ``make run`` to start a local development server, the build fails with an error message similar to the following:

.. terminal::
    :output-only:

    [sphinx-autobuild] Serving on http://127.0.0.1:8000
    [sphinx-autobuild] Waiting to detect changes...
                       ERROR:    [:vale-ignore:`Errno 98``] error while attempting to bind on address ('127.0.0.1', 8000): address already in use

This happens when the port number specified for the local development server is already in use by another process. By default, the local development server runs on port 8000, but if another process is using that port, the build will fail. Many times, ``Ctrl-C`` is used to stop the local development server, but the process may not have been terminated properly, leaving the port in use. The underlying socket may still be in use, preventing the local development server from starting on the same port.

Resolution
~~~~~~~~~~

You can use the ``SPHINX_PORT`` environment variable to specify a different port number for the local development server. For example, to use port 8081, run the following command:

.. code-block:: bash

    SPHINX_PORT=8081 make run

An elegant solution is to use ``SPHINX_PORT=0`` to let the system automatically select an available port for the local development server. Make sure to check the output of the command to see which port was selected, and use the system selected port number to access the local development server in your web browser.

