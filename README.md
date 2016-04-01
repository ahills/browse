# browse

This script is a wrapper around [tabbed](http://tools.suckless.org/tabbed/) and
[vimb](https://github.com/fanglingsu/vimb/) which manages persistence, cache,
and initial URL. The simplest usage is to give the desired starting URL as the
first argument and options for the browser following. More configuration is
possible through environment variables and a configuration directory, described
below.

In most configurations, the `browse` command will create a temporary directory
in which the session's persistent files, cache, and downloads will be stored,
and which will be erased on exit.

It is intended for this script to be POSIX-compliant, or at least run on many
different shells. It has only been tested on Linux with `dash` as `/bin/sh`, so
please report problems and other shells and operating systems with which it
does or doesn't work.

## Installation

Just copy `browse` to somewhere on your path. Create links to the executable to
invoke configured behavior (see below).

## Environment variables

| Variable Name  | Feature                                              |
| -------------- | ---------------------------------------------------- |
| `$BROWSE_HOME` | URL to browse to, if not given on the command line   |
| `$BROWSE_DIR`  | Working directory, rather than a temporary directory |
| `$BROWSE_CFG`  | Configuration directory, instead of `~/.browse`      |

## Command line flags

`-P` prints the names of persisted files and exits.

`-V` prints the version and exits.

## Configuration

Configuration goes in `$BROWSE_CFG`, or `~/.browse` by default. Profiles are
invoked by launching `browse` with the profile name as the program name. The
easiest way to accomplish this is usually to create symbolic links. A profile's
configuration file is either a file named after the profile with `.cfg`
appended, or a file named `config` in a directory named after the profile. For
example, a `search` profile's configuration may be `$BROWSE_CFG/search.cfg` or
`$BROWSE_CFG/search/config`. (The default profile, of course, would be called
`browse`.) The configuration file uses a simple `key = value` format.
Whitespace around the `=` will be ignored, but the key must not be preceded by
whitespace.

The `url` key determines the initial URL for the session when not provided on
the command line. For example, the `search` profile may contain the following:
```
search = https://duckduckgo.com
```
The other keys are names of files that `vimb` stores in its profile directory
and copies into the working directory when launching a session. A complete list
of files is obtained with `browse -P`. The value of the key may be `all`, to
copy the file verbatim to the profile directory after quitting; or it may be a
regular expression used to filter the file. For example, to save only cookies
from GitHub, the configuration file would contain the following:
```
cookies = /github\.com/
```
Or, to save just the cookies needed to stay logged into GMail:
```
cookies = /^\(mail\|accounts\|#HttpOnly_[^.]*\)\?\.\(google\|youtube\)\.com/
```
Any lines from the `cookies` file matching that pattern (using `sed`) will be
stored in the `cookies` file of the profile directory after quitting. The
persistence rules are not evaluated until quitting, so they may be modified
while running a session of a profile, as long as the configuration file existed
when the session was launched.

## Change log

- v3:
  - fix `-V` flag semantics
  - change configuration format from save file to save directory
  - add changelog
- v2:
  - add `-V` flag
  - replace surf with vimb
- v1: add surf patch for command line cache

## Planned features

- Specify base directory & temporary directory template
- Support other browsers (e.g., [surf](http://surf.suckless.org/)), if it can be done sensibly

Copyright © 2015-2016 Andrew Hills. See LICENSE for details.
