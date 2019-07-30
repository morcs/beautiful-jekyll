---
layout: post
title: Null - The Billion Dollar Mistake
bigimg: /img/gearshift.jpeg
tags: [musings]
---

Tony Hoare invented `null`, and famously later called it his "billion dollar mistake". But what's wrong with it, and what could he have done instead?

## The problem will `null`

Imagine you import a library containing the following function (N.b. I'm using TypeScript as I want to make the types explicit, but hopefully this should be clear enough to anyone that knows JavaScript):

```
function getFerrari(): Ferrari { ... }
```

Quite a nice function to have access to! It's literally saying if you call it, you can have a Ferrari:

```
const myCar = getFerrari();
```

I now have a variable called `myCar`, and TypeScript's implicit typing will confidently tell me that it is indeed a `Ferrari`.

...unfortunately though, the following a perfectly valid implementation of `getFerrari`:

```
function getFerrari(): Ferrari {
  return null;
}
```

TypeScript will have absolutely no issue with this, and my code from before will still compile perfectly without even a warning.

Oblivious to this, I might later try to get the key to my Ferrari:

```
const key = myCar.getKey();
```

Again, the compiler will have no issue with this. `myCar` is a `Ferrari`, and `Ferrari` has a `getKey` method on it, so we're all good.

Of course, when I go to run it, I'll end up getting this heart-sinking runtime error:

```
TypeError: Cannot read property 'getKey' of null
```

It's a bit late now, but I've just found out that I don't have a Ferrari at all.

If I'm not thorough with my testing, I might not even hit this branch of code and could easily end up putting it into production. Then I'd have to debug to work out what actually caused the error, which might involve tracing back through the code to find out where the null came from, which could be pretty time consuming. (Imagine how many times over this has been repeated and it's easy to imagine how this adds up to being a billion dollar mistake).

The problem is that null is like a sort of evil gremlin. It can masquerade as any value in your code and pretend to be something it isn't, until someone tries to treat it how it was advertised, at which point the gremlin jumps out and causes your application to explode, leaving you having to work out where on earth the gremlin was introduced.

## What if other stuff worked this way?

So how should it work instead? Lets look at some other similar programming structures and how they behave.

Imagine instead of returning a Ferrari or null, the function we call returns an array of Ferraris:

```
function getFerraris(): Array<Ferrari> { ... }

const myFerraris = getFerraris();
```

Obviously you can't now call getKey on this, it won't even compile:

```
const key = myFerraris.getKey();
```

Instead the compiler forces you to acknowledge that it's a collection, and use the relevant boilerplate code to allow you to operate on individual Ferraris. So something like this, which will give you an array of keys:

```
let keys = [];
for(ferrari of myFerraris)
{
    keys.push(ferrari.getKey());
}
```

In the case where you don't actually have a Ferrari, i.e. `myFerraris` is an empty list, this code will return an empty list of keys, which I think you'll agree is preferable to a gremlin jumping out and exploding.
