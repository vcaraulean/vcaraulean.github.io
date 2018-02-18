---
layout: post
title: "Migrating to new csproj format"
date: 2016-07-12 21:02
comments: true
categories: .net, csproj
---

We've got a bunch of projects at work that are using the _old style_ project file format. And since we're already on Visual Studio 2017 and Rider, we were looking to new csproj format were the most interesting benefit would be it's simplicity.

This is a *how to* guide on how to approach a such migration and description of few tricky moments and issues encountered.

## Upgrading the project

Simplest part. Replace the context of csproj with these lines:

```
<Project Sdk="Microsoft.NET.Sdk" ToolsVersion="15.0">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net462</TargetFramework>
  </PropertyGroup>
</Project>
```

## Upgrading package references

_Preparetive step, not strictly necessary._ Go to Nuget settings of your IDE of choice and set "Default package management format" to "PackageReference".

There's an important difference while using new `PackageReference` compared to previous `packages.config`. New format includes only "top level" packages - ones that you reference directly in your code. As such don't blindly add every package found in `packages.config` as a `PackageReference`. Instead:

 - Compile the project without any references
 - Follow compilation errors and edge them out one-by-one by adding missing packages

## Cleanup & miscellaneous
 
 - Remove AssemblyInfo.cs. Check if it contains any useful attributes & set them in project properties or csproj directly
 - Remove packages.config
 - Check if project contains any content/resource files. Include them explicitly in csproj.
```
	  <ItemGroup>
	    <Content Include="Mappings.csv">
	      <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
	    </Content>
	  </ItemGroup>
```

