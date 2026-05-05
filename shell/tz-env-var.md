# Change the timezone for a command with `TZ`

The `TZ` environment variable changes the selected timezone for the duration of a command (or at least commands that are aware of `TZ`). It's handy to use with the `date` command:

```
$ date
Tue May  5 09:34:58 EDT 2026

$ TZ=UTC date
Tue May  5 13:34:58 UTC 2026
```

I actually added `alias utc="TZ=UTC date"` to my shell so that I can double-check the time when I'm looking at logs with UTC times.
