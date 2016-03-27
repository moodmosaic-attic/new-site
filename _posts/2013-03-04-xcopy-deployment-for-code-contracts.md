---
layout: post
title: XCOPY deployment for Code Contracts
categories: ['White label']
summary: How to manually install Microsoft Code Contracts on your system.
---

If you don't wish to install [Code Contracts](http://research.microsoft.com/en-us/projects/contracts/) through the Windows Installer:

* Go to `\Program Files (x86)\MSBuild\(version)\Microsoft.Common.Targets\ImportAfter` folder.
* Create a new file with name `CodeContractsAfter.targets` and paste the following XML:

{% highlight xml %}
<Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <CodeContractsInstallDir Condition="'$(CodeContractsInstallDir)'==''">CONTRACTS_PATH</CodeContractsInstallDir>
  </PropertyGroup>
  <Import Condition="'$(CodeContractsImported)' != 'true' AND '$(DontImportCodeContracts)' != 'true'" 
          Project="$(CodeContractsInstallDir)MsBuild\v4.0\Microsoft.CodeContracts.targets" />
</Project>
{% endhighlight %}

* Extract the contents from the Code Contracts Windows Installer using the [lessmsi](https://code.google.com/p/lessmsi/) tool.
* Locate the `Contracts` folder in the extracted contents.
* Replace the `CONTRACTS_PATH` in `CodeContractsAfter.targets` with the actual `Contracts` folder location.