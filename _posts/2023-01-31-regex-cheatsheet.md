---
title: 'Regex Cheatsheet'
date: 2023-01-31
permalink: /posts/2023/01/regex_cheatsheet
tags:
  - SWE
  - regular expressions
  - cheatsheet

excerpt_separator: "<!--more-->"
---

This guide details the quintessential parts of regular expressions (at least for me). A more detailed list of operators can be found [here](https://www.rexegg.com/regex-quickstart.html).<!--more-->

```
// in the following example, the `/` is used to wrap the regex. `\d+` represents numbers from 0 to 9. // the `g` tag indicates global search
var r = /\d+/g 
```

# Regex Basics
The following characters indicate some category of characters. Using these symbols alone will represent one character that matches its parameter. Note that capitalizing the symbol will indicate the opposite of its parameter.

Symbol | Meaning
\d | Any one digit (0 to 9) *Python: unicode digit in any script
\w | ASCII letter, digit or underscore *Python: unicode letter, ideogram, digit or underscore
\s | Whitespace characters (tab, newline, space etc.) *Python/JS: unicode separator
\D | Opposite of \d (NOT a digit)
\W | Opposite of \w (NOT a word character)
\S | Opposite of \s  (NOT a whitespace)

## Logic Operators
These logic operators allow you to chain multiple search parameters

Symbol | Meaning | Example
\| | Alternation OR operand | `a|b`
( ... ) | Group
\x | Contents of group x (x is an int) | '(\d\d) + (\d\d) = \2 + \1' > '3 + 4 = 4 + 3'
(?: ... ) | Exclude group

## Quantifiers
These indicate number of times to search for the pattern.

Symbol | Meaning 
+ | Greedy search (one or more)
{x} | Exactly x times
{x, y} | x to y number of times
{x,} | x or more times
* | Zero or more times
? | Once or none

## Lookaround Characters
These are sort of like conditional operators. This [post](https://stackoverflow.com/questions/2973436/regex-lookahead-lookbehind-and-atomic-groups) explains it in more detail.
In all honesty I rarely use these, but they are cool to know.

Symbol | Meaning | Example
(?=…) | Positive lookahead | Asserts that `...` comes after the current position
(?<=…) | Positive lookbehind | Asserts that `...` comes before current position
(?!…) | Negative lookahead | Asserts that `...` is definitely not after current position
(?<!…) | Negative lookbehind | Asserts that `...` is definitely not before current position


