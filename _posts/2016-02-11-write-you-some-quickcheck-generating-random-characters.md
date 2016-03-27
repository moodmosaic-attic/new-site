---
layout: post
title: Write you some QuickCheck - Generating random characters
categories: ['QuickCheck']
summary: Blog post series on implementing a minimal version of QuickCheck from scratch.
---

*This post is part of a [series of posts on implementing a minimal version of QuickCheck](/2016/02/08/write-you-some-quickcheck/) from scratch. The source code is [available on GitHub](https://gist.github.com/moodmosaic/65c576732722b3b7a200).*

In the [previous](/2016/02/10/write-you-some-quickcheck-generating-random-bytes/) post I've generated random bytes. In this post, I'll be generating random characters.

I'm going to need a couple of generators for this:

* one that picks numbers from the range (in HEX)[^1]
  * `[20h..7Eh]` for ASCII printable characters
  * `[80h..FFh]` for ASCII extended characters
* a generator that randomly uses one of the above generators
* a generator that applies a function (in this case [`Operators.char<^T>`](https://msdn.microsoft.com/library/ee370353.aspx) to the above)

Porting Gen's `oneof`

<!-- Until rouge highlights F# syntax, use OCaml -->
{% highlight ocaml %}
/// <summary>
/// Randomly uses one of the given generators.
/// </summary>
/// <param name="gens">The input list of generators to use.</param>
/// <remarks>
/// The input list must be non-empty.
/// </remarks>
let oneof gens =
    let join x = Gen.bind x id
    join (Gen.elements gens)
{% endhighlight %}

Porting Gen's `elements` since `oneof` depends on it

<!-- Until rouge highlights F# syntax, use OCaml -->
{% highlight ocaml %}
/// <summary>
/// Generates one of the given values.
/// </summary>
/// <param name="xs">The input list.</param>
/// <remarks>
/// The input list must be non-empty.
/// </remarks>
let elements xs =
    // http://stackoverflow.com/a/1817654/467754
    let flip f x y = f y x
    Gen.choose (0, (Seq.length xs) - 1) |> Gen.map (flip Seq.item xs)
{% endhighlight %}

All the pieces are now in place, and so a char generator can be written as:

<!-- Until rouge highlights F# syntax, use OCaml -->
{% highlight ocaml %}
let char =
    Gen.oneof [ Gen.choose ( 32, 126)
                Gen.choose (127, 255) ]
    |> Gen.map Operators.char

val char : Gen<char>
{% endhighlight %}

Finally, here are some sample characters:

<!-- Until rouge highlights F# syntax, use OCaml -->
{% highlight ocaml %}
> Gen.char |> Gen.generate;;
val it : char = 'æ'

> Gen.char |> Gen.generate;;
val it : char = 'W'

> Gen.char |> Gen.generate;;
val it : char = 'h'

> Gen.char |> Gen.generate;;
val it : char = 'à'

> Gen.char |> Gen.generate;;
val it : char = 'ì'

> Gen.char |> Gen.generate;;
val it : char = '&'

> Gen.char |> Gen.generate;;
val it : char = ' '

> Gen.char |> Gen.generate;;
val it : char = 'o'

> Gen.char |> Gen.generate;;
val it : char = 'X'

{% endhighlight %}

---

[^1]: I used the ASCII codes from [this](/images/ascii-codes-table.png) table.
