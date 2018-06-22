---
title: 默认架构的 EF 核心
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
ms.technology: entity-framework-core
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 26106deb2d4e35ecf33e97790a83f9af77991aed
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052747"
---
# <a name="default-schema"></a>默认架构

> [!NOTE]  
> 一般情况下，此部分中的配置是适用于关系数据库。 此处所示的扩展方法将变为可用时安装关系数据库提供程序 (由于共享*Microsoft.EntityFrameworkCore.Relational*包)。

默认架构是将在中创建对象，如果为该对象未显式配置架构的数据库架构。

## <a name="conventions"></a>约定

按照约定，数据库提供程序将选择最合适的默认架构。 例如，Microsoft SQL Server 将使用`dbo`架构和 SQLite 将不使用架构 （因为 SQLite 中不支持架构）。

## <a name="data-annotations"></a>数据注释

不能设置使用数据注释的默认架构。

## <a name="fluent-api"></a>Fluent API

可以使用 Fluent API 指定默认架构。

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultSchema.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasDefaultSchema("blogging");
    }
}
```
