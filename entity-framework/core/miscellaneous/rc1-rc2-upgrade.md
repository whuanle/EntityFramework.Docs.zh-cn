---
title: 从 EF 核心 1.0 RC1 升级到 RC2-EF 核心
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
ms.technology: entity-framework-core
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: e76886729248101ccac024568cf9abcd945fca33
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678622"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a>从 EF 核心 1.0 RC1 升级到 1.0 RC2

本文提供了将用到 RC2 RC1 包生成应用程序迁移指南。

## <a name="package-names-and-versions"></a>包名称和版本

RC1，RC2，我们从更改"实体框架 7"为"实体框架 Core"。 你可以阅读更多有关的更改的原因[由 Scott Hanselman 的此博客文章](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx)。 由于此更改，我们包名称更改从`EntityFramework.*`到`Microsoft.EntityFrameworkCore.*`和我们版本的`7.0.0-rc1-final`到`1.0.0-rc2-final`(或`1.0.0-preview1-final`程序工具)。

**你将需要完全删除 RC1 包，然后安装 RC2 的。** 下面是一些常见的包的映射。

| RC1 包                                               | RC2 等效项                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final      |
| EntityFramework.SQLite                    7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final      |
| EntityFramework7.Npgsql                   3.1.0-rc1-3     | NpgSql.EntityFrameworkCore.Postgres             <to be advised>      |
| EntityFramework.SqlServerCompact35        7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final      |
| EntityFramework.SqlServerCompact40        7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final      |
| EntityFramework.InMemory                  7.0.0-rc1-final | Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final      |
| EntityFramework.IBMDataServer             7.0.0-beta1     | Rc2 尚不可用                                            |
| EntityFramework.Commands                  7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final |
| EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final      |

## <a name="namespaces"></a>命名空间

命名空间从包名称，以及更改`Microsoft.Data.Entity.*`到`Microsoft.EntityFrameworkCore.*`。 你可以处理此更改与查找/替换的`using Microsoft.Data.Entity`与`using Microsoft.EntityFrameworkCore`。

## <a name="table-naming-convention-changes"></a>命名约定更改的表

我们在 RC2 中所用的一个重要的功能变化是使用的名称`DbSet<TEntity>`属性为给定实体为表名它将映射到，而不是只是类的名称。 你可以阅读更多有关这一更改[相关的公告问题](https://github.com/aspnet/Announcements/issues/167)。

对于现有 RC1 应用程序，我们建议将以下代码添加到开始你`OnModelCreating`方法以保持 RC1 命名策略：

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

如果你想要采用新的命名策略，我们建议成功完成的升级步骤然后删除代码并创建要应用的表的迁移的其余部分将重命名。

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a>AddDbContext / Startup.cs 更改 （仅适用于 ASP.NET Core 项目）

在 RC1，你必须将实体框架服务添加到应用程序服务提供商，在`Startup.ConfigureServices(...)`:

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

在 RC2，你可以删除对的调用`AddEntityFramework()`， `AddSqlServer()`，等等。:

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

你还需要添加一个构造函数，为您派生的上下文，，它接受上下文选项并将其传递给基构造函数。 这被必须的因为我们删除了一些在后台 snuck 在可怕幻数：

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a>IServiceProvider 传入

如果必须通过的 RC1 代码`IServiceProvider`到上下文，这现在已移到`DbContextOptions`，而不是单独的构造函数参数。 使用`DbContextOptionsBuilder.UseInternalServiceProvider(...)`设置服务提供程序。

### <a name="testing"></a>正在测试

执行此操作的最常见方案是在测试时控制 InMemory 数据库的范围。 请参阅更新[测试](testing/index.md)文，以执行此操作使用 RC2 的示例。

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a>解析内部服务从应用程序服务提供商 （仅适用于 ASP.NET Core 项目）

如果你有一个 ASP.NET Core 应用并且想 EF 若要解决应用程序服务提供商提供的内部服务，则的重载`AddDbContext`，可以对此进行配置：

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> 我们建议允许 EF 内部管理其自身的服务，除非你有其他原因将内部 EF 服务合并到你的应用程序服务提供商。 你可能想要执行此操作的主要原因是使用你的应用程序服务提供商来替换 EF 在内部使用的服务

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a>DNX 命令 = >.NET CLI （仅适用于 ASP.NET Core 项目）

如果你以前使用`dnx ef`对于 ASP.NET 5 项目的命令，这些现在已移到`dotnet ef`命令。 相同的命令语法仍然适用。 你可以使用`dotnet ef --help`语法信息。

注册命令的方式已更改 RC2，由于.NET CLI 被取代的 DNX 中。 在现在注册命令`tools`主题中`project.json`:

``` json
"tools": {
  "Microsoft.EntityFrameworkCore.Tools": {
    "version": "1.0.0-preview1-final",
    "imports": [
      "portable-net45+win8+dnxcore50",
      "portable-net45+win8"
    ]
  }
}
```

> [!TIP]  
> 如果你使用 Visual Studio，你现在可以使用程序包管理器控制台运行 ASP.NET 核心项目 （这不支持在 RC1 中） 的 EF 命令。 你仍需要注册中的命令`tools`部分`project.json`要这样做。

## <a name="package-manager-commands-require-powershell-5"></a>包管理器命令需要 PowerShell 5

如果在 Visual Studio 中的包管理器控制台中使用实体框架命令，你将需要确保已安装的 PowerShell 5。 这是在下一个版本中将删除的临时要求 (请参阅[发出 #5327](https://github.com/aspnet/EntityFramework/issues/5327)有关详细信息)。

## <a name="using-imports-in-projectjson"></a>Project.json 中使用"导入"

一些 EF 核心依赖关系不支持.NET 标准尚未。 标准.NET 和.NET 核心项目中的 EF 核心可能会要求添加"导入"到 project.json 临时的解决方法。

在添加 EF，NuGet 还原将显示此错误消息：

``` Console
Package Ix-Async 1.2.5 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Ix-Async 1.2.5 supports:
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8 (.NETPortable,Version=v0.0,Profile=Profile78)
Package Remotion.Linq 2.0.2 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Remotion.Linq 2.0.2 supports:
  - net35 (.NETFramework,Version=v3.5)
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8+wpa81 (.NETPortable,Version=v0.0,Profile=Profile259)
```

解决方法是手动导入"可移植 net451 + win8"的可移植配置文件。 此强制将匹配此此二进制文件的 NuGet 提供作为.NET standard 兼容框架，即使它们不是。 虽然"可移植 net451 + win8"不是 100%兼容.NET 标准，它是兼容的 PCL 从转换为.NET 标准。 EF 的依赖关系最终将升级到.NET Standard 时，可能会删除导入。

数组语法中，可以将多个框架添加到"导入"。 如果将其他库添加到你的项目，则可能需要采用其他导入。

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

请参阅[发出 #5176](https://github.com/aspnet/EntityFramework/issues/5176)。
