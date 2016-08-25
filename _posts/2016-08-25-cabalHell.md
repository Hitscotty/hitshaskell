---
layout: post
title: My first Haskell Cabal Project
---
So stoked! I finally made it to chapter 13! So my task today in the wonderfull world of Haskell, was a little bit confusing at first because it required editing cabal files which I have never done before, but
when you throw around the words *"dependencies"* and *"sandbox"* you begin to feel like you're working on something important, so why not? I followed the instructions on setting up
a new cabal project to the T and still had compilation errors.<br></br> All I needed to do was fix the missing libraries cabal kept yelling at me to install, but ```cabal install --only-dependencies``` wasn't doing the trick and quickly turned into the desperate cry for "please just build" resorting to what I thought would work: ```cabal install --reinstall --only-dependencies```, but come the 5th time it still didn't work. I had to go beyond the books for this one and simply sat there studying my .cabal file for a good 30 mins and reasoned as to why my project wasn't building. The cabal *split* and *random* packages couldn't be found, but I realized that in the cabal file the build was depending on specific versions for these packages. <br></br> My exectuable looked this:

<pre>
executable hangman
      main-is:             Main.hs
      -- other-modules:
      -- other-extensions:
      build-depends:       base >=4.7 && <5
                         , random == 1.1
                         , split == 0.2.2
      hs-source-dirs:      src
      default-language:    Haskell2010
</pre>

So I figured maybe my cabal wasnt updated, or maybe my ghc was out of date, I don't know! I ```cabal update``` 'd and still I was missing these dependencies. I checked my version of ghc, but it was up to date too! I spent a lot of time trying to work through this and when I looked up the [*split*](https://hackage.haskell.org/package/split) and [*random*](https://hackage.haskell.org/package/random) libraries I found that there were actually many versions of these libraries. I decided to simply specifiy in my executable that it was ok to use an older version:
<pre>
executable hangman
  main-is:             Main.hs
  -- other-modules:
  -- other-extensions:
  build-depends:       base >=4.7 && <5
                     , random < 1.1
                     , split < 0.2.2
  hs-source-dirs:      src
  default-language:    Haskell2010
</pre>

This time when my dependencies installed! My cabal project "build'ed", "built", whatever the proper past tense of ```cabal build``` would be. After spending so much time trying to accomplish this, and I haven't even touched the code yet, I was not only proud of having deciphered cabal, but eager and excited to finally start working/coding this hangman game up.
