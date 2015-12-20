---
title: So I got my own Lisp now (mostly)
tags: haskell, lisp
---

Over the past week, I have been trying to work through [Write Yourself a Scheme in 48 Hours](https://en.wikibooks.org/wiki/Write_Yourself_a_Scheme_in_48_Hours/).
Let's first establish the fact that this book is awesome, because a free, comprehensive tutorial to write one's own Lisp in Haskell practically spells A-W-E-S-O-M-E.
Given my beginner's background in Haskell and the fact that I haven't touched Scheme since 2008, this exercise has helped me gain a new appreciation for both languages,
especially Haskell. I have started to see why Grabiel Gonzalez rated the language as the [best in class for writing compilers](http://www.haskellforall.com/2015/08/state-of-haskell-ecosystem-august-2015.html#compilers)
Anyway, I got pretty far through the book without too much trouble. In fact, I was able to correct some of the outdated information such as deprecating Error monad
in favor of the newer Except and fixing some stylistic inconsistencies on my own. It has increased my confidence in continuing with the language. Nevertheless, I got
a bit overwhelmed towards the end of the book, especially after the introduction of Monad transformers and stateful environment. I still don't feel like I have a complete
grasp of the concept yet, even though I could follow along with the instructions. So I am putting this project on hold and take a detour to learn about these two concepts
in details before completing it. I still have a fully functional Lisp implementation with a REPL up and running though, just no I/O and standard library yet.

### Highlights

- I completely understand Monad now and have familiarized myself with quite a few of them, including the Parser monad.
- I also have a firm grasp on Error handling with [Control.Monad.Except](https://hackage.haskell.org/package/mtl-2.2.1/docs/Control-Monad-Except.html), which is pretty nice.
- It always feels good to get a refresher on Lisp's grammar.
- Learning more about parser combinator is amazing.

> <img src="../images/leme_demo.gif" width="600px">
> <figcaption><strong>Fig. 1</strong> | Leme demo</figcaption>

### Caveats

- The book gets really dense towards the end, especially for a beginner. I would have enjoyed it a lot more if I had been acquainted with Monad transformers and stateful environments first.
- Some of the information is a bit outdated, most notably the Error monad. It's not a bigge though.
- Sometimes the code switches between if/else, guards, case statement and function composition a bit arbitrarily. Again, very minor concern.

### Conclusion

As mentioned above, I will take a break from this project and dive into monad transformers as well as state handling instead. It feels good to have a clear direction
after yet another great exercise with Haskell.
