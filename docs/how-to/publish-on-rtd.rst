.. meta::
   :description: How to publish your documentation on Read the Docs.

.. _publish-on-rtd:


Publish on Read the Docs
========================

Canonical documentation, both internal and external, is hosted on Read the Docs. This guide walks you through the process of setting up your Sphinx Stack project on Read the Docs. 


Prepare your project and account
---------------------------------

- Ensure your documentation :ref:`builds without errors or warnings <build-clean>`.
- Ensure you have permission to manage webhooks for the documentation repository, or get in touch with someone who does. A webhook is needed if you want Read the Docs to automatically build your documentation whenever a change occurs. 
- In the root of your documentation repository, create the `.readthedocs.yaml <https://docs.readthedocs.com/platform/stable/config-file/index.html>`__ file.
- Create a `Read the Docs account <https://docs.readthedocs.com/platform/stable/tutorial/index.html#creating-a-read-the-docs-account>`__. If your documentation is hosted on a supported Git provider (such as GitHub, GitLab, or Bitbucket), `connect the account to your Git provider <https://docs.readthedocs.com/platform/stable/guides/connecting-git-account.html>`__. 
  
  .. note:: 
    
    For Canonical staff, a Read the Docs account is created during onboarding and connected to our Google SSO:

    1. Log in to the `Read the Docs dashboard <https://readthedocs.com/dashboard/>`__ using the :guilabel:`Sign in with Google` option.
    2. Ask on the `~Documentation <https://chat.canonical.com/canonical/channels/documentation>`__ Mattermost channel to be added to your specific team.
    3. Accept the invitation that is sent to your email. Make sure you are logged in to Read the Docs with your Google account when you do this.


Add your project to Read the Docs
---------------------------------

Log in to the `Read the Docs dashboard <https://readripman.readthedocs.io/en/latest/glossary.html#term-dashboard>`__ to add a project. If you are the repository administrator, `add the project automatically <https://docs.readthedocs.com/platform/stable/intro/add-project.html#automatically-add-your-project>`__. If you are not a repository administrator or your documentation is not hosted on a supported Git provider, `add the project manually <https://docs.readthedocs.com/platform/stable/intro/add-project.html#manually-add-your-project>`__. A build will be triggered after the project is added.

When adding a project, most fields are self-explanatory, and you can leave the default values. Check and update the following fields:

- **Organization and team**: This determines who gets admin access to your Read the Docs project.
- **Name**: This is the slug used in the documentation URL. You cannot change the name after you create the project, so choose carefully!
- **Default branch**: This determines which branch "latest" points to.

  .. note::

     If your repository does not have a main branch and you don't specify the correct default branch, the build will default to main and will not be able to check out the repository and find the correct branch. If this happens, the solution is to add a main branch to the project until you're able to build it once, then specify the correct branch.

`Check the first build <https://docs.readthedocs.com/platform/stable/tutorial/index.html#checking-the-first-build>`__. After it completes successfully, you can view the live documentation from the project home page. 

If there are any build errors, fix them: 

- If your project is hosted in a private repository, your first build will fail because your repository is not configured to allow Read the Docs to clone the repository. To fix the access issue, follow the steps in `Configuring your repository <https://docs.readthedocs.com/platform/stable/guides/creating-project-private-repository.html#configuring-your-repository>`__.
- If your project is hosted in a public repository, your documentation should build successfully. If you get any errors, check the build log for indications of what the problem is.


Canonical-specific configuration
---------------------------------

After the initial build, some Canonical-specific configurations are required, as listed below.


Change the host URL to documentation.ubuntu.com
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

By default, your documentation URL is ``https://canonical-<name>.readthedocs-hosted.com``, where ``<name>`` is the slug you specified when adding the project. Change the host URL to ``https://documentation.ubuntu.com``:

1. Log in to the `Read the Docs dashboard <https://readthedocs.com/dashboard/>`__.
2. Locate the "Ubuntu documentation library" project and go to :guilabel:`Settings > Subprojects` to add a subproject.
3. Select your project to connect as a subproject from the Subproject dropdown list.
4. (Optional) To use a different name in the URL structure, specify the name in the :guilabel:`Alias` field. Leave it blank to use the name you specified when adding your project.
5. Click :guilabel:`Add subproject`.

Your documentation URL has changed to ``https://documentation.ubuntu.com/<name>``.


Limit documentation access
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To limit the documentation access to Canonical users, the project needs to be added to the `All Canonical Users <https://app.readthedocs.com/organizations/canonical/teams/all-canonical-users/>`__ group. All Canonical users who log in to Read the Docs with Google SSO will be automatically added to this user group and granted read-only permissions to the associated projects.

This operation requires the Owner's permission to Read the Docs. Contact one of the `owners of the Canonical organization <https://app.readthedocs.com/organizations/canonical/members/?teams__slug=all-canonical-users&access=owner>`__ or reach out to the `~Documentation <https://chat.canonical.com/canonical/channels/documentation>`__ channel.


Change URL versioning scheme
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The default versioning scheme is `Multiple versions with translations <https://docs.readthedocs.com/platform/stable/versioning-schemes.html#multiple-versions-with-translations>`__. Unless your documentation has multiple languages, do not use the scheme with translation. `Change the URL versioning scheme <https://docs.readthedocs.com/platform/stable/versioning-schemes.html#how-to-change-the-url-versioning-scheme-of-your-project>`__ under your project Settings to either a single version without translations or multiple versions without translations.


Additional configuration
------------------------

Configurations in this section are all optional. Follow the instructions here to configure your Read the Docs project according to your preferences.


Enable automatic builds
^^^^^^^^^^^^^^^^^^^^^^^

The Read the Docs integration webhook can automatically build your documentation when your GitHub repository changes. If you added the GitHub repository to Read the Docs automatically, the integration webhook was already created. Otherwise, the webhook must be set up by someone with the appropriate permission.

To check if the webhook was created, open the project Settings and click :guilabel:`Integrations`. If the webhook is not listed, someone with the appropriate permissions must `manually set it up <https://docs.readthedocs.com/platform/stable/guides/setup/git-repo-manual.html#how-to-manually-configure-a-git-repository-integration>`__.

Automatic builds are not supported on Launchpad because its webhooks aren't compatible with Read the Docs. Builds must be triggered manually on Read The Docs.


Enable pull request builds
^^^^^^^^^^^^^^^^^^^^^^^^^^

To build and preview your documentation whenever a pull request is opened or updated, open the project Settings and click :guilabel:`Pull request builds` to enable it for your project.


Make your documentation public (RTD for Business only)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

By default, Read the Docs publishes your documentation for logged-in users only. 

To make your documentation publicly accessible, you must configure the privacy level for each version of the documentation separately. Navigate to the Versions tab to configure the `Privacy Level <https://docs.readthedocs.com/platform/stable/guides/pull-requests.html#privacy-levels>`__ for each version.

If your documentation publishes tagged versions that should be public by default, add an `Automation Rule <https://docs.readthedocs.com/platform/stable/guides/automation-rules.html#adding-a-new-automation-rule>`__ under the project Settings with the following configuration:

.. list-table:: 
   :widths: 60 40
   :header-rows: 1

   * - Configuration field
     - Value
   * - Version type
     - Tag
   * - Version predefined match pattern
     - Any version
   * - Action
     - Make version public



Add support for Git Large File Storage
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

By default, Read the Docs can't access large files stored with Git LFS. If your docs project contains large files, `add support for Git LFS <https://docs.readthedocs.com/platform/stable/build-customization.html#support-git-lfs-large-file-storage>`__.
