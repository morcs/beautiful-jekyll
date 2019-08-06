# What is Functional Programming?

Simply put, functional programming (FP for short) is programming using only _pure_ functions.

## Go on then, "pure" functions?

Whilst maybe not particularly rigorous, I find the following definition pretty intuitive:

> A _pure_ function is a lot like a simple table, mapping every possible value\* of a given input type to a value of a given output type.

Below are some examples of pure functions, represented in this way.

...

\* Note that "every possible value" often means the table is infinitely long. That is, if the input type is `string` you end up with "... mapping every possible value of type string" in the definition above. There are infinitely many possible sequences of characters that could make up a string, but this is fine!

---

I think looking at pure functions this way makes clear all the things you _can't_ do with them, how simple they are, and how easy this makes them to reason about and test. And yet, it's perfectly possible to build entire programs using only pure functions!

For example, the only variable that the output can depend on is the input itself. There's no way the function could be reaching out and reading or writing global variables. It can't be reading anything from external services such as the console, or a database or web service, because if it used them in any meaningful way, it would have to affect the output, and you can't represent that in a simple table. Pure functions are deterministic.

Likewise there can't be any side-effects of running the function such as writing to a database or calling an API, because again that would be impossible to represent in the table.

Note that many types (e.g. `string`) can have an infinite number of possible values, and since we map every possible value of the input type, we quite often end up with an infinite number of rows.

For our purposes, the type of each column should be strictly enforced. Every cell in the table should contain a valid value of the column's type. No exceptions, blanks, `null`s, `NaN`s or `undefined`s.

\* Technically this makes it a "total" function (as opposed to a "partial" function)
