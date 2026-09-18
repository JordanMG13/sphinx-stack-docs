.. meta::
   :description: Troubleshooting guidance for runtime issues related to Sphinx rendering peculiarities or link configurations.

.. _runtime_errors_troubleshooting:

Runtime errors
==============

"&" character in the URL breaks links
--------------------------------------


A link in the documentation set is broken when the URL contains an "&" character. For example, a link to ``https://example.com/?param1=value1&param2=value2`` may be rendered as ``https://example.com/?param1=value1&amp;amp;param2=value2`` in the generated HTML, causing the link to break.

Cause
~~~~~

This is a `known bug <https://github.com/executablebooks/MyST-Parser/issues/1028>`__ that has been resolved in the myst-parser extension. However, the latest version of the extension is incompatible with the Sphinx Stack, which retains support for Python 3.10 and Sphinx 7.

Resolution
~~~~~~~~~~

In ``.rst`` sources, keep ``&`` in the URL; Sphinx will escape it correctly in the generated HTML. For example: ``https://example.com/?param1=value1&param2=value2``.

If you are using `MyST <https://myst-parser.readthedocs.io/en/latest/>`_, add the link in an ``eval-rst`` directive. You may also want to include a comment explaining the workaround so it'll be easier to undo later on.

.. code-block:: markdown

   ```{eval-rst}
   .. This URL includes an & which is broken by the current version of myst-parser (fixed in v5.1.0)

   `Link text <https://example.com/?param1=value1&param2=value2>`_
   ```
