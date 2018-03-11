---
layout: post
title: Avoiding Exceptions with Total Functions
tags: [functional-programming]
bigimg: /img/total-functions-1.jpeg
---

In the previous post I picked out what I think is one of the most compelling answers to the question “Why Functional Programming?”. That is, in order to avoid dealing with exceptions and nulls.

I gave an example of using functional techniques in C# to do this, without really explaining what makes it functional programming.

To go further I think it’s worth going a bit deeper into what avoiding exceptions and nulls really means, in functional programming terms.

## Partial Functions

I used this snippet of C# code to talk about a problem:


The problem is the Single method. Its signature here essentially boils down to:

```C#
public static Customer Single(...)
```

`Single`’s signature tells us it returns a Customer, but it’s lying!

In reality, `Single` *sometimes* returns a Customer. If the input causes no match, it throws an exception. In the strict language of mathematics it’s a partial function, it doesn’t return a value for every possible input. We’re expected to try and remember that each time we use it. Wouldn’t it be better to get the compiler to pick that up instead?

## Total Functions?

Maybe (pun-intended) we can avoid this trap by using something that does return a value for every possible input, thus making it a total function (a “function”, to give its proper name 😜)

I’ll choose to ignore that the companies collection could be null here, its just the same problem showing up somewhere else.

Well SingleOrDefault seems to do the trick. With SingleOrDefault every input should return an output, which will either be a Customer or null.

So what’s different about the signature of SingleOrDefault?
```C#
public static Customer Single(...)
public static Customer SingleOrDefault(...)
```

The difference is…there isn’t one! Another liar, but this one’s worse! At least when Single actually returns something, that thing is what it says it is — a Customer. Now it might be a null pretending to be a Customer.

This is chaos! A null result can now flow into unsuspecting code that’s meant to work with exactly one Customer. That code will blow up when it encounters the impostor, and we won’t even know where the problem started.

We’ll get the dreaded null reference exception, famous not only for it’s inability to tell us where it came from, but also sometimes it’s inability to even tell us what was null! (If avoiding this isn’t a good argument for Functional Programming, what is?)

So again, the developer, not the compiler, must remember this and account for it:


## Total Functions (Honestly this time)

The signature for Where looks like:
```C#
public static IEnumerable<Customer> Where(...)
```

This solves the issues outlined above:

* Unlike `Single`, it will return a value for every input. (It’s a total function)
* Unlike `SingleOrDefault`, the signature tells us everything it could return. It’s honest!

This time we can’t just pretend the result is a single Customer like we could with `SingleOrDefault`. We can’t do:

This obviously won’t compile. Instead we need to get real and deal with the fact that the first line will return a collection of Customers.

## Conclusion

We’ve removed the source of exceptions and nulls by making sure we use total functions, but there are still at least a couple of things that probably stand out:

* The signature of Where is almost too broad, whilst we know it should only return at most one Customer, the signature suggests there could be any number of Customers. I’ll deal with that in the next post.
* If we do this everywhere that there might have been a null before, won’t we have to write a tonne of boilerplate code to deal with checking whether the returned collection is empty or not? That’s going to be just as much work as checking for exceptions/nulls was, except now we’re being forced to do it every time.

On the latter point I’ve kind of jumped the gun a bit by showing how `SelectMany` allows us to skip the boilerplate. I think it’s worth another post looking into how that works though. It might just be the key to the real essence and power of monads.
