---
layout: post
title: Introduction to Elm
bigimg: /img/elm-leaf.jpg
hidden: true
tags: [ functional-programming ]
---

## What is Elm?

Elm is primarily a programming language, but it also comes with some very nice tools for building web applications, meaning it can be used essentially as a front-end web framework.

### Elm is a programming language

The Elm language was designed specifically for creating browser-based applications. In that sense, it's a bit like JavaScript. Don't worry though, in most other ways though, it's nothing like JavaScript!

Elm is a pure functional language (more on that later), with a syntax very similar languages such as Haskell and F#. Don't let that scare you off either though, Elm is deliberately designed to be easy to understand. 

In fact, if you're interested in learning more about functional programming, almost everything you learn from Elm can help you break into those more "hardcore" functional languages. As a traditional "object-oriented" developer I've found that once you know some Elm, those languages start to look a lot more familiar.

### Elm is a front-end framework

Elm also comes with a runtime, which provides everything you need to build a browser application, such as dynamically rendering HTML and receiving events from users. In that respect it's a lot like Angular, Vue and especially React. 

I've found though that after a while of looking at Elm, those other frameworks start to look very complicated indeed!

## Why should you use it?

For me, the main reason for using Elm is that it's so easy to write solid, bug-free code.

If you're used to writing front-end code in JavaScript (or a related language like TypeScript), how's this for a list of compelling reasons to prefer Elm:

* There is no `null` or `undefined` in Elm
* There is no `this` keyword in Elm
* There are no exceptions (and no need for `try` and `catch`) in Elm
* Elm is statically typed

> It's really fast

Another good reason to learn Elm is if you're interested in learning functional programming. Even if you never end up using it, the syntax is essentially identical to Haskell and F#

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

The thing that clicked for me was - if a function is pure - it can be thought of as a table of inputs to outputs. For example, a function that adds 2 to its input is a pure function. The table would look like this:

| Input | Output |
| ----- | ------ |
| ...   | ...    |
| -1    | 1      |
| 0     | 2      |
| 1     | 3      |
| 2     | 4      |
| 3     | 5      |
| ...   | ...    |

<table>
  <caption>AND function</caption>
  <thead>
    <tr>
      <th>Input</th>
      <th>Output</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>False</td>
      <td>
        <table>
          <thead>
            <tr>
              <th>Input</th>
              <th>Output</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>False</td>
              <td>False</td>
            </tr>
            <tr>
              <td>True</td>
              <td>False</td>
            </tr>
          </tbody>
        </table>
      </td>
    </tr>
    <tr>
      <td>True</td>
      <td>
        <table>
          <thead>
            <tr>
              <th>Input</th>
              <th>Output</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>False</td>
              <td>False</td>
            </tr>
            <tr>
              <td>True</td>
              <td>True</td>
            </tr>
          </tbody>
        </table>
      </td>
    </tr>
  </tbody>
</table>

* A pure function, given the same input, will always return the same output
* A pure function produces no side effects.

These two rules have some profound consequences. The thing that really made that click for me was realising that it means you can think of a pure function as nothing more than a lookup table!

For example, I could create a pure function that takes an integer and adds 2 to it.

In Elm this looks like:

```elm
addTwo x = x + 2
```


### More parameters

| Input | Output |
| ----- | ------ |
| ...   | ...    |
| -1    | `\y -> -1 + y` |
| 0     | `\y -> 0 + y` |
| 1     | `\y -> 1 + y` |
| 2     | `\y -> 2 + y` |
| 3     | `\y -> 3 + y` |
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

[Maybe/Result example](https://ellie-app.com/b3vJ4YBkFa1/0)

[Starter](https://ellie-app.com/s7vPbXKXca1/1)

```
module Main exposing (main)

import Html exposing (..)
import Html.Attributes exposing (..)
import Html.Events exposing (..)


catGifUrl =
    "https://media3.giphy.com/media/nyDuytA5bRdbW/giphy.gif"


main =
    Html.beginnerProgram { model = model, update = update, view = view }


type Msg
    = Increment
    | Decrement
    | Change String


model : Int
model =
    0


update msg model =
    case msg of
        Decrement ->
            model - 1

        Increment ->
            model + 1

        Change value ->
            case value of
                "" ->
                    0

                value ->
                    case String.toInt value of
                        Ok newNumber ->
                            newNumber

                        Err _ ->
                            model


view : Int -> Html Msg
view model =
    div []
        [ div []
            [ button [ onClick Decrement ] [ text "-" ]
            , input [ type_ "text", value (toString model), onInput Change ] []
            , button [ onClick Increment ] [ text "+" ]
            ]
        , renderCats model |> div []
        ]


renderCats amount =
    List.range 1 amount
        |> List.map (\_ -> img [ src catGifUrl ] [])
```
https://ellie-app.com/mGr6RPpjda1/0

