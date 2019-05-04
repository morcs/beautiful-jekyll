---
layout: post
title: Typing Redux
tags: [functional-programming]
bigimg: /img/bench-couple-date-6051.jpg
---

A lot of people are now using TypeScript when building React apps, but I've noticed that generally people don't add types to their Redux actions and reducers.

I've done a bit of searching around this and there are definitely people doing it, but the results seem to end up looking quite complicated, and I can see why people give up on it!

This seems a shame because I know from [Elm](https://elm-lang.org/), that adding static typing in this area tends to eliminate an awful lot of common bugs.

## FP vs OOP?

Meanwhile, over the last few years, as I've been delving more and more into the world of functional programming, I can't help noticing that whilst at first it feels completely different to object-oriented programming, most of the concepts in the one paradigm actually have parallells on the other. It's just that in some cases you have to look quite hard to see them!

With that in mind, I can't help thinking that whilst Elm is a purely functional language, it should be possible to translate the way Elm uses types in its update funtions (the equivalent of Redux reducers) into the more object-oriented language of TypeScript.

## The Experiment

For this experiment, I'm going to take a reducer from the [Redux Todos Example](https://github.com/reduxjs/redux/tree/master/examples/todos), try implementing it in Elm, then try to recreate the Elm design in the Redux version by adding TypeScript.

For clarity, I've extracted the code dealing with each case/action into its own function (`addTodo` and `toggleTodo`) just so that we can focus on the higher level concepts. The actual implementation of these functions isn't really important, but it should be clear what they do.

With that in mind, here's the JavaScript version:

```javascript
const todos = (state = [], action) => {
  switch (action.type) {
    case "ADD_TODO":
      return addTodo(state, action.id, action.text);
    case "TOGGLE_TODO":
      return toggleTodo(state, action.id);
    default:
      return state;
  }
};
```

...and here's the equivalent in Elm:

```elm
todos action state =
    case action of
        AddTodo id text ->
            addTodo id text state
        ToggleTodo id ->
            toggleTodo id state
```

I've noticed that when I first show Elm to people who've not seen an ML-based language before, their reaction is usually somewhere between disgust and horror. Maybe it's the unfamiliar lack of punctuation, but I'm hoping that by showing it next to the React/JS equivalent, and by tweaking some of the names to be more familiar to React/Redux devs\*, I can overcome this issue. So whilst it looks a little different, this code is essentially doing exactly the same thing as the React/Redux example above it.

\* Elm conventionally uses `update`, `msg` and `model` rather than `reducer`, `action` and `state` respectively.

## Adding Types

The purpose of this post is to look at typing, so I'll also show the type declaration behind the `action` parameter:

```elm
type Action
    = AddTodo Int String
    | ToggleTodo Int
```

Hopefully it's fairly clear what's being stated. Essentially if something is an `Action`, then it must be either an `AddTodo` or a `ToggleTodo`, each of which carries some simple data which we've also given types for.

Since Elm makes us give actions a type, its compiler can spot when we do something silly, and show us exactly what and where the issue is.

To give an example, the classic mistake I always seem to make with Redux is to add an action, write the code to dispatch it, then completely forget to add a case for it to the reducer. I'll then run the program, try performing the action through the browser (which will do precisely nothing), then spend several minutes scratching my head and trying out all the possible things I could have done wrong before realizing what the actual issue is.

In Elm, if I forget to add the new action to the case expression, it just won't compile. It'll tell me exactly where to go in the code to fix the issue and it'll pretty much tell me what code I need to add as well. This is a huge time-saver!

## Union types

The `Action` type declaration above is an example of a "union" or "sum" type. Without going too far into what these are, it's worth noting that TypeScript actually has its own form of union type, so lets have a go at adding that to our Redux example:

To be continued...
