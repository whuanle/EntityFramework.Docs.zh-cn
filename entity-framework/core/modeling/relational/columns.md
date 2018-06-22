---
title: 列映射的 EF 核心
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
ms.technology: entity-framework-core
uid: core/modeling/relational/columns
ms.openlocfilehash: 697b966dbac892e332fe65feaa4dd11f00dd8298
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052897"
---
# <a name="column-mapping"></a><span data-ttu-id="472bd-102">列映射</span><span class="sxs-lookup"><span data-stu-id="472bd-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="472bd-103">一般情况下，此部分中的配置是适用于关系数据库。</span><span class="sxs-lookup"><span data-stu-id="472bd-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="472bd-104">此处所示的扩展方法将变为可用时安装关系数据库提供程序 (由于共享*Microsoft.EntityFrameworkCore.Relational*包)。</span><span class="sxs-lookup"><span data-stu-id="472bd-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="472bd-105">应从查询哪些列数据，并将其保存到数据库中，将标识列映射。</span><span class="sxs-lookup"><span data-stu-id="472bd-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="472bd-106">约定</span><span class="sxs-lookup"><span data-stu-id="472bd-106">Conventions</span></span>

<span data-ttu-id="472bd-107">按照约定，每个属性将是安装程序以映射到与属性同名的列。</span><span class="sxs-lookup"><span data-stu-id="472bd-107">By convention, each property will be setup to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="472bd-108">数据注释</span><span class="sxs-lookup"><span data-stu-id="472bd-108">Data Annotations</span></span>

<span data-ttu-id="472bd-109">数据注释可用于配置的属性映射的列。</span><span class="sxs-lookup"><span data-stu-id="472bd-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=3)] -->
``` csharp
public class Blog
{
    [Column("blog_id")]
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="472bd-110">Fluent API</span><span class="sxs-lookup"><span data-stu-id="472bd-110">Fluent API</span></span>

<span data-ttu-id="472bd-111">Fluent API 可用于配置的属性映射的列。</span><span class="sxs-lookup"><span data-stu-id="472bd-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/Column.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.BlogId)
            .HasColumnName("blog_id");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
