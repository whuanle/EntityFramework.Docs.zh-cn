---
title: .NET Framework 入门 - 现有数据库 - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: 3cd69109e3cf8dbc103f9eea6e2553df17f29a98
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812620"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a><span data-ttu-id="501a1-102">通过现有数据库在 .NET Framework 上开始使用 EF Core</span><span class="sxs-lookup"><span data-stu-id="501a1-102">Getting started with EF Core on .NET Framework with an Existing Database</span></span>

<span data-ttu-id="501a1-103">在本演练中，你将构建一个控制台应用程序，该程序使用 Entity Framework 对 Microsoft SQL Server 数据库执行基本数据访问。</span><span class="sxs-lookup"><span data-stu-id="501a1-103">In this walkthrough, you will build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="501a1-104">你将使用反向工程基于现有数据库创建 Entity Framework 模型。</span><span class="sxs-lookup"><span data-stu-id="501a1-104">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="501a1-105">可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb)。</span><span class="sxs-lookup"><span data-stu-id="501a1-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="501a1-106">系统必备</span><span class="sxs-lookup"><span data-stu-id="501a1-106">Prerequisites</span></span>

<span data-ttu-id="501a1-107">完成本演练需要以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="501a1-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* [<span data-ttu-id="501a1-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="501a1-108">Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/)

