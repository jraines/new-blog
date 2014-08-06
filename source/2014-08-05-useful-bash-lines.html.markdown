---
title: Useful Bash Lines
date: 2014-08-05
tags: bash
---

#Useful Bash One Liners

Delete files older than 7 days

```bash
find /your_directory -mtime +7 -exec rm -f {} \;
```

Find largest 10 files in directory. [Explanation](http://explainshell.com/explain?cmd=du+-hsx+*+%7C+sort+-rh+%7C+head+-10)

```bash
du -hsx * | sort -rh | head -10
```

Get specific columns from a csv and display in columns.  Here, the first column and sixth through tenth:

```bash
cut -d "," -f 1,6-10 example.csv | column -t -s','
```

Sort a csv by a number in the fifth field

```bash
sort -n -k5 items.csv
```

Average the 5th column of a file, but only consider rows where that column is greater than 150

```bash
awk -F, ' $5 > 150 {n++; sum+=$5} END{ print sum/n}' items.csv
```
