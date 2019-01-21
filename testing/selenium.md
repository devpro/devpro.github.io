# Selenium

[Home](../readme.md) > [Testing](./testing.md) > [Selenium](./selenium.md)

This is a set of resources and documentation about [Selenium](https://www.seleniumhq.org/).

## Recipes

### Quick start

Steps (_last update on 2018-02-26_):

- Create a new Console project (.NET Core 2 for example) from Visual Studio or from the command line.

- Add references to Selenium NuGet packages.

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>netstandard2.0</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Selenium.Firefox.WebDriver" Version="0.19.1" />
    <PackageReference Include="Selenium.Support" Version="3.9.1" />
    <PackageReference Include="Selenium.WebDriver" Version="3.9.1" />
    <PackageReference Include="Selenium.WebDriver.ChromeDriver" Version="2.35.0" />
  </ItemGroup>
</Project>
```

- For Mozilla, download [geckodriver](https://github.com/mozilla/geckodriver) at [github.com/mozilla](https://github.com/mozilla/geckodriver/releases). Extract the dll in a program directory, you can add this directory in the PATH environment variable but this is not mandatory.

- Write the following code

```csharp
using OpenQA.Selenium;
using OpenQA.Selenium.Firefox;

namespace MyNameSpace
{
    public class MyClass
    {
        public void DummyTest()
        {
            var service = FirefoxDriverService.CreateDefaultService("D:\\Programs\\geckodriver-v0.19.1-win64");
            using (var browser = new FirefoxDriver(service))
            {
                browser.Navigate().GoToUrl("http://saucelabs.com");
                var header = browser.FindElement(By.Id("site-header"));
                browser.Close();
            }
        }
    }
}
```

### Container

- Launch container from Selenium Docker image

```csharp
docker run -d -p 4444:4444 selenium/standalone-chrome
```

- Open [http://localhost:4444/wd/hub](http://localhost:4444/wd/hub)

- Execute .NET Core script

```csharp
//using OpenQA.Selenium;
//using OpenQA.Selenium.Chrome;
//using OpenQA.Selenium.Remote;
var options = new ChromeOptions();
using (var browser = new RemoteWebDriver(new Uri("http://localhost:4444/wd/hub"), options.ToCapabilities()))
{
    browser.Navigate().GoToUrl("http://saucelabs.com");
    var header = browser.FindElement(By.Id("site-header"));
    browser.Close();
}
```
