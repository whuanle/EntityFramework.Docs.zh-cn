---
title: 自定义迁移历史记录表的 EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
ms.openlocfilehash: 1a253972a8f4e410421ec8a77c079e588d368819
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "45488811"
---
<a name="custom-migrations-history-table"></a>自定义迁移历史记录表
===============================
默认情况下，EF Core将跟踪的哪些迁移已应用于数据库中名为的表记录`__EFMigrationsHistory`。 由于各种原因，你可能想要自定义此表，以更好地满足你的需求。

> [!IMPORTANT]
> 如果自定义迁移历史记录表*后*应用迁移，您应负责更新数据库中的现有表。

<a name="schema-and-table-name"></a>架构和表名称
----------------------
您可以更改架构和表名称使用`MigrationsHistoryTable()`中的方法`OnConfiguring()`(或`ConfigureServices()`上 ASP.NET Core)。 此处是一个示例使用 SQL Server EF Core提供程序。

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a>其他更改
-------------
若要配置的表的其他方面，重写并替换为特定于提供程序的`IHistoryRepository`服务。 下面是更改到 MigrationId 列名称的示例*Id* SQL 服务器上。

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> `SqlServerHistoryRepository` 放内部命名空间内，在未来版本可能会更改。

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
