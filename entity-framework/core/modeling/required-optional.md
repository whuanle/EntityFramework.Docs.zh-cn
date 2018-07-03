---
title: 必需/可选属性-EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
ms.technology: entity-framework-core
uid: core/modeling/required-optional
ms.openlocfilehash: 2af1d49e12ef980f81cb9c00556dee471673ccae
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052847"
---
# <a name="required-and-optional-properties"></a>必选和可选属性

该属性被视为可选如果有效它才能包含`null`。 如果`null`不是有效的值分配给属性中，则可以认为是必需的属性。

## <a name="conventions"></a>约定

按照约定，其 CLR 类型可以包含 null 的属性将被配置为可选 (`string`， `int?`，`byte[]`等。)。 将配置其 CLR 类型不能包含 null 的属性所需的方式 (`int`， `decimal`，`bool`等。)。

> [!NOTE]  
> 其 CLR 类型不能包含 null 的属性不能配置为可选。 始终将所需的实体框架视为属性。

## <a name="data-annotations"></a>数据注释

可以使用数据注释以指示需要一个属性。

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Fluent API

可以使用 Fluent API 以指示需要一个属性。

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
