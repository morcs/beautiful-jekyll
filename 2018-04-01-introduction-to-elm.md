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

I think a lot of what scares people away from functional languages is the syntax for defining and calling functions looks so unfamiliar and terse compared to other languages like C and JavaScript. That said, if you're familiar with more modern JavaScript, hopefully this code will make sense:

```javascript
let addTwo = x => x + 2;
```
You can actually write something very similar in Elm (and in Haskell and F#):

```elm
addTwo = \x -> x + 2
```

Elm is a pure functional language. That means that all functions in Elm are "pure" functions. 

But what is a pure function?

* A pure function, given the same input, will always return the same output
* A pure function produces no side effects.

These two rules have some profound consequences. The thing that really made that click for me was realising that it means you can think of a pure function as nothing more than a lookup table!

For example, I could create a pure function that takes an integer and adds 2 to it.

In Elm this looks like:

```elm
addTwo x = x + 2
```

As a lookup table, it would look like this.

| Input | Output |
| ----- | ------ |
| ...   | ...    |
| -1    | 1      |
| 0     | 2      |
| 1     | 3      |
| 2     | 4      |
| 3     | 5      |
| ...   | ...    |

### More parameters

| Input | Output |
| ----- | ------ |
| ...   | ...    |
| -1    | \y -> -1 + y |
| 0     | \y -> 0 + y |
| 1     | \y -> 1 + y |
| 2     | \y -> 2 + y |
| 3     | \y -> 3 + y |
| ...   | ...    |



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