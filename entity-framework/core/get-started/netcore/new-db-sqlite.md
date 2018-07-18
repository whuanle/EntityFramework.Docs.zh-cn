---
title: NET Core 入门 - 新数据库 - EF Core
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: 通过 Entity Framework Core 开始使用 .NET Core
keywords: .NET Core, Entity Framework Core, VS Code, Visual Studio Code, Mac, Linux
ms.date: 06/05/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: e4eafed037325237345efbc3d7d42b32270a54e3
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/08/2018
ms.locfileid: "37911497"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="bb55e-104">使用新数据库在 NET Core 控制台应用程序上开始使用 EF Core</span><span class="sxs-lookup"><span data-stu-id="bb55e-104">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="bb55e-105">在本演示中，创建通过 Entity Framework Core 针对 SQLite 数据库执行数据访问的 .NET Core 控制台应用。</span><span class="sxs-lookup"><span data-stu-id="bb55e-105">In this walkthrough, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="bb55e-106">你将通过迁移基于此模型创建数据库。</span><span class="sxs-lookup"><span data-stu-id="bb55e-106">You use migrations to create the database from the model.</span></span> <span data-ttu-id="bb55e-107">有关使用 ASP.NET Core MVC 的 Visual Studio 版本，请参阅 [ASP.NET Core - 新数据库](xref:core/get-started/aspnetcore/new-db)。</span><span class="sxs-lookup"><span data-stu-id="bb55e-107">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

> [!TIP]  
> <span data-ttu-id="bb55e-108">可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite)。</span><span class="sxs-lookup"><span data-stu-id="bb55e-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bb55e-109">系统必备</span><span class="sxs-lookup"><span data-stu-id="bb55e-109">Prerequisites</span></span>

