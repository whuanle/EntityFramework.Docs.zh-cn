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
# <a name="default-schema"></a><span data-ttu-id="c016a-102">默认架构</span><span class="sxs-lookup"><span data-stu-id="c016a-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="c016a-103">一般情况下，此部分中的配置是适用于关系数据库。</span><span class="sxs-lookup"><span data-stu-id="c016a-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="c016a-104">此处所示的扩展方法将变为可用时安装关系数据库提供程序 (由于共享*Microsoft.EntityFrameworkCore.Relational*包)。</span><span class="sxs-lookup"><span data-stu-id="c016a-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="c016a-105">默认架构是将在中创建对象，如果为该对象未显式配置架构的数据库架构。</span><span class="sxs-lookup"><span data-stu-id="c016a-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="c016a-106">约定</span><span class="sxs-lookup"><span data-stu-id="c016a-106">Conventions</span></span>

<span data-ttu-id="c016a-107">按照约定，数据库提供程序将选择最合适的默认架构。</span><span class="sxs-lookup"><span data-stu-id="c016a-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="c016a-108">例如，Microsoft SQL Server 将使用`dbo`架构和 SQLite 将不使用架构 （因为 SQLite 中不支持架构）。</span><span class="sxs-lookup"><span data-stu-id="c016a-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="c016a-109">数据注释</span><span class="sxs-lookup"><span data-stu-id="c016a-109">Data Annotations</span></span>

<span data-ttu-id="c016a-110">不能设置使用数据注释的默认架构。</span><span class="sxs-lookup"><span data-stu-id="c016a-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="c016a-111">Fluent API</span><span class="sxs-lookup"><span data-stu-id="c016a-111">Fluent API</span></span>

<span data-ttu-id="c016a-112">可以使用 Fluent API 指定默认架构。</span><span class="sxs-lookup"><span data-stu-id="c016a-112">You can use the Fluent API to specify a default schema.</span></span>

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
