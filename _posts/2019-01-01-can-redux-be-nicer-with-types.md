---
layout: post
title: Can Redux be nicer with types?
tags: [functional-programming]
bigimg: /img/bench-couple-date-6051.jpg
---

TBC

```
const todos = (state = [], action) => {
  switch (action.type) {
    case 'ADD_TODO':
      // return a new state with an extra todo added
      // the new todo should have id and text based on action.id and action.text
    case 'TOGGLE_TODO':
      // return state with todo complete state toggled
      // the todo item to toggle should be identified using action.id
    default:
      return state
  }
}

export default todos
```
