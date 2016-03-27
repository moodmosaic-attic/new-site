---
layout: post
title: How to enable Code Analysis for F# projects
categories: ['FSharp']
summary: Code Analysis (FxCop) is disabled by default for F# projects. But if you're eager to know what FxCop can report on F# code, here's how to enable it.
---

As of today, in Visual Studio 2012 the *Run Code Analysis* command is not available for F# projects.

<p class="message"><a href="http://msdn.microsoft.com/en-us/library/3z0aeatx.aspx">Code Analysis</a> is a Visual Studio feature for analyzing managed code assemblies and gathering information such as possible design, localization, performance, and security improvements.</p>

Code Analysis can be used as a stand-alone tool with the [FxCopCmd](http://msdn.microsoft.com/en-us/library/bb429474.aspx) command-line application and  it is also possible to be included to automated build processes such is [MSBuild](http://msdn.microsoft.com/en-us/library/wea2sca5.aspx).

## Enable Code Analysis in MSBuild

To enable Code Analysis in MSBuild, include the following elements in the project file:

{% highlight xml %}
<CodeAnalysisModuleSuppressionsFile>GlobalSuppressions.fs</CodeAnalysisModuleSuppressionsFile>
<RunCodeAnalysis>true</RunCodeAnalysis>
<CodeAnalysisRuleSet>My.ruleset</CodeAnalysisRuleSet>    
{% endhighlight %}

## Problem

Unfortunately, even with the above elements, Code Analysis will not run for F# projects during the MSBuild process.

## Solution

Edit the Microsoft.CodeAnalysis.Targets file which is located at:  
C:\Program Files (x86)\MSBuild\Microsoft\VisualStudio\v11.0\CodeAnalysis

Append `or '$(Language)'=='F#'` at the following locations:

{% highlight xml %}
<DefineConstants Condition="'$(Language)'=='C#' ">CODE_ANALYSIS;$(DefineConstants)</DefineConstants>
<PropertyGroup Condition="'$(Language)'=='C#' or '$(Language)'=='VB'">
{% endhighlight %}

You may also download the modified Microsoft.CodeAnalysis.Targets file from [here](/downloads/Microsoft.CodeAnalysis.Targets.zip).