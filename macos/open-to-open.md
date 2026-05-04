# Use `open` to open in other applications

The `open` command on the macOS CLI lets you open things in another application. By default, it uses the application that corresponds to the file type as determined by LaunchServices. You can specify a different one with `-a`:

```sh
open index.html # opens in Chrome, my default application for *.html
open -a "Sublime Text" index.html
open -a "Safari" index.html
```

One great use for this is with directories. The Finder is the default application for them, which means you can easily open your current directory in the Finder with `open .` (some things are easier in a GUI!) .

![Animated screenshot of running `open .` in Terminal, which opens the current directory in a new Finder window.](terminal-to-finder.gif)

If you need to go the other direction (Finder to terminal), you can drag a file into the terminal window or copy/paste it.

![Animated screenshot of dragging a file from Finder into Terminal, then copy-pasting another file from Finder into Terminal. Both produce the file's full path at the prompt.](drag-copy-paste-finder-to-terminal.gif)

