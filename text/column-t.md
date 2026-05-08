# Pretty-print column-based data with `column -t`

I wanted to share some tab-separated data in Slack but it was hard to read:

```
File  Unchecked Partial Checked
chat.php 7467 910 2333
job_queue.php 4025 194 726
reminders.php 4989 274 1411
unfurl.php 3739 383 1328
users.php 5631  454 1669
```

I had already sent a few of these snippets and was tired of manually aligning them. I was on the verge of writing a script to format the data for me when I thought "surely there is something that does it?"

There is! It was a bit hard to figure out how to search for it, but eventually I found `column`. From the manual:

> ```
>  column -- columnate lists
>
>  -t      Determine the number of columns the input contains and create a table. Columns are delimited with whitespace, by default, or with the characters supplied using the -s option. Useful for pretty-printing displays.
> ```

I can't say that I'm familiar with the verb "columnate" but it works like a charm.

```
$ cat data.txt | column -t
File           Unchecked  Partial  Checked
chat.php       7467       910      2333
job_queue.php  4025       194      726
reminders.php  4989       274      1411
unfurl.php     3739       383      1328
users.php      5631       454      1669
```

By default `column` works with any whitespace-delimited separator, whether it's tabs or spaces. Note that on the macOS version of `column` you need a newline at the end of your text or it will drop the last line.
