---
layout: post
title: Typing Redux
tags: [functional-programming]
bigimg: /img/bench-couple-date-6051.jpg
---

From what I gather, a lot of people are now using TypeScript when building React apps, but I've noticed that generally people don't add types to their Redux actions and reducers.

I've done a bit of searching around this and there are definitely people doing it, but the results seem to end up looking really complicated, and I can see why people give up on it!

This seems a shame because I know from [Elm](https://elm-lang.org/) that adding static typing in this area tends to eliminate an awful lot of common bugs.

## FP vs OOP?

Meanwhile, over the last couple of years, as I've been delving more and more into the world of functional programming (FP), I can't help noticing that whilst at first it feels completely different to object-oriented programming (OOP), most of the concepts on the one side actually have parallells on the other. It's just that in some cases you have to look quite hard to see them!

With that in mind, I can't help thinking that whilst Elm is a purely functional language, it should be possible to translate the way Elm uses types in its messages and update functions (actions and reducers in Redux speak) into the more object-oriented language of TypeScript.

## The Experiment

For this experiment, I'm going to take a reducer from the [Redux Todos Example](https://github.com/reduxjs/redux/tree/master/examples/todos), try implementing it in Elm, then try to recreate the Elm design in the Redux version by adding TypeScript.

For clarity, I've extracted the code dealing with each case/action into its own function (`addTodo` and `toggleTodo`) just so that we can focus on the higher level reducer. The actual implementation of these functions isn't really important, but it should be clear what they do.

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

I've noticed that when I first show Elm to people who've not seen an ML-based language before, their reaction is usually somewhere between disgust and horror. I'm not sure if it's the lack of punctuation that puts them off, but I'm hoping that by showing the direct equivalent in React first, and by tweaking some of the names in the Elm version to be more familiar to React/Redux devs\*, I can overcome this issue.

\* Elm conventionally uses `update`, `msg` and `model` rather than `reducer`, `action` and `state` respectively.

Whilst it looks a little different, this code is essentially doing exactly the same thing as the React/Redux reducer above it.

## Adding Types

Of course, the purpose of this post is to look at typing, so here I'll add the type declaration for the action:

```elm
type Action
    = AddTodo Int String
    | ToggleTodo Int
```

I like to think this mostly explains itself, but fundamentally it means that if you have a value of type `Action`, then either it must be an `AddTodo`, with an integer for the new ID and a string for the text, or it must be a `ToggleTodo` with an integer representing the ID of the item to toggle.

Obviously the benefit of this sort of static typing is that the compiler will stop you doing silly things like trying to access the `text` value on the `ToggleTodo` action. Another benefit with Elm which might not be apparent is the compiler will tell you if you've forgotten to account for a particular case of `Action`. This happens to me all the time with Redux, where I've added a new action but have forgotten to add a case to the reducer. When this happens it shows up only at runtime by nothing happening, followed by a few minutes head-scratching before working out where the issue is. For me, having the compiler prevent me doing this is worth the effort of typing on its own!

## Union types

The `Action` type declaration above is an example of a "union" or "sum" type. Without going too far into what these are, it's worth noting that TypeScript actually has its own form of union type, so lets have a go at adding that to our Redux example:

To be continued...
