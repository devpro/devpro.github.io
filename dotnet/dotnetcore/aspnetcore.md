# ASP.NET Core

[Home](../../readme.md) > [.NET](../readme.md) > [.NET Core](./readme.md) > [ASP.NET Core](./aspnetcore.md)

Quick lins:

* [Configuration](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/)

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
