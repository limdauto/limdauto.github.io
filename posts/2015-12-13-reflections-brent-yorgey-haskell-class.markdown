---
title: Reflection on Brent Yorgey's Haskell Class
---

I have just finished working through [Brent Yorgey's Haskell class](http://www.seas.upenn.edu/~cis194/spring13/). Although I skipped one week on I/O, or rather I switched to some other exercises, because I disliked that particular assignment, I have to say this is probably best free Haskell guide for beginners on the Internet.

### 1. Roadmap

- **Before**: Admittedlly, before starting with this course, I had read through [Learn you a Haskell](http://learnyouahaskell.com/chapters) without attempting anycode. Needless to say, nothing really stuck. Although I developed a very high level appreciation for the language, I was still starting from 0.
- **Process**: I spent roughly a month on 12-week worth of material and assignments, excluding the final project. Each assignment took me between 3 to 8 hours to complete. Here is the repository of [my solutions](https://github.com/limdauto/learning-haskell) and below is the commit graph:

> <img src="../images/cis194.png" width="550px">
> <figcaption><strong>Fig. 1</strong> | Commit graph</figcaption>

- **After**: After finishing all the exercises, I am now confident in my understanding of all major concepts and feel ready to deep dive into real-world projects. In particular, I think I get Applicative Functor and Monad now. I really, really like the applicative style of programming. Besides, it feels to me that the applicative laws are a lot more intuitive to understand than the monadic laws.

### 2. Highlights:

<blockquote class="typl8-pull-quote">
<p>
[...] the essence of Applicative programming: computations have a fixed structure, given by the pure function, and a sequence of subcomputations, given by the effectful arguments.
</p>
*McBride & Paterson*
</blockquote>

- I highly recommend the exercise in [week 6](https://github.com/limdauto/learning-haskell/blob/master/cis194/week6/Fibs.hs), which teaches you how to calculate Fibonacci sequence using generating functions with an infinite data structure. That's f*cking awesome.
- The Applicative Parser exercises in week 10-11 are also worth thinking deeply about. In addition, reading the [applicative paper](http://www.staff.city.ac.uk/~ross/papers/Applicative.html) with a pen and paper provides a lot of good insights into how the applicative typeclass came about and how it is used.
- Everyone should read [The Trivial Monad](http://blog.sigfpe.com/2007/04/trivial-monad.html) if they are struggling to understand this structure. As I feel like monad's been hyped way above its real difficulty level, this article will bring it back down to earth.
- I was tripped up by some tricky concept and asked about it on [SO](http://stackoverflow.com/questions/34244574/trouble-understanding-the-type-of-sequence-just-just). The answer blew me away.

### 3. Next step:

I'm pretty happy with my progress at the moment. Next week I will follow the instructions in [Write yourself a Scheme in 48 hours](https://en.wikibooks.org/wiki/Write_Yourself_a_Scheme_in_48_Hours) and finally write my own SQL parser for a project I have been dreaming of.
