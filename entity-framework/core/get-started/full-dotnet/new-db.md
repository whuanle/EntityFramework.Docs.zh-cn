---
title: .NET Framework 入门 - 新数据库 - EF Core
author: rowanmiller
ms.date: 08/06/2018
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: 66d9011a5978fc3d8253a963bdb9df27848e9ff9
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997582"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a><span data-ttu-id="e9cb6-102">使用新数据库在 .NET Framework 上开始使用 EF Core</span><span class="sxs-lookup"><span data-stu-id="e9cb6-102">Getting started with EF Core on .NET Framework with a New Database</span></span>

<span data-ttu-id="e9cb6-103">在本教程中，你将构建一个控制台应用程序，它使用 Entity Framework 对 Microsoft SQL Server 数据库执行基本数据访问。</span><span class="sxs-lookup"><span data-stu-id="e9cb6-103">In this tutorial, you build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="e9cb6-104">你将通过迁移基于模型创建数据库。</span><span class="sxs-lookup"><span data-stu-id="e9cb6-104">You use migrations to create the database from a model.</span></span>

<span data-ttu-id="e9cb6-105">[在 GitHub 上查看此文章的示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb)。</span><span class="sxs-lookup"><span data-stu-id="e9cb6-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e9cb6-106">系统必备</span><span class="sxs-lookup"><span data-stu-id="e9cb6-106">Prerequisites</span></span>

* [<span data-ttu-id="e9cb6-107">Visual Studio 2017 版本 15.7 或更高版本</span><span class="sxs-lookup"><span data-stu-id="e9cb6-107">Visual Studio 2017 version 15.7 or later</span></span>](https://www.visualstudio.com/downloads/)

## <a name="create-a-new-project"></a><span data-ttu-id="e9cb6-108">创建新项目</span><span class="sxs-lookup"><span data-stu-id="e9cb6-108">Create a new project</span></span>

* <span data-ttu-id="e9cb6-109">打开 Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="e9cb6-109">Open Visual Studio 2017</span></span>

* <span data-ttu-id="e9cb6-110">“文件”>“新建”>“项目...”</span><span class="sxs-lookup"><span data-stu-id="e9cb6-110">**File > New > Project...**</span></span>

* <span data-ttu-id="e9cb6-111">从左侧菜单中选择“已安装”>“Visual C#”->“Windows Desktop”</span><span class="sxs-lookup"><span data-stu-id="e9cb6-111">From the left menu select **Installed > Visual C# > Windows Desktop**</span></span>

* <span data-ttu-id="e9cb6-112">选择“控制台应用(.NET Framework)”项目模板</span><span class="sxs-lookup"><span data-stu-id="e9cb6-112">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="e9cb6-113">确保项目面向 **.NET Framework 4.6.1** 或更高版本</span><span class="sxs-lookup"><span data-stu-id="e9cb6-113">Make sure that the project targets **.NET Framework 4.6.1** or later</span></span>

* <span data-ttu-id="e9cb6-114">将项目命名为 ConsoleApp.NewDb，并单击“确定”</span><span class="sxs-lookup"><span data-stu-id="e9cb6-114">Name the project *ConsoleApp.NewDb* and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="e9cb6-115">安装 Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e9cb6-115">Install Entity Framework</span></span>

<span data-ttu-id="e9cb6-116">要使用 EF Core，请为要作为目标对象的数据库提供程序安装程序包。</span><span class="sxs-lookup"><span data-stu-id="e9cb6-116">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="e9cb6-117">本教程使用 SQL Server。</span><span class="sxs-lookup"><span data-stu-id="e9cb6-117">This tutorial uses SQL Server.</span></span> <span data-ttu-id="e9cb6-118">有关可用提供程序的列表，请参阅[数据库提供程序](../../providers/index.md)。</span><span class="sxs-lookup"><span data-stu-id="e9cb6-118">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="e9cb6-119">“工具”>“NuGet 包管理器”>“包管理器控制台”</span><span class="sxs-lookup"><span data-stu-id="e9cb6-119">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="e9cb6-120">运行 `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="e9cb6-120">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="e9cb6-121">在本教程的后面部分，你会使用某些 Entity Framework Tools 维护数据库。</span><span class="sxs-lookup"><span data-stu-id="e9cb6-121">Later in this tutorial you use some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="e9cb6-122">因此，请同时安装该工具包。</span><span class="sxs-lookup"><span data-stu-id="e9cb6-122">So install the tools package as well.</span></span>

* <span data-ttu-id="e9cb6-123">运行 `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="e9cb6-123">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="e9cb6-124">创建模型</span><span class="sxs-lookup"><span data-stu-id="e9cb6-124">Create the model</span></span>

<span data-ttu-id="e9cb6-125">现在是时候定义构成模型的上下文和实体类了。</span><span class="sxs-lookup"><span data-stu-id="e9cb6-125">Now it's time to define a context and entity classes that make up the model.</span></span>

* <span data-ttu-id="e9cb6-126">“项目”>“添加类...”</span><span class="sxs-lookup"><span data-stu-id="e9cb6-126">**Project > Add Class...**</span></span>

* <span data-ttu-id="e9cb6-127">输入“Model.cs”作为名称，然后单击“确定”</span><span class="sxs-lookup"><span data-stu-id="e9cb6-127">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="e9cb6-128">将此文件的内容替换为以下代码</span><span class="sxs-lookup"><span data-stu-id="e9cb6-128">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] 

