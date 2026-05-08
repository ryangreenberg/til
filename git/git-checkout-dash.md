# `git checkout -` for last branch

I move back and forth between two branches somewhat often in git ("The tests are failing on my branch--are they failing on main?" (They were not)).

I found `git checkout -` which is great for this. It checks out the branch or ref that you were on most recently. It reminds me of `cd -`, which switches to your most recent directory. Running this multiple times will bounce you back and forth between the branches.

```
$ git checkout -b git_commands
Switched to a new branch 'git_commands'

$ git checkout -
Switched to branch 'main'
Your branch is up to date with 'origin/main'.

$ git checkout -
Switched to branch 'git_commands'
```

I grew up in the era where `git checkout` was the command you used for five different things. [Git 2.23 introduced `git switch`](https://github.blog/open-source/git/highlights-from-git-2-23/#experimental-alternatives-for-git-checkout) explicitly for switching branches. `git switch -` works the same way.
