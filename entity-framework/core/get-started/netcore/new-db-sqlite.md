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
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="59c15-103">使用新数据库在 NET Core 控制台应用程序上开始使用 EF Core</span><span class="sxs-lookup"><span data-stu-id="59c15-103">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="59c15-104">在本教程中，将创建一个 .NET Core 控制台应用，该应用使用 Entity Framework Core 对 SQLite 数据库执行数据访问。</span><span class="sxs-lookup"><span data-stu-id="59c15-104">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="59c15-105">你将通过迁移基于此模型创建数据库。</span><span class="sxs-lookup"><span data-stu-id="59c15-105">You use migrations to create the database from the model.</span></span> <span data-ttu-id="59c15-106">有关使用 ASP.NET Core MVC 的 Visual Studio 版本，请参阅 [ASP.NET Core - 新数据库](xref:core/get-started/aspnetcore/new-db)。</span><span class="sxs-lookup"><span data-stu-id="59c15-106">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

<span data-ttu-id="59c15-107">在 GitHub 上查看此文章的示例 (https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite))。</span><span class="sxs-lookup"><span data-stu-id="59c15-107">View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="59c15-108">系统必备</span><span class="sxs-lookup"><span data-stu-id="59c15-108">Prerequisites</span></span>

* [<span data-ttu-id="59c15-109">.NET Core 2.1 SDK</span><span class="sxs-lookup"><span data-stu-id="59c15-109">The .NET Core 2.1 SDK</span></span>](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a><span data-ttu-id="59c15-110">创建新项目</span><span class="sxs-lookup"><span data-stu-id="59c15-110">Create a new project</span></span>

* <span data-ttu-id="59c15-111">新建控制台项目：</span><span class="sxs-lookup"><span data-stu-id="59c15-111">Create a new console project:</span></span>

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  cd ConsoleApp.SQLite/
  ```

## <a name="install-entity-framework-core"></a><span data-ttu-id="59c15-112">安装 Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="59c15-112">Install Entity Framework Core</span></span>

<span data-ttu-id="59c15-113">要使用 EF Core，请为要作为目标对象的数据库提供程序安装程序包。</span><span class="sxs-lookup"><span data-stu-id="59c15-113">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="59c15-114">本演练使用 SQLite。</span><span class="sxs-lookup"><span data-stu-id="59c15-114">This walkthrough uses SQLite.</span></span> <span data-ttu-id="59c15-115">有关可用提供程序的列表，请参阅[数据库提供程序](../../providers/index.md)。</span><span class="sxs-lookup"><span data-stu-id="59c15-115">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="59c15-116">安装 Microsoft.EntityFrameworkCore.Sqlite 和 Microsoft.EntityFrameworkCore.Design</span><span class="sxs-lookup"><span data-stu-id="59c15-116">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

  ```Console
  dotnet add package Microsoft.EntityFrameworkCore.Sqlite
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

* <span data-ttu-id="59c15-117">运行 `dotnet restore` 来安装新的程序包。</span><span class="sxs-lookup"><span data-stu-id="59c15-117">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="59c15-118">创建模型</span><span class="sxs-lookup"><span data-stu-id="59c15-118">Create the model</span></span>

<span data-ttu-id="59c15-119">定义构成模型的上下文和实体类：</span><span class="sxs-lookup"><span data-stu-id="59c15-119">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="59c15-120">使用以下内容创建一个新的 Model.cs 文件。</span><span class="sxs-lookup"><span data-stu-id="59c15-120">Create a new *Model.cs* file with the following contents.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="59c15-121">提示：在实际应用程序中，将每个类放在单独的文件中，并将连接字符串放在配置文件或环境变量中。</span><span class="sxs-lookup"><span data-stu-id="59c15-121">Tip: In a real application, you put each class in a separate file and put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="59c15-122">为简化本教程，所有内容均放在一个文件中。</span><span class="sxs-lookup"><span data-stu-id="59c15-122">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="59c15-123">创建数据库</span><span class="sxs-lookup"><span data-stu-id="59c15-123">Create the database</span></span>

<span data-ttu-id="59c15-124">有了模型后，即可通过[迁移](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)创建数据库。</span><span class="sxs-lookup"><span data-stu-id="59c15-124">Once you have a model, you use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="59c15-125">运行 `dotnet ef migrations add InitialCreate` 以为迁移搭建基架，并为模型创建一组初始表。</span><span class="sxs-lookup"><span data-stu-id="59c15-125">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="59c15-126">运行 `dotnet ef database update` 以将新迁移应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="59c15-126">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="59c15-127">在应用迁移之前，此命令可创建数据库。</span><span class="sxs-lookup"><span data-stu-id="59c15-127">This command creates the database before applying migrations.</span></span>

<span data-ttu-id="59c15-128">blogging.db\* SQLite DB 位于项目目录中。</span><span class="sxs-lookup"><span data-stu-id="59c15-128">The *blogging.db*\* SQLite DB is in the project directory.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="59c15-129">使用模型</span><span class="sxs-lookup"><span data-stu-id="59c15-129">Use the model</span></span>

* <span data-ttu-id="59c15-130">打开 Program.cs 并将内容替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="59c15-130">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="59c15-131">测试应用：</span><span class="sxs-lookup"><span data-stu-id="59c15-131">Test the app:</span></span>

  `dotnet run`

  <span data-ttu-id="59c15-132">将一个博客保存到数据库，那么所有博客的详细信息都会显示在控制台中。</span><span class="sxs-lookup"><span data-stu-id="59c15-132">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="59c15-133">更改模型：</span><span class="sxs-lookup"><span data-stu-id="59c15-133">Changing the model:</span></span>

- <span data-ttu-id="59c15-134">如果对模型进行了更改，可使用 `dotnet ef migrations add` 命令为新[迁移](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)搭建基架。</span><span class="sxs-lookup"><span data-stu-id="59c15-134">If you make changes to the model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations).</span></span> <span data-ttu-id="59c15-135">检查已搭建基架的代码（并进行了必要更改）后，可使用 `dotnet ef database update` 命令将架构更改应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="59c15-135">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the schema changes to the database.</span></span>
- <span data-ttu-id="59c15-136">EF Core 使用数据库中的 `__EFMigrationsHistory` 表来跟踪哪些迁移已应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="59c15-136">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="59c15-137">对于大多数其他关系数据库支持的某些架构更改，SQLite 数据库引擎并不支持。</span><span class="sxs-lookup"><span data-stu-id="59c15-137">The SQLite database engine doesn't support certain schema changes that are supported by most other relational databases.</span></span> <span data-ttu-id="59c15-138">例如，不支持 `DropColumn` 操作。</span><span class="sxs-lookup"><span data-stu-id="59c15-138">For example, the `DropColumn` operation is not supported.</span></span> <span data-ttu-id="59c15-139">EF Core 迁移将为这些操作生成代码。</span><span class="sxs-lookup"><span data-stu-id="59c15-139">EF Core Migrations will generate code for these operations.</span></span> <span data-ttu-id="59c15-140">但是，如果试图将它们应用于数据库或生成脚本，则 EF Core 会引发异常。</span><span class="sxs-lookup"><span data-stu-id="59c15-140">But if you try to apply them to a database or generate a script, EF Core throws exceptions.</span></span> <span data-ttu-id="59c15-141">请参阅 [SQLite 限制](../../providers/sqlite/limitations.md)。</span><span class="sxs-lookup"><span data-stu-id="59c15-141">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="59c15-142">进行新的开发时，考虑删除数据库并创建新的数据库，而不是在模型更改时使用迁移。</span><span class="sxs-lookup"><span data-stu-id="59c15-142">For new development, consider dropping the database and creating a new one rather than using migrations when the model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="59c15-143">其他资源</span><span class="sxs-lookup"><span data-stu-id="59c15-143">Additional Resources</span></span>

* [<span data-ttu-id="59c15-144">Mac 或 Linux 上的 ASP.NET Core MVC 简介</span><span class="sxs-lookup"><span data-stu-id="59c15-144">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="59c15-145">具有 Visual Studio 的 ASP.NET Core MVC 简介</span><span class="sxs-lookup"><span data-stu-id="59c15-145">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="59c15-146">借助 Visual Studio 使用 ASP.NET Core 和 Entity Framework Core 入门</span><span class="sxs-lookup"><span data-stu-id="59c15-146">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