<span data-ttu-id="bb55e-110">[The .NET Core SDK](https://www.microsoft.com/net/core) 2.1</span><span class="sxs-lookup"><span data-stu-id="bb55e-110">[The .NET Core SDK](https://www.microsoft.com/net/core) 2.1</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="bb55e-111">创建新项目</span><span class="sxs-lookup"><span data-stu-id="bb55e-111">Create a new project</span></span>

* <span data-ttu-id="bb55e-112">新建控制台项目：</span><span class="sxs-lookup"><span data-stu-id="bb55e-112">Create a new console project:</span></span>

``` Console
dotnet new console -o ConsoleApp.SQLite
cd ConsoleApp.SQLite/
```

## <a name="install-entity-framework-core"></a><span data-ttu-id="bb55e-113">安装 Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="bb55e-113">Install Entity Framework Core</span></span>

<span data-ttu-id="bb55e-114">要使用 EF Core，请为要作为目标对象的数据库提供程序安装程序包。</span><span class="sxs-lookup"><span data-stu-id="bb55e-114">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="bb55e-115">本演练使用 SQLite。</span><span class="sxs-lookup"><span data-stu-id="bb55e-115">This walkthrough uses SQLite.</span></span> <span data-ttu-id="bb55e-116">有关可用提供程序的列表，请参阅[数据库提供程序](../../providers/index.md)。</span><span class="sxs-lookup"><span data-stu-id="bb55e-116">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="bb55e-117">安装 Microsoft.EntityFrameworkCore.Sqlite 和 Microsoft.EntityFrameworkCore.Design</span><span class="sxs-lookup"><span data-stu-id="bb55e-117">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* <span data-ttu-id="bb55e-118">运行 `dotnet restore` 来安装新的程序包。</span><span class="sxs-lookup"><span data-stu-id="bb55e-118">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="bb55e-119">创建模型</span><span class="sxs-lookup"><span data-stu-id="bb55e-119">Create the model</span></span>

<span data-ttu-id="bb55e-120">定义构成模型的上下文和实体类：</span><span class="sxs-lookup"><span data-stu-id="bb55e-120">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="bb55e-121">使用以下内容创建一个新的 Model.cs 文件。</span><span class="sxs-lookup"><span data-stu-id="bb55e-121">Create a new *Model.cs* file with the following contents.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="bb55e-122">提示：在实际的应用程序中，将每个列放在单独的文件中，并将连接字符串放在配置文件中。</span><span class="sxs-lookup"><span data-stu-id="bb55e-122">Tip: In a real application, you put each class in a separate file and put the connection string in a configuration file.</span></span> <span data-ttu-id="bb55e-123">为简化本教程，所有内容均放在一个文件中。</span><span class="sxs-lookup"><span data-stu-id="bb55e-123">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="bb55e-124">创建数据库</span><span class="sxs-lookup"><span data-stu-id="bb55e-124">Create the database</span></span>

<span data-ttu-id="bb55e-125">有了模型后，即可通过[迁移](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)创建数据库。</span><span class="sxs-lookup"><span data-stu-id="bb55e-125">Once you have a model, you use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="bb55e-126">运行 `dotnet ef migrations add InitialCreate` 以为迁移搭建基架，并为模型创建一组初始表。</span><span class="sxs-lookup"><span data-stu-id="bb55e-126">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="bb55e-127">运行 `dotnet ef database update` 以将新迁移应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="bb55e-127">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="bb55e-128">在应用迁移之前，此命令可创建数据库。</span><span class="sxs-lookup"><span data-stu-id="bb55e-128">This command creates the database before applying migrations.</span></span>

<span data-ttu-id="bb55e-129">blogging.db\* SQLite DB 位于项目目录中。</span><span class="sxs-lookup"><span data-stu-id="bb55e-129">The *blogging.db*\* SQLite DB is in the project directory.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="bb55e-130">使用模型</span><span class="sxs-lookup"><span data-stu-id="bb55e-130">Use your model</span></span>

* <span data-ttu-id="bb55e-131">打开 Program.cs 并将内容替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="bb55e-131">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="bb55e-132">测试应用：</span><span class="sxs-lookup"><span data-stu-id="bb55e-132">Test the app:</span></span>

  `dotnet run`

  <span data-ttu-id="bb55e-133">将一个博客保存到数据库，那么所有博客的详细信息都会显示在控制台中。</span><span class="sxs-lookup"><span data-stu-id="bb55e-133">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="bb55e-134">更改模型：</span><span class="sxs-lookup"><span data-stu-id="bb55e-134">Changing the model:</span></span>

- <span data-ttu-id="bb55e-135">如果对模型进行更改，则可以使用 `dotnet ef migrations add` 命令为新[迁移](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)搭建基架，以便对数据库进行相应的架构更改。</span><span class="sxs-lookup"><span data-stu-id="bb55e-135">If you make changes to your model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)  to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="bb55e-136">检查已搭建基架的代码（并进行了必要更改）后，就可以使用 `dotnet ef database update` 命令将更改应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="bb55e-136">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the changes to the database.</span></span>
- <span data-ttu-id="bb55e-137">EF 使用数据库中的 `__EFMigrationsHistory` 表来跟踪哪些迁移已经应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="bb55e-137">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="bb55e-138">由于 SQLite 的限制，SQLite 不支持所有迁移（架构更改）。</span><span class="sxs-lookup"><span data-stu-id="bb55e-138">SQLite does not support all migrations (schema changes) due to limitations in SQLite.</span></span> <span data-ttu-id="bb55e-139">请参阅 [SQLite 限制](../../providers/sqlite/limitations.md)。</span><span class="sxs-lookup"><span data-stu-id="bb55e-139">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="bb55e-140">进行新的开发时，考虑删除数据库并创建新的数据库，而不是在模型更改时使用迁移。</span><span class="sxs-lookup"><span data-stu-id="bb55e-140">For new development, consider dropping the database and creating a new one rather than using migrations when your model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bb55e-141">其他资源</span><span class="sxs-lookup"><span data-stu-id="bb55e-141">Additional Resources</span></span>

* <span data-ttu-id="bb55e-142">[ - 通过 SQLite 新建数据库](xref:core/get-started/netcore/new-db-sqlite) - 跨平台的控制台 EF 教程。</span><span class="sxs-lookup"><span data-stu-id="bb55e-142">[.NET Core - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="bb55e-143">Mac 或 Linux 上的 ASP.NET Core MVC 简介</span><span class="sxs-lookup"><span data-stu-id="bb55e-143">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="bb55e-144">具有 Visual Studio 的 ASP.NET Core MVC 简介</span><span class="sxs-lookup"><span data-stu-id="bb55e-144">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="bb55e-145">借助 Visual Studio 使用 ASP.NET Core 和 Entity Framework Core 入门</span><span class="sxs-lookup"><span data-stu-id="bb55e-145">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
