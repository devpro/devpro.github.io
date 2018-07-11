# Redis

## Introduction

- Presentation by [Asmaa](https://github.com/abennehhou): [slides.com](http://slides.com/abennehhou/redis#/)
- Introduction on [redistogo.com](http://redistogo.com/documentation/introduction_to_redis)

## Run

### Docker

#### Running in standalone on default port 6379

This command works on Windows 10 with Hyper-V and Docker.

```dos
docker run --name redis -d -p 6379:6379 redis
docker stop redis
docker start redis
```

## Clients

### Applications

- Redis Desktop Manager [redisdesktop.com](https://redisdesktop.com/download)

### Command line

- [redis-cli](https://redis.io/topics/rediscli)

On Ubuntu: `sudo apt install redis-tools`

> $ redis-cli  
> 127.0.0.1:6379> keys *

### Code

#### .NET

- Nuget library: StackExchange.Redis [stackexchange.github.io](https://stackexchange.github.io/StackExchange.Redis/)
- Example: ASP.NET MVC app [github.com](https://github.com/abennehhou/mvc-redis-example), .NET Console [github.com](https://github.com/abennehhou/redis-csharp-console-app)
- Serilog configuration example:

  ```json
  "Serilog": {
      // Log levels include: Verbose, Debug, Information, Warning, Error, Fatal
      "MinimumLevel": "Information",
      "WriteTo": [
        {
          "Name": "LiterateConsole",
          "Args": {
            "outputTemplate": "{Timestamp:HH:mm:ss} {Level} | {RequestId} - {Message}{NewLine}{Exception}"
          }
        },
        {
          "Name": "RedisList",
          "Args": {
            "redisUris": "localhost:6379",
            "keyName": "MyNameSpace.MyApp"
          }
        }
      ]
    }
  ```

## Hosting

### Azure

- Getting Started with Redis - Manage Redis - in .Net [azure.microsoft.com](https://azure.microsoft.com/en-us/resources/samples/redis-cache-dotnet-manage-cache/)
- How to Use Azure Redis Cache [docs.microsoft.com](https://docs.microsoft.com/en-us/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache)
- How to configure Azure Redis Cache [docs.microsoft.com](https://docs.microsoft.com/en-us/azure/redis-cache/cache-configure)
