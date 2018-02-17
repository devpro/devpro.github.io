# Logging in .NET Core

[Home](../../readme.md) > [.NET](../readme.md) > [.NET Core](./readme.md) > [Logging](./logging.md)

Quick links:

* [Introduction to logging in ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging/)

Tips:

* For a **netstandard** library
  * Add `Microsoft.Extensions.Logging` to the project (do not add a strong dependency to a logging framework such as log4net or NLog!).

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
      <PropertyGroup>
        <TargetFramework>netcoreapp2.0</TargetFramework>
      </PropertyGroup>
      <ItemGroup>
        <PackageReference Include="Microsoft.Extensions.Logging" Version="2.0.0" />
      </ItemGroup>
      <!-- ... -->
    </Project>
    ```
  * Add IController to the class constructor and the related private field

    ```csharp
    using Microsoft.Extensions.Logging;
    // ...

    public class MyClass
    {
        private readonly ILogger _logger;

        public MyClass(ILogger<PuppetFacterService> logger)
        {
            _logger = logger;
        }

        public MyMethod()
        {
            _logger.LogInformation("MyMethod called");
        }
    }
    ```
