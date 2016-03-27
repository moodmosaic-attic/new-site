---
layout: post
title: Write you some QuickCheck - Generating random bytes
categories: ['QuickCheck']
summary: Blog post series on implementing a minimal version of QuickCheck from scratch.
---

*This post is part of a [series of posts on implementing a minimal version of QuickCheck](/2016/02/08/write-you-some-quickcheck/) from scratch. The source code is [available on GitHub](https://gist.github.com/moodmosaic/65c576732722b3b7a200).*

We already have a function that runs a generator `Gen<'a>` and returns random test data of type `'a`.

<!-- Until rouge highlights F# syntax, use OCaml -->
{% highlight ocaml %}
val generate : Gen<'a> -> 'a
{% endhighlight %}

Now, in order to be able to generate random bytes, we need a couple of generators:

* a generator that picks numbers from a given range (in this case [0..255])
* a generator that applies a function (in this case [`Operators.byte<^T>`](https://msdn.microsoft.com/en-us/library/ee353839.aspx)) to an existing generator

Porting Gen's `choose`

<!-- Until rouge highlights F# syntax, use OCaml -->
{% highlight ocaml %}
/// <summary>
/// Generates a random element in the given inclusive range, uniformly
/// distributed in the closed interval [lo,hi].
/// </summary>
/// <param name="lo">The lower bound.</param>
/// <param name="hi">The upper bound.</param>
let choose (lo, hi) = Gen(fun n r -> r) |> Gen.map (Random.range (lo, hi) >> fst)
{% endhighlight %}

Porting Gen's `map`, `bind`, and `return`[^1] since `choose` depends on them

<!-- Until rouge highlights F# syntax, use OCaml -->
{% highlight ocaml %}
/// <summary>
/// Sequentially compose two actions, passing any value produced by the first
/// as an argument to the second.
/// </summary>
/// <param name="f">
/// The action that produces a value to be passed as argument to the generator.
/// </param>
let bind (Gen m) f =
    Gen(fun n r ->
        let (r1, r2) = r |> Random.split
        let (Gen m') = f (m n r1)
        m' n r2)

/// <summary>
/// Injects a value into a generator.
/// </summary>
/// <param name="a">The value to inject into a generator.</param>
let init a = Gen(fun n r -> a)

/// <summary>
/// Returns a new generator obtained by applying a function to an existing
/// generator.
/// </summary>
/// <param name="f">The function to apply to an existing generator.</param>
/// <param name="m">The existing generator.</param>
let map f m =
    bind m (fun m' ->
        init (f m'))
{% endhighlight %}

All the pieces are now in place, and so a byte generator can be written as:

<!-- Until rouge highlights F# syntax, use OCaml -->
{% highlight ocaml %}
let byte = Gen.choose (0, 255) |> Gen.map Operators.byte

val byte : Gen<byte>
{% endhighlight %}

Finally, here are some sample bytes:

<!-- Until rouge highlights F# syntax, use OCaml -->
{% highlight ocaml %}
> Gen.byte |> Gen.generate;;
val it : byte = 125uy

> Gen.byte |> Gen.generate;;
val it : byte = 201uy

> Gen.byte |> Gen.generate;;
val it : byte = 3uy
{% endhighlight %}

---

[^1]: In F#, `return` is already taken, so I used `init` instead.