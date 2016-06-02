---
title: More useful shell incantations
date: 2016-06-02 02:48 UTC
tags: chain
---

## More useful shell incantations

`ls -ltr`

List dir contents in descending order by date modified

`zip -Dr zipdest.zip /zipsource/stuff`

Recursively zip contents of a directory but flatten out paths so all files are at one level in the zip file.

`rename 's/.png.+/.png/' *`

Rename files based on regex substitution pattern. Note for this you should not use the old `rename` binary.  The perl one is available via Homebrew: `brew install rename`

`curl -O url`

Do not change the filename of the downloaded file. I always forget the correct option.

