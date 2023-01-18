<img src="https://www.polystat.org/logo.svg" height="92px"/>

[![EO principles respected here](https://www.elegantobjects.org/badge.svg)](https://www.elegantobjects.org)
[![DevOps By Rultor.com](http://www.rultor.com/b/polystat/far)](http://www.rultor.com/p/polystat/far)
[![We recommend IntelliJ IDEA](https://www.elegantobjects.org/intellij-idea.svg)](https://www.jetbrains.com/idea/)

This is a command line tool that takes a directory with
[XMIR](https://news.eolang.org/2022-11-25-xmir-guide.html) files
(XML representation of [EO](https://www.eolang.org) programs)
and produces a directory with `.eog` files, each of which is a textual
representaiton of a [Control Flow Graph (CFG)](https://en.wikipedia.org/wiki/Control-flow_graph)
in EOG language. For example, this is the input EO program:

```
[x] > foo
  if. > @
    x.lt 0
    print "cold"
    print "warm"
```

This is how a generated EOG program may look:

```
1: a1 := D(x); ->2
2: a2 := LT(a1, 0); ->3; ->4;
3: CALL(print, "cold");
4: CALL(print, "warm");
```

There is also a Java API, which helps you read a file with EOG program
and then access it in a graph-traversing mode.
