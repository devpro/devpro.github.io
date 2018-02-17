# NuGet

[Home](../readme.md) > [.NET](./readme.md) > [NuGet](./nuget.md)

## How-to

### Host your NuGet feeds

There is a summary available on [docs.microsoft.com](https://docs.microsoft.com/en-us/nuget/hosting-packages/overview).

Solutions:

- [MyGet](https://www.myget.org/)
- [VSTS](https://docs.microsoft.com/en-us/vsts/package/get-started-nuget) with [Package Management VSTS extension](https://marketplace.visualstudio.com/items?itemName=ms.feed)

### Publish a NuGet package from VSTS ([docs.microsoft.com](https://docs.microsoft.com/en-us/vsts/build-release/packages/nuget-pack-publish))

Tips:

- As of today (Feb 2018), you cannot use "NuGet pack in VSTS" but you can do a "dotnet pack" instead (ref discussion on [github](https://github.com/NuGet/Home/issues/4808)).

- By default, the NuGet package will always have the version 1.0.0 ([question raised on Stackoverflow](https://stackoverflow.com/questions/42797993/package-version-is-always-1-0-0-with-dotnet-pack)). There are 3 solutions:

  1/ Update your build definition in VSTS  
  2/ You update your project file and add `VersionPrefix` and `VersionSuffix`  
  3/ You use MSBuild to control how you build your NuGet packages.
