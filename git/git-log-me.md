# Find my own commits

The `--author` flag for `git log` lets you filter commits by a specific person. It matches the entire author line so you can search for the name or email address: `git log --author Alice` and `git log --author bob@example.com` both work. From the man page:

```
--author=<pattern>, --committer=<pattern>
    Limit the commits output to ones with author/committer header lines that match the <pattern> regular
    expression. With more than one --author=<pattern>, commits whose author matches any of the <pattern> are
    chosen (similarly for multiple --committer=<pattern>).
```

Of course, being egocentric, the author I am most interested in is myself. I made this alias to find my own commits:

```sh
alias glm="git log --author=$(git config user.name)"
```

I use this all the time to jog my memory ("What have I merged recently?" `glm origin/main`) and to dig up changes I remember making, by combining with other filters:

- `git log --grep` to search commit messages
- `git log -S` ([the "pickaxe"](https://git-scm.com/docs/git-log#Documentation/git-log.txt--Sstring)) to searches for commits that added or removed a fragment of code, ignoring when it simply moved around in a file.
- `git log [file path]` to find places I changed a certain file or directory

You can use `--author` multiple times with OR semantics: `git log --author Alice --author Bob`. It accepts regular expressions when combined with `-E`: `git log -E --author "Alice|Bob|Eve"` finds commits from all three people.
