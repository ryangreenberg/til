# `tldr` for simpler, example-driven help

If you type `help` in most shells what you get is not very helpful:

```
$ help
zsh: command not found: help
```

bash is slightly better; [fish](https://fishshell.com/) is actually good.

Eventually you learn that help is obtained from the _manual_ by running `man`. After you chuckle at the combinations this permits--`man less`, `man cat`, `man man`, `man date`--you confront a second reality which is that the manual can be overwhelming to the point of not being helpful.

```
$ man ls

NAME
     ls – list directory contents

SYNOPSIS
     ls [-@ABCFGHILOPRSTUWabcdefghiklmnopqrstuvwxy1%,] [--color=when] [-D format] [file ...]

[...]
```

Enter [tldr pages](https://tldr.sh/), "collaborative cheatsheets for console commands," which provide a few examples of common usage. Here's the page for `ls`:

```
$ tldr ls

ls

List directory contents.
More information: <https://www.gnu.org/software/coreutils/manual/html_node/ls-invocation.html>.

- List files one per line:
    ls -1

- List all files, including hidden files:
    ls [-a|--all]

- List files with a trailing symbol to indicate file type (directory/, symbolic_link@, executable*, ...):
    ls [-F|--classify]

- List all files in [l]ong format (permissions, ownership, size, and modification date):
    ls [-la|-l --all]

- List files in [l]ong format with size displayed using human-readable units (KiB, MiB, GiB):
    ls [-lh|-l --human-readable]

- List files in [l]ong format, sorted by [S]ize (descending) recursively:
    ls [-lSR|-lS --recursive]

[...]
```

You can install `tldr` with your package manager (e.g. `brew install tldr`) or read at https://tldr.inbrowser.app. It's easy to contribute new commands and examples; I always forget how to use `units` so I [added examples to tldr](https://github.com/tldr-pages/tldr/pull/2038).
