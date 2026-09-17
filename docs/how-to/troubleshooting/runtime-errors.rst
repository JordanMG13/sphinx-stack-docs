.. meta::
   :description: Troubleshooting guidance for runtime issues related to Sphinx rendering peculiarities or link configurations.

.. _runtime_errors_troubleshooting:

Runtime errors
==============

"&" character in the URL breaks links
--------------------------------------


A link in the documentation set is broken when the URL contains an "&" character. For example, a link to ``https://example.com/?param1=value1&param2=value2`` may be rendered as ``https://example.com/?param1=value1&amp;amp;param2=value2`` in the generated HTML, causing the link to break.

Possible cause
~~~~~~~~~~~~~~

This is a known+fixed bug: https://github.com/executablebooks/MyST-Parser/pull/1126

But it is currently unavailable to us due to myst-parser being pinned to avoid conflicts: https://github.com/canonical/sphinx-stack/pull/496

The fix is in ``myst-parser`` 5.1.0, which would require dropping Python 3.10 and Sphinx 7 (which is not an option in late 2026).

Resolution
~~~~~~~~~~

In ``.rst`` sources, keep ``&`` in the URL; Sphinx will escape it correctly in the generated HTML. For example: ``https://example.com/?param1=value1&param2=value2``.

If you are using `Markdown <https://daringfireball.net/projects/markdown/>`_, you can use raw HTML syntax to include the link in the documentation. For example, you can use:

.. code:: html

     <a href="https://example.com/?param1=value1&amp;param2=value2">Link text</a>

If you are using `MyST <https://myst-parser.readthedocs.io/en/latest/>`_, you can use an ``eval-rst`` - this is helpful because it supports including a clear comment why the link has been treated specially (so will be easier to spot later when a fixed ``myst-parser`` is in use):

.. code-block:: markdown

   ```{eval-rst}
   .. This URL includes an & which is broken by the current version of myst-parser (fixed in v5.1.0)

   `Link text <https://example.com/?param1=value1&param2=value2>`_
   ```
