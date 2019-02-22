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

export default todos
```

```
update : Msg -> Model -> Model
update msg model =
    case msg of
        AddTodo id text ->
            addTodo model id text
        ToggleTodo id ->
            toggleTodo model id
```
