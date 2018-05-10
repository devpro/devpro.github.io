# ASP.NET Core

[Home](../../readme.md) > [.NET](../dotnet.md) > [.NET Core](./dotnetcore.md) > [ASP.NET Core](./aspnetcore.md)

## Table of Contents

1. [Documentation](#documentation)
2. [Case study: New project](#case-study-new-project)
3. [Hosting](#hosting)
    * [On Windows with IIS](#on-windows-with-iis)

## Documentation

* [Configuration](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/)
* [Application startup](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/startup)
* [Error handling](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/error-handling)
* [Filters](https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/filters)

## Case study: New project

Tips:

* If you use a Visual Studio template, change the "Copy to Output Directory" property of `appsettings.json` file to "Copy Always" to make sure it is present in the output folder (_Editor's note: I don't know why it's not like this by default_).
* In `Program.cs` file, a lot of magic happens with this lines (see [WebHost.CreateDefaultBuilder Method](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder?view=aspnetcore-2.0)):
  ```csharp
  public static IWebHost BuildWebHost(string[] args) =>
      WebHost.CreateDefaultBuilder(args)
          .UseStartup<Startup>()
          .Build();
  ```

## Hosting

### On Windows with IIS

Reference: [Host ASP.NET Core on Windows with IIS](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/iis/index?tabs=aspnetcore2x)

On Windows 2008 R2 (and above) with .NET Core 2.*, you need to make sure:

* Enable IIS feature and roles
* Install .NET Core Windows Server Hosting bundle
  * Download and install from [aka.ms](https://aka.ms/dotnetcore-2-windowshosting)

  > The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/servers/aspnet-core-module?tabs=aspnetcore2x). The module creates the reverse proxy between IIS and the Kestrel server. See [ASP.NET Core Module configuration reference](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/aspnet-core-module).

  ![image](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/servers/aspnet-core-module/_static/ancm.png)

  * Restart IIS services

  ```bash
  net stop was /y
  net start w3svc
  ```
