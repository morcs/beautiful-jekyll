---
layout: post
title: Null - The Billion Dollar Mistake
bigimg: /img/mistake.jpg
tags: [musings]
---

Tony Hoare invented `null`, and famously later called it his "billion dollar mistake". But what's wrong with it, and what could he have done instead?

## The problem will `null`

Imagine you import a library containing the following function (N.b. I'm using TypeScript for clarity here, hopefully little enough that it should be clear enough to anyone that knows JavaScript):

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

If I'm not thorough with my testing, I might not even hit this branch of code and could easily end up putting it into production. Then I'd have to debug to work out what actually caused the error, which might involve tracing back through the code to find out where the null came from, which could be pretty time consuming.

I think all developers have hit this type of error more times than they dare to imagine, and I think it's easy to see how this could add up to being a billion dollar mistake. Especially if you changed the example to something like:

```
const controlRod = myNuclearReactor.getControlRod();
```

The problem is that null is like a sort of evil gremlin. It can masquerade as any value in your code, pretending to be something benign. That is, until someone tries to treat it how it was advertised, at which point the previously hidden null gremlin has no choice but to jump out and make your application explode, leaving you having to work out where on earth the little blighter was introduced.

Why do we accept this? Wouldn't it be better if the compiler could prevent us from doing it in the first place?

## What if nullables were more like other things?

So how do we fix this? What should we be doing instead? Lets look at some other similar programming structures and how they behave much better.

Imagine instead of returning a Ferrari or null, the function we call returns an array of Ferraris:

```
function getFerraris(): Array<Ferrari> { ... }

const myFerraris = getFerraris();
```

Obviously you can't call getKey on this, it won't even compile:

```
const key = myFerraris.getKey();
```

Instead the compiler forces you to acknowledge that it's a collection, and use the relevant boilerplate code to allow you to get at the individual Ferraris. So something like this, which will give you an array of keys:

```
let keys = [];
for(ferrari of myFerraris)
{
    keys.push(ferrari.getKey());
}
```

N.b. You're probably wondering why I don't just use `map` for this, don't worry that's coming!

This code can still represent the case where you don't actually have a Ferrari (i.e. `myFerraris` is an empty list). Except this time the code will return an empty list of keys, which I think is preferable to an undetectable gremlin.

Of course we could add some similar boilerplate code around the nullable example, and acknowledge the fact that it could be null:

```
const maybeAFerrari = getFerrari();
let key = null;
if(maybeAFerrari) {
    key = maybeAFerrari.getKey();
}
```

TBC
