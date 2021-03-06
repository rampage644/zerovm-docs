
Contributing to ZeroVM
======================

We love contributors! We want to make ZeroVM a successful platform
with a great community. For this to happen, we want to hear how you
use ZeroVM and work with you to make it better for your use cases!

To make contributing easy, we've documented the process we follow
below. Please read through it so you know how we like to work.

.. admonition:: tl;dr

   We work like most other open source projects on GitHub. Make your
   changes as small commits and submit them as a pull request. We'll
   be in touch and work with you to get the changes into the main
   tree as fast as possible!


Pull Requests
-------------

Please use GitHub pull requests when contributing code. We will try to
quickly review and merge your code.

When you submit a pull request, a review cycle is started. The typical
process looks like this:

* Jenkins makes a checkout of your branch and runs tests. This will
  tell you if something simple broke (so you avoid breaking the
  build).

* Someone will take a look at the code and give you feedback. The
  feedback can be about the code or the design.

* Based on the feedback the pull request is either merged directly or
  some changes may be needed.

* If changes are needed, you should update the commits and push again.
  You will need to use ``git push -f`` this time. The pull request
  will automatically update and this starts a new review cycle.

Feel free to remind us on :ref:`IRC or the mailinglist <contact-us>`
if it takes more than a couple of days for us to respond to a pull
request. The IRC channel is also a great place to get some real-time
feedback on a pull request.


Rebasing
""""""""

You should make sure to rebase your code to the head of ``master``
before submitting the pull request. That ensures that there will be no
merge conflicts initially.

If the pull request is not merged quickly, it can become outdated.
This happens when other pull requests are merge in front of it and
cause merge conflicts with the code you changed. You should then
rebase the code again. See :ref:`git-rebase` for detailed
instructions.

It will generally be appreciated if you keep your code up to date in
this way -- even when there are no conflicts. Rebasing regularly
simplifies the history.

Ordering Commits
""""""""""""""""

Your commits should tell a story to the reviewer. You should therefore
make sure to order them so they can be read like that. This means that
you should:

* Begin with the easy and non-controversial changes first. Consider
  putting these changes into their own pull request so you can get
  them out of the way.

* Make sure each commit depends on the past only, not the future. It
  is very confusing for a reviewer to read commit A if it calls a
  function introduced later in commit B or C.

See :ref:`git-reorder` for details.



Fixing Your Own Mistakes
""""""""""""""""""""""""

Nobody writes perfect code in the first try, so it often happens that
you discover a mistake in an earlier commit. Please pay attention to
this situation. When a reviewer sees a bug or some bad design in
commit A, he will likely stop and begin write a comment about it. Even
if you fix the problem yourself in commit B or C, it would have been
much more helpful if you had avoided introducing it at all.

Please see :ref:`git-amend` for details on how to fix such mistakes
with Git.


Writing Good Commits
--------------------

We have collected some guidelines on how to create good commits --
commits that are easy to review and understand later.

.. admonition:: Further reading

   * `What's in a Good Commit?`_


.. _one-change-per-commit:

One Change Per Commit
"""""""""""""""""""""

Please take care to ensure that each commit only deals with one
logical change. Make your commits small -- it is much easier to review
five small commits than one massive commit. So try to err on the side
of making too many commits. If you decide to split a commit, then see
:ref:`git-split` for help.

.. hint::

   It is much easier to combine commits than to split them: use ``git
   rebase -i`` and change ``pick`` into ``squash`` for the commits
   that should be combined into one. Done!

The question is then what one "logical change" is. Here you will have
to use your best judgment. Some examples of what *not* to do:

* Do not fix a bug *and* move a function to a more logical location.

* Do not fix a bug *and* fix an unrelated typo.

* Do not fix a bug *and* reformat the code for readability.

While each change might be good, please use two commits in these
cases. To put it in another way, different classes of changes should
not be mixed in the same commit:

* Bug fixes.

* New features.

* Moving code around (refactoring).

* Whitespace and formatting changes.

* Style changes.

* Unrelated typo fixes.

Your goal should always be to make sure that trivial commits stay
trivial: a typo fix is trivial to review so you should make it trivial
for the reviewer to accept the commit.

In general, you should *stop* when you see yourself include the word
"and" in a commit message. If you feel the need to make a bullet list,
then you are likely including too much in the commit. In any case, you
should work with your reviewer. Try to follow his advice or explain to
him why the changes really belong together.


