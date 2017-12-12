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
ms.openlocfilehash: 22fc0446dee71dd0d2402b47d76cc8b7307fbe5f
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="738a4-104">使用新数据库在 NET Core 控制台应用程序上开始使用 EF Core</span><span class="sxs-lookup"><span data-stu-id="738a4-104">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="738a4-105">在本演练中，你将创建一个 .NET Core 控制台应用程序，该程序使用 Entity Framework Core 对 SQLite 数据库执行基本数据访问。</span><span class="sxs-lookup"><span data-stu-id="738a4-105">In this walkthrough, you will create a .NET Core console app that performs basic data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="738a4-106">你将使用迁移功能从模型创建数据库。</span><span class="sxs-lookup"><span data-stu-id="738a4-106">You will use migrations to create the database from your model.</span></span> <span data-ttu-id="738a4-107">有关使用 ASP.NET Core MVC 的 Visual Studio 版本，请参阅 [ASP.NET Core - 新数据库](xref:core/get-started/aspnetcore/new-db)。</span><span class="sxs-lookup"><span data-stu-id="738a4-107">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

> [!NOTE]  
> <span data-ttu-id="738a4-108">[.NET Core SDK](https://www.microsoft.com/net/download/core) 不再支持 `project.json` 或 Visual Studio 2015。</span><span class="sxs-lookup"><span data-stu-id="738a4-108">The [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or Visual Studio 2015.</span></span> <span data-ttu-id="738a4-109">我们建议你[从 project.json 迁移到 csproj](https://docs.microsoft.com/dotnet/articles/core/migration/)。</span><span class="sxs-lookup"><span data-stu-id="738a4-109">We recommend you [migrate from project.json to csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span></span> <span data-ttu-id="738a4-110">如果你使用的是 Visual Studio，我们建议迁移到 [Visual Studio 2017](https://www.visualstudio.com/downloads/)。</span><span class="sxs-lookup"><span data-stu-id="738a4-110">If you are using Visual Studio, we recommend you migrate to [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

> [!TIP]  
> <span data-ttu-id="738a4-111">可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite)。</span><span class="sxs-lookup"><span data-stu-id="738a4-111">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="738a4-112">先决条件</span><span class="sxs-lookup"><span data-stu-id="738a4-112">Prerequisites</span></span>

