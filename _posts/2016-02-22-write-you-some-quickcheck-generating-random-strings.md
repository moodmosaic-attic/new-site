---
layout: post
title: Write you some QuickCheck - Generating random strings
categories: ['QuickCheck']
summary: Blog post series on implementing a minimal version of QuickCheck from scratch.
---

*This post is part of a [series of posts on implementing a minimal version of QuickCheck](/2016/02/08/write-you-some-quickcheck/) from scratch. The source code is [available on GitHub](https://gist.github.com/moodmosaic/65c576732722b3b7a200).*

In this post I'll be generating random strings from randomly selected characters. I will also describe a way of customizing the whole creation process.

The following generators are required for this:

* a [generator for characters](/2016/02/11/write-you-some-quickcheck-generating-random-characters)
* a [generator for lists](/2016/02/19/write-you-some-quickcheck-generating-random-lists) of a given generator
* a [generator that applies a function](/2016/02/10/write-you-some-quickcheck-generating-random-bytes/) to an existing generator

Given the above, a generator for strings can be written as:

<!-- Until rouge highlights F# syntax, use OCaml -->
{% highlight ocaml %}
/// <summary>
/// Generates a random string.
/// </summary>
let string =
    Gen.char
    |> Gen.list
    |> Gen.map (List.toArray >> System.String)
{% endhighlight %}

Here are some sample strings:

<!-- Until rouge highlights F# syntax, use OCaml -->
{% highlight ocaml %}
> Gen.string |> Gen.generate;;
val it : String = "lof5ºÛ"

> Gen.string |> Gen.generate;;
val it : String = "'fv*ì UÇ"

> Gen.string |> Gen.generate;;
val it : String = "Ðê

{% endhighlight %}

## Customizations ##

Generarting **upper-case strings**

<!-- Until rouge highlights F# syntax, use OCaml -->
{% highlight ocaml %}
let upperCase = Gen.choose (65,  90) |> Gen.map Operators.char

val upperCase : Gen<char>


let customString g =
    g
    |> Gen.list
    |> Gen.map (List.toArray >> System.String)

val customString : g:Gen<char> -> Gen<String>
{% endhighlight %}

Sample output:

<!-- Until rouge highlights F# syntax, use OCaml -->
{% highlight ocaml %}
> upperCase |> customString |> Gen.generate;;
val it : String = "DGVHAYFAWIHQBOD"

> upperCase |> customString |> Gen.generate;;
val it : String = "HEDDZYBATTYL"

> upperCase |> customString |> Gen.generate;;
val it : String = "XLYJSMEYYZXN"

{% endhighlight %}

Generating **lower-case strings**

<!-- Until rouge highlights F# syntax, use OCaml -->
{% highlight ocaml %}
let lowerCase = Gen.choose (97, 122) |> Gen.map Operators.char

val lowerCase : Gen<char>


let customString g =
    g
    |> Gen.list
    |> Gen.map (List.toArray >> System.String)

val customString : g:Gen<char> -> Gen<String>
{% endhighlight %}

Sample output:

<!-- Until rouge highlights F# syntax, use OCaml -->
{% highlight ocaml %}
> lowerCase |> customString |> Gen.generate;;
val it : String = "qsnj"

> lowerCase |> customString |> Gen.generate;;
val it : String = "cltfvqckqxeirmuqvjn"

> lowerCase |> customString |> Gen.generate;;
val it : String = "wzwfojfjlrjglu"

{% endhighlight %}