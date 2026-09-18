.. meta::
   :description: Troubleshooting guidance for issues related to building and publishing documentation using the Sphinx Stack and Read the Docs.

.. _build_errors_troubleshooting:

Build errors
============

In this guide, you will find information on how to troubleshoot local or GitHub PR build errors that may occur when building a documentation set.

'Module not found' error
------------------------

The build fails with a ``ModuleNotFoundError: No module named '<module>'`` error message when you run it locally (or in a GitHub PR check).

Probable cause
~~~~~~~~~~~~~~

A ``ModuleNotFoundError`` means the Python interpreter running the build cannot locate the named module at all. The most common causes for a local build are:

* **The package is missing from** ``requirements.txt``. The module is not installed because it is not declared as a dependency. If it works for you but fails for a colleague or in CI, it is likely installed in your environment but absent from ``requirements.txt``.
* **A local module or extension is not on** ``sys.path``. Custom extensions or ``conf.py`` helpers that live inside the repository are not importable unless their directory is added to the path in ``conf.py``.

Resolution
~~~~~~~~~~

Read the full traceback and note the exact module name reported after ``No module named`` error message. If it is a third-party package (for example ``myst-parser``), check whether it is listed in ``requirements.txt``. If it is missing, add it and reinstall. If it is present, confirm it is actually installed in the active environment with ``pip show <package>``. If a newer version is available, consider updating it and adjusting the version constraints in ``requirements.txt`` accordingly.

Rebuild to confirm the fix

.. code-block:: bash

     make clean; make run

.. tip::

   If the build works for you but fails for a colleague or in a GitHub PR check, the module is almost always installed in your local environment but missing from ``requirements.txt``. Add it there so every environment installs it.

For local extensions, you need to append the source to your system path by adding the following line to your ``conf.py`` file:

.. code-block:: python  

     sys.path.insert(0, os.path.abspath('<path>'))  

'Pip resolution too deep' error
--------------------------------

This error occurred recently while building several Sphinx Stack based documentation sets independently on different machines on a specific day when a new version of ``myst-parser`` was released. The issue could be resolved by pinning the ``myst-parser`` version in the ``requirements.txt`` file.

Probable cause
~~~~~~~~~~~~~~

This error may occur when the ``requirements.txt`` file has conflicting dependencies or a dependency tree is too complex for ``pip`` to resolve efficiently.

Documentation based on the Sphinx Stack often hit this error due to an unpinned or incompatible version of a package.

Resolution
~~~~~~~~~~

Review the dependencies listed in the ``requirements.txt`` file and resolve any conflicts caused by incompatible or unpinned package versions or newly released versions. 

To fix the issue, try:

* Restricting problem packages to recent versions (using ``package~=version``)
* Using a constraints file

Sporadic build failures due to a "No such file or directory" error
------------------------------------------------------------------

If your Readthedocs builds are sporadically failing due to a file missing error, it is best to
first check the build output to make sure the issue isn't related to dependencies.

Once you have verified this is not the case, you can double check your conf.py configuration.

Probable cause
~~~~~~~~~~~~~~~

sphinx-llm by default creates a parallel process that touches build files while the main process
(or other extensions, like sphinx-tags) could still be using them. This can cause the missing
file error.

Resolution
~~~~~~~~~~

*Please note that this workaround can cause your build times to grow rather large,
as the parallel nature of the normal config cuts build times considerably.*

To fix this, simply tell sphinx-llm in your conf.py to not build in parallel:

.. code-block:: python

   # Run sphinx-llm markdown generation sequentially to prevent race conditions
   llms_txt_build_parallel = False