---
layout: post
title: Typing Redux
tags: [functional-programming]
bigimg: /img/bench-couple-date-6051.jpg
---

From what I gather, a lot of people are now using TypeScript when building React apps, but I've noticed that generally people don't add types to their Redux actions and reducers.

I've done a bit of searching around this and there are definitely people doing it, but the results seem to end up looking really complicated, and I can see why people give up on it!

This seems a shame because I know from [Elm](https://elm-lang.org/) that adding static typing in this area tends to eliminate an awful lot of common bugs.

Meanwhile, over the last couple of years, as I've been delving more and more into the world of functional programming (FP), I can't help noticing that whilst at first it feels completely different to object-oriented programming (OOP), most of the concepts on the one side actually have parallells on the other. It's just that in some cases you have to look quite hard to see them!

With that in mind, I can't help thinking that whilst Elm is a purely functional language, it should be possible to translate the way Elm uses types in its messages and update functions (actions and reducers in Redux speak) into the more object-oriented language of TypeScript.

I'm going to take an example reducer from the [Redux Todos Example](https://github.com/reduxjs/redux/tree/master/examples/todos).

For clarity, I've extracted the code dealing with each case/action into its own function (`addTodo` and `toggleTodo`) just so that we can focus on the higher level reducer. The actual implementation of these functions isn't really important, but it should be clear what they do.

```
const todos = (state = [], action) => {
  switch (action.type) {
    case 'ADD_TODO':
      return addTodo(state, action.id, action.text);
    case 'TOGGLE_TODO':
      return toggleTodo(state, action.id);
    default:
      return state
  }
}
```

Now I've noticed that when I first show Elm to people who've not seen an ML-based language before, their reaction is usually somewhere between disgust and horror. I'm not sure if it's the lack of punctuation that puts them off, but I'm hoping that by showing the direct equivalent in React first, and by tweaking some of the names in the Elm version to be more familiar to React/Redux devs\*, I can overcome this issue.

\* Elm conventionally uses `update`, `msg` and `model` rather than `reducer`, `action` and `state`.

With that in mind, here's the Elm equivalent of the reducer above.

```
todos action state =
    case action of
        AddTodo id text ->
            addTodo id text state
        ToggleTodo id ->
            toggleTodo id state
```

Hopefully it's fairly obvious that whilst it looks a little different, this code is essentially doing exactly the same thing as the React/Redux reducer.

Now as I mentioned, Elm is a strongly typed language, and in order to carry on with this post, for now I'll need to show you the type definition for `action` (again with Elm's naming conventions replaced by more familiar Reduxy ones)

```
type Action
    = AddTodo Int String
    | ToggleTodo Int
```

To be continued...
