ANTIFOLD - selectively prevent Stow folding
===========================================

This repository is my simple but slightly ugly workaround for the fact
that Stow does not allow
[tree folding](https://www.gnu.org/software/stow/manual/html_node/Installing-Packages.html)
to be selectively prevented for certain directories.

Preventing folding is desirable in situations where only one Stow
package is installing to a directory (i.e. symlinking from it), but
where it is expected that other software or manual processes will add
files to that directory in the future, and it is desired for those new
files to land in that absolute path, not within a Stow package which
happens to currently "own" that path via a folding symlink.

This repository achieves selective anti-folding by taking advantage of
the fact that if two Stow packages try to install into the same
directory, Stow will ensure that directory is unfolded.  Therefore it
simply places an empty `.no-stow-folding` file in each directory where
I don't want folding to be used.  Then I can simply stow this package
and it will achieve the desired effect.

Notice that while Stow would achieve the desired effect even with
empty directories, git does not track directories, so the
`.no-stow-folding` files are required in order to ensure the presence
of these directories when checking out this repository via `git
clone`.

Weakness
--------

This repository suffers from the same weakness as any other other:
when it is the only Stow package "owning" a directory, tree folding
will point to the package tree for this repository.  So files could
accidentally end up in this package tree, although this is arguably
a better situation since at least then they all end up in the same
place, and are clearly visible by running `git status` on this repository.

However this risk can be mitigated by stowing this package with the
`--no-folding` option introduced in Stow 2.2.0.  With earlier versions
it is instead recommended to stow all packages which share directories
with this repository as soon as possible.


`unfold` utility
------------------

[`bin/unfold`](bin/unfold) is a script which makes it easy to add new
paths to this collection of directories for which folding is
prevented.  Since it will get stowed to `bin/` under Stow's target directory,
if you have that directory on your `$PATH` you can run it as a command
without specifying the full path.  For example, run:

    unfold -h

or

    unfold --help

to see usage help.
