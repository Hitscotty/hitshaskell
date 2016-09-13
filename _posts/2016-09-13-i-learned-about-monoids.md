---
layout: post
title: I learned Monoids
---

> A Monoid is an abstract mathematical concept. The concept is better known as an algebraic structure and a Monoid happens to be one of them. Specifically a Monoid is an "algebra", a binary associative operation constrained by the law of Idenitity and the law of associativity.

# First translating some vocabulary

> **Algebra** - "Study of mathematical symbols and rules governing their manipulation"<br>
> **Haskell Algebra** - Functions and how they are composed/combined<br><br>
> **Algebraic Structure** - "A set with a finite number of operations constrained by axioms"<br>
> **Haskell Algebraic Structure** - A type with functions that follow some laws for arithmetic<br><br>
> **"algebra"** - " A vector space equipped with a bilinear product"<br>
> **Haskell "algebra"** - Objects of the same type with two operations summation and multiplication.<br>
> <br>
> **Arithmetic** - "dealing with properties and manipulation of numbers"<br>
> **Haskell Arithmetic** - using what we know in the realm of arithmetic and applying it to types

# Summarizing my new vocabulary

An Algebra is the overall study of Algebraic Structures. In Haskell this can be seen as the different types of patterns that can be found in functions/types. Algebraic structures are types that already have said patterns. An "algebra" is a specific pattern, it is an algebraic structure where objects/types combine through summation/multiplication. A Monoid defines a pattern between types when being combined. This pattern is generalized and the particular generalization is defined through already established mathematical concepts/terminology. We use laws of arithmetic to dictate how a certain type should behave when combined, if the types follow an identity and associativity it can be classified as an algebra called Monoid. When we declare/write a Monoid for a type that type is an algebraic structure.

# So what is a Monoid ?

A Monoid is a function that takes two inputs where the inputs must be associative and have an identity value. Associativity meaning that the order in which we group our inputs will have no effect on the result. Normally I see a lot of examples using three values such as: `(a x b) x c = a x (b x c)`. But, I'm slow and I need even that trivial example explained. Here's how I've broken it down to myself. Given the computation `a x b x c`, the Monoid is `x` because it can only take two inputs.The only possible ways for the Monoid `x` to execute `a x b x c`, since it only takes two inputs is: `(a x b) x c` or `a x (b x c)`. Now this difference in how execution is grouped, first `(a x b)` or first `(b x c)` must have no effect on the end result when the full expression is evaluated. To have no effect on the end result or more commonly known `(a x b) x c = a x (b x c)` is associativity. So to recap given that we have a function `(x)` that takes two inputs :

prefix:

```
 (x) input1 input2
```

infix:

```
_ x _
```

, the inputs must be associative. Now additionally there needs to be an identity value. If you're like me you question everything and so the reason why there must be an identity is because it gives us an entirely unique set of operations that can be applied to a type. A Monoid is just an algebra and since we have defined it to have these rules, we can generalize these rules across types using the mathematical symbol we attach to it. The mathematical symbol in question is a function. So if we write an instance of Monoid for a type and define the Monoid to be `(+)`, the `(+)` is the Monoid, but similarly you can define it to be a custom function that does some computation called `plus :: a -> a -> a` or `plusMinusTen :: a -> a -> a` and if we implement our Monoid typeclass with these functions they will become a Monoid.

# How do we implement a Monoid?

Since Monoid structure is already written for us in the Monoid typeclass all we have to do is override it or implement an instance of it and then define it for the type thats implementing it.

This is the Monoid typeclass:

```
class Monoid m where
    mempty :: m
    mappend :: m -> m -> m
    mconcat :: [m] -> m
    mconcat = foldr mappend mempty
```

To use the Monoid typeclass above on a type we have our type implement it by writing an instance of our type that implements the Monoid typeclass :

```
instance customType a => Monoid a where
    mempty =
    mappend =
```

We then define how `mempty` and `mappend` behave with our type `customType a`. To define how they behave we must understand that `mempty` and `mappend` are binary functions, because that's what a Monoid is, and there are two of these because similarly there are two laws a Monoid obeys: _Identity_ and _Associativity_. First we think about what value will be the Identity of our type and define `mempty` to be just that and then we think about how our type `customType a` will _associate_ ...

# What is this useful for?

We deal with creating types in Haskell, by realizing that our type will behave as a Monoid we can accomplish more with less code by creating a framework that works for all elements of that type. The framework in question is "combining" values and we do it by implementing the abstracted out pattern, the Monoid typeclass.

_~ A value can be a function_

--------------------------------------------------------------------------------

<center><a href="https://en.wikipedia.org/wiki/Algebra_over_a_field">Wikipedia</a>
<a href="http://haskellbook.com">Haskell Programming</a>
<a href="https://wiki.haskell.org/Monoid">HaskellWiki</a>
<a href="http://math.stackexchange.com/questions/1925456/what-is-the-difference-between-an-algebra-and-an-algebraic-structure">StackOverFlow</a>
<a href="http://www.cut-the-knot.org/WhatIs/WhatIsArithmetic.shtml">Arithmetic</a></center>
