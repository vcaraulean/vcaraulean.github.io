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

## Pitfalls & possible issues

 - New project will pick up *all* .cs files. Some leftovers might get in the project. Fix: review & remove unnecessary.
 - Warnings about binding redirects. Fix: open `app.config` and remove mentioned assemblies. 
 - Compilation errors in `AssemblyInfo.cs` about duplicated definitions. Fix: remove AssemblyInfo.cs
 - Output folder now includes target framework moniker. Example: `bin\Debug\net462`. If any build or deployment scripts are relying on old path, then it has to be fixed.
 - Additional files that have to exist in output folder aren't automatically included. Fix: any additional resource files have to be included explicitly in new csproj. 
 - `Directory.GetCurrentDirectory()` returns a different thing than before. Affects `Directory.Exists`, `Directory.CreateDirectory` and probably any other API that are accepting relative paths. Apparently, new CLI tooling is setting default *Working Directory* (in both VS2017 and Rider) to location of the project file, contrary to output folder.

 
