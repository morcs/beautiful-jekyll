---
layout: post
title: Can Redux be nicer with types?
tags: [functional-programming]
bigimg: /img/bench-couple-date-6051.jpg
---

WIP

I'm going to take an example reducer from the [Redux Todos Example](https://github.com/reduxjs/redux/tree/master/examples/todos).

To recap, a _reducer_ is a pure function. It takes the existing state and an "action" representing a requested change. The reducer's job is to apply the action to the existing state and return a new state for us to use.

Since we'll usually have more than one type of action, the reducer usually ends up as a switch statement with a case for each type of action. If you think about it, the code within each case statement is effectively a reducer in itself, which works only for one specific type of action. 

In the example below I've extracted each of the implementations into their own functions (`addTodo` and `toggleTodo`) just so that we can focus on the higher level reducer. The actual implementation of these functions isn't really important, but it should be clear what they do.

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

Here's the equivalent in Elm. I've used Redux's names for things* to help compare to the version above.

* Elm usually uses msg and model rather than action and state

```
update action state =
    case action of
        AddTodo id text ->
            addTodo id text state
        ToggleTodo id ->
            toggleTodo id state
```

Doesn't look that different really, apart from maybe a slightly scary on first appearance lack of punctuation? The key difference we care about here is the fact that the actions types are explicit. To show that I need to show the type of `state`:

```
type Ssg
    = AddTodo Int String
    | ToggleTodo Int

...

So I'm going to add TypeScript to the Redux version and try and get some order up in this chaos.

It turns out that union types in F# compile down to the equivalent of a C# class heirarchy. Maybe functional and object-oriented programming aren't that different after all...
