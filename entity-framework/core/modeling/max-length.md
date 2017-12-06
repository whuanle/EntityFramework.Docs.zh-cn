---
title: "最大长度-EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
ms.technology: entity-framework-core
uid: core/modeling/max-length
ms.openlocfilehash: 7325c0c3328477473392bf9e7c82f1696bb4f424
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="maximum-length"></a>最大长度

配置的最大长度提供了关于给定属性适用于使用适当的数据类型与数据存储的提示。 最大长度仅适用于数组数据类型，如`string`和`byte[]`。

> [!NOTE]  
> 实体框架不执行最大长度的任何的验证，然后再将数据传递给提供程序。 负责要验证如果相应的提供程序或数据存储区。 例如，面向 SQL Server，超过最大长度将结果作为基础列的数据类型引发异常时将不允许过多的数据存储。

## <a name="conventions"></a>约定

按照约定，它将保持最多要选择适当的数据类型的属性的数据库提供程序。 对于具有长度的属性，数据库提供程序通常将选择的数据类型必须允许的最长长度的数据。 例如，Microsoft SQL Server 将使用`nvarchar(max)`为`string`属性 (或`nvarchar(450)`如果列用作键)。

## <a name="data-annotations"></a>数据注释

数据注释可用于配置属性的最大长度。 在此示例中，面向 SQL Server，这将导致`nvarchar(500)`正在使用的数据类型。

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

Fluent API 可用于配置属性的最大长度。 在此示例中，面向 SQL Server，这将导致`nvarchar(500)`正在使用的数据类型。

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
