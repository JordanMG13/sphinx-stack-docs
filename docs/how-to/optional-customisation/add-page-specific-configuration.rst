.. meta::
   :description: How to override some global configuration for specific pages in your documentation.

.. _how-to-add-page-specific-configuration:

Add page-specific configuration
===============================

You can override some global configuration for specific pages.

For example, you can configure whether to display Previous/Next buttons at the bottom of
pages by setting the ``sequential_nav`` variable in the ``docs/conf.py`` file.

.. code:: python

    html_context = {
        ...
        "sequential_nav": "both"
    }

You can then override this default setting for a specific page (for example, to turn off
the Previous/Next buttons by default, but display them in a multi-page tutorial).

To do so, add `file-wide metadata
<https://www.sphinx-doc.org/en/master/usage/restructuredtext/field-lists.html>`__ at the
top of a page. See the following examples for how to enable Previous/Next buttons for
one page:

|RST|:

.. code-block::

    :sequential_nav: both

    [Page contents]

MyST:

.. code-block::

    ---
    sequential_nav: both
    ---

    [Page contents]

Possible values for the ``sequential_nav`` field are ``none``, ``prev``, ``next``, and
``both``. See :ref:`ui-behavior` for descriptions of each value.

Another example for page-specific configuration is the ``hide-toc`` field (provided by
`Furo <https://pradyunsg.me/furo/quickstart/>`__), which can be used to hide the
page-internal table of content. See `Hiding Contents sidebar
<https://pradyunsg.me/furo/customisation/toc/>`__.