---
layout: post
title: Late Nights
---
It's 3:13 AM, there's no milk and I can't have my cereal without it! I should be sleeping, but what is more comforting right now than a late night snack is programming. I finally made it through chapter 12 and when I look back on it I'm super stoked at all that I've learned. This chapter was about ```Maybe``` and ```Either``` types and ```unfold```. I spent the majority of the day playing with these parametric types. I just finished figuring out what "unfold" is and it's soooo beautiful! I couldn't ask for a better day; I want to dig even deeper:

>
``` haskell
treeBuild :: Integer -> BinaryTree Integer
treeBuild 0 = Leaf
treeBuild n = unfold (\b -> case b >= n of
    True  -> Nothing
    False -> (Just (b+1,b,b+1))) 0
```

This was was my end result, but boy was it a journey getting there. First I was only given the type of unfold ```(a -> Maybe (a, b, a)) -> a -> BinaryTree b``` and wasn't quite sure what the hell it did, but after some tinkering my unfold type checked *(I was exuberant)*. Great I now had the unfold, now the hard part; implement ```treeBuild :: Integer -> BinaryTree Integer```. This is where it gets exciting and I find myself many hours later where I am now. I spent the majority of that time wondering how I would build a tree using an infinite unfold. After some time I decided to hit the IRC for haskell because I was scared that a google search would reveal the answer. A simple sentence, *"should you feed it nothing to make it finite" -  @parsnip* and I quickly realized that unfold takes a function that returns a ```Maybe``` type so it could be ```Just``` or ```Nothing```. That's how I came to this:

>
``` haskell
unfold :: (a -> Maybe (a, b, a)) -> a -> BinaryTree b
unfold f a =
    case f a of
        Nothing        -> Leaf
        Just (l, m, r) -> Node (unfold f l) m (unfold r)
```

In order to reason about my code I tossed around undefined functions with the types I needed to use:

``` haskell
f :: (a -> Maybe (a, b, a))
f = undefined
```

Building data-structures with a functional language like Haskell is so much more fun then in my normal imperative languages and it also helps me learn/understand them more in depth. I only wish I could keep churning through into chapter 13, but I like my mind to be fully focused to absorb details. I think instead I'll end my night with some follow-up resources.
