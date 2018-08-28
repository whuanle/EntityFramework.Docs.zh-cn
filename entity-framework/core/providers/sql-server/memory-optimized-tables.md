---
title: Microsoft SQL Server 数据库提供程序的内存优化表的 EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 63d2cbf8b69e4f1945ad60914e284fb42c48e8db
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995797"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="84b2c-102">内存优化表支持在 SQL Server EF Core数据库提供程序</span><span class="sxs-lookup"><span data-stu-id="84b2c-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

> [!NOTE]  
>
> <span data-ttu-id="84b2c-103">EF Core 1.1 中已引入此功能。</span><span class="sxs-lookup"><span data-stu-id="84b2c-103">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="84b2c-104">[内存优化表](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables)是整个表在内存中驻留在其中的 SQL Server 的功能。</span><span class="sxs-lookup"><span data-stu-id="84b2c-104">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="84b2c-105">表数据的第二个副本被保留在磁盘上，但仅用于持续性目的。</span><span class="sxs-lookup"><span data-stu-id="84b2c-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="84b2c-106">内存优化表中的数据只能从磁盘中读取在数据库恢复期间。</span><span class="sxs-lookup"><span data-stu-id="84b2c-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="84b2c-107">例如，服务器后，重新启动。</span><span class="sxs-lookup"><span data-stu-id="84b2c-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="84b2c-108">配置内存优化表</span><span class="sxs-lookup"><span data-stu-id="84b2c-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="84b2c-109">你可以指定实体映射到的表是内存优化表。</span><span class="sxs-lookup"><span data-stu-id="84b2c-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="84b2c-110">当使用 EF Core创建和维护的数据库基于你的模型 (通过迁移或`Database.EnsureCreated()`)，将为这些实体创建内存优化表。</span><span class="sxs-lookup"><span data-stu-id="84b2c-110">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
