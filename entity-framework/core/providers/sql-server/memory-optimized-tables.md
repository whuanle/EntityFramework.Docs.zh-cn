---
title: Microsoft SQL Server 数据库提供程序的内存优化表的 EF 核心
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 83a0decffc5946d036903b8b8add59f0ea31b21f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052637"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="69665-102">内存优化表支持在 SQL Server EF 核心数据库提供程序</span><span class="sxs-lookup"><span data-stu-id="69665-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

> [!NOTE]  
>
> <span data-ttu-id="69665-103">EF 核心 1.1 中已引入此功能。</span><span class="sxs-lookup"><span data-stu-id="69665-103">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="69665-104">[内存优化表](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables)是整个表驻留在内存中的 SQL 服务器的功能。</span><span class="sxs-lookup"><span data-stu-id="69665-104">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="69665-105">表数据的另一个副本维护在磁盘上，但仅用于持续性目的。</span><span class="sxs-lookup"><span data-stu-id="69665-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="69665-106">在数据库恢复期间，内存优化的表中的数据只能从磁盘读取。</span><span class="sxs-lookup"><span data-stu-id="69665-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="69665-107">例如，在服务器重新启动后。</span><span class="sxs-lookup"><span data-stu-id="69665-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="69665-108">配置的内存优化表</span><span class="sxs-lookup"><span data-stu-id="69665-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="69665-109">你可以指定一个实体映射到表是内存优化。</span><span class="sxs-lookup"><span data-stu-id="69665-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="69665-110">当使用 EF 核心创建和维护的数据库基于你的模型 (通过迁移或`Database.EnsureCreated()`)，将为这些实体创建内存优化表。</span><span class="sxs-lookup"><span data-stu-id="69665-110">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
