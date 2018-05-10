# .NET

[Home](../readme.md) > [.NET](./dotnet.md)

This is a set of resources and documentation about Microsoft .NET framework & technologies.

Pages:

* [.NET Core](./dotnetcore/dotnetcore.md)
* [NuGet](./nuget.md)
* [Obfuscation](./obfuscation.md)
* [Visual Studio Team Services (VSTS)](./vsts.md)

Password hashing:

* [Password Hashing](https://docs.microsoft.com/en-us/aspnet/core/security/data-protection/consumer-apis/password-hashing)
* [Hashing passwords in .NET Core with tips](https://www.codeproject.com/articles/1104467/hashing-passwords-in-net-core-with-tips)
* [Encode and decode a string, with password and salt, in dotnet core](https://stackoverflow.com/questions/42459487/encode-and-decode-a-string-with-password-and-salt-in-dotnet-core)
* [Exploring the ASP.NET Core Identity PasswordHasher](https://andrewlock.net/exploring-the-asp-net-core-identity-passwordhasher/)
* [IPasswordHasher Interface](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.identity.ipasswordhasher-1?view=aspnetcore-2.0)

Synchronous call to an asynchronous method:

```csharp
// if CallServiceAsync returns Task (void)
var task = Task.Run(() => CallServiceAsync(url, action));
task.Wait();
```
