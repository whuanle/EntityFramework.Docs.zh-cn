---
title: "NET Core 入门 - 新数据库 - EF Core"
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: "通过 Entity Framework Core 开始使用 .NET Core"
keywords: .NET Core, Entity Framework Core, VS Code, Visual Studio Code, Mac, Linux
ms.date: 04/05/2017
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: 3becf75e7a513a3aa18c3c2daf628b65327365b0
ms.sourcegitcommit: 0858f157b806f4a881b94ddbeecf1ece1d53e1e0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/21/2018
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a>使用新数据库在 NET Core 控制台应用程序上开始使用 EF Core

在本演练中，你将创建一个 .NET Core 控制台应用程序，该程序使用 Entity Framework Core 对 SQLite 数据库执行基本数据访问。 你将使用迁移功能从模型创建数据库。 有关使用 ASP.NET Core MVC 的 Visual Studio 版本，请参阅 [ASP.NET Core - 新数据库](xref:core/get-started/aspnetcore/new-db)。

> [!NOTE]  
> [.NET Core SDK](https://www.microsoft.com/net/download/core) 不再支持 `project.json` 或 Visual Studio 2015。 我们建议你[从 project.json 迁移到 csproj](https://docs.microsoft.com/dotnet/articles/core/migration/)。 如果你使用的是 Visual Studio，我们建议迁移到 [Visual Studio 2017](https://www.visualstudio.com/downloads/)。

> [!TIP]  
> 可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite)。

## <a name="prerequisites"></a>系统必备

完成本演练需要以下先决条件：
* 支持 .NET Core 的操作系统。
* [.NET Core SDK ](https://www.microsoft.com/net/core) 2.0（虽然可以使用指令并经过少量修改来创建较早版本的应用程序）。

## <a name="create-a-new-project"></a>创建新项目

* 为项目创建一个新的 `ConsoleApp.SQLite` 文件夹，并使用 `dotnet` 命令通过 .NET Core 应用程序对其进行填充。

``` Console
mkdir ConsoleApp.SQLite
cd ConsoleApp.SQLite/
dotnet new console
```

## <a name="install-entity-framework-core"></a>安装 Entity Framework Core

要使用 EF Core，请为要作为目标对象的数据库提供程序安装程序包。 本演练使用 SQLite。 有关可用提供程序的列表，请参阅[数据库提供程序](../../providers/index.md)。

* 安装 Microsoft.EntityFrameworkCore.Sqlite 和 Microsoft.EntityFrameworkCore.Design

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* 手动编辑 `ConsoleApp.SQLite.csproj` 以将 DotNetCliToolReference 添加到 Microsoft.EntityFrameworkCore.Tools.DotNet：

  ``` xml
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  </ItemGroup>
  ```

`ConsoleApp.SQLite.csproj` 现在应包含以下代码：

[!code[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/ConsoleApp.SQLite.csproj)]

 注意：上面使用的版本号在发布时是正确的。

*  运行 `dotnet restore` 来安装新的程序包。

## <a name="create-the-model"></a>创建模型

定义构成模型的上下文和实体类：

* 使用以下内容创建一个新的 Model.cs 文件。

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

提示：在实际的应用程序中，你可以将每个类放在单独的文件中，并将连接字符串放在配置文件中。 为简化本教程，我们会将所有内容放在一个文件中。

## <a name="create-the-database"></a>创建数据库

有了模型后，你可以使用[迁移](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)创建数据库。

* 运行 `dotnet ef migrations add InitialCreate` 以为迁移搭建基架，并为模型创建一组初始表。
* 运行 `dotnet ef database update` 以将新迁移应用到数据库。 在应用迁移之前，此命令可创建数据库。

> [!NOTE]  
> 在 SQLite 中使用相对路径时，路径将与应用程序的主程序集相关。 在此示例中，主要的二进制文件是 `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`，因此 SQLite 数据库将在 `bin/Debug/netcoreapp2.0/blogging.db` 中。

## <a name="use-your-model"></a>使用模型

* 打开 Program.cs 并将内容替换为以下代码：

 [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* 测试应用：

 `dotnet run`

 将一个博客保存到数据库，那么所有博客的详细信息都会显示在控制台中。

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a>更改模型：

- 如果对模型进行更改，则可以使用 `dotnet ef migrations add` 命令为新[迁移](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)搭建基架，以便对数据库进行相应的架构更改。 检查已搭建基架的代码（并进行了必要更改）后，就可以使用 `dotnet ef database update` 命令将更改应用到数据库。
- EF 使用数据库中的 `__EFMigrationsHistory` 表来跟踪哪些迁移已经应用到数据库。
- 由于 SQLite 的限制，SQLite 不支持所有迁移（架构更改）。 请参阅 [SQLite 限制](../../providers/sqlite/limitations.md)。 进行新的开发时，考虑删除数据库并创建新的数据库，而不是在模型更改时使用迁移。

## <a name="additional-resources"></a>其他资源

* [ - 通过 SQLite 新建数据库](xref:core/get-started/netcore/new-db-sqlite) - 跨平台的控制台 EF 教程。
* [Mac 或 Linux 上的 ASP.NET Core MVC 简介](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [具有 Visual Studio 的 ASP.NET Core MVC 简介](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [借助 Visual Studio 使用 ASP.NET Core 和 Entity Framework Core 入门](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
