.. meta::
   :description: Troubleshooting guidance for issues related to RTD builds and deployments.
.. _rtd_troubleshooting:

Read the Docs failures
======================

In this guide, you will find information on how to troubleshoot issues related to building the documentation set on Read the Docs.

.. _stable-version-wont-build-from-latest-tag:

Stable version won't build from the latest tag
----------------------------------------------

If your project has the ``stable`` version configured to build from tags, such as with
the default `semantic versioning behavior
<https://docs.readthedocs.com/platform/stable/versions.html#versions-are-git-tags-and-branches>`_,
your ``stable`` version can become out-of-step and continue building a particular tag,
even when the repository has newer tags.


Possible causes
~~~~~~~~~~~~~~~

An unwanted tag might have been pushed to the repository and then removed. Once
Read the Docs creates a version from a tag, it doesn't later verify that the tag still
exists, so the version will persist and become a zombie.

If the unwanted tag is a higher iterator than any existing tag, the zombie version will
always take precedence. For example, if tag ``20.2.0`` was pushed by accident and then
replaced with ``2.20.0``, the corresponding version 20.2.0 will persist, and ``stable``
will continue pointing to it.


Diagnosis
~~~~~~~~~

There's a roundabout procedure to verify whether your project is affected. Start by
opening your project dashboard on Read the Docs.

On the :guilabel:`Builds` tab, locate the most recent ``stable`` build. For that version, hover
over the status indicator. In the hover box, open the :guilabel:`stable` link. If the resulting
GitHub page is a 404, then your project has a zombie version.

.. image:: /how-to/assets/troubleshoot-stable-zombie-version.png


Resolution
~~~~~~~~~~

On your project dashboard, open the :guilabel:`Versions` tab and click :guilabel:`Add version`.

Find the zombie version, deactivate it, then update it.

Rebuild ``stable`` by retriggering it on the dashboard or pushing a new tag to the
repository.


Issue tracking
~~~~~~~~~~~~~~

`readthedocs/readthedocs.org#12450
<https://github.com/readthedocs/readthedocs.org/issues/12450>`_

Creating a new version slug fails
----------------------------------

When you try to create a new version, you may see an error stating that the slug is already in use,
even though you don't visually see the version occupying it in the RTD GUI.

Possible causes
~~~~~~~~~~~~~~~

This occurs when a version is deactivated in the RTD GUI, for example by turning
off its **Active** switch, and someone later tries to create a new version with
the same slug. Read the Docs has no delete option in its web GUI, so the
deactivated version continues to occupy that slug indefinitely.

When a version is deactivated
(see `Inactive versions <https://docs.readthedocs.com/platform/stable/versions.html#version-states>`_),
its documentation content is deleted and builds can no longer be triggered from
the GUI. However, the version itself remains and retains its slug. It may not
appear in the **Versions** tab, making it effectively invisible in the web GUI
while still causing the "slug is already in use" error.

Resolution
~~~~~~~~~~

The easiest way to deal with an invisible inactive version is to open its settings
page directly via URL:

.. code-block:: text

   https://app.readthedocs.com/dashboard/<project-name>/version/<slug>/edit/

For example:

.. code-block:: text

   https://app.readthedocs.com/dashboard/canonical-kafka-charm/version/latest/edit/

From there, you can make the version active again, which will make it visible in the
web GUI, or change its slug.

Authentication errors
---------------------

At times, documentation projects that were building successfully may suddenly experience build failures due to authentication issues with GitHub. 

Possible cause
~~~~~~~~~~~~~~

A common cause of sudden authentication failures is that the GitHub Webhook goes out of sync due to infrastructure issues on RTD or GitHub. Those issues are usually resolved with time, rather than manual action.

Resolution
~~~~~~~~~~

Confirm that the git repository URL setting in Read the Docs points to a valid repository. Verify that the public SSH key from your Read the Docs project is installed as a deploy key on your the GitHub repository. If these are already in place then try to resynchronize the webhook.

If the above steps do not solve the problem, delete and `reinstate the webhook <https://docs.readthedocs.com/platform/latest/guides/setup/git-repo-manual.html#manual-integration-setup>`_.

`readthedocs.yaml` file issues
------------------------------

The GitHub pull request build fails stating the ``.readthedocs.yaml`` file is not accessible.

Probable cause
~~~~~~~~~~~~~~

Inaccessibility of the  ``.readthedocs.yaml`` file in the build system can be caused if the file is missing or if it is not in the location configured at RTD.


Resolution
~~~~~~~~~~

Check the location of the ``.readthedocs.yaml`` file in your repository to ensure it is in the root directory (default location). Read the Docs requires a ``.readthedocs.yaml`` file in the repository root to trigger a build; if this file is missing, the build will fail. 

The Canonical Sphinx Stack assumes, by default, that documentation content lives under ``/docs/`` and that ``.readthedocs.yaml`` is in the repository root, but neither location is a hard requirement. 

If your project does not use the default root directory, ensure that ``.readthedocs.yaml`` exists and specify its correct location in the RTD build configuration file as described in the `Read the Docs documentation <https://docs.readthedocs.com/platform/stable/guides/setup/monorepo.html#how-to-use-a-readthedocs-yaml-file-in-a-sub-folder>`_.