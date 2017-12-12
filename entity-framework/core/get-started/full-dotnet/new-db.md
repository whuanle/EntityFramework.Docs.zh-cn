---
title: ".NET Framework 入门 - 新数据库 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: bd7054c6834ae11bfdc352d63654e4304771e432
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a><span data-ttu-id="11ed7-102">使用新数据库在 .NET Framework 上开始使用 EF Core</span><span class="sxs-lookup"><span data-stu-id="11ed7-102">Getting started with EF Core on .NET Framework with a New Database</span></span>

<span data-ttu-id="11ed7-103">在本演练中，你将构建一个控制台应用程序，该程序使用 Entity Framework 对 Microsoft SQL Server 数据库执行基本数据访问。</span><span class="sxs-lookup"><span data-stu-id="11ed7-103">In this walkthrough, you will build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="11ed7-104">你将使用迁移功能从模型创建数据库。</span><span class="sxs-lookup"><span data-stu-id="11ed7-104">You will use migrations to create the database from your model.</span></span>

> [!TIP]  
> <span data-ttu-id="11ed7-105">可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb)。</span><span class="sxs-lookup"><span data-stu-id="11ed7-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="11ed7-106">先决条件</span><span class="sxs-lookup"><span data-stu-id="11ed7-106">Prerequisites</span></span>

<span data-ttu-id="11ed7-107">完成本演练需要以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="11ed7-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* [<span data-ttu-id="11ed7-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="11ed7-108">Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/)

