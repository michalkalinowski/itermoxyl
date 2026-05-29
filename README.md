![iTermoxyl](banner.png)

# iTermoxyl

iTermoxyl is a command-line tool to automatically open multiple ssh connections in [iTerm2](https://iterm2.com/) by finding hosts from your SSH environment or by providing an explicit list.

## About this fork

This is a modernized and actively maintained fork of the original **[iTermoxyl by Lucio Paiva](https://github.com/luciopaiva/itermoxyl)**. We extend our sincere thanks and credit to Lucio for creating this wonderfully simple and effective tool.

The original project appears to be unmaintained, having not seen updates for several years. This fork aims to carry the torch forward. It is more than just a simple update; it's a significant evolution of the original concept, which justifies the major version bump to **2.x.x**.

Key improvements include:

- A complete port to **Python 3**, ensuring compatibility with modern macOS systems where Python 2.7 is no longer standard.
- A redesigned Command-Line Interface (CLI) with two distinct, powerful modes: **`--find`** for regex searching and **`--list`** for explicit hosts.
- A simplified and more robust host discovery mechanism that now includes `~/.ssh/known_hosts`.

iTermoxyl is designed to be simple to use, with minimal interaction needed to get it running. Once ssh connections are established, use iTerm2's broadcast input feature to send commands to all machines at once (`Shell -> Broadcast input` or simply `cmd+alt+i`).

## How to install

`cd` to a directory in your `$PATH` and run:

    curl -O https://raw.githubusercontent.com/luciopaiva/itermoxyl/master/itermoxyl
    chmod +x itermoxyl

The script requires **Python 3**.

## How to use

iTermoxyl works in one of two modes: **find** or **list**.

### Find mode (`-f`, `--find`)

Searches hosts discovered from `~/.ssh/config` and `~/.ssh/known_hosts` using regex. Multiple terms are matched loosely in sequence — useful for quickly narrowing down a group of machines without typing full names.

Consider your SSH files contain entries for: `foo-1`, `foo-2`, `foo-3`, `bar-1`, `bar-2`, `server-1-a`, `server-1-b`, `server-2-a`, `server-2-b`.

1.  **Open all `foo` machines:**

        itermoxyl -f foo

2.  **Open `server-1-a` and `server-2-a`** (hosts containing both `server` and `a`, in that order):

        itermoxyl -f server a

3.  **Open all `bar` machines using a more specific regex:**

        itermoxyl -f 'bar-\d'

### List mode (`-l`, `--list`)

Connects to the exact hosts you provide, in the order you provide them. No host discovery — you name them explicitly.

1.  **Open `foo-1`, `foo-3`, and `bar-2`:**

        itermoxyl -l foo-1 foo-3 bar-2

## Additional options

| Flag                  | Description                                                                                               |
| --------------------- | --------------------------------------------------------------------------------------------------------- |
| `-C N`, `--columns N` | Fix the number of columns; rows grow as needed. Mutually exclusive with `--rows`.                         |
| `-R N`, `--rows N`    | Fix the number of rows; columns grow as needed. Mutually exclusive with `--columns`.                      |
| `-r`, `--run`         | Skip the confirmation prompt and open panes immediately.                                                  |
| `-d`, `--debug`       | Print the generated AppleScript to stdout and exit without opening any panes. Useful for troubleshooting. |
| `-v`, `--version`     | Print the version and exit.                                                                               |

### Layout

By default, iTermoxyl picks a grid shape automatically based on the number of hosts, aiming for a near-square layout that makes good use of the screen:

| Hosts | Columns × Rows |
| ----- | -------------- |
| 2     | 1 × 2          |
| 3–4   | 2 × 2          |
| 5–6   | 2 × 3          |
| 7     | 2 × 4          |
| 8–9   | 3 × 3          |
| 16    | 4 × 4          |
| 20    | 4 × 5          |

Use `--columns` or `--rows` to override — for example, `--columns 3` on an ultra-wide monitor, or `--rows 1` to open everything side by side.
