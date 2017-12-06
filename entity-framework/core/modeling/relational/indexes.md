---
title: "索引的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: 683b580bb155e0416f13c5d63e3280078fbcee21
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="indexes"></a><span data-ttu-id="1e720-102">索引</span><span class="sxs-lookup"><span data-stu-id="1e720-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="1e720-103">一般情况下，此部分中的配置是适用于关系数据库。</span><span class="sxs-lookup"><span data-stu-id="1e720-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="1e720-104">此处所示的扩展方法将变为可用时安装关系数据库提供程序 (由于共享*Microsoft.EntityFrameworkCore.Relational*包)。</span><span class="sxs-lookup"><span data-stu-id="1e720-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="1e720-105">关系数据库中的索引映射到索引中的实体框架的核心概念相同。</span><span class="sxs-lookup"><span data-stu-id="1e720-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="1e720-106">约定</span><span class="sxs-lookup"><span data-stu-id="1e720-106">Conventions</span></span>

<span data-ttu-id="1e720-107">按照约定，名为索引`IX_<type name>_<property name>`。</span><span class="sxs-lookup"><span data-stu-id="1e720-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="1e720-108">对于组合索引`<property name>`将成为下划线分隔列表的属性名称。</span><span class="sxs-lookup"><span data-stu-id="1e720-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="1e720-109">数据注释</span><span class="sxs-lookup"><span data-stu-id="1e720-109">Data Annotations</span></span>

<span data-ttu-id="1e720-110">不能使用数据注释配置索引。</span><span class="sxs-lookup"><span data-stu-id="1e720-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="1e720-111">Fluent API</span><span class="sxs-lookup"><span data-stu-id="1e720-111">Fluent API</span></span>

<span data-ttu-id="1e720-112">Fluent API 可用于配置索引的名称。</span><span class="sxs-lookup"><span data-stu-id="1e720-112">You can use the Fluent API to configure the name of an index.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/IndexName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .HasName("Index_Url");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