* [<span data-ttu-id="501a1-109">最新版本的 NuGet 包管理器</span><span class="sxs-lookup"><span data-stu-id="501a1-109">Latest version of NuGet Package Manager</span></span>](https://dist.nuget.org/index.html)

* [<span data-ttu-id="501a1-110">最新版本的 Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="501a1-110">Latest version of Windows PowerShell</span></span>](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

* [<span data-ttu-id="501a1-111">博客数据库</span><span class="sxs-lookup"><span data-stu-id="501a1-111">Blogging database</span></span>](#blogging-database)

### <a name="blogging-database"></a><span data-ttu-id="501a1-112">博客数据库</span><span class="sxs-lookup"><span data-stu-id="501a1-112">Blogging database</span></span>

<span data-ttu-id="501a1-113">本教程使用 LocalDb 实例上的博客数据库作为现有数据库。</span><span class="sxs-lookup"><span data-stu-id="501a1-113">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="501a1-114">如果你已在其他教程中创建了博客数据库，则可以跳过这些步骤。</span><span class="sxs-lookup"><span data-stu-id="501a1-114">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="501a1-115">打开 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="501a1-115">Open Visual Studio</span></span>

* <span data-ttu-id="501a1-116">“工具”->“连接到数据库...”</span><span class="sxs-lookup"><span data-stu-id="501a1-116">Tools > Connect to Database...</span></span>

* <span data-ttu-id="501a1-117">选择“Microsoft SQL Server”，然后单击“继续”</span><span class="sxs-lookup"><span data-stu-id="501a1-117">Select **Microsoft SQL Server** and click **Continue**</span></span>

* <span data-ttu-id="501a1-118">输入“(localdb)\mssqllocaldb”作为服务器名称</span><span class="sxs-lookup"><span data-stu-id="501a1-118">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>

* <span data-ttu-id="501a1-119">输入“master”作为数据库名称，然后单击“确定”</span><span class="sxs-lookup"><span data-stu-id="501a1-119">Enter **master** as the **Database Name** and click **OK**</span></span>

* <span data-ttu-id="501a1-120">Master 数据库现在显示在“服务器资源管理器”的“数据连接”中</span><span class="sxs-lookup"><span data-stu-id="501a1-120">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>

* <span data-ttu-id="501a1-121">右键单击“服务器资源管理器”中的数据库，然后选择“新建查询”</span><span class="sxs-lookup"><span data-stu-id="501a1-121">Right-click on the database in **Server Explorer** and select **New Query**</span></span>

* <span data-ttu-id="501a1-122">将下面列出的脚本复制到查询编辑器中</span><span class="sxs-lookup"><span data-stu-id="501a1-122">Copy the script, listed below, into the query editor</span></span>

* <span data-ttu-id="501a1-123">右键单击查询编辑器，然后选择“执行”</span><span class="sxs-lookup"><span data-stu-id="501a1-123">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="501a1-124">创建新项目</span><span class="sxs-lookup"><span data-stu-id="501a1-124">Create a new project</span></span>

* <span data-ttu-id="501a1-125">打开 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="501a1-125">Open Visual Studio</span></span>

* <span data-ttu-id="501a1-126">“文件”>“新建”>“项目...”</span><span class="sxs-lookup"><span data-stu-id="501a1-126">File > New > Project...</span></span>

* <span data-ttu-id="501a1-127">从左侧菜单中选择“模板”>“Visual C#”>“Windows”</span><span class="sxs-lookup"><span data-stu-id="501a1-127">From the left menu select Templates > Visual C# > Windows</span></span>

* <span data-ttu-id="501a1-128">选择“控制台应用程序”项目模板</span><span class="sxs-lookup"><span data-stu-id="501a1-128">Select the **Console Application** project template</span></span>

* <span data-ttu-id="501a1-129">确保面向 .NET Framework 4.5.1 或更高版本</span><span class="sxs-lookup"><span data-stu-id="501a1-129">Ensure you are targeting **.NET Framework 4.5.1** or later</span></span>

* <span data-ttu-id="501a1-130">为项目提供名称，然后单击“确定”</span><span class="sxs-lookup"><span data-stu-id="501a1-130">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="501a1-131">安装 Entity Framework</span><span class="sxs-lookup"><span data-stu-id="501a1-131">Install Entity Framework</span></span>

<span data-ttu-id="501a1-132">要使用 EF Core，请为要作为目标对象的数据库提供程序安装程序包。</span><span class="sxs-lookup"><span data-stu-id="501a1-132">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="501a1-133">本演练使用 SQL Server。</span><span class="sxs-lookup"><span data-stu-id="501a1-133">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="501a1-134">有关可用提供程序的列表，请参阅[数据库提供程序](../../providers/index.md)。</span><span class="sxs-lookup"><span data-stu-id="501a1-134">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="501a1-135">“工具”>“NuGet 包管理器”>“包管理器控制台”</span><span class="sxs-lookup"><span data-stu-id="501a1-135">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="501a1-136">运行 `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="501a1-136">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="501a1-137">若要从现有数据库启用反向工程，我们还需要安装几个其他程序包。</span><span class="sxs-lookup"><span data-stu-id="501a1-137">To enable reverse engineering from an existing database we need to install a couple of other packages too.</span></span>

* <span data-ttu-id="501a1-138">运行 `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="501a1-138">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="501a1-139">对模型实施反向工程</span><span class="sxs-lookup"><span data-stu-id="501a1-139">Reverse engineer your model</span></span>

<span data-ttu-id="501a1-140">现在是时候基于现有数据库创建 EF 模型了。</span><span class="sxs-lookup"><span data-stu-id="501a1-140">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="501a1-141">“工具”–>“NuGet 包管理器”–>“包管理器控制台”</span><span class="sxs-lookup"><span data-stu-id="501a1-141">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="501a1-142">运行以下命令以从现有数据库创建模型</span><span class="sxs-lookup"><span data-stu-id="501a1-142">Run the following command to create a model from the existing database</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="501a1-143">反向工程过程基于现有数据库的架构创建实体类和派生上下文。</span><span class="sxs-lookup"><span data-stu-id="501a1-143">The reverse engineer process created entity classes and a derived context based on the schema of the existing database.</span></span> <span data-ttu-id="501a1-144">实体类是简单的 C# 对象，代表要查询和保存的数据。</span><span class="sxs-lookup"><span data-stu-id="501a1-144">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)] -->
``` csharp
using System;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class Blog
    {
        public Blog()
        {
            Post = new HashSet<Post>();
        }

        public int BlogId { get; set; }
        public string Url { get; set; }

        public virtual ICollection<Post> Post { get; set; }
    }
}
```

<span data-ttu-id="501a1-145">上下文表示与数据库的会话，并允许查询和保存实体类的实例。</span><span class="sxs-lookup"><span data-stu-id="501a1-145">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Metadata;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class BloggingContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Blog>(entity =>
            {
                entity.Property(e => e.Url).IsRequired();
            });

            modelBuilder.Entity<Post>(entity =>
            {
                entity.HasOne(d => d.Blog)
                    .WithMany(p => p.Post)
                    .HasForeignKey(d => d.BlogId);
            });
        }

        public virtual DbSet<Blog> Blog { get; set; }
        public virtual DbSet<Post> Post { get; set; }
    }
}
```

## <a name="use-your-model"></a><span data-ttu-id="501a1-146">使用模型</span><span class="sxs-lookup"><span data-stu-id="501a1-146">Use your model</span></span>

<span data-ttu-id="501a1-147">你现在可以使用模型执行数据访问。</span><span class="sxs-lookup"><span data-stu-id="501a1-147">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="501a1-148">打开 Program.cs</span><span class="sxs-lookup"><span data-stu-id="501a1-148">Open *Program.cs*</span></span>

* <span data-ttu-id="501a1-149">将此文件的内容替换为以下代码</span><span class="sxs-lookup"><span data-stu-id="501a1-149">Replace the contents of the file with the following code</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blog.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blog)
                {
                    Console.WriteLine(" - {0}", blog.Url);
                }
            }
        }
    }
}
```

* <span data-ttu-id="501a1-150">“调试”>“开始执行(不调试)”</span><span class="sxs-lookup"><span data-stu-id="501a1-150">Debug > Start Without Debugging</span></span>

<span data-ttu-id="501a1-151">你将看到，把一个博客保存到数据库后，所有博客的详细信息都将显示在控制台中。</span><span class="sxs-lookup"><span data-stu-id="501a1-151">You will see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

![图像](_static/output-existing-db.png)
