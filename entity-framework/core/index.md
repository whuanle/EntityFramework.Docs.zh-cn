---
title: 概述 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: d9fcafb35248b1af54e1ac707e2ff7d4e80e4aa2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995646"
---
# <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework (EF) Core 是轻量化、可扩展和跨平台版的常用 Entity Framework 数据访问技术。

EF Core 可用作对象关系映射程序 (O/RM)，以便于 .NET 开发人员能够使用 .NET 对象来处理数据库，这样就不必经常编写大部分数据访问代码了。

EF Core 支持多个数据库引擎，请参阅[数据库提供程序](providers/index.md)了解详细信息。

如果希望通过编写代码进行了解，建议你阅读其中一篇[入门](get-started/index.md)指南以开始使用 EF Core。

## <a name="what-is-new-in-ef-core"></a>EF Core 中的新增功能

如果已熟悉 EF Core 并希望直接了解新版本的详细信息，请参阅：

- **[EF Core 2.1中的新增功能](xref:core/what-is-new/ef-core-2.1)**
- **[将现有应用程序升级至 EF Core 2.x](xref:core/miscellaneous/1x-2x-upgrade)**


## <a name="get-entity-framework-core"></a>获取 Entity Framework Core

针对要使用的数据库提供程序，[安装 NuGet 包](https://docs.nuget.org/ndocs/quickstart/use-a-package)。 例如，要在跨平台开发中安装 SQL Server 提供程序，请在命令行中使用 `dotnet` 工具：

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

或者在 Visual Studio 中，使用包管理器控制台：

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
有关可用提供程序的信息，请参阅[数据库提供程序](providers/index.md)；有关更多详细的安装步骤，请参阅[安装 EF Core](get-started/install/index.md)。

## <a name="the-model"></a>模型

对于 EF Core，使用模型执行数据访问。 模型由实体类和表示数据库会话的派生上下文构成，用于查询和保存数据。 有关详细信息，请参阅[创建模型](modeling/index.md)。

可从现有数据库生成模型，手动编码模型使之与数据库相匹配，或使用 EF 迁移基于模型创建数据库（并在模型随时间推移发生更改后进行相应改进）。

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

## <a name="querying"></a>查询

使用语言集成查询 (LINQ) 从数据库检索实体类的实例。 有关详细信息，请参阅[查询数据](querying/index.md)。

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a>保存数据

使用实体类的实例在数据库中创建、删除和修改数据。 有关详细信息，请参阅[保存数据](saving/index.md)。

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```
