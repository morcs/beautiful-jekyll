---
layout: post
title: Why functional programming?
tags: [functional-programming]
---

As a recent convert, I've already been asked several times to explain what’s so great about functional programming. It’s something I find surprisingly difficult to answer.

Apparently it’s not just me:

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">I have that exactly problem with &quot;functional programming&quot; entirely. Seems like everyone loves it but no-one can tell me why/how it&#39;s really better or what for.</p>&mdash; Neil Barnwell (@neilbarnwell) <a href="https://twitter.com/neilbarnwell/status/932756550770479104?ref_src=twsrc%5Etfw">November 20, 2017</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Neil’s definitely not stupid. I’ve learned about subjects such as CQRS and event sourcing from attending his talks and I know he’ll have spoken to plenty of people that you’d think should be able to answer this question. Maybe it is genuinely hard to answer. Or maybe it’s not actually better at all?

It’s something to do with “monads”
There seems to be this rite of passage for any developer making the shift from object-oriented to functional programming which involves trying to explain to the world what monads are. Often the motivation given is something along the lines of “the best way to learn is to teach”. I think there’s also a bit of this:

Monads have a reputation for being difficult to understand.
Once they finally “click”, monads don’t seem difficult to understand.
Maybe this pushes the developer ego to the conclusion that it was everyone else’s fault it took them so long to understand what monads were. Everyone else was just over-complicating it.

This phenomenon seems to have been reciprocating for years now, leading to this wonderful quote from Douglas Crockford

The curse of the monad is that once you get the epiphany, once you understand ‘oh that’s what it is’, you lose the ability to explain it to anybody else.
So of course, despite knowing all of this, I’m going to blunder onto the internet and try to explain monads.

More specifically I’m going to try to explain monads by trying to answer the question “Why functional programming?”.

You probably already love functional programming
Of course in reality a lot of this functional stuff has already made its way into object-oriented languages. For example, C# has higher-order functions, and I can’t think of a much better example of the delights afforded by higher-order functions than .NET’s LINQ extension methods.

I dare say you’ll struggle to find a developer that uses LINQ that doesn’t love it!

LINQ can be used to show that one of the benefits of functional programming is in preventing runtime exceptions, and who doesn’t want to prevent runtime exceptions?

C# developers will know straight away the most likely source of errors in the following snippet:


There are lots of ways that this code could fail, but I think the most obvious/likely one is when companyID doesn’t match any companies.

Assuming in this case it makes sense for totalSalary to be zero when no companies match. We can cover the exception by using SingleOrDefault (which returns null if there’s no match), then check that it didn’t return null before trying to enumerate its employees:


This is painful though, and no sane developer is going to write checking code like this every time a function could fail. There’d be more error checking than actual code. There must be a neater way.

I’ve come across this issue countless times doing C# programming. At some point I noticed that if I ignored the fact that I expected exactly one company to match, I could just leave the result as a collection and avoid the need for a null check:


Here if no companies match, the Where clause will return an empty collection, rather than the null we had to guard for above. The rest of the code will work just fine with an empty collection, logically returning zero as its sum.

Now that I’m learning functional programming concepts, I can see that what I was doing here is actually an example of using monads.

So what’s special about "functional languages"?
I actually think this Tweet answers the "Why functional programming?" question pretty well:


In the C# example above, functional techniques were used to avoid exceptions, but the point is, we could write code that threw exceptions.

In fact there are still lots of ways the above code could fail. What if one of the companies, or the employees collection, or one of the employees in it is null?

What if we were to write similar code but to calculate the average rather than the total salary? What would the average salary be when the company has no employees? A naive implementation would throw an exception, because averaging an empty collection means dividing by zero. Would we definitely remember to check for that? I know from experience I’d forget!

Pure functional languages attempt to solve this problem by making it impossible to compile, let alone run code that would throw these sorts of exceptions.

In further posts I’ll try to break down what is actually happening in the LINQ trick above. Hopefully from there I’ll be able to explain how the same general pattern is used within functional languages to do everything other languages can do, whilst maintaining the promise of “purity” and the profound implications that brings.
