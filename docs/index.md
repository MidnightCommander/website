# Welcome to Midnight Commander

[![GitHub Tag](https://img.shields.io/github/v/tag/MidnightCommander/mc?label=latest%20release)](https://github.com/MidnightCommander/mc/tags)
[![License](https://img.shields.io/badge/license-GPLv3+-blue)](https://github.com/MidnightCommander/mc)
[![GitHub top language](https://img.shields.io/github/languages/top/MidnightCommander/mc)](https://github.com/MidnightCommander/mc)
[![GitHub Issues or Pull Requests](https://img.shields.io/github/issues/MidnightCommander/mc)](https://github.com/MidnightCommander/mc/issues)
[![GitHub Issues or Pull Requests](https://img.shields.io/github/issues-pr/MidnightCommander/mc)](https://github.com/MidnightCommander/mc/pulls)
[![CI](https://github.com/MidnightCommander/mc/actions/workflows/ci.yml/badge.svg)](https://github.com/MidnightCommander/mc)

GNU Midnight Commander is a visual file manager released under the GNU General Public License and therefore qualifies as Free Software.

It's a feature-rich, full-screen, text-mode application that allows you to copy, move, and delete files and entire directory trees, search for files, and execute commands in the subshell. Internal viewer, editor and diff viewer are included.

Midnight Commander uses versatile text interface libraries such as [ncurses](https://invisible-island.net/ncurses/) or [S-Lang](https://www.jedsoft.org/slang/), which allows it to work on a regular console, inside an X Window terminal, over `ssh` connections, and in all kinds of remote shells.

This site is the new home of Midnight Commander. The main project repository has been moved from [Savannah](https://savannah.gnu.org/projects/mc) to a new Git repository hosted on [GitHub](https://github.com/MidnightCommander/mc).

## Installation

The easiest way to install `mc` is to use your system package manager:

=== "Debian / Ubuntu"

    ```
    # apt-get install mc
    ```

=== "Fedora / Red Hat"

    ```
    # dnf install mc
    ```

=== "FreeBSD"

    ```
    # pkg install mc
    ```

=== "macOS"

    ```
    % brew install midnight-commander
    ```

To compile from source, refer to the [installation instructions]({{ config.repo_url }}blob/master/doc/INSTALL).

## Documentation

The primary way to learn about `mc` is to use the context-sensitive online help available via ++f1++.

We also have extensive manual pages, which are the primary source of official documentation:

=== "mc"

    ```
    $ man mc
    ```

=== "mcedit"

    ```
    $ man mcedit
    ```

=== "mcview"

    ```
    $ man mcview
    ```

=== "mcdiff"

    ```
    $ man mcdiff
    ```

## Contributing & support

* For support, see the [Communication](communication.md) page.
* To contribute to `mc`, proceed to the ["Development" section](source-code.md).
