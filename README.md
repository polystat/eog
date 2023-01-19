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
[n] > fibo
  if. > @
    n.lt 2
    1
    plus.
      fibo (n.minus 1)
      fibo (n.minus 2)
```

This is how a generated EOG program may look (a very very early draft):

```
@fibo(n):
  x1 := @if(n)
  Return x1

@if(n):
  x1 := Dataized @lt(n)
  Jump #A If x1
  x2 := @plus(n)
  Jump #B
  #A
  x2 := Int 1
  #B
  Return x2

@lt(n):
  x1 := Dataized n
  x2 := Int 2
  x3 := Math.Lt x1 x2
  Return x3

@plus(n):
  x1 := Dataized @fibo1(n)
  x2 := Dataized @fibo2(n)
  x3 := Math.Plus x1 x2
  Return x3

@fibo1(n):
  x1 := @minus1(n)
  x2 := @fibo(n=x1)
  Return x2

@minus1(n):
  x1 := Dataized n
  x2 := Int 1
  x3 := Math.Minus x1 x2
  Return x3

@fibo2(n):
  x1 := @minus1(n)
  x2 := @fibo(n=x1)
  Return x2

@minus2(n):
  x1 := Dataized n
  x2 := Int 2
  x3 := Math.Minus x1 x2
  Return x3
```

This is how it can be sequentialized:

```
@fibo(n):
  @if(n):
    @lt(n):
      x1.1.1 := Dataized n
      x1.1.2 := Int 2
      x1.1.3 := Math.Lt x1.1.1 x1.1.2
      x1.1 := Dataized x1.1.3
    Jump #A If x1.1
          x1.2.1.1.1 := Dataized n
          x1.2.1.1.2 := Int 1
          x1.2.1.1.3 := Math.Minus x1.2.1.1.1 x1.2.1.1.2
          x1.2.1.1 := x1.2.1.1.3
        x1.2.1.2 := @fibo(n=x1.2.1.1)
        x1.2.1 := Dataized x1.2.1.2
          x1.2.2.1.1 := Dataized n
          x1.2.2.1.2 := Int 2
          x1.2.2.1.3 := Math.Minus x1.2.2.1.1 x1.2.2.1.2
          x1.2.2.1 := x1.2.2.1.3
        x1.2.2.2 := @fibo(n=x1.2.2.1)
        x1.2.2 := Dataized x1.2.2.2
      x1.2.3 := Math.Plus x1.2.1 x1.2.2
      x1.2 := x1.2.3
    Jump #B
    #A
    x1.2 := Int 1
    #B
    x1 := x1.2
  Return x1
```

The EOG-script of the `if` atom may look like this:

```
@if(ρ, α0, α1):
  x1 := ρ Dataized
  x2 := α0 If x1
  x3 := α1 Unless x1
  Return x3
```

There is also a Java API, which helps you read a file with EOG program
and then access it in a graph-traversing mode.
