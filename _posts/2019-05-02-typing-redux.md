---
layout: post
title: Typing Redux Actions and Reducers
tags: [functional-programming]
bigimg: /img/bench-couple-date-6051.jpg
---

I recently worked on a React project where, like a lot of others it seems, we decided to use TypeScript rather than plain old JavaScript. Why? The usual reasons for wanting static typing. For me, I like having the compiler point out my silly mistakes immediately, rather than waiting to discover them myself at runtime (or worse, through bug reports from users).

One frustration I have though is how difficult it seems to be to add types/type annotations for Redux actions and reducers.

This is a shame because I know from playing with [Elm](https://elm-lang.org/), that having static types here tends to eliminate an awful lot of common bugs.

One example that comes to mind is adding a new action but forgetting to add a new case statement to the reducer. If you do the equivalent in Elm, your program won't compile, and it'll tell you exactly where to go and fix it.

If you do this in Redux? Usually everything builds and runs fine, no exceptions are thrown. You run the app, click the button/perform the action and nothing happens. This behaviour could be the result of various different mistakes, so it usually takes a bit of head scratching to track it down.

It's certainly possible to add the type annotations using TypeScript, [the Redux documentation includes some examples](https://redux.js.org/recipes/usage-with-typescript), but this feels really...complicated! Especially when you look at the equivalent in Elm.

I wonder if it's possible to simplify things by looking at how Elm does it, then trying to translate that to TypeScript instead.

## The Experiment

To this end, I'm going to take a reducer from the [Redux Todos Example](https://github.com/reduxjs/redux/tree/master/examples/todos). Then I'll try implementing the equivalent in Elm. Since Elm is strictly typed, I'll have to add types for all of the code. Then I'll go back to the Redux vesion, add TypeScript, and see if I can make it mimic what I end up with in Elm.

With that in mind, here's the Redux version in plain JavaScript.

_N.b. For clarity, I've extracted the code dealing with each case/action into its own function (`addTodo` and `toggleTodo`) just so that we can focus on the higher level concepts. The actual implementation of these functions isn't really important, it's the reducer code we care about._

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

Here's the equivalent in Elm:

```elm
todos action state =
    case action of
        AddTodo { id, text } ->
            addTodo id text state
        ToggleTodo { id } ->
            toggleTodo id state
```

I'm hoping that by breaking Elm's naming conventions, I've made this a bit more familiar to React/Redux devs\*. Hopefully you can see just how similar to the Redux version above this is.

\* In Elm you'd usually see `update`, `msg` and `model` rather than `reducer`, `action` and `state` respectively.

## Adding Types

Whilst the Elm and Redux versions look essentially the same, the big difference we care about is that the Elm version is strongly and statically typed. Here is the type declaration behind the `action` parameter in the Elm version:

```elm
type Action
    = AddTodo { id: Int, text: String }
    | ToggleTodo { id: Int }
```

Essentially this says that if something is an `Action`, then it must be either an `AddTodo` or a `ToggleTodo`, each of which carries some simple data, which we've also given the types for.

_N.b. To readers familiar with Elm. I appreciate that usually you wouldn't bother with the record types and would just have e.g. `AddTodo Int String`. Whilst it's tempting to show how much more terse the language can be, I think (from experience!) people unfamiliar with ML-like languages can be initially put off by the unfamiliar terseness!_

## Union types

The `Action` type declaration above is an example of a "union" or "sum" type. Without going too far into what these are, it's worth noting that TypeScript actually has union types, so lets have a go at adding that to our Redux example, by adding some TypeScript:

```typescript
type Action = AddTodo | ToggleTodo;

interface AddTodo {
  type: "ADD_TODO";
  id: number;
  text: string;
}

interface ToggleTodo {
  type: "TOGGLE_TODO";
  id: number;
}
```

It's a bit more verbose than the Elm version, and it does look a bit weird with the magic strings, but it does bring some of the type safety we want. The reducer now looks like this:

```typescript
const todos = (state = [], action: Action) => {
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

Almost exactly the same as the original! That's good I suppose, but is the compiler actually looking after us now? Well - if I try and refer to `action.text` inside `case "TOGGLE_TODO"`, the compiler spots my mistake and reports _"Type error: Property 'text' does not exist on type 'ToggleTodo'."_. Excellent!

But wait...I said that Elm would help me if I added a new type of action and forgot to add the relevant case to the reducer. Will this TypeScript enhanced Redux version do that?

Sadly not. It turns out `switch`/`case` statements are not really quite the same thing as the pattern matching `case` expression in functional languages. But it turns out there _is_ a parallel in object-oriented languages, it's just that it's far from obvious on first glance. That's one for another post though :)
