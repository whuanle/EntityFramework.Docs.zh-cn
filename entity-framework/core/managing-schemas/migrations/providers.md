---
title: 多个提供程序的 EF Core 的迁移
author: bricelam
ms.author: bricelam
ms.date: 11/8/2017
ms.openlocfilehash: 7ae695037992323337a780cda29d8c8ed8a13458
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997968"
---
<a name="migrations-with-multiple-providers"></a>多个提供程序的迁移
==================================
[EF Core 工具][ 1]仅创建为活动提供程序的迁移的基架。 有时，但是，你可能想要将多个提供程序 （例如 Microsoft SQL Server 和 SQLite） 用于 DbContext。 有两种方法来处理这种情况与迁移。 你可以保留两个集的迁移-另一个用于每个提供程序--或合并到单个这些设置，可以处理两者。

<a name="two-migration-sets"></a>两个迁移设置
------------------
在第一种方法，生成两个迁移的每个模型更改。

一个方法可以解决这是为了放置每个迁移一组[在单独的程序集中][ 2]并手动添加两个迁移之间进行切换的活动提供程序 （和迁移程序集）。

使用这些工具更轻松的另一种方法是创建派生自 DbContext 和重写活动提供程序的新类型。 此类型用于在设计时添加或应用迁移的时间。

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> 由于每个迁移集使用其自己的 DbContext 类型，此方法不需要使用单独迁移程序集。

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
> 不需要指定以后的迁移的输出目录，因为它们创建为与最后一个同级。

<a name="one-migration-set"></a>一个迁移组
-----------------
如果您不喜欢使用两个集的迁移，可以手动将它们合并成一组可以应用于两个提供程序。

批注可以共存，因为提供程序将忽略任何不理解的注释。 例如，适用于 Microsoft SQL Server 和 SQLite 的主键列可能如下所示。

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

如果操作只能应用一个提供程序上 （或它们以不同的方式之间提供程序），使用`ActiveProvider`属性告诉哪个提供程序处于活动状态。

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
