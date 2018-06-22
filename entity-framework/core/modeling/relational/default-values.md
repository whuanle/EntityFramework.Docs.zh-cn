---
title: 默认值的 EF 核心
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
ms.technology: entity-framework-core
uid: core/modeling/relational/default-values
ms.openlocfilehash: 73b916b6d9f9c984c8ea010f2319eafa7d031a58
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052757"
---
# <a name="default-values"></a><span data-ttu-id="a5623-102">默认值</span><span class="sxs-lookup"><span data-stu-id="a5623-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="a5623-103">一般情况下，此部分中的配置是适用于关系数据库。</span><span class="sxs-lookup"><span data-stu-id="a5623-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="a5623-104">此处所示的扩展方法将变为可用时安装关系数据库提供程序 (由于共享*Microsoft.EntityFrameworkCore.Relational*包)。</span><span class="sxs-lookup"><span data-stu-id="a5623-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="a5623-105">列的默认值是如果插入新行，但未指定值的列将插入的值。</span><span class="sxs-lookup"><span data-stu-id="a5623-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="a5623-106">约定</span><span class="sxs-lookup"><span data-stu-id="a5623-106">Conventions</span></span>

<span data-ttu-id="a5623-107">按照约定，未配置默认值。</span><span class="sxs-lookup"><span data-stu-id="a5623-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="a5623-108">数据注释</span><span class="sxs-lookup"><span data-stu-id="a5623-108">Data Annotations</span></span>

<span data-ttu-id="a5623-109">不可以设置使用数据注释的默认值。</span><span class="sxs-lookup"><span data-stu-id="a5623-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="a5623-110">Fluent API</span><span class="sxs-lookup"><span data-stu-id="a5623-110">Fluent API</span></span>

<span data-ttu-id="a5623-111">可以使用 Fluent API 指定属性的默认值。</span><span class="sxs-lookup"><span data-stu-id="a5623-111">You can use the Fluent API to specify the default value for a property.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultValue.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Rating)
            .HasDefaultValue(3);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public int Rating { get; set; }
}
```

<span data-ttu-id="a5623-112">你还可以指定用于计算默认值的 SQL 片段。</span><span class="sxs-lookup"><span data-stu-id="a5623-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultValueSql.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Created)
            .HasDefaultValueSql("getdate()");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public DateTime Created { get; set; }
}
```
