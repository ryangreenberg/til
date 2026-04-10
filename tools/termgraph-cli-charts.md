# `termgraph` for CLI charts

I love git archaeology so I enjoyed the commands Ally Piechowski shared in [The Git Commands I Run Before Reading Any Code](https://piechowski.io/post/git-commands-before-reading-code/).

This one shows you the monthly commits in a repository:

```
git log --format='%ad' --date=format:'%Y-%m' | sort | uniq -c
```

For example, in the open-source [sentry](https://github.com/getsentry/sentry) codebase:

```
git log --after '2024-06-01' --format='%ad' --date=format:'%Y-%m' | sort | uniq -c
1231 2024-06
1334 2024-07
1012 2024-08
1077 2024-09
1242 2024-10
1029 2024-11
 911 2024-12
1408 2025-01
1403 2025-02
1777 2025-03
1980 2025-04
1594 2025-05
1738 2025-06
1816 2025-07
1202 2025-08
1765 2025-09
1557 2025-10
1282 2025-11
1153 2025-12
1459 2026-01
1302 2026-02
1841 2026-03
 471 2026-04
```

It's easier to see numbers than read and compare, so I went looking for CLI charts and found [termgraph](https://github.com/mkaz/termgraph), a Python CLI and library. In the past I have been somewhat reluctant to use Python CLIs because I get into messes with `pip install` and environments, but when combined with uvx it's easy to use:

```
git log --after '2024-06-01' --format='%ad' --date=format:'%Y-%m' | sort | uniq -c  | awk '{print $2, $1}' | uvx termgraph
2024-06: ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 1.23 K
2024-07: ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 1.33 K
2024-08: ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 1.01 K
2024-09: ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 1.08 K
2024-10: ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 1.24 K
2024-11: ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 1.03 K
2024-12: ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 911.00
2025-01: ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 1.41 K
2025-02: ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 1.40 K
2025-03: ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 1.78 K
2025-04: ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 1.98 K
2025-05: ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 1.59 K
2025-06: ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 1.74 K
2025-07: ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 1.82 K
2025-08: ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 1.20 K
2025-09: ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 1.76 K
2025-10: ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 1.56 K
2025-11: ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 1.28 K
2025-12: ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 1.15 K
2026-01: ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 1.46 K
2026-02: ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 1.30 K
2026-03: ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 1.84 K
2026-04: ▇▇▇▇▇▇▇▇▇▇▇ 471.00
```

The default formatting for termgraph is `"{:<5.2f}"`. I tried to fix 538.00 to 538 with `--format '{:.0f}'` but that also truncated 1.78K to 1K. Then I found `--format {:g}`, which strips trailing zeros so 1.82 is still rendered as 1.82 but 538.00 becomes 538. 'g' is the ["general format"](https://docs.python.org/3/library/string.html#format-specification-mini-language).
