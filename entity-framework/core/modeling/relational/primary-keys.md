---
title: 主键的 EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
ms.technology: entity-framework-core
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: fcb1871149c0f20a2576864028b4171904de1982
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052717"
---
# <a name="primary-keys"></a><span data-ttu-id="37aa4-102">主键</span><span class="sxs-lookup"><span data-stu-id="37aa4-102">Primary Keys</span></span>

> [!NOTE]  
> <span data-ttu-id="37aa4-103">一般情况下，此部分中的配置是适用于关系数据库。</span><span class="sxs-lookup"><span data-stu-id="37aa4-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="37aa4-104">此处所示的扩展方法将变为可用时安装关系数据库提供程序 (由于共享*Microsoft.EntityFrameworkCore.Relational*包)。</span><span class="sxs-lookup"><span data-stu-id="37aa4-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="37aa4-105">每个实体类型的键是引入了主键约束。</span><span class="sxs-lookup"><span data-stu-id="37aa4-105">A primary key constraint is introduced for the key of each entity type.</span></span>

## <a name="conventions"></a><span data-ttu-id="37aa4-106">约定</span><span class="sxs-lookup"><span data-stu-id="37aa4-106">Conventions</span></span>

<span data-ttu-id="37aa4-107">按照约定，将名为数据库中的主键`PK_<type name>`。</span><span class="sxs-lookup"><span data-stu-id="37aa4-107">By convention, the primary key in the database will be named `PK_<type name>`.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="37aa4-108">数据注释</span><span class="sxs-lookup"><span data-stu-id="37aa4-108">Data Annotations</span></span>

<span data-ttu-id="37aa4-109">可以使用数据注释不配置为主键的任何关系数据库特定方面。</span><span class="sxs-lookup"><span data-stu-id="37aa4-109">No relational database specific aspects of a primary key can be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="37aa4-110">Fluent API</span><span class="sxs-lookup"><span data-stu-id="37aa4-110">Fluent API</span></span>

<span data-ttu-id="37aa4-111">Fluent API 可用于配置数据库中的主键约束的名称。</span><span class="sxs-lookup"><span data-stu-id="37aa4-111">You can use the Fluent API to configure the name of the primary key constraint in the database.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/KeyName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasKey(b => b.BlogId)
            .HasName("PrimaryKey_BlogId");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