> [!TIP]  
> <span data-ttu-id="e9cb6-129">在实际的应用程序中，可以将每个类放在单独的文件中，并将连接字符串放在配置文件或环境变量中。</span><span class="sxs-lookup"><span data-stu-id="e9cb6-129">In a real application you would put each class in a separate file and put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="e9cb6-130">为简单起见，在本教程中，所有内容都在单个代码文件中。</span><span class="sxs-lookup"><span data-stu-id="e9cb6-130">For the sake of simplicity, everything is in a single code file for this tutorial.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="e9cb6-131">创建数据库</span><span class="sxs-lookup"><span data-stu-id="e9cb6-131">Create the database</span></span>

<span data-ttu-id="e9cb6-132">现在有了模型，就可以使用迁移创建数据库。</span><span class="sxs-lookup"><span data-stu-id="e9cb6-132">Now that you have a model, you can use migrations to create a database.</span></span>

* <span data-ttu-id="e9cb6-133">“工具”>“NuGet 包管理器”>“包管理器控制台”</span><span class="sxs-lookup"><span data-stu-id="e9cb6-133">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="e9cb6-134">运行 `Add-Migration InitialCreate` 为迁移搭建基架，以便为模型创建一组初始表。</span><span class="sxs-lookup"><span data-stu-id="e9cb6-134">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for the model.</span></span>

* <span data-ttu-id="e9cb6-135">运行 `Update-Database` 以将新迁移应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="e9cb6-135">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="e9cb6-136">由于数据库尚不存在，因此将在应用迁移之前进行创建。</span><span class="sxs-lookup"><span data-stu-id="e9cb6-136">Because the database doesn't exist yet, it will be created before the migration is applied.</span></span>

> [!TIP]  
> <span data-ttu-id="e9cb6-137">如果对模型进行更改，可以使用 `Add-Migration` 命令为新迁移搭建基架，以便对数据库进行相应的架构更改。</span><span class="sxs-lookup"><span data-stu-id="e9cb6-137">If you make changes to the model, you can use the `Add-Migration` command to scaffold a new migration to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="e9cb6-138">检查已搭建基架的代码（并进行了必要更改）后，就可以使用 `Update-Database` 命令将更改应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="e9cb6-138">Once you have checked the scaffolded code (and made any required changes), you can use the `Update-Database` command to apply the changes to the database.</span></span>
>
> <span data-ttu-id="e9cb6-139">EF 使用数据库中的 `__EFMigrationsHistory` 表来跟踪哪些迁移已经应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="e9cb6-139">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="e9cb6-140">使用模型</span><span class="sxs-lookup"><span data-stu-id="e9cb6-140">Use the model</span></span>

<span data-ttu-id="e9cb6-141">现在可以使用模型执行数据访问。</span><span class="sxs-lookup"><span data-stu-id="e9cb6-141">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="e9cb6-142">打开 Program.cs</span><span class="sxs-lookup"><span data-stu-id="e9cb6-142">Open *Program.cs*</span></span>

* <span data-ttu-id="e9cb6-143">将此文件的内容替换为以下代码</span><span class="sxs-lookup"><span data-stu-id="e9cb6-143">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)]

* <span data-ttu-id="e9cb6-144">“调试”>“开始执行(不调试)”</span><span class="sxs-lookup"><span data-stu-id="e9cb6-144">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="e9cb6-145">可以看到，把一个博客保存到数据库后，所有博客的详细信息都将显示在控制台中。</span><span class="sxs-lookup"><span data-stu-id="e9cb6-145">You see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

  ![图像](_static/output-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="e9cb6-147">其他资源</span><span class="sxs-lookup"><span data-stu-id="e9cb6-147">Additional Resources</span></span>

* [<span data-ttu-id="e9cb6-148">通过现有数据库在 .NET Framework 上使用 EF Core</span><span class="sxs-lookup"><span data-stu-id="e9cb6-148">EF Core on .NET Framework with an existing database</span></span>](xref:core/get-started/full-dotnet/existing-db)
* <span data-ttu-id="e9cb6-149">[通过新数据库在 .NET Core 上使用 EF Core - SQLite](xref:core/get-started/netcore/new-db-sqlite) - 跨平台控制台 EF 教程。</span><span class="sxs-lookup"><span data-stu-id="e9cb6-149">[EF Core on .NET Core with a new database - SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
