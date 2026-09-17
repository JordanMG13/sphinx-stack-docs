.. meta::
    :description: How to make a documentation change to the Sphinx Stack. Develop, test and review documents in the standard workflow.

:relatedlinks: https://diataxis.fr, [Canonical&#32;Documentation&#32;Style&#32;Guide](https://docs.ubuntu.com/styleguide/en), [Conventional&#32;Commits](https://www.conventionalcommits.org/en/v1.0.0/)

.. _contribute-documentation:

Contribute to documentation
===========================

This guide describes how to contribute changes to the `Sphinx Stack documentation <https://github.com/canonical/sphinx-stack-docs>`__,
including fixing issues, improving existing content and adding new guides.

Contributing requires familiarity with Git, GitHub and basic terminal commands.

Before starting, review the :ref:`contribution requirements <standards-and-expectations>`.


Set up your work environment
----------------------------

`Create a personal fork <https://github.com/canonical/sphinx-stack-docs/fork>`__ of the
repository on GitHub.

Next, clone your fork onto your local machine:

.. tab-set::

    .. tab-item:: With SSH

        If you authenticate your GitHub account with `SSH
        <https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account>`__,
        run:

        .. code-block:: bash

            git clone git@github.com:<username>/sphinx-stack-docs
            cd sphinx-stack-docs
            git remote add upstream git@github.com:canonical/sphinx-stack-docs
            git fetch upstream

    .. tab-item:: With HTTPS

        If you don't authenticate with SSH, `clone with HTTPS
        <https://docs.github.com/en/get-started/git-basics/about-remote-repositories#cloning-with-https-urls>`__
        instead:

        .. code-block:: bash

            git clone https://github.com/<username>/sphinx-stack-docs
            cd sphinx-stack-docs
            git remote add upstream https://github.com/canonical/sphinx-stack-docs
            git fetch upstream

Inside the project directory, install the dependencies and verify the build:

.. code-block:: bash

    cd docs
    make install
    make html

If these commands complete without error, your environment is ready.


Plan your contribution
----------------------

Before making changes, consider whether the content already exists, where the change fits
in the documentation structure and whether it follows the project's documentation
patterns.

Significant work should be tied to an existing `GitHub issue
<https://github.com/canonical/sphinx-stack-docs/issues>`__. Before you start, comment on the
issue to have it assigned to you.

Simple tasks
~~~~~~~~~~~~

For simple changes that are small and self-contained, like fixing spelling, grammar, or
punctuation, check the existing `GitHub issues
<https://github.com/canonical/sphinx-stack-docs/issues>`__. If none exists, `open one
<https://github.com/canonical/sphinx-stack-docs/issues/new>`__ and state your interest in
working on it.

Complex tasks
~~~~~~~~~~~~~

For complex changes, like a page rewrite or a new page, describe your proposal in the issue
thread, including the page's `Diátaxis <https://diataxis.fr>`__ category and structure.


Make your changes
-----------------

Create a branch
~~~~~~~~~~~~~~~

Before you begin your work, sync your local copy of the code and create a new branch:

.. code-block:: bash

    git fetch upstream
    git checkout -b <new-branch-name>

Format your branch name as ``<ticket-id>-<description>``. For example, if you're working
on GitHub issue #235, and it's about adding a text sanitizer, you'd name your branch
``issue-235-add-text-sanitizer``. Keep the name under 80 characters.


.. _contribute-documentation-write:

Write documentation
~~~~~~~~~~~~~~~~~~~

All documentation lives in the ``docs`` directory. The directory structure,
file names and ``index.rst`` files define how content is organized and displayed.

When adding or updating content:

- Place content in the appropriate Diátaxis category
- Follow existing terminology, style and formatting patterns
- Add labels and cross-references where appropriate
- Avoid duplicating information that already exists elsewhere

For small changes, update existing pages. For new features or major changes, add new
content where users need to discover or reference it.

If you're adding a page, rewriting a page or making changes across multiple files,
document the change in the *Documentation* section of the release notes.

Test your changes
~~~~~~~~~~~~~~~~~

