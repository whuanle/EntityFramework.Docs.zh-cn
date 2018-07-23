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

也可指定性地要求索引的值唯一，也就是说，对于给定的属性，任何两个实体的值都不能相同。

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

还可以跨多个列指定索引。

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
> 每个独特的属性集只有一个索引。对于已经通过约定或以前的配置定义了某个索引的一组属性，如果使用 Fluent API 在其上配置索引，则会更改该索引的定义。如果需要对通过约定创建的索引进行进一步的配置，则可使用此方法。
