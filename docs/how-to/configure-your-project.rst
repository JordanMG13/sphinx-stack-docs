.. meta::
   :description: How to configure project-specific parameters.

.. _configure-your-project:

Configure your project
======================

Sphinx Stack documentation projects are configured in the ``docs/conf.py`` file. The default configuration will work for most projects, but you still need to set some project-specific values, such as your product's name and documentation URL.

.. important::

    The Sphinx Stack is updated periodically. After you set up your documentation repository with the Sphinx Stack, you need to track the changes made to the Sphinx Stack and manually maintain your repository.

    Use the Sphinx Stack `release notes <https://documentation.ubuntu.com/sphinx-stack/latest/release-notes>`__ or `changelog <https://github.com/canonical/sphinx-stack/blob/main/CHANGELOG.md>`__ to track changes to the Sphinx Stack. Subscribe to the repository releases to get notified whenever there is a new release. For the recommended way of manually updating your Sphinx Stack, see :ref:`update-sphinx-stacks`.

Mandatory configuration values are marked with ``TODO`` in the ``conf.py`` file. Set these to align with your project and ensure you comment out any settings that aren't relevant to your environment. 

Steps 1 – 5 cover the required configuration. For details, see :ref:`Required configuration <conf-py-required-configuration>`.

1. Set project identity, branding, and repository metadata.
2. Set the documentation site URL.
3. Configure feedback and page navigation behavior.
4. Define LLM metadata.
5. Configure how Sphinx should validate links.
6. Add :ref:`optional configuration <conf-py-optional-configuration>` as needed. The following links can help you with additional configuration:

In addition, you can find some optional parameters or add your own configuration
parameters to the file.


Required customisation
----------------------

You must check and update some of the parameters specific to your project. Mandatory
parameters are commented with the ``TODO`` keyword.

The following are some highlights of the available configuration parameters.


Update the project information
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Edit the ``docs/conf.py`` file and update the configuration in the ``Project
information`` section. See the comments in the file for more information about each
setting.


Open Graph configuration
^^^^^^^^^^^^^^^^^^^^^^^^

When you post a link to your documentation somewhere (for example, on Mattermost or
Discourse), it might be shown with a preview. This preview is configured through the
Open Graph Protocol (:vale-ignore:`OGP`) configuration.

If you don't know yet where your documentation will be hosted, you can leave the URL
empty. If you do, specify the hosting URL. You can leave the defaults for the website
name and the preview image or specify your own.


Optional customisation
----------------------

The Sphinx Stack contains several features that you can configure or turn off if they
aren't suitable for your documentation.


Modify the template
~~~~~~~~~~~~~~~~~~~

The default Sphinx Stack templates provide an initial configuration for your
documentation set, including:

- Header template - The top section of the page that contains your product's tag image
  and name, a link to your product's page (if available), and a drop-down menu for "More
  resources".
- Footer template - The bottom section of the page that contains sequential navigation
  controls, copyright information, licensing details, and other relevant links.

These are configured in ``docs/conf.py`` and are sufficient for most cases. However, if
you have additional requirements -- such as adding links to announcements or videos that
are not part of the documentation -- you can override the default templates to customize
them as needed.

See the :ref:`custom-html-templates` guide for details on how to do so.


Deactivate the feedback button
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

By default, the Sphinx Stack includes a feedback button at the top of each page. This
button redirects users to your GitHub issues page, and populates an issue for them with
details of the page they were on when they clicked the button.

If your project does not use GitHub issues, set the ``github_issues`` variable in the
``docs/conf.py`` file to an empty value to disable both the feedback button and the
issue link in the footer.

If you want to deactivate the feedback button, but keep the link in the footer, set
``disable_feedback_button`` in the ``docs/conf.py`` file to ``True``.


Add redirects
~~~~~~~~~~~~~

If you rename a source file, its URL will change. To prevent broken links, you should
add a redirect from the old URL to the new URL in this case.

You can add redirects in the ``docs/redirects.txt`` file. The paths for internal
redirects are relative to the root of the docs project.

.. code-block::

    "path/to/old/page" "path/to/new/page"

Destination paths can also be external URLs.

.. code-block::

    "path/to/old/page" "https://example.com/new-page"


Configure included extensions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Sphinx Stack includes a set of extensions that are useful for all documentation
sets. Some extensions are :ref:`enabled by default
<reference-default-sphinx-extensions>` within the Sphinx Stack, but you can customize
the selection in the  ``docs/conf.py`` file.

The canonical-sphinx extension is required for the Sphinx Stack and provides the
Furo-based theme and custom templates.

To add an extension to your documentation set, add its Python package to the
``docs/requirements.txt`` file and add the module to the ``extensions`` list in the
``docs/conf.py`` file.

.. admonition:: Extension support
    :class: note

    The only extensions formally supported by the Sphinx Stack are those included in its
    `default requirements.txt file
    <https://github.com/canonical/sphinx-stack/blob/main/docs/requirements.txt>`__. If
    you add any extensions to this list, it's your responsibility to ensure that they're
    compatible with the rest of the Sphinx Stack.

   The Sphinx Stack fully supports the third-party extensions included in the Starter pack. Support is not guaranteed for extensions you add or customize in requirements.txt

Add page-specific configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

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
``both``. See the ``docs/conf.py`` file for more information.

Another example for page-specific configuration is the ``hide-toc`` field (provided by
`Furo <https://pradyunsg.me/furo/quickstart/>`__), which can be used to hide the
page-internal table of content. See `Hiding Contents sidebar
<https://pradyunsg.me/furo/customisation/toc/>`__.


Add your own configuration
--------------------------

Custom configuration parameters for your project can be used to extend or override the
common configuration, or to define additional configuration that is not covered by the
common ``conf.py`` file.

The following links can help you with additional configuration:

- `Sphinx configuration <https://www.sphinx-doc.org/en/master/usage/configuration.html>`__
- `Sphinx extensions <https://www.sphinx-doc.org/en/master/usage/extensions/index.html>`__
- `Furo documentation <https://pradyunsg.me/furo/quickstart/>`__

If you need additional Python packages for any custom processing you do in your
documentation, add them to the ``docs/requirements.txt`` file.


Disable failure on warning
--------------------------

The docs build is, by default, set to fail when a warning (``WARNING`` in the build log)
is encountered. To disable this setting, remove the ``--failure-on-warning`` option from
the command specified in the ``html`` target in the ``Makefile``.
   - `Sphinx configuration <https://www.sphinx-doc.org/en/master/usage/configuration.html>`__
   - `Sphinx extensions <https://www.sphinx-doc.org/en/master/usage/extensions/index.html>`__
   - `Furo documentation <https://pradyunsg.me/furo/quickstart/>`__