Commit Messages
"""""""""""""""

Writing good commit messages is an art. You want the message to be
concise and to clearly explain the proposed change. Please follow this
format::

  topic: short summary line (less than 50 characters)

  After a blank line, you can include a bigger description of the
  changes. Wrap the text at about 72 characters -- this makes it
  nicely centered when viewed in "git log".

Include relevant keywords for the GitHub bug tracker. Adding "fixes
#123" to the commit message will make GitHub close issue #123 when the
commit is merged into the main repository.

When explaining the change remember to focus on two things:

* Explain *what* the change is. The diff technically shows this, so
  you should describe the change at a more high level. An excellent
  way to do this is to show the output before and after the change.
  Reviewers often have limited context so this is very helpful.

* Explain *why* you make the change. This is extremely important and
  the part most often left out. The commit message is often all that
  is left of the intent and reasoning behind a change when someone
  looks at it again a year later because they found a bug that seems
  to have been introduced by your change.

  Knowing what you changed is good, but what is really helpful in that
  situation is to know *why* you changed things the way you did. So
  please explain why this solution is the good solution. Explain what
  other solutions you investigated and why they won't work. Doing so
  will save time for the poor programmer who is debugging your code in
  the future.

The second point is the more important point, so please try to put
emphasis on that.


Coding Style
""""""""""""

For Python-based projects, we enforce PEP8_ and Pyflakes_ standards. Checks are
run automatically on each pull request to signal if there is a style violation.


Why make small commits?
-----------------------

Easy to Review
""""""""""""""

As mentioned several times above, small commits are easier to review
than large commits. Trying to understand what a patch that touches 100
lines does is often more than twice as hard as understanding two
changes that each touch 50 lines.


Makes the History Useful
""""""""""""""""""""""""

When each commit is an atomic step that takes the code from one
working state to another, the project history becomes a very useful
tool when looking for bugs. The ``git bisect`` command helps you here.
The command runs a binary search on the commit history to help you
find the commit that introduced a particular bug.

When ``git bisect`` finds the first commits that triggers the bug, the
real debugging can begin. If the commit it finds is small and does
just one thing, then it is normally easy to understand why the
problem.


Enables Revert
""""""""""""""

Sometimes it is decided that the change in the commit should be undone
-- maybe the commit was found using ``git bisect`` and it is
determined that it introduced a regression or a bug.

The ``git revert`` command can then be used to undo the commit. It
will apply an inverse patch to the repository. Since the inverse patch
is based on a commit, this only works if the entire commit should be
undone. If there are good, but unrelated changes in the commit, it
becomes more work to revert it.


Selected Independently
""""""""""""""""""""""

A reviewer might tell you that some part of your pull request is
great, while another part is not so great. If you have already split
your work into small logical units, then it is easy for you to drop a
commit that is not needed. Alternatively, the reviewer can easily use
``git cherrypick`` to select the commits he like and ignore the rest.

.. admonition:: Further reading

   * `Debugging with Git: Binary Search`_
   * `5 Reasons for Keeping Your Git Commits as Small as You Can`_
   * `git help bisect`_
   * `git help revert`_
   * `git help cherrypick`_


Branches
--------

We follow a workflow similar to Git_ where we maintain a branch called
``stable`` for bugfix releases. This branch is continuously merged
into ``master`` during normal development -- this ensures that
bugfixes are incorporated with the newest features.

As ASCII art it looks like this::

   master: ----- o --- o --- o --- o
                /           /
   stable: --- o --------- o

All releases are made from the ``stable`` branch. We :ref:`release
bugfixes <bugfix-releases>` once per month by tagging and releasing
whatever code we have in the ``stable`` branch. We make a
:ref:`feature release <feature-releases>` every three months. These
are also made from ``stable``, but the are preceded by a merge of
``master`` into ``stable``. It looks like this::

   master: ----- o --- o --- o --- o
                /           /       \
   stable: --- o --------- o ------- o

This merge brings all the new features developed since the last
release onto the ``stable`` branch.


Git Tips and Tricks
-------------------

We have collected some tips and tricks for solving common problems
when using Git.

.. admonition:: Further reading

   * `Pro Git`_

.. _git-rebase:

Rebasing Stale Pull Requests
""""""""""""""""""""""""""""

When other pull requests are merged infront of your pull request,
conflicts can occur. You as a contributor, is often the one who can
solve these conflicts best -- rebasing the code to the head of
``master`` will ensure this.

You rebase your pull request with::

   $ git checkout master
   $ git pull upstream master
   $ git rebase master your-branch

This will first make sure that your ``master`` is up to date with
regards to the upstream repository. The upstream should be the
repository you forked on GitHub (the repository living under
``github.com/zerovm/``).

Now push the branch to GitHub again with ``git push -f origin
your-branch``. The pull request will automatically update.

.. admonition:: Further reading

   * `Branching - Rebasing`_
   * `git help rebase`_


.. _git-reorder:

Reordering Commits
""""""""""""""""""

Modern distributed version control systems like Git gives you the
tools to reorder commits. Using the interactive mode of ``git
rebase``, you can easily reorder commits. While having your feature
branch checked out, you run::

   $ git rebase -i master

This will open your editor with a file that shown an "execution plan"
for the interactive rebase. Each line represents a commit and by
reordering the lines you instruct Git to reorder the corresponding
commits.

After you save the file and close the editor, Git will begin
reordering commits. If conflicts occur, you should use ``git
mergetool`` to solve them. This starts your three-way merge tool which
should let you figure out how to best solve the conflicts.

.. admonition:: Further reading

   * `Rewriting History: Reordering Commits`_
   * `git help rebase`_


.. _git-amend:

Amending Commits
""""""""""""""""

