---
title: Microsoft SQL Server 数据库提供程序的内存优化表的 EF Core
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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>内存优化表支持在 SQL Server EF Core数据库提供程序

> [!NOTE]  
>
> EF Core 1.1 中已引入此功能。

[内存优化表](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables)是整个表驻留在内存中的 SQL 服务器的功能。 表数据的另一个副本维护在磁盘上，但仅用于持续性目的。 在数据库恢复期间，内存优化的表中的数据只能从磁盘读取。 例如，在服务器重新启动后。

## <a name="configuring-a-memory-optimized-table"></a>配置的内存优化表

你可以指定一个实体映射到表是内存优化。 当使用 EF Core创建和维护的数据库基于你的模型 (通过迁移或`Database.EnsureCreated()`)，将为这些实体创建内存优化表。

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
