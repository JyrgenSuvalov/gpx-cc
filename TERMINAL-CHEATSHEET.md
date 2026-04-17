# Terminal Basics Cheat Sheet

A quick reference for navigating your Mac's terminal. Keep this open during the workshop.

---

## Where am I?

When you open a terminal, you're always "standing" inside some folder on your computer. The terminal just doesn't show you a graphical window of that folder — you interact with it by typing commands.

`pwd` — **print working directory.** Shows you which folder you're currently in.

```
$ pwd
/Users/yourname/workshop/my-project
```

---

## Looking around

`ls` — **list.** Shows the files and folders in your current directory.

```
$ ls
ANALYST.md    ARCHITECT.md    openspec/    src/
```

`ls -la` — list with details. Shows hidden files (ones starting with `.`), file sizes, and modification dates. Useful when you need to see things like `.claude/` or `.git/`.

```
$ ls -la
drwxr-xr-x   4 you  staff   128 Apr 17 10:30 .claude
drwxr-xr-x  12 you  staff   384 Apr 17 10:25 .git
-rw-r--r--   1 you  staff  1240 Apr 17 09:15 ANALYST.md
drwxr-xr-x   5 you  staff   160 Apr 17 10:30 openspec
```

---

## Moving around

`cd` — **change directory.** Moves you into a different folder.

```
$ cd my-project          # go into the "my-project" folder
$ cd openspec/changes    # go deeper, into openspec/changes
$ cd ..                  # go up one level (back to the parent folder)
$ cd ../..               # go up two levels
$ cd ~                   # go to your home folder (/Users/yourname)
$ cd ~/workshop          # go directly to a specific path from anywhere
```

Think of `cd` as double-clicking a folder in Finder, and `cd ..` as pressing the back button.

---

## Creating things

`mkdir` — **make directory.** Creates a new folder.

```
$ mkdir my-project              # create a folder called "my-project"
$ mkdir -p .claude/agents       # create nested folders (the -p flag creates
                                # parent folders if they don't exist yet)
```

`touch` — creates an empty file, or updates the timestamp of an existing one.

```
$ touch DEV.md                  # create an empty file called DEV.md
```

---

## Reading files

`cat` — prints the entire contents of a file to the terminal.

```
$ cat ANALYST.md                # show the full contents of ANALYST.md
```

`head` and `tail` — show just the beginning or end of a file.

```
$ head -20 ANALYST.md           # first 20 lines
$ tail -10 ANALYST.md           # last 10 lines
```

---

## Searching inside files

`grep` — searches for a piece of text inside files. Think of it as `Cmd + F` but for the terminal.

```
$ grep "session" DEV.md                  # find lines containing "session" in DEV.md
$ grep -r "TODO" src/                    # search for "TODO" in all files inside src/
$ grep -i "error" logs.txt              # case-insensitive search (-i)
$ grep -n "import" app.js               # show line numbers (-n)
```

The `-r` flag (recursive) is especially useful — it searches through an entire folder and all its subfolders at once.

---

## Copying, moving, deleting

`cp` — **copy** a file.

```
$ cp ANALYST.md ANALYST-backup.md
```

`mv` — **move** (or rename) a file.

```
$ mv old-name.md new-name.md             # rename
$ mv DEV.md ~/workshop/my-project/       # move to another folder
```

`rm` — **remove** (delete) a file. There is no trash can — this is permanent.

```
$ rm old-file.md                # delete a file
$ rm -r old-folder/             # delete a folder and everything inside it
```

> ⚠️ Be careful with `rm`, especially `rm -r`. There's no undo.

---

## Other handy bits

`clear` — clears the terminal screen. Doesn't delete anything, just gives you a clean view.

`history` — shows your recent commands. Useful if you need to re-run something but forgot the exact syntax.

**Arrow keys ↑ ↓** — press up/down to scroll through your previous commands. Much faster than retyping.

**Tab completion** — start typing a file or folder name and press `Tab`. The terminal will auto-complete it for you. If there are multiple matches, press `Tab` twice to see the options.

`Ctrl + C` — cancel whatever is currently running. If a command hangs or you want to stop something, this is your emergency brake.

`Ctrl + L` — same as `clear`, clears the screen.

---

## Moving your cursor in a command

When you're typing a long command and need to fix a typo or change something at the beginning, don't hold down the left arrow key for ten seconds. These shortcuts jump your cursor around instantly:

| Shortcut | What it does |
|---|---|
| `Ctrl + A` | Jump to the **beginning** of the line |
| `Ctrl + E` | Jump to the **end** of the line |
| `Ctrl + W` | Delete the **word** before the cursor |
| `Ctrl + U` | Delete everything **before** the cursor (clear the whole line) |
| `Ctrl + K` | Delete everything **after** the cursor |
| `Option + ←` | Jump back one word |
| `Option + →` | Jump forward one word |

The `Option` (or `Alt`) arrow shortcuts are the ones you'll use most — they let you hop word by word through a command instead of character by character.

> **💡 Note:** If `Option + arrow` keys type weird characters instead of jumping between words, your terminal might need a setting tweak. In Ghostty, this should work out of the box. If not, check that your macOS input source isn't intercepting the key combo.

---

## Chaining commands

You can run multiple commands in sequence using `&&`. The second command only runs if the first one succeeds:

```
$ mkdir my-project && cd my-project
```

This is the same as typing them separately — just saves a step.

---

## Piping

The pipe symbol `|` takes the output of one command and feeds it as input to another. It's one of the most powerful ideas in the terminal — you can chain simple tools together to do complex things.

```
$ history | grep "git"           # search your command history for anything with "git"
$ ls -la | grep ".md"           # list files, but only show the ones ending in .md
$ cat REQUIREMENTS.md | head -5  # show just the first 5 lines of a file
```

The way to read a pipe is left to right: "run this command, then pass its output to this next command." You can chain as many pipes as you want, though in practice two or three is usually enough.

A common real-world example during the workshop:

```
$ pnpm list -g | grep openspec   # check if openspec is installed globally
```

---

## A note about `sudo`

You'll occasionally see instructions that start with `sudo` (pronounced "soo-doo"). It stands for **superuser do** — it runs a command with administrator privileges, like right-clicking and choosing "Run as Administrator" on Windows.

```
$ sudo some-command
```

It will ask for your Mac login password (the one you use to unlock your computer). When you type it, nothing will appear on screen — no dots, no asterisks. This is normal. Just type your password and press Enter.

**When you might need it:** Installing system-level packages, changing permissions on protected files, or if a command fails with a "permission denied" error.

**When to be cautious:** `sudo` gives a command full access to your system. Don't run `sudo` on commands you don't understand, and don't use it just because something didn't work — the error might be something else entirely. During this workshop, you should rarely (if ever) need `sudo`. If you find yourself reaching for it, ask first.

---

## The terminal isn't scary

It feels unfamiliar at first, but you're really just doing the same things you do in Finder — opening folders, looking at files, creating things, moving things around. The difference is you're typing instructions instead of clicking. Once you get the muscle memory for `cd`, `ls`, and `pwd`, everything else follows naturally.
