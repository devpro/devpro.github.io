# .NET CLI cheatsheet

[Home](../../readme.md) > [.NET](../dotnet.md) > [.NET Core](./dotnetcore.md) > [.NET CLI cheatsheet](./dotnet-cli-cheatsheet.md)

Commands:

- [new](#new-command)
- [add](#add-command)
- [sln](#sln-command)
- [run](#run-command)
- [publish](#publish-command)

## New command

| Command | Action |
| - | - |
| `dotnet new` | View the available templates (see [docs.microsoft.com](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-new)) |

Examples:

- `dotnet new webapi --output src/PalTracker --name PalTracker` will use the template "ASP.NET Core Web API"
- `dotnet new xunit --output test/PalTrackerTests --name PalTrackerTests`will use the template "xUnit Test Project"
- `dotnet new sln --name PalTracker` will use the template "Solution File"

## Add command

| Command | Action |
| - | - |
| `dotnet add reference` | Adds project-to-project (P2P) references (see [docs.microsoft.com](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-add-reference)) |
| `dotnet add package` | Adds a package reference to a project file (see [docs.microsoft.com](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-add-package)) |

Examples:

- `dotnet add test/PalTrackerTests reference src/PalTracker/PalTracker.csproj`
- `dotnet add test/PalTrackerTests package Microsoft.AspNetCore.TestHost --version 2.2.0`

## Sln command

| Command | Action |
| - | - |
| `dotnet sln` | Modifies a .NET Core solution file (see [docs.microsoft.com](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-sln)) |

Examples:

- `dotnet sln PalTracker.sln add src/PalTracker/PalTracker.csproj`

## Run command

| Command | Action |
| - | - |
| `dotnet run` | Runs source code without any explicit compile or launch commands (see [docs.microsoft.com](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-run)) |

Examples:

- `dotnet run --project src/PalTracker`

## Publish command

| Command | Action |
| - | - |
| `dotnet publish` | Packs the application and its dependencies into a folder for deployment to a hosting system (see [docs.microsoft.com](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-publish)) |

Examples:

- `dotnet publish src/PalTracker --configuration Release`

## Test command

Examples:

- `dotnet test test/PalTrackerTests --filter PalTrackerTests.InMemoryTimeEntryRepositoryTest`
