---
title: 自定义迁移历史记录表的 EF Core
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
<a name="custom-migrations-history-table"></a><span data-ttu-id="5682d-102">自定义迁移历史记录表</span><span class="sxs-lookup"><span data-stu-id="5682d-102">Custom Migrations History Table</span></span>
===============================
<span data-ttu-id="5682d-103">默认情况下，EF Core将跟踪的哪些迁移已应用于数据库中名为的表记录`__EFMigrationsHistory`。</span><span class="sxs-lookup"><span data-stu-id="5682d-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="5682d-104">由于各种原因，你可能想要自定义此表来更好地满足您的需要。</span><span class="sxs-lookup"><span data-stu-id="5682d-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5682d-105">如果你自定义迁移历史记录表*后*应用迁移，你将负责更新数据库中的现有表。</span><span class="sxs-lookup"><span data-stu-id="5682d-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

<a name="schema-and-table-name"></a><span data-ttu-id="5682d-106">架构和表名称</span><span class="sxs-lookup"><span data-stu-id="5682d-106">Schema and table name</span></span>
----------------------
<span data-ttu-id="5682d-107">你可以更改架构和表名称使用`MigrationsHistoryTable()`中的方法`OnConfiguring()`(或`ConfigureServices()`ASP.NET Core 上)。</span><span class="sxs-lookup"><span data-stu-id="5682d-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="5682d-108">此处是一个示例使用 SQL Server EF Core提供程序。</span><span class="sxs-lookup"><span data-stu-id="5682d-108">Here is an example using the SQL Server EF Core provider.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a><span data-ttu-id="5682d-109">其他更改</span><span class="sxs-lookup"><span data-stu-id="5682d-109">Other changes</span></span>
-------------
<span data-ttu-id="5682d-110">若要配置的表的其他方面，覆盖，并将特定于提供程序的`IHistoryRepository`服务。</span><span class="sxs-lookup"><span data-stu-id="5682d-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="5682d-111">下面是更改到的 MigrationId 列名称的一个示例*Id* SQL Server 上。</span><span class="sxs-lookup"><span data-stu-id="5682d-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> <span data-ttu-id="5682d-112">`SqlServerHistoryRepository`放是内部命名空间内，在将来版本可能会更改。</span><span class="sxs-lookup"><span data-stu-id="5682d-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

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
