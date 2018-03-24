---
layout: post
title: Introduction to Elm
bigimg: /img/elm-leaf.jpg
hidden: true
tags: [ functional-programming ]
---

## What is Elm?

### Elm is a programming language

Elm is a programming language designed specifically for creating browser-based applications. In that sense, it's a bit like JavaScript. In many other ways though - it's nothing like JavaScript. 

One way it's not like JavaScript is that it's not constantly trying to trick you into creating bugs.

### Elm is a front-end framework

Elm 

## Why should you use it?

I think the main argument for using Elm is that it's much easier to write bug-free web applications than it is with any other method I'm aware of.

> There is no `null` or `undefined` in Elm

Most bugs in JavaScript 

> There is no `this` keyword in Elm

> It's really fast

Elm is a true statically typed language. The compiler will catch many errors for you


## Functions in Elm

Elm is a pure functional language. That means that all functions in Elm are "pure" functions. But what is a pure function?

A pure function is a function which:
* Given the same input, will always return the same output.
* Produces no side effects.



```javascript
let add = (x, y) => x + y;
```

```elm
add : number -> number -> number
add x y = 
  x + y
```

- types

- more about union types
- nulls
- errors

- elm architecture?
- subscriptions?

[Try to implement the Reset button](https://ellie-app.com/b3DHf8863a1/0)
