---
layout: post
title: Can Redux be nicer with types?
tags: [functional-programming]
bigimg: /img/bench-couple-date-6051.jpg
---

TBC

https://github.com/reduxjs/redux/tree/master/examples/todos

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
