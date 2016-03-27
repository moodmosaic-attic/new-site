---
layout: post
title: Auto-Mocking with Foq and AutoFixture
categories: ['AutoFixture']
summary: A new auto-mocking extension have been recently added to AutoFixture allowing the mock instances to be created by Foq.
---

A new Auto-mocking extension have been recently added to [AutoFixture](http://github.com/autofixture/autofixture) allowing the mock instances to be created by [Foq](http://foq.codeplex.com/). 

This brings the total number of Auto-mocking extensions for AutoFixture up to five:

* [AutoFixture.AutoMoq](http://nuget.org/packages/AutoFixture.AutoMoq)
* [AutoFixture.AutoRhinoMocks](http://nuget.org/packages/AutoFixture.AutoRhinoMocks)
* [AutoFixture.AutoFakeItEasy](http://nuget.org/packages/AutoFixture.AutoFakeItEasy)
* [AutoFixture.AutoNSubstitute](http://nuget.org/packages/AutoFixture.AutoNSubstitute)
* [AutoFixture.AutoFoq](http://nuget.org/packages/AutoFixture.AutoFoq)

## Install

To install AutoFixture with Auto Mocking using Foq, run the following command in the Package Manager Console:

{% highlight text %}
PM> Install-Package AutoFixture.AutoFoq
{% endhighlight %}

To use it, add an `AutoFoqCustomization` to the `Fixture` instance:

<!-- Until rouge highlights F# syntax, use OCaml -->
{% highlight ocaml %}
let fixture = Fixture().Customize(AutoFoqCustomization())
{% endhighlight %}

## Usage

In the test below the mocked instance is created automatically by Foq:

<!-- Until rouge highlights F# syntax, use OCaml -->
{% highlight ocaml %}
[<Fact>]
let AutoMockInterfaceAndSetupExpectations() =
    // Fixture setup
    let fixture = Fixture().Customize(AutoFoqCustomization())
    let dummy = obj()
    // Exercise system
    let sut = fixture.Create<IInterface>()
    sut.MakeIt(dummy) |> ignore
    // Verify outcome
    Mock.Verify(<@ sut.MakeIt(dummy) @>, Times.Once)
    // Teardown
{% endhighlight %}

The above test can be written declaratively using [AutoData](http://blog.ploeh.dk/2010/10/08/AutoDataTheoriesWithAutoFixture.aspx) theories:

<!-- Until rouge highlights F# syntax, use OCaml -->
{% highlight ocaml %}
[<Theory>][<AutoFoqData>]
let AutoMockInterfaceAndSetupExpectationsDeclaratively 
  (sut: IInterface, dummy: obj) =
    sut.MakeIt(dummy) |> ignore
    Mock.Verify(<@ sut.MakeIt(dummy) @>, Times.Once)
{% endhighlight %}

The `[AutoFoqData]` attribute is defined as:

<!-- Until rouge highlights F# syntax, use OCaml -->
{% highlight ocaml %}
type AutoFoqDataAttribute() = 
    inherit AutoDataAttribute(
        Fixture().Customize(AutoFoqCustomization()))
{% endhighlight %}

An automatically published release created from the latest successful build can be also downloaded from [here](http://autofixture.codeplex.com/releases/).