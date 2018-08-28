---
title: 最大长度-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: e54d3671f378b96a49eaf4cb312e72072813fc6d
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996186"
---
# <a name="maximum-length"></a>最大长度

配置最大长度可提供对有关要使用的给定属性的相应的数据类型的数据存储区的提示。 最大长度仅适用于数组数据类型，如`string`和`byte[]`。

> [!NOTE]  
> 实体框架不会执行操作最大长度然后再将数据传递给提供程序的任何的验证。 负责要验证是否适当的提供程序或数据存储区。 例如，针对 SQL Server，超出最大长度会导致异常，因为基础列的数据类型时将不允许过多数据存储。

## <a name="conventions"></a>约定

按照约定，它是应由数据库提供程序选择适当的数据类型的属性。 对于具有长度的属性，数据库提供程序通常将选择允许的最长长度的数据的数据类型。 例如，将使用 Microsoft SQL Server`nvarchar(max)`有关`string`属性 (或`nvarchar(450)`如果列用作键)。

## <a name="data-annotations"></a>数据注释

可以使用数据注释来配置属性的最大长度。 在此示例中，面向 SQL Server，这将导致`nvarchar(500)`所使用的数据类型。

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [MaxLength(500)]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Fluent API

可以使用 Fluent API 配置属性的最大长度。 在此示例中，面向 SQL Server，这将导致`nvarchar(500)`所使用的数据类型。

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/MaxLength.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .HasMaxLength(500);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
