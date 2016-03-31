# browse

This script is a wrapper around [tabbed](http://tools.suckless.org/tabbed/) and
[vimb](https://github.com/fanglingsu/vimb/) which manages cookies, cache, and
initial URL. The simplest usage is to give the desired starting URL as the first
argument and options for the browser following. More configuration is possible
through environment variables and a configuration directory, described below.

In most configurations, the `browse` command will create a temporary directory
in which the session's cookies, cache, and downloads will be stored, and which
will be erased on exit.

It is intended for this script to be POSIX-compliant, or at least run on many
different shells. It has only been tested on Linux with `dash` as `/bin/sh`, so
please report problems and other shells and operating systems with which it
does or doesn't work.

## Installation

Just copy `browse` to somewhere on your path. Create links to the executable to
invoke configured behavior (see below).

## Environment Variables

| Variable Name  | Feature                                              |
| -------------- | ---------------------------------------------------- |
| `$BROWSE_HOME` | URL to browse to, if not given on the command line   |
| `$BROWSE_DIR`  | Working directory, rather than a temporary directory |
| `$BROWSE_CFG`  | Configuration directory, instead of `~/.browse`      |

## Configuration

Configuration goes in `$BROWSE_CFG`, or `~/.browse` by default. A file named
`urls` contains initial URLs for each invocation in a simple `key = value`
format, where the `key` is the name of the executable and the `value` is the
URL. For example, if your `search` command is a symbolic link to the `browse`
command, your `~/.browse/urls` might contain this line:
```
search = https://duckduckgo.com
```
A file named `save` contains search patterns for the cookie file in case some
cookies should be saved for the next session, again in `key = value` format.
For example, if your `github` command is a symbolic link to the `browse`
command, your `~/.browse/save` might contain this line:
```
github = /github\.com/
```
This will save any lines from the cookie file which match that pattern for the
next session. The cookies are saved in the `$BROWSE_CFG/cookies` directory as
the name of the command with a `.txt` extension, e.g.
`~/.browse/cookies/github.txt`. Note that the cookie save pattern is not read
until needed, so it can be modified while `browse` is running, so long as a
pattern exists.

Copyright © 2015-2016 Andrew Hills. See LICENSE for details.
