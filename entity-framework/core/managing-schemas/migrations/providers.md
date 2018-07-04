---
title: 多个提供程序的 EF Core的迁移
author: bricelam
ms.author: bricelam
ms.date: 11/8/2017
ms.technology: entity-framework-core
ms.openlocfilehash: d950e74ed4cef7d4274aabcf3eda7b0b735574c6
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2018
ms.locfileid: "30002800"
---
<a name="migrations-with-multiple-providers"></a>多个提供程序的迁移
==================================
[EF Core工具][ 1]仅创建为活动提供程序的迁移的基架。 有时，但是，你可能想要用于多个提供程序 （例如 Microsoft SQL Server 和 SQLite） 你 DbContext。 有两种方法来处理这与迁移。 你可以维护两组迁移-的情况下，为每个提供程序--或合并到单个设置的其中一个可以工作。

<a name="two-migration-sets"></a>两个迁移集
------------------
在第一个方法中，你将生成两个迁移的每个模型更改。

一种办法这是将每个迁移集[中单独的程序集][ 2]和手动添加两个迁移之间进行切换的活动提供程序 （和迁移程序集）。

轻松使用这些工具的另一种方法是创建从 DbContext 派生，并重写活动提供程序的新类型。 此类型用于在设计时添加或将应用迁移的时间。

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> 由于每个迁移集使用其自己的 DbContext 类型，此方法不需要使用具有单独迁移的程序集。

在添加新的迁移时，指定上下文类型。

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> 你不需要指定后续迁移的输出目录，因为它们将创建为与最后一个同级。

<a name="one-migration-set"></a>有一次迁移集
-----------------
如果你不喜欢具有两个集的迁移，你可以手动将它们合并到一组可以应用于两个提供程序中。

批注可以共存，因为提供程序会忽略它不了解任何批注。 例如，适用于 Microsoft SQL Server 和 SQLite 的主键列可能如下所示。

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

如果操作只能应用一个提供程序上 （或它们以不同的方式提供程序之间），使用`ActiveProvider`属性来通知的提供程序处于活动状态。

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