When you want to change a commit to fix a bug, you *amend* it in the
Git terminology. If the fix concerns the last commit you made, then
simply use ``git commit --amend`` to redo the commit. You can use
``git commit --amend`` as many times you want to fine-tune a commit.

If you want to fix something that committed further in the past, you
should instead follow this procedure:

1. Commit the fix by itself. Use ``git add -p`` to stage just the fix
   by itself if there are other changes in the same file.

2. Use ``git rebase -i`` to reorder the commits so that the bugfix is
   right after the commit that introduced the bug. In addition to
   reordering the commits, change the action from ``pick`` to
   ``fixup``.

This will do the same as if you had used ``git commit --amend`` to fix
the bug. With these steps, you can easily fix past mistakes.

.. admonition:: Further reading

   * `Interactive Staging: Staging Patches`_
   * `git help commit`_
   * `git help add`_


.. _git-split:

Splitting Commits
"""""""""""""""""

The general advice is to make :ref:`small commits that do one thing
<one-change-per-commit>`. Even when you try to make small commits at
commit-time, you will inevitably end up with some commits that you
later decide that you want to split.

We will distinguish between two cases: if the commit you want to split
is the previous commit or a commit further back in the history.

* If you want to split the last commit, you run::

     $ git reset HEAD^
     $ git add foo.c
     $ git commit -m 'foo: fixed #123'
     $ git add bar.c
     $ git commit -m 'bar: fixed typo'

  The important command is ``git reset``, which will undo the commit.
  The working tree is not touched (so your modifications are still
  present), but the branch is rewinded and the index is reset. This
  means that your modifications show up again in ``git diff``, for
  example.

  As shown, you can now commit the changes in as many commits as you
  like. Use ``git add -p`` to interactively add part of a file to be
  committed, for example. You will find the previous commit message as
  ``.git/COMMIT_EDITMSG``, so you can refer to it when making new
  commits.

* If you want to split an earlier commit ``X``, you run::

     $ git rebase -i X^

  In the line for ``X``, change ``pick`` to ``edit`` (or just ``e``),
  save the file, and close the editor. Git will then update to ``X``
  to allow you to edit the commit. To actually split the commit, you
  will now use the procedure described above for splitting the last
  commit. That is, you run::

     $ git reset HEAD^

  to undo the commit. Then commit the files in as many small commits
  as you like and finally run::

     $ git rebase --continue

  to finish the rebase operation.

.. admonition:: Further reading

   * `Rewriting History: Splitting a Commit`_
   * `git help reset`_
   * `git help rebase`_


.. _flake8: http://flake8.readthedocs.org/
.. _pep8: http://legacy.python.org/dev/peps/pep-0008/
.. _pyflakes: https://launchpad.net/pyflakes
.. _git: https://www.kernel.org/pub/software/scm/git/docs/gitworkflows.html

.. _`what's in a good commit?`:
   http://dev.solita.fi/2013/07/04/whats-in-a-good-commit.html

.. _5 Reasons for Keeping Your Git Commits as Small as You Can:
    http://blog.crealytics.com/2010/07/09/
    5-reasons-keeping-git-commits-small-admin/

.. _pro git: http://git-scm.com/book

.. _`interactive staging: staging patches`:
   http://git-scm.com/book/en/Git-Tools-Interactive-Staging#Staging-Patches

.. _branching - rebasing:
   http://git-scm.com/book/en/Git-Branching-Rebasing

.. _rewriting history:
   http://git-scm.com/book/en/Git-Tools-Rewriting-History

.. _`rewriting history: reordering commits`:
   http://git-scm.com/book/en/Git-Tools-Rewriting-History#Reordering-Commits

.. _`rewriting history: splitting a commit`:
   http://git-scm.com/book/en/Git-Tools-Rewriting-History#Splitting-a-Commit

.. _`Debugging with Git: Binary Search`:
   http://git-scm.com/book/en/Git-Tools-Debugging-with-Git#Binary-Search

.. _git help add: http://git-scm.com/docs/git-add
.. _git help commit: http://git-scm.com/docs/git-commit
.. _git help reset: http://git-scm.com/docs/git-reset
.. _git help rebase: http://git-scm.com/docs/git-rebase

.. _git help bisect: http://git-scm.com/docs/git-bisect
.. _git help cherrypick: http://git-scm.com/docs/git-cherrypick
.. _git help revert: http://git-scm.com/docs/git-revert
