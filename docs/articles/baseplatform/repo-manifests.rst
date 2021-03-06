:orphan:

Manifests
=========

Repo [#repotool]_ is a tool for using several git repositories in the same
build. It features a `manifest` format using XML where one defines the git
repositories and revisions one wants to use. It is possible to include manifests
in other manifests.

Repo feature overview
---------------------
* Download several git repositories simultaneously.
* Use different revisions for different repositories.

    * Revisions can be branches, tags or commit hashes.

* Combine manifests together.

    * Create more specialized manifests from a common base.

* Default settings for revision etc.
* Separate remotes and actual repositories.

Using repo with yocto
---------------------
When doing yocto builds, the repo tool is useful to have the various yocto
layers synced towards the same yocto release (typically, a poky release).

Revisions in repo manifests can be branch names, tags, or commit
hashes. Commit hashes are a good solution for reproducing builds, but
in order to be able to follow layers as they move, branch names are
preferred.

If one wants to support several yocto releases, it is possible to use different
branches in the manifest repository, where the manifest in each branch would then
point to the corresponding branch (or a commit on that branch) in the layers
used.

Example
-------
Below is an example of a manifest from PELUX [#pelux-manifests]_ showcasing the
format.

.. literalinclude:: snippets/manifest.xml
    :language: xml

Note the ``path`` setting, which tells repo where to put the checked out git repo,
relative to the current working directory.

Using the repo tool
-------------------
In the yocto setting, two repo commands are commonly used: ``repo
init`` and ``repo sync``. The first sets up repo in the current
working directory, and the second downloads all the git repositories
in the selected manifest.

.. code-block:: bash

    $ mkdir <some-directory>
    $ cd <some-directory>
    $ repo init -u <manifest repo URL> -m <manifest> -b <branch>
    $ repo sync

The curious reader can poke around in ``.repo`` after running repo sync, where
one can also change what manifest being used by changing the symlink for
``manifest.xml``.

.. [#repotool] https://source.android.com/source/using-repo
.. [#pelux-manifests] https://github.com/Pelagicore/pelux-manifests
