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

`unfold`
---------

[`bin/unfold`](bin/unfold) is a script which makes it easy to add new
paths to this collection of directories for which folding is
prevented.  Since it will get stowed to `bin/` under Stow's target directory,
if you have that directory on your `$PATH` you can run it as a command
without specifying the full path.  For example, run:

    unfold -h

or

    unfold --help

to see usage help.
