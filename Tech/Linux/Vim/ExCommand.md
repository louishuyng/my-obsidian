---
tags: Vim, Linux
---
[https://nosarthur.github.io/productivity/2021/08/27/ex-vim](https://nosarthur.github.io/productivity/2021/08/27/ex-vim)

![[Pasted image 20230529152739.png]]
This post is about the colon commands in Vim. For example, the substitution command takes the form of

```
:[range]s/<pattern>/<replacement>/[options]
```

Such commands are inherited from a line editor called [Ex](https://en.wikipedia.org/wiki/Ex_(text_editor)) (or even Ex’ predecessor [Ed](https://en.wikipedia.org/wiki/Ed_(text_editor))). Other common Ex commands include `:e`, `:q`, `:r`, `:w`, `:3` (or any other line number).

## commands and symbols

A full list of Ex commands and range symbols are

|keys|meaning|
|---|---|
|`a`|append text|
|`c`|change (replace) text|
|`d`|delete, e.g., `3,5d`|
|`e`|edit file|
|`i`|insert text|
|`f`|show file information or switch file name|
|`g`|global action, e.g., `g/bad/d`|
|`m`|move, e.g., `1,3m$`|
|`p`|print lines|
|`q`|quit|
|`r`|read file into buffer|
|`s`|substitute|
|`t`|copy|
|`u`|undo the last command|
|`v` or `g!`|global action on lines that don’t contain a pattern|
|`w`|write to file|
|`/`|context search, e.g., `/word/+1d`|
|`.`|current line|
|`$`|last line|
|`%`|every line|
|`+` and `-`|line number arithmetics|
|`,`|range, e.g., `8,20`|
|`;`|relative range, e.g., `.;+5`|
|`0`|before 1st line|

Here the commands `a`, `c`, `i` trigger an input terminal at the bottom of Vim and a line of a single `.` ends the input.

You can test these keys directly in Vim, or read the help information. For example, `:help :a`. [B. W. Kernighan](https://en.wikipedia.org/wiki/Brian_Kernighan)’s [A Tutorial Introduction to the UNIX Text Editor](http://www.psue.uni-hannover.de/wise2017_2018/material/ed.pdf) is also a good short reference.