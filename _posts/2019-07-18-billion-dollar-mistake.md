---
layout: post
title: Null - The Billion Dollar Mistake
bigimg: /img/mistake.jpg
tags: [musings]
---

_This post is unfinished, I'm trying to write something to go along with the talk I'm scheduled to be doing and figured if I just publish it as it is...I'm more likely to finish it!_

Tony Hoare invented `null`, and famously later called it his "billion dollar mistake". But what's wrong with it, and what could he have done instead?

## The problem with `null`

Imagine you import a library containing the following function (N.b. I'm using TypeScript as the static typing will help to explain the idea.):

```
function getFerrari(): Ferrari { ... }
```

What a function! It's literally saying if you call it, you can have a Ferrari:

```
const myCar = getFerrari();
```

I now have a variable called `myCar`, and I know it's `Ferrari`. TypeScript will confirm this if I check what type it has inferred.

Unfortunately though, the following is a perfectly valid implementation of `getFerrari`:

```
function getFerrari(): Ferrari {
  return null;
}

const myCar = getFerrari();
```

TypeScript will have absolutely no issue with this, and the code will still compile without even a warning.

Oblivious to this, I might later try to get the key to my Ferrari:

```
const key = myCar.getKey();
```

Again, the compiler will have be fine with this. `myCar` is a `Ferrari`, and `Ferrari` has a `getKey` method on it, so we're all good.

Of course, when I go to run the code, I'll end up getting this familiar, heart-sinking error:

```
TypeError: Cannot read property 'getKey' of null
```

It's a bit late now, but I've just found out that I don't have a Ferrari at all.

If I'm not thorough with my testing, I might not even hit this branch of code. I could easily end up putting it into production, where my users would find out that the Ferraris don't exist. Then I'd have to debug to work out what actually caused the error, which will involve tracing back through the code to find out where the null came from.

The problem is that null is like a sort of evil gremlin. It can hide from the compiler anywhere in your code, pretending to be anything it likes. Any value could actually be the null gremlin. That is, until someone tries to treat it as the thing it's advertising itself as, at which point the null gremlin will expose itself and make your application explode.

Why do we accept this? Wouldn't it be better if the compiler could prevent us from doing it in the first place?

## What if nullables were more like other things?

So how do we fix this? What should we be doing instead? Lets look at some different cases and how they behave much better.

Imagine instead of returning a Ferrari or null, the function we call returns an array of Ferraris:

```
function getFerraris(): Array<Ferrari> { ... }

const ferraris = getFerraris();
```

Obviously you can't call getKey directly on the result, that won't even compile:

```
const key = ferraris.getKey();
```

Instead the compiler forces you to acknowledge that it's a collection, and use the special map function to call the `getKey` method on individual cars. For example:

```
const keys = ferraris.map(ferrari => ferrari.getKey());
```

This code can still represent the case where you don't actually have a Ferrari (i.e. `ferraris` is an empty list). Except this time instead of exploding, the code will return an empty list of keys. No gremlins in sight. The behaviour seems obvious, if you ask for all of the keys to my Ferraris, and I don't have any Ferraris, you get zero keys.

Similarly, if we look at promises, again we can't just try and get the key immediately from the value, the compiler won't let us:

```
const promiseOfFerrari: Promise<Ferrari>;
const key = promiseOfFerrari.getKey();
```

Just like for the array, we need to acknowledge that it's a promise use a special mapping function (this tiem called `then`) to allow us to use the `getKey` method.

```
const promiseOfKey = promiseOfFerrari.then(ferrari => ferrari.getKey());
```

Lets have a look at these two cases together:

```
const value: Array<Ferrari>;
const result: Array<Key> = value.map(ferrari => ferrari.getKey());

const value: Promise<Ferrari>;
const result: Promise<Key> = value.then(ferrari => ferrari.getKey());
```

Arrays and promises seem like unrelated things, but these snippets of code are remarkably similar.

In both cases we have a Something<Ferrari>, with a mapping function that can take the function we want to apply to the Ferrari to get a Key, and apply it to the Something<Ferrari> in order to obtain a Something<Key>.

<!--So the billion dollar mistake was to allow `null` to be treated as a value of any type.

Of course we could add some similar boilerplate code around the nullable example, and acknowledge the fact that it could be null:

```
const maybeAFerrari = getFerrari();
let key = null;
if(maybeAFerrari) {
    key = maybeAFerrari.getKey();
}
```
-->

TBC
