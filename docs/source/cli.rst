===============
nbdime commands
===============

nbdime provides the following CLI commands::

    nbshow
    nbdiff
    nbdiff-web
    nbmerge
    nbmerge-web

Pass ``--help`` to each command to see help text for the command's usage.

Additional commands are available for :ref:`git-integration`.

nbshow
======

:command:`nbshow` gives you a nice, terminal-optimized summary view of a notebook.
You can use it to quickly peek at notebooks without launching the full notebook web application.

.. TODO:: screenshot

Diffing
=======

nbdime offers two commands for viewing the diff between two notebooks:

- :command:`nbdiff` for command-line diffing
- :command:`nbdiff-web` for rich web-based diffing of notebooks

.. seealso::

    For more technical details on how nbdime compares notebooks, see :doc:`diffing`.

nbdiff
------

:command:`nbdiff` does a terminal-optimized rendering of notebook diffs.
Pass it the two notebooks you would like to compare,
and it returns a nice, readable presentation of the changes in the notebook.

.. TODO:: screenshot of console diff


nbdiff-web
----------

Like :command:`nbdiff`, :command:`nbdiff-web` compares two notebooks.
Instead of a terminal rendering, :command:`nbdiff-web` opens a web browser,
compares the two notebooks, and displays the rich rendered diff of images and
other outputs.

.. TODO:: screenshot of web diff

Merging
=======

Merging notebook changes and dealing with merge conflicts are important parts
of a development workflow. With notebooks, merging changes is a non-trivial
technical task. Traditional, line-based tools can produce invalid notebooks
that you have to fix by hand,
which is no fun at all, or can risk unintended data loss.

nbdime provides some improved tools for merging notebooks,
taking into account knowledge of the notebook file format
to ensure that a valid notebook is always produced.
Further, by understanding details of the notebook format,
nbdime can automatically resolve conflicts on generated fields.

.. seealso::

    For more details on how nbdime merges notebooks, see :doc:`merging`.

nbmerge
-------

:command:`nbmerge` merges two notebooks with a common parent.
If there are conflicts, they are stored in metadata of the destination file.
:command:`nbmerge` will exit with nonzero status if there are any unresolved
conflicts.

:command:`nbmerge` writes the output to ``stdout`` by default,
so you can use pipes to send the result to a file,
or the ``-o`` argument to specify an output file in which to save the merged
notebook.

Because there are several categories of data in a notebook (such as input, output, and metadata),
nbmerge has several ways to deal with conflicts,
and can take different actions based on the type of data with the conflict.

.. important::

    Conflict-resolution in nbmerge is under active development
    and is subject to change.

The ``-m, --merge-strategy`` option lets you select a global strategy to use.
The following options are currently implemented::

.. TODO:: make sure these are accurate, work:

inline
    **This is the default.**
    Conflicts in input and output are recorded with conflict markers on input and output,
    Inline output merged with conflict-markers.
    This gives you a valid notebook that you can open in your usual notebook editor
    and resolve conflicts, just like you might for a regular Python script.

use-base
    When a conflict is seen, pick the version in the base notebook.

use-local
    When a conflict is seen, pick the version in the local notebook.

use-remote
    When a conflict is seen, pick the version in the remote notebook.

mergetool
    Used by the merge tool, not for human consumption.

fail
    Don't try to resolve conflicts, just exit

clear
    .. TODO:: Will we have this?

To use nbmerge, pass it three notebooks:

- ``base``: the base, common parent notebook
- ``local``: your local changes to base
- ``remote``: other changes to base that you want to merge with yours

For example::

    nbmerge base.ipynb local.ipynb remote.ipynb > merged.ipynb

.. TODO:: screenshot of auto merge


nbmerge-web
-----------

:command:`nbmerge-web` is just like :command:`nbmerge` above,
but instead of automatically resolving or failing on conflicts,
a webapp for manually resolving conflicts is displayed::

    nbmerge-web base.ipynb local.ipynb remote.ipynb -o merged.ipynb

.. TODO:: screenshot of merge tool
