---
layout: post
title: Null - The Billion Dollar Mistake
bigimg: /img/mistake.jpg
tags: [musings]
---

Tony Hoare invented `null`, and famously later called it his "billion dollar mistake". But what's wrong with it, and what could he have done instead?

## The problem will `null`

Imagine you import a library containing the following function (N.b. I've using TypeScript for the code examples here):

```
function getFerrari(): Ferrari { ... }
```

A rather nice function to have available. It's literally saying if you call it, you can have a Ferrari:

```
const myCar = getFerrari();
```

I now have a variable called `myCar`, and TypeScript (through type inference) will confidently agree that it is indeed a `Ferrari`.

Unfortunately though, the following is a perfectly valid implementation of `getFerrari`:

```
function getFerrari(): Ferrari {
  return null;
}

const myCar = getFerrari();
```

TypeScript will have absolutely no issue with this, and the code will still compile perfectly without even a warning.

Oblivious to this, I might later try to get the key to my Ferrari:

```
const key = myCar.getKey();
```

Again, the compiler will have be fine with this. `myCar` is a `Ferrari`, and `Ferrari` has a `getKey` method on it, so we're all good.

Of course, when I go to run the code, I'll end up getting this well known heart-sinking runtime error:

```
TypeError: Cannot read property 'getKey' of null
```

It's a bit late now, but I've just found out that I don't have a Ferrari at all.

If I'm not thorough with my testing, I might not even hit this branch of code. I could easily end up putting it into production. Then I'd have to debug to work out what actually caused the error, which will involve tracing back through the code to find out where the null came from, which could be pretty laborious.

The problem is that null is like a sort of evil gremlin. It can hide from the compiler anywhere in your code, pretending to be anything it likes. That is, until someone tries to treat it as the advertised thing, at which point the previously hidden null gremlin will jump out and make your application explode, leaving you having to work out where on earth the little blighter was introduced.

Why do we accept this? Wouldn't it be better if the compiler could prevent us from doing it in the first place?

## What if nullables were more like other things?

So how do we fix this? What should we be doing instead? Lets look at some different cases and how they behave much better.

Imagine instead of returning a Ferrari or null, the function we call returns an array of Ferraris:

```
function getFerraris(): Array<Ferrari> { ... }

const myFerraris = getFerraris();
```

Obviously you can't call getKey on this, it won't even compile:

```
const key = myFerraris.getKey();
```

Instead the compiler forces you to acknowledge that it's a collection, and use the relevant boilerplate code to allow you to get to the individual Ferraris. For example:

```
let keys = [];
for(ferrari of myFerraris)
{
    keys.push(ferrari.getKey());
}
```

N.b. You're probably wondering why I don't just use `map`, don't worry that's coming!

This code can still represent the case where you don't actually have a Ferrari (i.e. `myFerraris` is an empty list). Except this time instead of exploding, the code will return an empty list of keys. No gremlins in sight. The behaviour seems obvious, if you ask for all of the keys to my Ferraris, and I don't have any Ferraris, you get zero keys.

Similarly, if we look at promises:

```
function promiseMeAFerrari(): Promise<Ferrari> { ... }

const promiseOfFerrari = promiseMeAFerrari();
```

Again we can't just try and get the key from the promise, the compiler won't let us:

```
const key = promiseOfFerrari.getKey();
```

As for the array, we need to use some promise specific boilderplate to allow us to get at the Ferrari itself:

```
const promiseOfKey = promiseOfFerrari.then(ferrari => ferrari.getKey());
```

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
