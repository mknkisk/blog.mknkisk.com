---
title: 'Input Validation using Regular Expressions'
date: 2016-04-06T02:57:55+09:00
draft: false
---

Injection Vulnerabilities
For example SQL Injection

Sample code

```
SELECT * FROM users WHERE id='$id'
```

$id is fetched from the user input.

if user input is below,

```
';DELETE FROM users --
```

Sample code to create an SQL statement to select

```
SELECT * FROM users WHERE id='';DELETE FROM users --'
```

## Check input values using regular expressions

Only alphanumeric and length 1-5

```
'/\A[a-z0-9]{1,5}\z/ui'
```

 * `u` : UTF-8 encoding
 * `i` : case-insensitive mode
 * `\A` `\z` : `\A` only ever matches at the start of the string. Likewise, `\Z` only
   ever matches at the end of the string.

`^` and `$`

Using `^` and `$` as Start of Line and End of Line Anchors.
$ still matches at the end of the string, and also before every line break.

ref: [Regex Tutorial - Turning Modes On and Off for Only Part of The Regular Expression](http://www.regular-expressions.info/modifiers.html)

Except control characters

```
'/\A[[:^cntrl:]]{1,30}\z/u'
```

Except control characters other than tab, carriage return and line feed

```
'/\A[\r\n\t[:^cntrl:]]{1,400}\z/u'
```
