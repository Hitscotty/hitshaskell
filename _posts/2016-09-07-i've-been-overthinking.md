---
layout: post
title: My problem is...
---

<img src="http://frescocapital.com/wp-content/uploads/2015/07/why.jpg"> </img>
So after reading many papers, wiki's, blogs and multiple re-readings of <b>Haskell
Programming</b>, I finally realized why I wasn't understanding Monoids/Functors
/Monads.
I was over thinking an abstract idea and in the process of over thinking, seeking
a concrete answer where the answer can't be concrete because the concept to be
understood deals with the abstraction of computation. If that didn't make any sense
to you just imagine it as the type constructor of my feelings

`data Scotty a = Thinking | OverThinking a`.

I was asking *why?* too stubbornly and all I could
find was the same answers where everyone just kept reiterating what things were,

`meinKampf = ["A Monoid is..", "A Functor is..", "A Monad is.." ..]`

So I realized that the answer to my *"why?"* is: because it's readable. The monoid
typeclass is named monoid because categorically it is a monoid and etc. After letting
go of this I began to see things more simply and the answers to my other questions
started to come.

## Why do we have laws? How do we follow laws?

The answer to this one came when I decided to give up on all the metaphors and explanations
kind people from the community were giving me. I've always understood everything pertaining to Haskell
well and this is largely do to the types and the type system. When it came to these more advanced topics
the internet has made them seem very *scary* and the explanations are so far from the types, I
thought there was some higher level *thing* I needed to know that the types would not tell me. In the end
the types tell you everything.
> "When in doubt always look at the types." - The Haskell Gods

When I returned to looking only at the
types I realized that we have laws because when dealing with polymorphic types the programmer truly has
all the power to define completely different computations or combinations using just the type signatures. The
laws help make sure that in the process of writing these instances it'll behave as it should so that regardless
of what we do write and however creative we may get in the code itself it'll be what is intended.
