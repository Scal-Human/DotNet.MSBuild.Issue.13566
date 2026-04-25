# DotNet.MSBuild.Issue.13566

Compile item group not populated without warning message.

Was wrongly titled "EnableDefaultCompileItems forced to false without warning message."

[See on GitHub](https://github.com/dotnet/msbuild/issues/13566)

## Context

While defining a custom Sdk that is a wrapper around the standard Microsoft.NET.Sdk,
I came to a situation where I defined properties and items early in the process, before the import of the standard Sdk.

I know now this is an error and items must be defined later in a target.

## The bug

Because of unrelated properties/items, all of a sudden the project does not compile anymore.

The bug is to set `EnableDefaultCompileItems` to `false` without informing the user resulting in an empty `Compile` item.

## Reproduce the error

Clone the repo
```
cd Source\SomeProject
dotnet build
```
result:
```
  Determining projects to restore...
  ProjectBeforeRestore: EnableDefaultCompileItems false
  ProjectBeforeRestore: EnableDefaultItems true
...\Source\SomeProject\SomeProject.csproj(15,9): warning : ProjectBeforeRestore: Compile ItemGroup is empty
  All projects are up-to-date for restore.
CSC : error CS5001: Program does not contain a static 'Main' method suitable for an entry point [C:\Scal26\DotNet.MSBuild.Issue.13566\Source\SomeProject\SomeProject.csproj]

Build FAILED.
```

## The guilty part

[See CustomSdk/Sdk/sdk.props](Source/CustomSdk/Sdk/Sdk.props)
