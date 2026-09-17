.. meta::
    :description: How to make a code change to the Sphinx Stack. Code, test and review changes in the project within the standard workflow.

:relatedlinks: [Conventional&#32;Commits](https://www.conventionalcommits.org/en/v1.0.0/), [Developer&#32;Certificate&#32;of&#32;Origin](https://developercertificate.org/)

.. _contribute-development:

Contribute to development
=========================

This guide describes how to contribute code changes to the `Sphinx Stack
<https://github.com/canonical/sphinx-stack>`__.

Development contributions improve the shared configuration, extensions, themes,
workflows and tooling that projects use to build documentation with the Sphinx Stack.

It requires familiarity with Git, GitHub, and the Sphinx documentation generator.

Before starting, review the :ref:`contribution requirements <standards-and-expectations>`.


Set up your work environment
----------------------------

`Create a personal fork <https://github.com/canonical/sphinx-stack/fork>`__ of the
repository on GitHub.

Next, clone your fork onto your local machine:

.. tab-set::

    .. tab-item:: With SSH

        If you authenticate your GitHub account with `SSH
        <https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account>`__,
        run:

        .. code-block:: bash

            git clone git@github.com:<username>/sphinx-stack
            cd sphinx-stack
            git remote add upstream git@github.com:canonical/sphinx-stack
            git fetch upstream

    .. tab-item:: With HTTPS

        If you don't authenticate with SSH, `clone with HTTPS
        <https://docs.github.com/en/get-started/git-basics/about-remote-repositories#cloning-with-https-urls>`__
        instead:

        .. code-block:: bash

            git clone https://github.com/<username>/sphinx-stack
            cd sphinx-stack
            git remote add upstream https://github.com/canonical/sphinx-stack
            git fetch upstream

Install the dependencies and build the documentation to
verify your environment:

.. code-block:: bash

    cd docs
    make install
    make html

If these commands complete without error, your environment is ready.


Plan your contribution
----------------------

All significant development work should be associated with an issue.

Before starting work, check the existing `GitHub issues
<https://github.com/canonical/sphinx-stack/issues>`__ to see whether the task is already
being discussed or tracked. Comment on the issue to have it assigned to you.

If no issue exists for your task, `create one
<https://github.com/canonical/sphinx-stack/issues/new>`__ before you begin.


Simple tasks
~~~~~~~~~~~~

For simple, self-contained tasks, such as bug fixes or tweaks to the
default configuration, check existing `GitHub issues
<https://github.com/canonical/sphinx-stack/issues>`__. If no suitable issue exists,
`open one <https://github.com/canonical/sphinx-stack/issues/new>`__ and discuss
the change you'd like to make with a maintainer.

Complex tasks
~~~~~~~~~~~~~

For complex tasks, such as introducing new features, changing workflows or adding stricter
checks, describe your proposal in the issue thread.

Include the implementation plan, expected impact on projects using the Sphinx Stack, and
any required tests and documentation updates. This helps the team decide whether the change
aligns with the project's goals.

Contribute a change
-------------------

Create a branch
~~~~~~~~~~~~~~~

Before you begin your work, sync your local copy of the code and create a new branch:

.. code-block:: bash

    git fetch upstream
    git checkout -b <new-branch-name>

Format your branch name as ``<ticket-id>-<description>``. For example, if you're working
on GitHub issue #235, and it's about adding a text sanitizer, you'd name your branch
``issue-235-add-text-sanitizer``. Keep the name under 80 characters.

Develop
~~~~~~~

Keep the Sphinx Stack minimal by default. Changes should improve the experience of
all downstream projects, rather than solve problems specific to one documentation project.

Optional features are best implemented by the projects that use the Sphinx Stack rather
than added to the shared foundation.

Test
~~~~

Verify your work by building the documentation and running quality checks.

.. code-block:: bash

    cd docs
    make html

Run the available quality checks:

.. code-block:: bash

    make spelling      # Check spelling
    make linkcheck     # Validate links
    make woke          # Check inclusive language
    make lint-md       # Check Markdown style
    make vale          # Check style guide compliance (optional)

To preview locally with live reload at ``http://127.0.0.1:8000``, run:

.. code-block:: bash

    make run

Document your work
~~~~~~~~~~~~~~~~~~

Document any changes that affect users.

For small changes, update the existing documentation that describes the affected
functionality. This may include how-to guides, reference pages or other related content.

For a new feature or a nontrivial change to an existing one, add documentation
explaining its usage and specifications. For guidance on contributing documentation,
refer to :ref:`contribute-documentation`.

All changes and fixes should be recorded in the `changelog <https://github.com/canonical/sphinx-stack/blob/main/CHANGELOG.md>`__.

Commit a change
~~~~~~~~~~~~~~~

Register your changes with a Git commit:

.. code-block:: bash

    git add -A
    git commit

Use the `Conventional Commits
<https://www.conventionalcommits.org/en/v1.0.0/>`__ format.

.. code-block:: text

    feat: add text sanitizer

Keep commits focused and separate unrelated changes or components into different
commits. Review similar commits with ``git log --oneline <filename>`` if you are
unsure which commit type to use.

Commit messages should be short and describe the purpose of the change.

.. admonition:: Complex commits
    :class: tip

    If a commit contains multiple unrelated changes, split it into smaller commits.
    For each commit, select the highest-ranked type that fits:

    - :vale-ignore:`ci`
    - build
    - feat
    - fix
    - :vale-ignore:`perf`
    - refactor
    - style
    - test
    - docs
    - chore

Sign your commits
~~~~~~~~~~~~~~~~~

All commits must be cryptographically signed. You can sign a commit by adding
``-S`` to the ``git commit`` command:

.. code-block:: bash

    git commit -S -m "feat: add text sanitizer"

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

When you've committed your draft and you're ready to have it reviewed, push it to your
fork:

.. code-block:: bash

    git push -u origin <branch-name>

Open a pull request (PR) on GitHub. Give the PR a short, descriptive title using the
`Conventional Commits <https://www.conventionalcommits.org/en/v1.0.0/>`__ format. For
single-commit branches, GitHub generates this automatically from the commit message.

Include the following details in your PR:

- **Description**: Problem solved, features added or bugs fixed
- **Relevant issues**: `Link related issues and PRs
  <https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/autolinked-references-and-urls>`__
- **Testing**: How reviewers can verify the change or test the fix
- **Reversibility**: Reasoning and steps for decisions that are difficult to reverse

PRs are typically reviewed within a week.

Address quality concerns
~~~~~~~~~~~~~~~~~~~~~~~~

Before the PR is merged, it must pass all automated checks that validate builds,
dependencies and documentation quality.

If there are any issues in your branch that your local testing didn't catch, then the
automatic checks will fail. To address these issues, review the logs in the failed
checks. The error messages in the logs will have remedies and hints for what needs
fixing.

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
- Theme compatibility, tested in both light and dark modes


Wrap up the review
~~~~~~~~~~~~~~~~~~

Once all suggestions are addressed, maintainers will approve and merge the PR.

After approval, **do not** force-push to your branch.

Once the PR is merged, your work is complete.


Get help
--------

Open source contribution can be challenging. Even experienced developers encounter
uncertainty when working on unfamiliar areas.

If you are unsure about a change or need guidance, ask the issue creator or a maintainer
in the `GitHub issue <https://github.com/canonical/sphinx-stack/issues>`__.
