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
<a name="custom-migrations-history-table"></a><span data-ttu-id="6cd19-102">自定义迁移历史记录表</span><span class="sxs-lookup"><span data-stu-id="6cd19-102">Custom Migrations History Table</span></span>
===============================
<span data-ttu-id="6cd19-103">默认情况下，EF Core将跟踪的哪些迁移已应用于数据库中名为的表记录`__EFMigrationsHistory`。</span><span class="sxs-lookup"><span data-stu-id="6cd19-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="6cd19-104">由于各种原因，你可能想要自定义此表，以更好地满足你的需求。</span><span class="sxs-lookup"><span data-stu-id="6cd19-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6cd19-105">如果自定义迁移历史记录表*后*应用迁移，您应负责更新数据库中的现有表。</span><span class="sxs-lookup"><span data-stu-id="6cd19-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

<a name="schema-and-table-name"></a><span data-ttu-id="6cd19-106">架构和表名称</span><span class="sxs-lookup"><span data-stu-id="6cd19-106">Schema and table name</span></span>
----------------------
<span data-ttu-id="6cd19-107">您可以更改架构和表名称使用`MigrationsHistoryTable()`中的方法`OnConfiguring()`(或`ConfigureServices()`上 ASP.NET Core)。</span><span class="sxs-lookup"><span data-stu-id="6cd19-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="6cd19-108">此处是一个示例使用 SQL Server EF Core提供程序。</span><span class="sxs-lookup"><span data-stu-id="6cd19-108">Here is an example using the SQL Server EF Core provider.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a><span data-ttu-id="6cd19-109">其他更改</span><span class="sxs-lookup"><span data-stu-id="6cd19-109">Other changes</span></span>
-------------
<span data-ttu-id="6cd19-110">若要配置的表的其他方面，重写并替换为特定于提供程序的`IHistoryRepository`服务。</span><span class="sxs-lookup"><span data-stu-id="6cd19-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="6cd19-111">下面是更改到 MigrationId 列名称的示例*Id* SQL 服务器上。</span><span class="sxs-lookup"><span data-stu-id="6cd19-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> <span data-ttu-id="6cd19-112">`SqlServerHistoryRepository` 放内部命名空间内，在未来版本可能会更改。</span><span class="sxs-lookup"><span data-stu-id="6cd19-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

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
