---
title: 计算的列的 EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
ms.technology: entity-framework-core
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: 95312504286bd34cc666b5a21273835c4b35d379
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052477"
---
# <a name="computed-columns"></a>计算列

> [!NOTE]  
> 一般情况下，此部分中的配置是适用于关系数据库。 此处所示的扩展方法将变为可用时安装关系数据库提供程序 (由于共享*Microsoft.EntityFrameworkCore.Relational*包)。

计算的列是在数据库中计算其值的列。 计算的列可以使用表中其他列，若要计算其值。

## <a name="conventions"></a>约定

按照约定，计算的列不在模型中创建。

## <a name="data-annotations"></a>数据注释

计算的列不可以使用数据注释配置。

## <a name="fluent-api"></a>Fluent API

可以使用 Fluent API 指定属性应映射到计算列。

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/ComputedColumn.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .Property(p => p.DisplayName)
            .HasComputedColumnSql("[LastName] + ', ' + [FirstName]");
    }
}

public class Person
{
    public int PersonId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string DisplayName { get; set; }
}
```
