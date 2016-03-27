---
layout: post
title: In-memory Drain abstractions applied to A Functional Architecture with F#
categories: ['FSharp']
summary: This post shows how to apply the Drain filter to the master branch of the Pluralsight course about A Functional Architecture with F# by Mark Seemann.
---

This post shows how to apply the [Drain](http://blog.ploeh.dk/2014/07/23/drain/) filter abstraction to the master branch of the Pluralsight course about [A Functional Architecture with F#](http://pluralsight.com/training/Courses/TableOfContents/functional-architecture-fsharp) by [Mark Seemann](http://blog.ploeh.dk/).

As the diff-output shows, applying filtering with Drains cuts the maintenance of multiple homogenous abstractions, and makes the code cleaner and easier to reason about.

{% gist 98d8d1854d66f58e0500 %}

<p class="message">For or a better diff-output highlighting experience, there is also a Gist available <a href="https://gist.github.com/moodmosaic/98d8d1854d66f58e0500">here</a>.</p>

To apply the diff, access the source code by getting [a Pluralsight subscription](http://pluralsight.com/training/Products/Individual) which is totally worth it for this course.