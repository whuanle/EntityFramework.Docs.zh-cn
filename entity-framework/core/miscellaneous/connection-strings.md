---
title: "连接字符串的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: b4ed01f0452d74ac49d3fde780caa5f1b25a6e97
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="connection-strings"></a>连接字符串

大多数的数据库提供程序需要某种形式的连接字符串以连接到数据库。 有时，此连接字符串包含需要保护的敏感信息。 你可能还需要根据环境，如开发、 测试和生产环境之间移动你的应用程序更改连接字符串。

## <a name="net-framework-applications"></a>.NET framework 应用程序

.NET framework 应用程序，如 WinForms、 WPF、 控制台中，和 ASP.NET 4 中，具有的经过验证和测试连接字符串模式。 应将该连接字符串添加到你的应用程序 App.config 文件 (如果你使用 ASP.NET Web.config)。 如果你的连接字符串包含敏感信息，如用户名和密码，你可以保护的配置文件使用的内容[受保护的配置](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration)。

``` xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>

  <connectionStrings>
    <add name="BloggingDatabase"
         connectionString="Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" />
  </connectionStrings>
</configuration>
```

> [!TIP]  
> `providerName`上 EF 核心连接字符串存储在 App.config，因为通过代码配置了数据库提供程序不需要设置。

然后可以读取连接字符串使用`ConfigurationManager`中上下文的 API`OnConfiguring`方法。 你可能需要添加对的引用`System.Configuration`framework 程序集要能够使用此 API。

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="universal-windows-platform-uwp"></a>通用 Windows 平台 (UWP)

在 UWP 应用程序中的连接字符串通常是一个 SQLite 连接，只需指定本地中的文件名。 它们通常不包含敏感信息，并不需要更改，因为应用程序部署。 在这种情况下，这些连接字符串将通常可以在代码中，保留的如下所示。 如果你想要使它们摆脱代码 UWP 支持设置的概念，请参阅[UWP 文档的应用设置部分](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)有关详细信息。

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
            optionsBuilder.UseSqlite("Data Source=blogging.db");
    }
}
```

## <a name="aspnet-core"></a>ASP.NET Core

在 ASP.NET 核心中配置系统非常灵活，并且连接字符串可存储在`appsettings.json`，环境变量、 用户机密存储中或另一个配置源。 请参阅[的 ASP.NET 核心文档中的配置节](https://docs.asp.net/en/latest/fundamentals/configuration.html)有关详细信息。 下面的示例演示中存储的连接字符串`appsettings.json`。

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

上下文通常在配置`Startup.cs`正在从配置中读取的连接字符串。 请注意`GetConnectionString()`方法查找它的键是一个配置值`ConnectionStrings:<connection string name>`。

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
