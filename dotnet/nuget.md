# NuGet

[Home](../readme.md) > [.NET](./dotnet.md) > [NuGet](./nuget.md)

## Presentation

From [nuget.org](https://www.nuget.org/):

  > NuGet is the package manager for .NET.

From [docs.microsoft.com/what-is-nuget](https://docs.microsoft.com/en-us/nuget/what-is-nuget):

> An essential tool for any modern development platform is a mechanism through which developers can create, share, and consume useful code. Often such code is bundled into "packages" that contain compiled code (as DLLs) along with other content needed in the projects that consume these packages.
>
> For .NET, the Microsoft-supported mechanism for sharing code is NuGet, which defines how packages for .NET are created, hosted, and consumed, and provides the tools for each of those roles.

## How-to

### Host your NuGet feeds (see summary on [docs.microsoft.com](https://docs.microsoft.com/en-us/nuget/hosting-packages/overview))

_Solutions available (list not exhaustive!)_:

  1/ [MyGet](https://www.myget.org/)

  **Pros**: very easy to setup (less than 5 minutes), secure, free account (limited but more than enough for personal projects and evaluate), available on internet, works well with VSTS, no maintenance of infra cost

  2/ [VSTS](https://docs.microsoft.com/en-us/vsts/package/get-started-nuget) with [Package Management VSTS extension](https://marketplace.visualstudio.com/items?itemName=ms.feed)

  **Pros**: natively integrated with VSTS Build, no maintenance of infra cost

  3/ Host & deploy a web application referencing [NuGet.Server](https://www.nuget.org/packages/NuGet.Server/)

  **Cons**: seems like the only free solution BUT time needed to setup (creation of the solution, build & deploy) and maintain the server hosting the solution (+ infra cost), by default no backup or feed on internet

  4/ Sonatype Nexus

  **Cons**: community version do not manage NuGet feeds AND infra/maintenance cost plus feeds not on internet by default

_Tips_:

- Do not forget to add a `NuGet.config` file at the root of the solutions that will use the library (see [Configuring NuGet behavior](https://docs.microsoft.com/en-us/nuget/consume-packages/configuring-nuget-behavior) and [NuGet.Config reference](https://docs.microsoft.com/en-us/nuget/reference/nuget-config-file)). Otherwise you won't be able to do [dotnet restore](https://docs.microsoft.com/fr-fr/dotnet/core/tools/dotnet-restore?tabs=netcore2x) on build systems such as VSTS. Example:

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <configuration>
    <packageSources>
      <add key="nuget.org" value="https://api.nuget.org/v3/index.json" />
      <add key="MyGet Devpro" value="https://www.myget.org/F/devpro-public/api/v3/index.json" />
    </packageSources>
  </configuration>
  ```

### Publish a NuGet package from VSTS ([docs.microsoft.com](https://docs.microsoft.com/en-us/vsts/build-release/packages/nuget-pack-publish))

_Prerequisites_:

- **NuGet server** (needs to be defined):

  - In your VSTS project Settings section (wheel icon) go in "Services" page
  - In "Endpoints" click on "New Service Endpoint" and select "NuGet"
  - Fill the different elements (this is very easy if you are using MyGet, the feed URL and ApiKey have been displayed when you configured your feed)

_Steps_:

- **.NET Core > Restore**: nothing particular here (don't forget the NuGet.config file if you are using other feeds than nuget.org)

- **.NET Core > Build**: nothing particular here

- **.NET Core > Test**: nothing particular here

- **.NET Core > dotnet pack**: as of today (Feb 2018), you cannot use "NuGet pack in VSTS" but you can do a "dotnet pack" instead (ref discussion on [github](https://github.com/NuGet/Home/issues/4808)).

- **NuGet > NuGet push**:

  Target feed location = External NuGet server (including other accounts/collections)

  Nuget Server = the name of the server you defined earlier

_Tips_:

- By default, the NuGet package will always have the version 1.0.0 ([question raised on Stackoverflow](https://stackoverflow.com/questions/42797993/package-version-is-always-1-0-0-with-dotnet-pack)). There are 3 solutions:

  1/ Update your build definition in VSTS  
  2/ Update your project file and add `VersionPrefix` and `VersionSuffix`  
  3/ Use MSBuild to control how you build your NuGet packages
