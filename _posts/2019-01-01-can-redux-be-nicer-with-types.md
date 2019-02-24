---
layout: post
title: Can Redux be nicer with types?
tags: [functional-programming]
bigimg: /img/bench-couple-date-6051.jpg
---

I'm going to take a reducer from the [Redux Todos Example](https://github.com/reduxjs/redux/tree/master/examples/todos).

To recap, a reducer is a pure function. It takes the existing state and an "action" representing a requested change. The reducer's job is to return a new state, which is the result of applying the requested change to the existing state.

Since we'll usually have more than one type of action, the reducer usually ends up branching into individual implementations for each possible type of action, using a switch statement. Usually each of these implementations is left inline but for this snippet I've extracted them into their own functions (`addToDo` and `toggleToDo`). These are basically implementations of the reducer for one specific type of action.

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

```
update action state =
    case action of
        AddTodo id text ->
            addTodo id text state
        ToggleTodo id ->
            toggleTodo id state
```