* [<span data-ttu-id="11ed7-109">最新版本的 NuGet 包管理器</span><span class="sxs-lookup"><span data-stu-id="11ed7-109">Latest version of NuGet Package Manager</span></span>](https://dist.nuget.org/index.html)

* [<span data-ttu-id="11ed7-110">最新版本的 Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="11ed7-110">Latest version of Windows PowerShell</span></span>](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

## <a name="create-a-new-project"></a><span data-ttu-id="11ed7-111">创建新项目</span><span class="sxs-lookup"><span data-stu-id="11ed7-111">Create a new project</span></span>

* <span data-ttu-id="11ed7-112">打开 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="11ed7-112">Open Visual Studio</span></span>

* <span data-ttu-id="11ed7-113">“文件”>“新建”>“项目...”</span><span class="sxs-lookup"><span data-stu-id="11ed7-113">File > New > Project...</span></span>

* <span data-ttu-id="11ed7-114">从左侧菜单中选择“模板”>“Visual C#”>“Windows 经典桌面”</span><span class="sxs-lookup"><span data-stu-id="11ed7-114">From the left menu select Templates > Visual C# > Windows Classic Desktop</span></span>

* <span data-ttu-id="11ed7-115">选择“控制台应用(.NET Framework)”项目模板</span><span class="sxs-lookup"><span data-stu-id="11ed7-115">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="11ed7-116">确保面向 .NET Framework 4.5.1 或更高版本</span><span class="sxs-lookup"><span data-stu-id="11ed7-116">Ensure you are targeting **.NET Framework 4.5.1** or later</span></span>

* <span data-ttu-id="11ed7-117">为项目提供名称，然后单击“确定”</span><span class="sxs-lookup"><span data-stu-id="11ed7-117">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="11ed7-118">安装 Entity Framework</span><span class="sxs-lookup"><span data-stu-id="11ed7-118">Install Entity Framework</span></span>

<span data-ttu-id="11ed7-119">要使用 EF Core，请为要作为目标对象的数据库提供程序安装程序包。</span><span class="sxs-lookup"><span data-stu-id="11ed7-119">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="11ed7-120">本演练使用 SQL Server。</span><span class="sxs-lookup"><span data-stu-id="11ed7-120">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="11ed7-121">有关可用提供程序的列表，请参阅[数据库提供程序](../../providers/index.md)。</span><span class="sxs-lookup"><span data-stu-id="11ed7-121">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="11ed7-122">“工具”>“NuGet 包管理器”>“包管理器控制台”</span><span class="sxs-lookup"><span data-stu-id="11ed7-122">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="11ed7-123">运行 `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="11ed7-123">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="11ed7-124">在本演练的后面部分，我们还将一些 Entity Framework Tools 用于维护数据库。</span><span class="sxs-lookup"><span data-stu-id="11ed7-124">Later in this walkthrough we will also be using some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="11ed7-125">因此，我们也会安装此工具包。</span><span class="sxs-lookup"><span data-stu-id="11ed7-125">So we will install the tools package as well.</span></span>

* <span data-ttu-id="11ed7-126">运行 `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="11ed7-126">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-your-model"></a><span data-ttu-id="11ed7-127">创建你自己的模型</span><span class="sxs-lookup"><span data-stu-id="11ed7-127">Create your model</span></span>

<span data-ttu-id="11ed7-128">现在是时候定义构成模型的上下文和实体类了。</span><span class="sxs-lookup"><span data-stu-id="11ed7-128">Now it's time to define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="11ed7-129">“项目”>“添加类...”</span><span class="sxs-lookup"><span data-stu-id="11ed7-129">Project > Add Class...</span></span>

* <span data-ttu-id="11ed7-130">输入“Model.cs”作为名称，然后单击“确定”</span><span class="sxs-lookup"><span data-stu-id="11ed7-130">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="11ed7-131">将此文件的内容替换为以下代码</span><span class="sxs-lookup"><span data-stu-id="11ed7-131">Replace the contents of the file with the following code</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }

        public List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
}
```

> [!TIP]  
> <span data-ttu-id="11ed7-132">在实际的应用程序中，你可以将每个类放在单独的文件中，并将连接字符串放在 `App.Config` 文件中，然后使用 `ConfigurationManager` 将其读出。</span><span class="sxs-lookup"><span data-stu-id="11ed7-132">In a real application you would put each class in a separate file and put the connection string in the `App.Config` file and read it out using `ConfigurationManager`.</span></span> <span data-ttu-id="11ed7-133">为简单起见，在本教程中，我们将所有内容都放在单个代码文件中。</span><span class="sxs-lookup"><span data-stu-id="11ed7-133">For the sake of simplicity, we are putting everything in a single code file for this tutorial.</span></span>

## <a name="create-your-database"></a><span data-ttu-id="11ed7-134">创建数据库</span><span class="sxs-lookup"><span data-stu-id="11ed7-134">Create your database</span></span>

<span data-ttu-id="11ed7-135">现在你已经有了模型，可以使用迁移为自己创建数据库。</span><span class="sxs-lookup"><span data-stu-id="11ed7-135">Now that you have a model, you can use migrations to create a database for you.</span></span>

* <span data-ttu-id="11ed7-136">“工具”–>“NuGet 包管理器”–>“包管理器控制台”</span><span class="sxs-lookup"><span data-stu-id="11ed7-136">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="11ed7-137">运行 `Add-Migration MyFirstMigration` 来为迁移搭建基架，从而为模型创建一组初始表。</span><span class="sxs-lookup"><span data-stu-id="11ed7-137">Run `Add-Migration MyFirstMigration` to scaffold a migration to create the initial set of tables for your model.</span></span>

* <span data-ttu-id="11ed7-138">运行 `Update-Database` 以将新迁移应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="11ed7-138">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="11ed7-139">由于数据库尚不存在，因此将在应用迁移之前进行创建。</span><span class="sxs-lookup"><span data-stu-id="11ed7-139">Because your database doesn't exist yet, it will be created for you before the migration is applied.</span></span>

> [!TIP]  
> <span data-ttu-id="11ed7-140">如果将来对模型进行更改，则可以使用 `Add-Migration` 命令为新迁移搭建基架，以便对数据库进行相应的架构更改。</span><span class="sxs-lookup"><span data-stu-id="11ed7-140">If you make future changes to your model, you can use the `Add-Migration` command to scaffold a new migration to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="11ed7-141">检查已搭建基架的代码（并进行了必要更改）后，就可以使用 `Update-Database` 命令将更改应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="11ed7-141">Once you have checked the scaffolded code (and made any required changes), you can use the `Update-Database` command to apply the changes to the database.</span></span>
>
><span data-ttu-id="11ed7-142">EF 使用数据库中的 `__EFMigrationsHistory` 表来跟踪哪些迁移已经应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="11ed7-142">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="11ed7-143">使用模型</span><span class="sxs-lookup"><span data-stu-id="11ed7-143">Use your model</span></span>

<span data-ttu-id="11ed7-144">你现在可以使用模型执行数据访问。</span><span class="sxs-lookup"><span data-stu-id="11ed7-144">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="11ed7-145">打开 Program.cs</span><span class="sxs-lookup"><span data-stu-id="11ed7-145">Open *Program.cs*</span></span>

* <span data-ttu-id="11ed7-146">将此文件的内容替换为以下代码</span><span class="sxs-lookup"><span data-stu-id="11ed7-146">Replace the contents of the file with the following code</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blogs)
                {
                    Console.WriteLine(" - {0}", blog.Url);
                }
            }
        }
    }
}
```

* <span data-ttu-id="11ed7-147">“调试”>“开始执行(不调试)”</span><span class="sxs-lookup"><span data-stu-id="11ed7-147">Debug > Start Without Debugging</span></span>

<span data-ttu-id="11ed7-148">你将看到，把一个博客保存到数据库后，所有博客的详细信息都将显示在控制台中。</span><span class="sxs-lookup"><span data-stu-id="11ed7-148">You will see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

![图像](_static/output-new-db.png)
