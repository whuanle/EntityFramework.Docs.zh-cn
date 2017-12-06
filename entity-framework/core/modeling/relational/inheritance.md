---
title: "继承 （关系数据库） 的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
ms.technology: entity-framework-core
uid: core/modeling/relational/inheritance
ms.openlocfilehash: a7f697dfe2b93c7b93a2dd14945732db4f37628c
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="inheritance-relational-database"></a>继承 （关系数据库）

> [!NOTE]  
> 一般情况下，此部分中的配置是适用于关系数据库。 此处所示的扩展方法将变为可用时安装关系数据库提供程序 (由于共享*Microsoft.EntityFrameworkCore.Relational*包)。

EF 模型中的继承用于控制如何在数据库中表示的实体类中的继承。

## <a name="conventions"></a>约定

按照约定，将使用的每个层次结构一张表 (TPH) 模式映射继承。 TPH 使用单个表来存储层次结构中的所有类型的数据。 鉴别器列用于标识的每一行代表的类型。

在模型中显式包含两个或多个继承的类型时，EF 将仅安装程序继承 (请参阅[继承](../inheritance.md)有关详细信息)。

下面是一个示例，演示一个简单的继承方案和使用 TPH 模式的关系数据库表中存储的数据。 *鉴别器*列标识哪种类型的*博客*存储在每个行。

<!-- [!code-csharp[Main](samples/core/relational/Modeling/Conventions/Samples/InheritanceDbSets.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<RssBlog> RssBlogs { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

![图像](_static/inheritance-tph-data.png)

## <a name="data-annotations"></a>数据注释

数据注释不能用于配置继承。

## <a name="fluent-api"></a>Fluent API

Fluent API 可用于配置的名称和类型鉴别器列和用于标识层次结构中的每种类型的值。

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/InheritanceTPHDiscriminator.cs?highlight=7,8,9,10)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("blog_type")
            .HasValue<Blog>("blog_base")
            .HasValue<RssBlog>("blog_rss");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```
