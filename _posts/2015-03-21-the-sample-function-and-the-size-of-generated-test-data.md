---
layout: fsquickcheck
title: Observing the size of generated test data
categories: ['FsCheck', 'QuickCheck']
summary: How generating some example values in QuickCheck differs from FsCheck.
---

## The *sample* function defined in QuickCheck ##

QuickCheck has a function called [`sample`](https://hackage.haskell.org/package/QuickCheck-2.8/docs/src/Test-QuickCheck-Gen.html#sample) which, given a generator, prints some examples values to the standard output (stdout).

It's signature is:

{% highlight haskell %}
sample :: Show a => Gen a -> IO ()
{% endhighlight %}

### Example ###

Using the [`choose`](https://hackage.haskell.org/package/QuickCheck-2.8/docs/src/Test-QuickCheck-Gen.html#choose) generator we can use `sample` to generate random integers in the inclusive range [0, 9] and print them:

{% highlight haskell %}
λ sample $ choose (0, 9 :: Int)
{% endhighlight %}

Output:

{% highlight text %}
1
5
0
2
3
4
6
5
5
4
1
{% endhighlight %}

## The *sample* function defined in FsCheck ##

FsCheck also defines a `sample` function, though it behaves in a slightly different way, as it takes two additional arguments `size` and `n`:

{% highlight fsharp %}
val sample : size:int -> n:int -> gn:Gen<'a> -> 'a list

// `size` is the size of generated test data
// `n` is the number of samples (defaults to 11 in QuickCheck)
{% endhighlight %}

Let's pause here and run `sample` by supplying different values for `size`.

### Using 0 for *size* ###

{% highlight fsharp %}

open FsCheck.Gen

let sample'  = sample 0 11 // Size = 0, Number of samples = 11
let sample'' = sample' <| choose (0, 9)
{% endhighlight %}

Output:

{% highlight text %}
Length = 11
    [0]: 4
    [1]: 0
    [2]: 4
    [3]: 0
    [4]: 3
    [5]: 9
    [6]: 3
    [7]: 9
    [8]: 2
    [9]: 8
    [10]: 2
{% endhighlight %}

However, since size is `0` the result should be:

{% highlight text %}
Length = 11
    [0]: 0
    [1]: 0
    [2]: 0
    [3]: 0
    [4]: 0
    [5]: 0
    [6]: 0
    [7]: 0
    [8]: 0
    [9]: 0
    [10]: 0
{% endhighlight %}

### Using 1000 for *size* ###

{% highlight fsharp %}
open FsCheck.Gen

let sample'  = sample 1000 11 // Size = 1000, Number of samples = 11
let sample'' = sample' <| choose (0, 9)
{% endhighlight %}

Output:

{% highlight text %}
Length = 11
    [0]: 8
    [1]: 6
    [2]: 2
    [3]: 3
    [4]: 7
    [5]: 3
    [6]: 1
    [7]: 2
    [8]: 6
    [9]: 2
    [10]: 0
{% endhighlight %}

The above result looks incorrect, in fact it looks the same as the first result, where size was 0.

Since size is `1000` the result should look like:

{% highlight text %}
Length = 11
    [0]: -78
    [1]: 347
    [2]: 242
    [3]: 707
    [4]: 414
    [5]: 49
    [6]: 551
    [7]: 45
    [8]: 897
    [9]: -99
    [10]: 77
{% endhighlight %}

At this point, we observed that if a generator ignores *size* (e.g. as `choose` does) then `sample` ignores it's `size` parameter.

---

## Just *sample* with a different generator, then. ##

Instead of the `choose` generator, the following example uses `generate<int>`.

### Using 0 for *size* ###

{% highlight fsharp %}
open FsCheck.Arb
open FsCheck.Gen

let sample'  = sample 0 11 // Size = 0, Number of samples = 11
let sample'' = sample' <| generate<int>
{% endhighlight %}

The results are now looking good:

{% highlight text %}
Length = 11
    [0]: 0
    [1]: 0
    [2]: 0
    [3]: 0
    [4]: 0
    [5]: 0
    [6]: 0
    [7]: 0
    [8]: 0
    [9]: 0
    [10]: 0
{% endhighlight %}

That's because `generate<int>` takes `size` into account.

### Using 1000 for *size* ###

{% highlight fsharp %}
open FsCheck.Arb
open FsCheck.Gen

let sample'  = sample 1000 11 // Size = 1000, Number of samples = 11
let sample'' = sample' <| generate<int>
{% endhighlight %}

The results are now looking good:

{% highlight text %}
Length = 11
    [0]: -98
    [1]: 307
    [2]: 142
    [3]: 507
    [4]: 414
    [5]: 39
    [6]: 501
    [7]: 4
    [8]: 807
    [9]: -81
    [10]: 31
{% endhighlight %}

---

## How to *always* control the size of test data, then? ##

That seems to be the purpose of the `sized` function.

For the sake of completeness, here's the original example with `choose` rewritten to use `sized`:

### *sample* and *choose*, using 0 for *size* ###

{% highlight fsharp %}
open FsCheck.Gen

let sample'  = sample 0 11 // Size = 0, Number of samples = 11
let sample'' = sample' <| (sized <| fun s -> choose (0, s))
{% endhighlight %}

Results to the following, which is now correct:

{% highlight text %}
Length = 11
    [0]: 0
    [1]: 0
    [2]: 0
    [3]: 0
    [4]: 0
    [5]: 0
    [6]: 0
    [7]: 0
    [8]: 0
    [9]: 0
    [10]: 0
{% endhighlight %}

### *sample* and *choose*, using 1000 for *size* ###

{% highlight fsharp %}
open FsCheck.Gen

let sample'  = sample 1000 11 // Size = 1000, Number of samples = 11
let sample'' = sample' <| (sized <| fun s -> choose (-s, s))
{% endhighlight %}

Results to the following, which now looks correct:

{% highlight text %}
Length = 11
    [0]: 105
    [1]: 682
    [2]: -239
    [3]: 658
    [4]: -6
    [5]: 9
    [6]: 203
    [7]: -43
    [8]: 304
    [9]: -125
    [10]: -423
{% endhighlight %}

## References ##

* QuickCheck
  * [Size in QuickCheck 1 (source code)](http://hackage.haskell.org/package/QuickCheck-1.2.0.1/docs/src/Test-QuickCheck.html#Config)
  * [Size in QuickCheck 2 (answer on Stack Overflow)](http://stackoverflow.com/a/9992112/467754)
  * [Sizing a test](http://www.scs.stanford.edu/11au-cs240h/notes/testing-slides.html#(30))
* FsCheck
  * [The size of test data](https://fsharp.github.io/FsCheck/TestData.html)
  * [Purpose of 'size' parameter in Gen.sample](https://github.com/fsharp/FsCheck/issues/87)

## Appendix ##

The above F# examples use the backward pipe `<|` operator to look similar to the Haskell examples. In F#, [it's idiomatic to use the forward pipe operator](http://stackoverflow.com/a/1459204/467754) instead.</p>

Using the forward pipe operator, the previous example would be:

{% highlight fsharp %}
let result =
    (fun s -> Gen.choose (-s, s))
    |> Gen.sized
    |> Gen.sample 1000 11
{% endhighlight %}
