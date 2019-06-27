# Window functions

I was really excited to learn about SQL window functions because you don't learn new concepts very often, and this one was new to me.

My motivation was that I had two different ranking systems I could use to `ORDER` a set of rows and I wanted to compare them. This would let me answer questions like "this row is first in this ranking, where is it in the other system?"

I was planning to run both queries and write a program to compare them when I found window functions.

(I'm going to show examples in [SQLite](https://sqlite.org/windowfunctions.html), but window functions are broadly supported in most dialects: [MySQL](https://dev.mysql.com/doc/refman/9.7/en/window-functions.html), [Postgres](https://www.postgresql.org/docs/current/tutorial-window.html), [Presto](https://prestodb.io/docs/current/functions/window.html), etc.)

You probably know about functions in SQL, which compute a value from the input:

```
SELECT UPPER('my name'), ROUND(1.248, 2), TRIM('  hello  ');
┌──────────────────┬─────────────────┬───────────────────┐
│ UPPER('my name') │ ROUND(1.248, 2) │ TRIM('  hello  ') │
├──────────────────┼─────────────────┼───────────────────┤
│ 'MY NAME'        │ 1.25            │ 'hello'           │
└──────────────────┴─────────────────┴───────────────────┘
```

And aggregate functions like `COUNT`, `MIN`, and `MAX` which compute a single value for the entire result set:

```
SELECT MAX(day_high), MIN(day_low)
FROM weather
WHERE date BETWEEN '2026-01-01' AND '2026-01-31'
AND airport = 'EWR';

┌───────────────┬──────────────┐
│ MAX(day_high) │ MIN(day_low) │
├───────────────┼──────────────┤
│ 54            │ 5            │
└───────────────┴──────────────┘
```

(values in Fahrenheit--it was a cold winter).

Window functions, in contrast, compute a value for a row relative to other rows in the result set.

For example, consider this weather table:

```
SELECT * FROM weather;
┌──────────────┬─────────┬──────────┬─────────┐
│     date     │ airport │ day_high │ day_low │
├──────────────┼─────────┼──────────┼─────────┤
│ '2026-01-01' │ 'EWR'   │ 35       │ 20      │
│ '2026-01-02' │ 'EWR'   │ 32       │ 21      │
│ '2026-01-03' │ 'EWR'   │ 32       │ 20      │
│~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~│
│                     ...                     │
│~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~│
│ '2026-01-31' │ 'EWR'   │ 26       │ 5       │
└──────────────┴─────────┴──────────┴─────────┘
```

Let's say I want to see the change in the temperature each day. To do that, I need the temperature from the current row and the previous row. You get that with the window function `LAG`:

```
SELECT
  date,
  day_high,
  LAG(day_high) OVER (ORDER BY date) AS yesterday_high
FROM weather
WHERE airport = 'EWR'
ORDER BY date;
┌──────────────┬──────────┬────────────────┐
│     date     │ day_high │ yesterday_high │
├──────────────┼──────────┼────────────────┤
│ '2026-01-01' │ 35 ───┐  │ NULL           │
│ '2026-01-02' │ 32    └───▶35             │
│ '2026-01-03' │ 32       │ 32             │
│~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~│
│                   ...                    │
│~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~│
│ '2026-01-30' │ 20 ───┐  │ 24             │
│ '2026-01-31' │ 26    └───▶20             │
└──────────────┴──────────┴────────────────┘
```

You can see that the `day_high` for a row becomes `yesterday_high` for the next row. The value for the first row is `NULL` because there is no previous row.

To see the change from one day to the next, you can use `day_high - LAG(day_high) OVER (ORDER BY date)`.

- The keyword that signals we're using window functions is `OVER`.
- You can create a named window with a `WINDOW` clause.
- You can use `PARTITION BY` to create subsets of rows for comparison (for example, to avoid comparing the weather at one airport to another).
- Some common functions are `ROW_NUMBER` (the number of a row in its partition set), `LAG` (previous row, used above), and `LEAD` (next row).

Even after writing all this up, I have to admit that I have never once written a window function from memory. But with many things, it doesn't matter if you know how to do it, what matters is that you know it's possible.