When you're ready to check your draft, build the documentation locally before submitting:

.. code-block:: bash

    cd docs
    make html

Then check your document sources for problems:

.. code-block:: bash

    make spelling      # Check spelling
    make linkcheck     # Validate links
    make woke          # Check inclusive language
    make lint-md       # Check Markdown style
    make vale          # Check style guide compliance (optional)

Preview your changes in a web browser. Make sure the elements you've changed render as
expected. Double-check nested elements, such as content inside tabs and admonitions.

To preview locally with live reload at ``http://127.0.0.1:8000``, run:

.. code-block:: bash

    make run

Commit a change
~~~~~~~~~~~~~~~

Register the changes to your branch with a Git commit:

.. code-block:: bash

    git add -A
    git commit

Use the `Conventional Commits
<https://www.conventionalcommits.org/en/v1.0.0/>`__ format. Most documentation changes
should use the `docs` commit type.

.. code-block:: text

    docs: add initial setup guide

Keep the message short so other contributors and the project
maintainers can see the gist of what you did.

Commit early and often. It's normal to make multiple commits for a single piece of work,
especially when you come back to review it later.

Use separate commits for unrelated changes or different components.


Sign your commits
~~~~~~~~~~~~~~~~~

All commits must be cryptographically signed. You can sign a commit by adding
``-S`` to the ``git commit`` command:

.. code-block:: bash

    git commit -S -m "docs: add initial setup guide"

Signed commits display a "Verified" badge in GitHub. To configure Git to sign commits
automatically, run:

.. code-block:: bash

    git config --global commit.gpgsign true

For more information, see `GitHub's documentation on signing commits
<https://docs.github.com/en/authentication/managing-commit-signature-verification/signing-commits>`__.


Submit your contribution
------------------------

Send for review
~~~~~~~~~~~~~~~

When you've committed your draft and you're ready to have it reviewed, push it to your fork:

.. code-block:: bash

    git push -u origin <branch-name>

Open a pull request (PR) on GitHub. Give the PR a short, descriptive title using the
`Conventional Commits <https://www.conventionalcommits.org/en/v1.0.0/>`__ format. For
single-commit branches, GitHub may generate this automatically from the commit message.

Your PR should include the following details:

- **Title**: Short, descriptive summary
- **Description**: Problem solved, features added or bugs fixed
- **Relevant issues**: `Link related issues and PRs
  <https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/autolinked-references-and-urls>`__

PRs are typically reviewed within a week.

Address quality concerns
~~~~~~~~~~~~~~~~~~~~~~~~

Before the PR is merged, it must pass all automatic checks. If there are any issues that
your local testing didn't catch, the automatic checks will fail.

To address these issues, review the logs in the failed checks.
The error messages will have remedies and hints for
what needs fixing.

Address review feedback
~~~~~~~~~~~~~~~~~~~~~~~

When the maintainers review the PR, they may suggest improvements or request changes. Address them in
follow-up commits to your branch, the same way you committed and pushed changes while
drafting. Commit locally rather than editing through the GitHub UI to avoid synchronization conflicts.

Before requesting review, rebase your branch on the latest changes from the base branch.
Once review has started, avoid rebasing unless necessary so reviewers can follow the
existing review history.

Reviewers commonly request:

- Wording, terminology or formatting changes
- Consistency with existing patterns
- Proper reST or MyST markup
- Cross-references that use proper reST or MyST syntax
- Minimal examples before listing options, with verification steps

Wrap up the review
~~~~~~~~~~~~~~~~~~

Once all suggestions are addressed, the maintainers will approve and merge the PR.

After approval, **do not** force-push to your branch, so the maintainers can clearly see any
additional changes.

Once the PR is merged, your work is complete.


Get help
--------

Open source contribution can be difficult. Even the most experienced writers
encounter uncertainty when working on unfamiliar areas.

If you are unsure about a change or need guidance, ask the issue creator or a maintainer
in the `GitHub issue <https://github.com/canonical/sphinx-stack-docs/issues>`__.
