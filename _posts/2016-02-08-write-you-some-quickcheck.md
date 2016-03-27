---
layout: post
title: Write you some QuickCheck
categories: ['QuickCheck']
summary: Blog post series on implementing a minimal version of QuickCheck from scratch.
---

In the next couple of posts, I'm going to implement a minimal version of QuickCheck _from scratch_. Follow along and you'll:

 * learn how [QuickCheck](https://hackage.haskell.org/package/QuickCheck) works
 * become better at using it[^1]
 * see a bit of [Quviq's QuickCheck in Erlang](http://quviq.com/documentation/eqc/index.html) as well

I'll be porting tons of Haskell code in F#[^2]. — Disclaimer: I've [done](http://blog.nikosbaxevanis.com/2011/11/19/notes-from-porting-java-code-to-net/) [similar](http://blog.nikosbaxevanis.com/2011/11/24/fare-finite-automata-regex-in-net/) [things](http://blog.nikosbaxevanis.com/2015/09/25/regex-constrained-strings-with-fscheck/) in the past. It's fun!

Here's a list of posts to follow:

 * [Write you some QuickCheck: Prelude](/2016/02/09/write-you-some-quickcheck-prelude/)

 * [Write you some QuickCheck: Generating random bytes](/2016/02/10/write-you-some-quickcheck-generating-random-bytes/)
 * [Write you some QuickCheck: Generating random characters](/2016/02/11/write-you-some-quickcheck-generating-random-characters/)
 * [Write you some QuickCheck: Generating random booleans](/2016/02/12/write-you-some-quickcheck-generating-random-booleans/)
 * [Write you some QuickCheck: Generating random 32-bit signed integers](/2016/02/13/write-you-some-quickcheck-generating-random-integers/)
 * [Write you some QuickCheck: Generating random 64-bit signed integers](/2016/02/17/write-you-some-quickcheck-generating-random-long-integers/)
 * [Write you some QuickCheck: Generating random 32-bit floating-points](/2016/02/25/write-you-some-quickcheck-generating-random-floats/)
 * [Write you some QuickCheck: Generating random lists](/2016/02/19/write-you-some-quickcheck-generating-random-lists/)
 * [Write you some QuickCheck: Generating random strings](/2016/02/22/write-you-some-quickcheck-generating-random-strings/)

 * [Write you some QuickCheck: Shrinking numbers](/2016/03/17/write-you-some-quickcheck-shrinking-numbers/)
 * Write you some QuickCheck: Shrinking bytes
 * Write you some QuickCheck: Shrinking characters
 * Write you some QuickCheck: Shrinking booleans
 * Write you some QuickCheck: Shrinking lists
 * Write you some QuickCheck: Shrinking strings

 * Write you some QuickCheck: Properties of type boolean
 * Write you some QuickCheck: Properties of type unit
 * Write you some QuickCheck: Properties that hold under certain conditions
 * Write you some QuickCheck: Properties that are labeled
 * Write you some QuickCheck: Properties that are labeled conditionally

 * Write you some QuickCheck: Putting everything together
 * Write you some QuickCheck: Where's the Arbitrary class?

---

[^1]: This is advanced material. Basic knowledge of QuickCheck and property-based testing is assumed. If you're in the beginning it's fine! Read this [post](http://fsharpforfunandprofit.com/posts/property-based-testing/) first by [Scott Wlaschin](http://fsharpforfunandprofit.com/), followed by this [Pluralsight course](https://www.pluralsight.com/courses/fsharp-property-based-testing-introduction) by [Mark Seemann](http://blog.ploeh.dk/), followed by these [slides](http://www.dcc.fc.up.pt/~pbv/aulas/tapf/slides/quickcheck.html) by [Pedro Vasconcelos](http://www.dcc.fc.up.pt/~pbv/index_en.html). There's also this [short intro](/2016/02/09/short-intro-to-quickcheck/) as well.

[^2]: If you're just looking for a production-ready QuickCheck based tool in F#, use [FsCheck](https://github.com/fscheck/FsCheck).
