---
title: 索引的 EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
ms.technology: entity-framework-core
uid: core/modeling/indexes
ms.openlocfilehash: f57b545d53613cec6887734bf434958ee8fff4d8
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054881"
---
# <a name="indexes"></a>索引

索引是跨多个数据存储的一个公共概念。 尽管数据存储中的其实现可能会有所不同，它们用于使基于列 （或一组列） 的查找更加高效。

## <a name="conventions"></a>约定

按照约定，为外键使用每个属性 （或组的属性） 中创建索引。

## <a name="data-annotations"></a>数据注释

不使用数据注释创建索引。

## <a name="fluent-api"></a>Fluent API

Fluent API 可用于的单个属性上指定索引。 默认情况下，索引将非唯一。

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Index.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

此外可以指定索引应是唯一的这意味着任何两个实体可以具有的给定属性的相同值。

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

你还可以通过多个列中指定索引。

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .HasIndex(p => new { p.FirstName, p.LastName });
    }
}

public class Person
{
    public int PersonId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
}
```

> [!TIP]  
> 没有每个非重复的属性集只有一个索引。 如果你使用 Fluent API 配置索引上的一组属性已索引定义，方法是约定或以前的配置，然后将会更改该索引的定义。 这是在你想进一步配置的索引，它由约定很有用。
