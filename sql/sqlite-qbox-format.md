# box, qbox modes for `sqlite3`

I love SQLite. It's an amazing piece of software and there are so many cool things you can do with it. It comes with `sqlite3`, a command to work with a database interactively. Out of the box, `sqlite3` formats query results poorly.

For example:

```
sqlite> SELECT * FROM weather LIMIT 5;
2026-01-01|EWR|35|20
2026-01-02|EWR|32|21
2026-01-03|EWR|32|20
2026-01-04|EWR|38|27
2026-01-05|EWR|37|24
```

The output has no headers and the pipes aren't great separators. If you're used to `psql` or `mysql`, this is worse.

You can make things better by running these commands (or putting them in your `~/.sqliterc`):

```
-- Show column headers in results
.header on

-- Show results in MySQL-like columns
.mode table
```

```
sqlite> SELECT * FROM weather LIMIT 5;
+------------+---------+----------+---------+
|    date    | airport | day_high | day_low |
+------------+---------+----------+---------+
| 2026-01-01 | EWR     | 35       | 20      |
| 2026-01-02 | EWR     | 32       | 21      |
| 2026-01-03 | EWR     | 32       | 20      |
| 2026-01-04 | EWR     | 38       | 27      |
| 2026-01-05 | EWR     | 37       | 24      |
+------------+---------+----------+---------+
```

These results are easier to read and similar to what you see in other database CLIs. They can be a bit of a mess with lots of columns or very long values, but that's understandable.

[SQLite 3.53](https://sqlite.org/releaselog/3_53_0.html) was released recently and has some big changes to query result formatting, which are exposed as [improvements in the .mode command](https://sqlite.org/climode.html). From "Default Output Formats":

> As of version 3.52.0 (2026-03-06) the default format is:
>
> - Show query results in a table composed from Unicode box drawing symbols. ("box")
> - Quote all output as SQL literals, except do not quote text literals when it is not necessary to disambiguate them. ("--quote relaxed")
> - Attempt to automatically squeeze the output table so that it fits within the width of the display. ("--screenwidth auto")
> - Truncate multi-line values to show only the first 5 lines. ("--linelimit 5")
> - Truncate long values to show only the first 300 characters. ("--charlimit 300")
> - Truncate column titles to 20 characters. (--titlelimit 20)
> - When displaying a BLOB that is really JSONB, show the equivalent JSON text rather than the BLOB value. ("--textjsonb on")
>

Now it can squeeze the entire table and columns to fit the available space and truncate long values. The new `.mode box` and `.mode qbox` use fancy unicode characters (qbox quotes values to distinguish their type; note that the strings below are quoted).

```
sqlite> .mode box
sqlite> SELECT * FROM weather LIMIT 5;
┌────────────┬─────────┬──────────┬─────────┐
│    date    │ airport │ day_high │ day_low │
├────────────┼─────────┼──────────┼─────────┤
│ 2026-01-01 │ EWR     │ 35       │ 20      │
│ 2026-01-02 │ EWR     │ 32       │ 21      │
│ 2026-01-03 │ EWR     │ 32       │ 20      │
│ 2026-01-04 │ EWR     │ 38       │ 27      │
│ 2026-01-05 │ EWR     │ 37       │ 24      │
└────────────┴─────────┴──────────┴─────────┘

sqlite> .mode qbox
sqlite> SELECT * FROM weather LIMIT 5;
┌──────────────┬─────────┬──────────┬─────────┐
│     date     │ airport │ day_high │ day_low │
├──────────────┼─────────┼──────────┼─────────┤
│ '2026-01-01' │ 'EWR'   │ 35       │ 20      │
│ '2026-01-02' │ 'EWR'   │ 32       │ 21      │
│ '2026-01-03' │ 'EWR'   │ 32       │ 20      │
│ '2026-01-04' │ 'EWR'   │ 38       │ 27      │
│ '2026-01-05' │ 'EWR'   │ 37       │ 24      │
└──────────────┴─────────┴──────────┴─────────┘
```

Although the documentation says this new mode is the default, I still see `.mode list` as my default so I set `.mode qbox` in my `.sqliterc`.