<span data-ttu-id="738a4-113">完成本演练需要以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="738a4-113">The following prerequisites are needed to complete this walkthrough:</span></span>
* <span data-ttu-id="738a4-114">支持 .NET Core 的操作系统。</span><span class="sxs-lookup"><span data-stu-id="738a4-114">An operating system that supports .NET Core.</span></span>
* <span data-ttu-id="738a4-115">[.NET Core SDK ](https://www.microsoft.com/net/core) 2.0（虽然可以使用指令并经过少量修改来创建较早版本的应用程序）。</span><span class="sxs-lookup"><span data-stu-id="738a4-115">[The .NET Core SDK](https://www.microsoft.com/net/core) 2.0 (although the instructions can be used to create an application with a previous version with very few modifications).</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="738a4-116">创建新项目</span><span class="sxs-lookup"><span data-stu-id="738a4-116">Create a new project</span></span>

* <span data-ttu-id="738a4-117">为项目创建一个新的 `ConsoleApp.SQLite` 文件夹，并使用 `dotnet` 命令通过 .NET Core 应用程序对其进行填充。</span><span class="sxs-lookup"><span data-stu-id="738a4-117">Create a new `ConsoleApp.SQLite` folder for your project and use the `dotnet` command to populate it with a .NET Core app.</span></span>

``` Console
mkdir ConsoleApp.SQLite
cd ConsoleApp.SQLite/
dotnet new console
```

## <a name="install-entity-framework-core"></a><span data-ttu-id="738a4-118">安装 Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="738a4-118">Install Entity Framework Core</span></span>

<span data-ttu-id="738a4-119">要使用 EF Core，请为要作为目标对象的数据库提供程序安装程序包。</span><span class="sxs-lookup"><span data-stu-id="738a4-119">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="738a4-120">本演练使用 SQLite。</span><span class="sxs-lookup"><span data-stu-id="738a4-120">This walkthrough uses SQLite.</span></span> <span data-ttu-id="738a4-121">有关可用提供程序的列表，请参阅[数据库提供程序](../../providers/index.md)。</span><span class="sxs-lookup"><span data-stu-id="738a4-121">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="738a4-122">安装 Microsoft.EntityFrameworkCore.Sqlite 和 Microsoft.EntityFrameworkCore.Design</span><span class="sxs-lookup"><span data-stu-id="738a4-122">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* <span data-ttu-id="738a4-123">手动编辑 `ConsoleApp.SQLite.csproj` 以将 DotNetCliToolReference 添加到 Microsoft.EntityFrameworkCore.Tools.DotNet：</span><span class="sxs-lookup"><span data-stu-id="738a4-123">Manually edit `ConsoleApp.SQLite.csproj` to add a DotNetCliToolReference to Microsoft.EntityFrameworkCore.Tools.DotNet:</span></span>

  ``` xml
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  </ItemGroup>
  ```

 <span data-ttu-id="738a4-124">注意：未来版本的 `dotnet` 将通过 `dotnet add tool` 支持 DotNetCliToolReferences</span><span class="sxs-lookup"><span data-stu-id="738a4-124">Note: A future version of `dotnet` will support DotNetCliToolReferences via `dotnet add tool`</span></span>

<span data-ttu-id="738a4-125">`ConsoleApp.SQLite.csproj` 现在应包含以下代码：</span><span class="sxs-lookup"><span data-stu-id="738a4-125">`ConsoleApp.SQLite.csproj` should now contain the following:</span></span>

[!code[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/ConsoleApp.SQLite.csproj)]

 <span data-ttu-id="738a4-126">注意：上面使用的版本号在发布时是正确的。</span><span class="sxs-lookup"><span data-stu-id="738a4-126">Note: The version numbers used above were correct at the time of publishing.</span></span>

*  <span data-ttu-id="738a4-127">运行 `dotnet restore` 来安装新的程序包。</span><span class="sxs-lookup"><span data-stu-id="738a4-127">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="738a4-128">创建模型</span><span class="sxs-lookup"><span data-stu-id="738a4-128">Create the model</span></span>

<span data-ttu-id="738a4-129">定义构成模型的上下文和实体类：</span><span class="sxs-lookup"><span data-stu-id="738a4-129">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="738a4-130">使用以下内容创建一个新的 Model.cs 文件。</span><span class="sxs-lookup"><span data-stu-id="738a4-130">Create a new *Model.cs* file with the following contents.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="738a4-131">提示：在实际的应用程序中，你可以将每个类放在单独的文件中，并将连接字符串放在配置文件中。</span><span class="sxs-lookup"><span data-stu-id="738a4-131">Tip: In a real application you would put each class in a separate file and put the connection string in a configuration file.</span></span> <span data-ttu-id="738a4-132">为简化本教程，我们会将所有内容放在一个文件中。</span><span class="sxs-lookup"><span data-stu-id="738a4-132">To keep the tutorial simple, we are putting everything in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="738a4-133">创建数据库</span><span class="sxs-lookup"><span data-stu-id="738a4-133">Create the database</span></span>

<span data-ttu-id="738a4-134">有了模型后，你可以使用[迁移](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)创建数据库。</span><span class="sxs-lookup"><span data-stu-id="738a4-134">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="738a4-135">运行 `dotnet ef migrations add InitialCreate` 以为迁移搭建基架，并为模型创建一组初始表。</span><span class="sxs-lookup"><span data-stu-id="738a4-135">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="738a4-136">运行 `dotnet ef database update` 以将新迁移应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="738a4-136">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="738a4-137">在应用迁移之前，此命令可创建数据库。</span><span class="sxs-lookup"><span data-stu-id="738a4-137">This command creates the database before applying migrations.</span></span>

> [!NOTE]  
> <span data-ttu-id="738a4-138">在 SQLite 中使用相对路径时，路径将与应用程序的主程序集相关。</span><span class="sxs-lookup"><span data-stu-id="738a4-138">When using relative paths with SQLite, the path will be relative to the application's main assembly.</span></span> <span data-ttu-id="738a4-139">在此示例中，主要的二进制文件是 `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`，因此 SQLite 数据库将在 `bin/Debug/netcoreapp2.0/blogging.db` 中。</span><span class="sxs-lookup"><span data-stu-id="738a4-139">In this sample, the main binary is `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`, so the SQLite database will be in `bin/Debug/netcoreapp2.0/blogging.db`.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="738a4-140">使用模型</span><span class="sxs-lookup"><span data-stu-id="738a4-140">Use your model</span></span>

* <span data-ttu-id="738a4-141">打开 Program.cs 并将内容替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="738a4-141">Open *Program.cs* and replace the contents with the following code:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="738a4-142">测试应用：</span><span class="sxs-lookup"><span data-stu-id="738a4-142">Test the app:</span></span>

 `dotnet run`

 <span data-ttu-id="738a4-143">将一个博客保存到数据库，那么所有博客的详细信息都会显示在控制台中。</span><span class="sxs-lookup"><span data-stu-id="738a4-143">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="738a4-144">更改模型：</span><span class="sxs-lookup"><span data-stu-id="738a4-144">Changing the model:</span></span>

- <span data-ttu-id="738a4-145">如果对模型进行更改，则可以使用 `dotnet ef migrations add` 命令为新[迁移](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)搭建基架，以便对数据库进行相应的架构更改。</span><span class="sxs-lookup"><span data-stu-id="738a4-145">If you make changes to your model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)  to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="738a4-146">检查已搭建基架的代码（并进行了必要更改）后，就可以使用 `dotnet ef database update` 命令将更改应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="738a4-146">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the changes to the database.</span></span>
- <span data-ttu-id="738a4-147">EF 使用数据库中的 `__EFMigrationsHistory` 表来跟踪哪些迁移已经应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="738a4-147">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="738a4-148">由于 SQLite 的限制，SQLite 不支持所有迁移（架构更改）。</span><span class="sxs-lookup"><span data-stu-id="738a4-148">SQLite does not support all migrations (schema changes) due to limitations in SQLite.</span></span> <span data-ttu-id="738a4-149">请参阅 [SQLite 限制](../../providers/sqlite/limitations.md)。</span><span class="sxs-lookup"><span data-stu-id="738a4-149">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="738a4-150">进行新的开发时，考虑删除数据库并创建新的数据库，而不是在模型更改时使用迁移。</span><span class="sxs-lookup"><span data-stu-id="738a4-150">For new development, consider dropping the database and creating a new one rather than using migrations when your model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="738a4-151">其他资源</span><span class="sxs-lookup"><span data-stu-id="738a4-151">Additional Resources</span></span>

* <span data-ttu-id="738a4-152">[ - 通过 SQLite 新建数据库](xref:core/get-started/netcore/new-db-sqlite) - 跨平台的控制台 EF 教程。</span><span class="sxs-lookup"><span data-stu-id="738a4-152">[.NET Core - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="738a4-153">Mac 或 Linux 上的 ASP.NET Core MVC 简介</span><span class="sxs-lookup"><span data-stu-id="738a4-153">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="738a4-154">具有 Visual Studio 的 ASP.NET Core MVC 简介</span><span class="sxs-lookup"><span data-stu-id="738a4-154">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="738a4-155">借助 Visual Studio 使用 ASP.NET Core 和 Entity Framework Core 入门</span><span class="sxs-lookup"><span data-stu-id="738a4-155">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
