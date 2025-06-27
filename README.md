![iTermoxyl](banner.png)

# iTermoxyl

iTermoxyl is a command-line tool to automatically open multiple ssh connections in [iTerm2](https://iterm2.com/) by finding hosts from your SSH environment or by providing an explicit list.

## About this fork

This is a modernized and actively maintained fork of the original **[iTermoxyl by Lucio Paiva](https://github.com/luciopaiva/itermoxyl)**. We extend our sincere thanks and credit to Lucio for creating this wonderfully simple and effective tool.

The original project appears to be unmaintained, having not seen updates for several years. This fork aims to carry the torch forward. It is more than just a simple update; it's a significant evolution of the original concept, which justifies the major version bump to **2.x.x**.

Key improvements include:
* A complete port to **Python 3**, ensuring compatibility with modern macOS systems where Python 2.7 is no longer standard.
* A redesigned Command-Line Interface (CLI) with two distinct, powerful modes: **`--find`** for regex searching and **`--list`** for explicit hosts.
* A simplified and more robust host discovery mechanism that now includes `~/.ssh/known_hosts`.

iTermoxyl is designed to be simple to use, with minimal interaction needed to get it running. Once ssh connections are established, use iTerm2's broadcast input feature to send commands to all machines at once (`Shell -> Broadcast input` or simply `cmd+alt+i`).

## Features

* **Zero Configuration:** Magically learns about existing hosts by reading from `~/.ssh/config` and `~/.ssh/known_hosts`.
* **No YAML required:** Unlike tools like `itermocil` or `i2cssh`, you don't need to manually create descriptions of your environments.
* **Two Powerful Modes:**
    * **Find Mode:** Search for hosts using one or more regular expression patterns.
    * **List Mode:** Connect to an explicit, space-separated list of hosts.
* **Loose Matching:** In find mode, patterns are loosely joined, allowing you to quickly filter for hosts without typing full names.

## How to install

`cd` to a directory in your `$PATH` and run:

    curl -O https://raw.githubusercontent.com/luciopaiva/itermoxyl/master/itermoxyl
    chmod u+x itermoxyl

The script requires **Python 3**.

## How to use

iTermoxyl works in one of two modes: **find** or **list**.

Consider your `~/.ssh/config` and `~/.ssh/known_hosts` files contain entries for the following hosts:

* `foo-1`
* `foo-2`
* `foo-3`
* `bar-1`
* `bar-2`
* `server-1-a`
* `server-1-b`
* `server-2-a`
* `server-2-b`

### Find mode (`-f`, `--find`)

Find mode searches all discovered hosts using regular expressions.

1.  **Open all `foo` machines:**

        itermoxyl -f foo

2.  **Open `server-1-a` and `server-2-a`:**
    The terms `server` and `a` are joined into a loose regex, finding hosts that contain both strings.

        itermoxyl -f server a

3.  **Open all `bar` machines using a more specific regex:**

        itermoxyl -f 'bar-\d'

### List mode (`-l`, `--list`)

List mode connects to the exact hosts you provide, in the order you provide them.

1.  **Open `foo-1`, `foo-3`, and `bar-2`:**

        itermoxyl -l foo-1 foo-3 bar-2

## Find mode logic

When you provide multiple patterns in find mode, they are joined together to form a single "loose" regular expression. For example, the command:

    itermoxyl -f TERM_A TERM_B

...is translated into the following regular expression:

    (?:TERM_A).*?(?:TERM_B)

This allows you to quickly filter hosts that match a sequence of patterns, regardless of what comes between them.

The script will always show all hosts matching your query and ask for confirmation before connecting to them (unless you use the `-r` or `--run` flag).