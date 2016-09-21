---
layout: post
title: I love Functors
---

Ok, I'll be honest I love all of it, but seriously Functors are really cool. So I like dissecting what I've learned about Haskell and functional programming so it took me quite awhile before I decided to write about it. I finally started having a real strong intuitive understanding of these algebraic structures, specifically today with Functors. I think I'll start by defining the general Idea of what a functor is and then enjoy explaining some of my code. For non-haskellers it might not make sense, but that part of the blog post is more for me then for the audience.

# So what is a Functor

A functor is a mapping between categories. Specifically it is a pattern of computation abstracted out into a typeclass which allows you to declare an instance of it so that you may re-use that pattern without needing to do much extra coding to implement it. Specifically this "pattern" is the application of a function onto the elements of some type of structure where the structure is some form of encapsulation. What this basically means is if you have an object that holds an element and a function that operates on those type of elements, a functor will allow you to map that function over the elements of that object while preserving that objects encapsulation of those elements. A more complete definition of a functor is an operation that is associative upon elements of a higher structure. This is best seen through examples:

(_Note: the first line of a haskell function usually defines the type of inputs and type of outputs in a matter like so: function_name :: input -> output. Function Application like f(x) is equal to f x_)

```haskell
f :: a -> b
f x = x + 1
```

`f` is a polymorphic function that takes something and transforms it into something else. `f` could be the function `f(x) = (x + 1)`, `f(x) = (x * 2)`, `f(x) = "hello"` and these are all perfectly valid the type of `f` is `a -> b` and that means the result doesn't have to match the input unless the function were `a -> a`.

```haskell
myBox = box ( 1)
```

`myBox` is a variable that holds the value `1` encapsulated into something called `box`. We don't know what that something is because I haven't specified it's type in order to focus on whats important here.

```haskell
fmap :: (a -> b) -> (f a) -> (f b)
fmap f myBox = box (f 1)
```

`fmap` takes two inputs: a function with a type of (a -> b) and an encapsulated type denoted by `f a` where the `a` is being encapsulated by `f` and`f` could be anything that encapsulates a value. Or you could simply view it as `fmap(f, (box (1) ) = box (f 1)`. If we look at what fmap is doing it is applying the function `f` to the contents of `box` and that is simply what a functor does. It is a way of applying functions to things with more structure. Don't be confused by the name a Functor is just a mathematical concept that helps us identify a the behavior it defines. The typeclass/interface is defined by :

```haskell
class Functor f where
 fmap :: (a -> b) -> f a -> f b
```

now when ever we want something to be a functor, to have functor behavior we just have to overload the functor typeclass and define its fmap on our data type. Two examples of writing functor instances that I'm really excited I wrote myself are:

```haskell
data Notorious g o a t =
 Notorious (g o) (g a) (g t)

instance (Functor a) => Functor (Notorious a b c) where
  fmap f (Notorious a b c) = Notorious a b (fmap f c)
```

so the data constructor here is `Notorious g o a t` it takes 3 parameters in the type constructor to fully a type of `Notorious g o a t`. These 3 parameters are `(g o) (g a) (g t)` and what constructs the type is `Notorious`. To make this a Functor we first write an instance of Functor which has a higher-kinded type of `* -> *` so we must make sure that what we make the functorial structure of `Notorious g o a t` has a kind of `* -> *`. In my code snippet I made the functorial structure `Notorious a b c` and thats perfectly valid since `a b c` gets binded to `Notorious g o a`. If we look at the types of `g o a` respectively we see that `g :: * -> *` and `Notorious :: * -> * -> *`. Because `g` is of kind `* -> *` we can also make that a functor which will allow us to fmap a function _(or lift)_ over things that a functorial structure of `g`. In our definition `fmap f (Notorious a b c) =` is binded to the type arguments `(g o) (g a) (g t)` and the functiorial structure of Notorious itself is `g o a` this means that `t` is the only thing that is not apart of the functorial structure and thus the only value we are allowed to apply functions to. The `t` however is inside another functorial structure `(g t)` , but since we also made `g` a functor we can fmap over it too `Notiorious a b (fmap f c)` which expands out to:

`Notorious (g o) (g a) (fmap f (g t))` -> `g (f t)`

The second example is:

```haskell
data TalkToMe a =
    Halt
  | Print String a
  | Read (String -> a)

instance Functor TalkToMe where
  fmap _ Halt = Halt
  fmap f (Print b a) = Print b (f a)
  fmap f (Read a) = Read (f . a)
```

The type here is `TalkToMe a` and it has 3 type constructors: `Halt`,`Print String a`, and `Read (String -> a)`. The first thing I realized was, "Oh my god? Is that a function inside a type constructor?". That was my first time seeing that folks I'm just a newb. Anyway I broke down the types and managed to come to a solution

> When in doubt look at the types

So to make this a functor the functorial structure needs to have a kind of `* -> *`, otherwise whats the point right? The kind of `TalkToMe a` is `*` so we should use `TalkToMe`. There is nothing we can do with a type of `Halt` so fmapping `Halt` is just `Halt`. `Print String a` is actually just a function that takes two inputs, `Print a b`. Since the functorial structure of `TalkToMe` leaves out `a` that is the only value we can apply functions to so we preserve its structure by applying our `f` in `fmap f (Print b a)` to only the `a` and returning the same structure `Print b (f a)` is just `Print String a` with `a` transformed; the structure is `Print String`. Now all that is left is to define how `fmap` will behave for the last type constructor `Read (String -> a)`. First thing I did was abstract out the fact that `(String -> a)` could just be a higher order polymorphic type `a` that has a type of `String -> a` . I pretty much just assigned it the name `a` and then realized I could probably compose it since I know that the type of `f` is `(a -> b)` from the type signature of `fmap :: (a -> b) -> (f a) -> (f b)`. Thats how I ended up with the really cool looking `= Read (f . a)`. Since the type of `( . ) :: (b -> c) -> (a -> b) -> (a -> c)`, the `(b -> c)` our `f` and the `(a -> b)` is our `a` so composition works just fine and this is also perfectly valid.

# Amazing Resources/Favorite Papers !!!!!!

I didn't explain Functors fully there are some underlying mechanics I did not address, but you can find all of it in this beautiful [paper](http://web.cecs.pdx.edu/~mpj/pubs/fpca93.pdf) by Mark Peyton Jones on overloading and implicit higher order polymorphism. It pretty much explains everything I couldn't such as "why" this works, "how" this works, "rules" for doing this. Yes there are some "rules" or better yet laws you must follow when writing Functor instances or any other algebraic structure to ensure that things behave the way they should. Being new to functional programming and Haskell these basic concepts absolutely astonish and excite me and I really wanted to write about it. If there's anything I didn't quite phrase correctly feel free to [email me](jportorreal77@gmail.com) I'd love to have good discussion on the topic
