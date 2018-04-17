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

[View Exercise](https://ellie-app.com/mwRph7znwa1/6)

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

[View Solution](https://ellie-app.com/mwRph7znwa1/5)

### Exercise 2 - Union types

[View Exercise](https://ellie-app.com/mwRph7znwa1/8)

Fix the code so that we can show loading and error states, using the HTML snippets shown below. Lean on the compiler to help you work out what's left to do!

```html
<div class="alert alert-success">Loading...</div>
```

```html
<div class="alert alert-danger">Error message goes here</div>
```
Since we're not actually doing an HTTP request yet, you can test each state by trying different values for `model`, e.g. changing from `model = Loading` to `model = CardList testCards`.

[View Solution](https://ellie-app.com/mwRph7znwa1/7)

### Exercise 3 - The update function

[View Exercise](https://ellie-app.com/mwRph7znwa1/10)

Fill in the update function so that the customer can test what the app looks like in all three different states.

If you have time, add a button to allow them to get back to the loading state, and maybe have two buttons to test different error message text.

[View Solution](https://ellie-app.com/mwRph7znwa1/9)

### Exercise 4 - Maybe

[View Exercise](https://ellie-app.com/mwRph7znwa1/13)

Complete the code so that the user can select a card.

When no card is selected, use the following HTML for the top bar:

```html
<div class="fixed-top bg-success text-white p-3">Please select a card</div>
```

When a card is selected, use this HTML:

```html
<div class="fixed-top bg-primary text-white p-3">Selected: <strong>{{card.name}}</strong></div>
```

[View Solution](https://ellie-app.com/mwRph7znwa1/12)

### Exercise 5 - Extending messages (HTTP)

[View Exercise](https://ellie-app.com/mwRph7znwa1/16)

Finish getting the code to compile so that cards can be loaded from the API.

N.b. The definition of the `Result` type is:

```
type Result error value
    = Ok value
    | Err error
```

Hint: Looking at the definition of the `Loaded` type along with the above should help.

[View Solution](https://ellie-app.com/mwRph7znwa1/14)

### Further Exercises

#### 1. Add a reload button

Add a button to be shown on the card list view that allows the user to reload a new set of random cards from the API. After pressing the button, no card should be selected (even if one was selected before).

N.b. This will give you chance to try out returning a command from the update function!

#### 2. Make the illegal state unrepresentable

In the above code, the `Select` branch of the update statement bothers me, because:

1. I don't like the nesting of `case` expressions.
2. The second part of the inner `case` expression represents an illegal state, that we should never be able to get to anyway!

I went on the Elm forums to get their opinion, and it turns out there is a fairly simple way to fix this. Can you work it out?

Hint <span style="color:#0000;background-color:#000000">Try changing the Select message type itself</span>

#### 3. There is another illegal state we can make unrepresentable!

The model currently could represent a situation where the selected card isn't one of the cards in the list. Can we prevent this? (The ["Making Impossible States Impossible" (video)](https://www.youtube.com/watch?v=IcgmSRJHu_8) shows how to do this).

#### 4. Automatically select a card after 30 seconds

Imagine this is part of a multiplayer game - we don't want to wait forever if one player walks away from their screen, so we could add a rule where their first card is automatically selected after 30 seconds. You'll need to use the `subscriptions` function to do this. 

See the [Clock example](http://elm-lang.org/examples/clock) and this [StackOverflow post](https://stackoverflow.com/questions/40599512/how-to-achieve-behavior-of-settimeout-in-elm?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa).

You could also adapt this to show a countdown in the top bar!

## Elm resources

Some useful links for those wanting to learn more about Elm:

* [elm-lang.org](http://elm-lang.org/) - The official Elm website.
* [Ellie App](https://ellie-app.com/) - A brilliant tool that lets you write, compile and run Elm apps in your browser, as used for all the code samples in this talk.
* ["Making Impossible States Impossible" (video)](https://www.youtube.com/watch?v=IcgmSRJHu_8) - Richard Feldman's talk on how to use Elm's type system to design models that make illegal states unrepresentable.
