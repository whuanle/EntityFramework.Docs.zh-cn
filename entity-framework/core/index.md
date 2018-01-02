---
title: "简要概述 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
ms.technology: entity-framework-core
uid: core/index
ms.openlocfilehash: 13de9cf98111b8e253e073c591fcec04206b4c4f
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
---
# <a name="entity-framework-core-quick-overview"></a><span data-ttu-id="df228-102">Entity Framework Core 简要概述</span><span class="sxs-lookup"><span data-stu-id="df228-102">Entity Framework Core Quick Overview</span></span>

<span data-ttu-id="df228-103">Entity Framework (EF) Core 是轻量化、可扩展和跨平台版的常用 Entity Framework 数据访问技术。</span><span class="sxs-lookup"><span data-stu-id="df228-103">Entity Framework (EF) Core is a lightweight, extensible, and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="df228-104">EF Core 是一种支持 .NET 开发人员使用 .NET 对象处理数据库的对象关系映射程序 (O/RM)。</span><span class="sxs-lookup"><span data-stu-id="df228-104">EF Core is an object-relational mapper (O/RM) that enables .NET developers to work with a database using .NET objects.</span></span> <span data-ttu-id="df228-105">它不要求提供开发人员通常需要编写的大部分数据访问代码。</span><span class="sxs-lookup"><span data-stu-id="df228-105">It eliminates the need for most of the data-access code that developers usually need to write.</span></span> <span data-ttu-id="df228-106">EF Core 支持多个数据库引擎，请参阅[数据库提供程序](providers/index.md)了解详细信息。</span><span class="sxs-lookup"><span data-stu-id="df228-106">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

<span data-ttu-id="df228-107">如果希望通过编写代码进行了解，建议你阅读其中一篇[入门](get-started/index.md)指南以开始使用 EF Core。</span><span class="sxs-lookup"><span data-stu-id="df228-107">If you like to learn by writing code, we'd recommend one of our [Getting Started](get-started/index.md) guides to get you started with EF Core.</span></span>

## <a name="latest-version-ef-core-20"></a><span data-ttu-id="df228-108">最新版本：EF Core 2.0</span><span class="sxs-lookup"><span data-stu-id="df228-108">Latest version: EF Core 2.0</span></span>

<span data-ttu-id="df228-109">如果已熟悉 EF Core 并希望直接了解新版本的详细信息，请参阅：</span><span class="sxs-lookup"><span data-stu-id="df228-109">If you are familiar with EF Core and want to jump straight into the details of the new version:</span></span>

- <span data-ttu-id="df228-110">**[EF Core 2.0 中的新增功能](what-is-new/index.md)**</span><span class="sxs-lookup"><span data-stu-id="df228-110">**[New features in EF Core 2.0](what-is-new/index.md)**</span></span>
- <span data-ttu-id="df228-111">**[将现有应用程序升级至 EF Core 2.0](miscellaneous/1x-2x-upgrade.md)**</span><span class="sxs-lookup"><span data-stu-id="df228-111">**[Upgrading existing applications to EF Core 2.0](miscellaneous/1x-2x-upgrade.md)**</span></span>

## <a name="get-entity-framework-core"></a><span data-ttu-id="df228-112">获取 Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="df228-112">Get Entity Framework Core</span></span>

<span data-ttu-id="df228-113">针对要使用的数据库提供程序，[安装 NuGet 包](https://docs.nuget.org/ndocs/quickstart/use-a-package)。</span><span class="sxs-lookup"><span data-stu-id="df228-113">[Install the NuGet package](https://docs.nuget.org/ndocs/quickstart/use-a-package) for the database provider you want to use.</span></span> <span data-ttu-id="df228-114">例如</span><span class="sxs-lookup"><span data-stu-id="df228-114">E.g.</span></span> <span data-ttu-id="df228-115">要在跨平台开发中安装 SQL Server 提供程序，请在命令行中使用 `dotnet` 工具：</span><span class="sxs-lookup"><span data-stu-id="df228-115">to install the SQL Server provider in cross-platform development using `dotnet` tool in the command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="df228-116">或者在 Visual Studio 中，使用包管理器控制台：</span><span class="sxs-lookup"><span data-stu-id="df228-116">Or in Visual Studio, using the Package Manager Console:</span></span>

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
<span data-ttu-id="df228-117">有关可用提供程序的信息，请参阅[数据库提供程序](providers/index.md)；有关更多详细的安装步骤，请参阅[安装 EF Core](get-started/install/index.md)。</span><span class="sxs-lookup"><span data-stu-id="df228-117">See [Database Providers](providers/index.md) for information on available providers and [Installing EF Core](get-started/install/index.md) for more detailed installation steps.</span></span>

## <a name="the-model"></a><span data-ttu-id="df228-118">模型</span><span class="sxs-lookup"><span data-stu-id="df228-118">The Model</span></span>

<span data-ttu-id="df228-119">对于 EF Core，使用模型执行数据访问。</span><span class="sxs-lookup"><span data-stu-id="df228-119">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="df228-120">模型由实体类和表示数据库会话的派生上下文构成，用于查询和保存数据。</span><span class="sxs-lookup"><span data-stu-id="df228-120">A model is made up of entity classes and a derived context that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="df228-121">有关详细信息，请参阅[创建模型](modeling/index.md)。</span><span class="sxs-lookup"><span data-stu-id="df228-121">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="df228-122">可从现有数据库生成模型，手动编码模型使之与数据库相匹配，或使用 EF 迁移基于模型创建数据库（并在模型随时间推移发生更改后进行相应改进）。</span><span class="sxs-lookup"><span data-stu-id="df228-122">You can generate a model from an existing database, hand code a model to match your database, or use EF Migrations to create a database from your model (and evolve it as your model changes over time).</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace Intro
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }
        public int Rating { get; set; }
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

## <a name="querying"></a><span data-ttu-id="df228-123">查询</span><span class="sxs-lookup"><span data-stu-id="df228-123">Querying</span></span>

<span data-ttu-id="df228-124">使用语言集成查询 (LINQ) 从数据库检索实体类的实例。</span><span class="sxs-lookup"><span data-stu-id="df228-124">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="df228-125">有关详细信息，请参阅[查询数据](querying/index.md)。</span><span class="sxs-lookup"><span data-stu-id="df228-125">See [Querying Data](querying/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a><span data-ttu-id="df228-126">保存数据</span><span class="sxs-lookup"><span data-stu-id="df228-126">Saving Data</span></span>

<span data-ttu-id="df228-127">使用实体类的实例在数据库中创建、删除和修改数据。</span><span class="sxs-lookup"><span data-stu-id="df228-127">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="df228-128">有关详细信息，请参阅[保存数据](saving/index.md)。</span><span class="sxs-lookup"><span data-stu-id="df228-128">See [Saving Data](saving/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```
