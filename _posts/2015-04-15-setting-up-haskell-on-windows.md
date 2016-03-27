---
layout: post
title: Setting up Haskell on Windows
categories: ['Haskell']
summary: A clean, minimal, Haskell installation with GHC, Cabal, and absolutely no globally installed packages.
---

<div class="message">
  <p>Why do that and not use <a href="https://www.haskell.org/platform/">Haskell Platform</a> instead?</p>
  <p>From what I have experienced so far, Haskell Platform falls behind the latest GHC release, and ships with Packages that <a href="http://www.reddit.com/r/haskell/comments/2al3vx/how_do_you_avoid_the_cabal_hell/">one might not need</a>.</p>
</div>

Essentially there are two things needed:

* [GHC](https://www.haskell.org/ghc/) – Haskell's compiler and interactive environment.
* [Cabal](https://www.haskell.org/cabal/) – Haskell's build system, which also doubles as a package manager.

## GHC ##

* Download GHC for Windows from [here](https://www.haskell.org/ghc/download).
* Extract GHC contents to a folder and add to PATH
 * \ghc-<em>x.y.z</em>\bin
 * \ghc-<em>x.y.z</em>\mingw\bin

## Cabal ##

* Download Cabal for Windows from [here](https://www.haskell.org/cabal/download.html).
* Extract Cabal executable to a folder, open a command prompt (cmd.exe) and execute:
 * cabal-<em>x.y.z</em>.exe `install`
 * cabal-<em>x.y.z</em>.exe `update`
* Open a MSYS-compatible (e.g. Git Bash) shell and execute:
 * cabal-<em>x.y.z</em>.exe install `cabal-install`

<div class="message">
  <p>In the beggining I was wondering what a <em>MSYS-compatible shell</em> is, but it turns out <a href="http://stackoverflow.com/questions/3144082/difference-between-msysgit-and-cygwin-git">the shell offered by Git Bash will do fine</a>.</p>
  <p>I was also wondering what's the <em>difference between Cygwin and MinGW</em> and came across <a href="http://stackoverflow.com/questions/771756/what-is-the-difference-between-cygwin-and-mingw">this</a> <em>great</em> Q&A on Stack Overflow.</p>
</div>

* Delete cabal-<em>x.y.z</em>.exe (after `cabal-install` is completed successfully)
* Add `C:\Users\[YOUR USERNAME]\AppData\Roaming\cabal\bin` to PATH

## Did it work out? ##

Open a command prompt (cmd.exe) window or a Git Bash window and execute `where cabal`, which should print the path where cabal executable resides:

{% highlight bash %}
C:\Users\[YOUR USERNAME]\AppData\Roaming\cabal\bin\cabal.exe
{% endhighlight %}

In the same command prompt executing `ghci` should launch GHCi (Haskell's interactive environment): 

{% highlight text %}
GHCi, version 7.8.4: http://www.haskell.org/ghc/  :? for help
Loading package ghc-prim ... linking ... done.
Loading package integer-gmp ... linking ... done.
Loading package base ... linking ... done.
>
{% endhighlight %}

To get the `>` character show up in the prompt, add the following to `C:\Users\[YOUR USERNAME]\AppData\Roaming\ghc\ghci.conf`:

{% highlight text %}
:set prompt "> "
{% endhighlight %}

That's it – Happy Haskell Programming!
