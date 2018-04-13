---
layout: page
title: Introduction to Elm
bigimg: /img/elm-leaf.jpg
hidden: true
---

This page exists to support my "Introduction to Elm" hands-on talk. The slides are available on [slides.com](http://slides.com/morcs/introduction-to-elm).

## Hands-on exercises

The exercises go along with the talk in order to provide the "hands-on" bits. In most cases the exercise will have at least one compiler error present, which should help to guide the solution.

### Exercise 1 - Markup

[View Exercise](https://ellie-app.com/mwRph7znwa1/2)

Fill in the card rendering code so that there's a table with a row for each "stat". The resulting HTML should look something like this:

```html
<table class="table mb-0">
  <tr>
    <th>Nonchalance</th>
    <td class="text-right">8</td>
  </tr>
  <tr>
    <th>Aggression</th>
     <td class="text-right">6</td>
  </tr>
  ...
</table>
```

[View Solution](https://ellie-app.com/mwRph7znwa1/1)

## Elm resources

Some useful links for those wanting to learn more about Elm:

* [elm-lang.org](http://elm-lang.org/) - The official Elm website.
* [Ellie App](https://ellie-app.com/) - A brilliant tool that lets you write, compile and run Elm apps in your browser, as used for all the code samples in this talk.
* ["Making Impossible States Impossible" (video)](https://www.youtube.com/watch?v=IcgmSRJHu_8) - Richard Feldman's talk on how to use Elm's type system to design models that make illegal states unrepresentable.
