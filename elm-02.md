## Functions in Elm

Elm is a pure functional language. That means that all functions in Elm are "pure" functions. 

But what is a pure function?

The thing that clicked for me was - if a function is pure - it can be completely represented as a table of inputs to outputs (as long as you don't mind allowing the table to be infinitely long!). For example, a function that adds 2 to its input is a pure function. The table would look like this:

| Input | Output |
| ----- | ------ |
| ...   | ...    |
| -1    | 1      |
| 0     | 2      |
| 1     | 3      |
| 2     | 4      |
| 3     | 5      |
| ...   | ...    |

I think that this way of looking at it makes the qualities of pure functions come out naturally.

> For any given input, you'll always get the same output

Obviously there's no way to encode a function like this so that an input of 1 sometimes returns 3 and sometimes 4. This fact alone
basically means no global variables

> There can't be any side-effects



I think a lot of what scares people away from functional languages is the syntax, especially around functions. Compared to other languages like C and JavaScript, the lack of parentheses and braces can make it look quite unfamiliar. That said, if you're familiar with more modern JavaScript, hopefully this code will make sense:

```javascript
let addTwo = x => x + 2;
```



You can actually write something very similar in Elm (and in Haskell and F#):

```elm
addTwo = \x -> x + 2
```


