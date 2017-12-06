---
title: "表映射的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
ms.technology: entity-framework-core
uid: core/modeling/relational/tables
ms.openlocfilehash: 73957d9c77e6856bfb65e10e6b373c337101f7d9
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="table-mapping"></a><span data-ttu-id="de0c1-102">表映射</span><span class="sxs-lookup"><span data-stu-id="de0c1-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="de0c1-103">一般情况下，此部分中的配置是适用于关系数据库。</span><span class="sxs-lookup"><span data-stu-id="de0c1-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="de0c1-104">此处所示的扩展方法将变为可用时安装关系数据库提供程序 (由于共享*Microsoft.EntityFrameworkCore.Relational*包)。</span><span class="sxs-lookup"><span data-stu-id="de0c1-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="de0c1-105">表映射标识应从查询的表数据，并将其保存到数据库中。</span><span class="sxs-lookup"><span data-stu-id="de0c1-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="de0c1-106">约定</span><span class="sxs-lookup"><span data-stu-id="de0c1-106">Conventions</span></span>

<span data-ttu-id="de0c1-107">按照约定，每个实体将安装程序以将映射到与同名表`DbSet<TEntity>`公开派生上下文上的该实体的属性。</span><span class="sxs-lookup"><span data-stu-id="de0c1-107">By convention, each entity will be setup to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="de0c1-108">如果没有`DbSet<TEntity>`包含对于给定的实体中，使用的类名称。</span><span class="sxs-lookup"><span data-stu-id="de0c1-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="de0c1-109">数据注释</span><span class="sxs-lookup"><span data-stu-id="de0c1-109">Data Annotations</span></span>

<span data-ttu-id="de0c1-110">数据注释可用于配置类型可以映射到表。</span><span class="sxs-lookup"><span data-stu-id="de0c1-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

``` csharp
using System.ComponentModel.DataAnnotations.Schema;
```
``` csharp
[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="de0c1-111">你还可以指定此表所属的架构。</span><span class="sxs-lookup"><span data-stu-id="de0c1-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="de0c1-112">Fluent API</span><span class="sxs-lookup"><span data-stu-id="de0c1-112">Fluent API</span></span>

<span data-ttu-id="de0c1-113">Fluent API 可用于配置类型可以映射到表。</span><span class="sxs-lookup"><span data-stu-id="de0c1-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
```
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .ToTable("blogs");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="de0c1-114">你还可以指定此表所属的架构。</span><span class="sxs-lookup"><span data-stu-id="de0c1-114">You can also specify a schema that the table belongs to.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
