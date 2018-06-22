---
title: 自定义迁移历史记录表的 EF 核心
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: cb9892241f3d7f1fae6293bd60a8a5c3e7120969
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
ms.locfileid: "26053807"
---
<a name="custom-migrations-history-table"></a>自定义迁移历史记录表
===============================
默认情况下，EF 核心将跟踪的哪些迁移已应用于数据库中名为的表记录`__EFMigrationsHistory`。 由于各种原因，你可能想要自定义此表来更好地满足您的需要。

> [!IMPORTANT]
> 如果你自定义迁移历史记录表*后*应用迁移，你将负责更新数据库中的现有表。

<a name="schema-and-table-name"></a>架构和表名称
----------------------
你可以更改架构和表名称使用`MigrationsHistoryTable()`中的方法`OnConfiguring()`(或`ConfigureServices()`ASP.NET Core 上)。 此处是一个示例使用 SQL Server EF 核心提供程序。

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a>其他更改
-------------
若要配置的表的其他方面，覆盖，并将特定于提供程序的`IHistoryRepository`服务。 下面是更改到的 MigrationId 列名称的一个示例*Id* SQL Server 上。

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> `SqlServerHistoryRepository`放是内部命名空间内，在将来版本可能会更改。

``` csharp
class MyHistoryRepository : SqlServerHistoryRepository
{
    public MyHistoryRepository(HistoryRepositoryDependencies dependencies)
        : base(dependencies)
    {
    }

    protected override void ConfigureTable(EntityTypeBuilder<HistoryRow> history)
    {
        base.ConfigureTable(history);

        history.Property(h => h.MigrationId).HasColumnName("Id");
    }
}
```
