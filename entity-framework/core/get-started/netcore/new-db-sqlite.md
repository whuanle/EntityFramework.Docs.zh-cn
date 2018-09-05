---
title: NET Core 入门 - 新数据库 - EF Core
author: rick-anderson
ms.author: riande
description: 通过 Entity Framework Core 开始使用 .NET Core
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: 51f5752eebce5603c663072f7b36dfecd4ddf227
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993687"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a>使用新数据库在 NET Core 控制台应用程序上开始使用 EF Core

在本教程中，将创建一个 .NET Core 控制台应用，该应用使用 Entity Framework Core 对 SQLite 数据库执行数据访问。 你将通过迁移基于此模型创建数据库。 有关使用 ASP.NET Core MVC 的 Visual Studio 版本，请参阅 [ASP.NET Core - 新数据库](xref:core/get-started/aspnetcore/new-db)。

在 GitHub 上查看此文章的示例 (https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite))。

## <a name="prerequisites"></a>系统必备

* [.NET Core 2.1 SDK](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a>创建新项目

* 新建控制台项目：

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  cd ConsoleApp.SQLite/
  ```

## <a name="install-entity-framework-core"></a>安装 Entity Framework Core

要使用 EF Core，请为要作为目标对象的数据库提供程序安装程序包。 本演练使用 SQLite。 有关可用提供程序的列表，请参阅[数据库提供程序](../../providers/index.md)。

* 安装 Microsoft.EntityFrameworkCore.Sqlite 和 Microsoft.EntityFrameworkCore.Design

  ```Console
  dotnet add package Microsoft.EntityFrameworkCore.Sqlite
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

* 运行 `dotnet restore` 来安装新的程序包。

## <a name="create-the-model"></a>创建模型

定义构成模型的上下文和实体类：

* 使用以下内容创建一个新的 Model.cs 文件。

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

提示：在实际应用程序中，将每个类放在单独的文件中，并将连接字符串放在配置文件或环境变量中。 为简化本教程，所有内容均放在一个文件中。

## <a name="create-the-database"></a>创建数据库

有了模型后，即可通过[迁移](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)创建数据库。

* 运行 `dotnet ef migrations add InitialCreate` 以为迁移搭建基架，并为模型创建一组初始表。
* 运行 `dotnet ef database update` 以将新迁移应用到数据库。 在应用迁移之前，此命令可创建数据库。

blogging.db* SQLite DB 位于项目目录中。

## <a name="use-the-model"></a>使用模型

* 打开 Program.cs 并将内容替换为以下代码：

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* 测试应用：

  `dotnet run`

  将一个博客保存到数据库，那么所有博客的详细信息都会显示在控制台中。

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a>更改模型：

- 如果对模型进行了更改，可使用 `dotnet ef migrations add` 命令为新[迁移](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)搭建基架。 检查已搭建基架的代码（并进行了必要更改）后，可使用 `dotnet ef database update` 命令将架构更改应用到数据库。
- EF Core 使用数据库中的 `__EFMigrationsHistory` 表来跟踪哪些迁移已应用到数据库。
- 对于大多数其他关系数据库支持的某些架构更改，SQLite 数据库引擎并不支持。 例如，不支持 `DropColumn` 操作。 EF Core 迁移将为这些操作生成代码。 但是，如果试图将它们应用于数据库或生成脚本，则 EF Core 会引发异常。 请参阅 [SQLite 限制](../../providers/sqlite/limitations.md)。 进行新的开发时，考虑删除数据库并创建新的数据库，而不是在模型更改时使用迁移。

## <a name="additional-resources"></a>其他资源

* [Mac 或 Linux 上的 ASP.NET Core MVC 简介](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [具有 Visual Studio 的 ASP.NET Core MVC 简介](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [借助 Visual Studio 使用 ASP.NET Core 和 Entity Framework Core 入门](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
