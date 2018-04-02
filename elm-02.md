## Functions in Elm

Functions in Elm are "pure". 

But what is a pure function?

I think the easiest way to think about it is, pure functions can be completely represented as a table mapping inputs to outputs.

For example, a function that doubles its input is pure. The table would look like this:

| Input | Output |
| ----- | ------ |
| ...   | ...    |
| -1    | -2     |
| 0     | 0      |
| 1     | 2      |
| 2     | 4      |
| 3     | 6      |
| ...   | ...    |

### Properties of pure functions

> For any given input, you'll always get the same output

In the lookup table, every possible input is listed exactly once. If you think about what this means - the function can't rely on getting information from global or member variables (so there's absolutely no reason to have a `this` keyword). It can't query a database to get different results and it can't ask the user for input. There's no point as it can't possibly affect the outcome.

> There can't be any side-effects

There's no way for this table to represent things like output being written to the screen, or a file, or a database.

I think a lot of what scares people away from functional languages is the syntax, especially around functions. Compared to other languages like C and JavaScript, the lack of parentheses and braces can make it look quite unfamiliar. That said, if you're familiar with more modern JavaScript, hopefully this code will make sense:

```javascript
let addTwo = x => x + 2;
```



You can actually write something very similar in Elm (and in Haskell and F#):

```elm
addTwo = \x -> x + 2
```


