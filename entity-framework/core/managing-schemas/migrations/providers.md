---
title: 多个提供程序的 EF 核心的迁移
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
<a name="migrations-with-multiple-providers"></a><span data-ttu-id="41407-102">多个提供程序的迁移</span><span class="sxs-lookup"><span data-stu-id="41407-102">Migrations with Multiple Providers</span></span>
==================================
<span data-ttu-id="41407-103">[EF 核心工具][ 1]仅创建为活动提供程序的迁移的基架。</span><span class="sxs-lookup"><span data-stu-id="41407-103">The [EF Core Tools][1] only scaffold migrations for the active provider.</span></span> <span data-ttu-id="41407-104">有时，但是，你可能想要用于多个提供程序 （例如 Microsoft SQL Server 和 SQLite） 你 DbContext。</span><span class="sxs-lookup"><span data-stu-id="41407-104">Sometimes, however, you may want to use more than one provider (for example Microsoft SQL Server and SQLite) with your DbContext.</span></span> <span data-ttu-id="41407-105">有两种方法来处理这与迁移。</span><span class="sxs-lookup"><span data-stu-id="41407-105">There are two ways to handle this with Migrations.</span></span> <span data-ttu-id="41407-106">你可以维护两组迁移-的情况下，为每个提供程序--或合并到单个设置的其中一个可以工作。</span><span class="sxs-lookup"><span data-stu-id="41407-106">You can maintain two sets of migrations--one for each provider--or merge them into a single set that can work on both.</span></span>

<a name="two-migration-sets"></a><span data-ttu-id="41407-107">两个迁移集</span><span class="sxs-lookup"><span data-stu-id="41407-107">Two migration sets</span></span>
------------------
<span data-ttu-id="41407-108">在第一个方法中，你将生成两个迁移的每个模型更改。</span><span class="sxs-lookup"><span data-stu-id="41407-108">In the first approach, you generate two migrations for each model change.</span></span>

<span data-ttu-id="41407-109">一种办法这是将每个迁移集[中单独的程序集][ 2]和手动添加两个迁移之间进行切换的活动提供程序 （和迁移程序集）。</span><span class="sxs-lookup"><span data-stu-id="41407-109">One way to do this is to put each migration set [in a separate assembly][2] and manually switch the active provider (and migrations assembly) between adding the two migrations.</span></span>

<span data-ttu-id="41407-110">轻松使用这些工具的另一种方法是创建从 DbContext 派生，并重写活动提供程序的新类型。</span><span class="sxs-lookup"><span data-stu-id="41407-110">Another approach that makes working with the tools easier is to create a new type that derives from your DbContext and overrides the active provider.</span></span> <span data-ttu-id="41407-111">此类型用于在设计时添加或将应用迁移的时间。</span><span class="sxs-lookup"><span data-stu-id="41407-111">This type is used at design time when adding or applying migrations.</span></span>

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> <span data-ttu-id="41407-112">由于每个迁移集使用其自己的 DbContext 类型，此方法不需要使用具有单独迁移的程序集。</span><span class="sxs-lookup"><span data-stu-id="41407-112">Since each migration set uses its own DbContext types, this approach doesn't require using a separate migrations assembly.</span></span>

<span data-ttu-id="41407-113">在添加新的迁移时，指定上下文类型。</span><span class="sxs-lookup"><span data-stu-id="41407-113">When adding new migration, specify the context types.</span></span>

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> <span data-ttu-id="41407-114">你不需要指定后续迁移的输出目录，因为它们将创建为与最后一个同级。</span><span class="sxs-lookup"><span data-stu-id="41407-114">You don't need to specify the output directory for subsequent migrations since they are created as siblings to the last one.</span></span>

<a name="one-migration-set"></a><span data-ttu-id="41407-115">有一次迁移集</span><span class="sxs-lookup"><span data-stu-id="41407-115">One migration set</span></span>
-----------------
<span data-ttu-id="41407-116">如果你不喜欢具有两个集的迁移，你可以手动将它们合并到一组可以应用于两个提供程序中。</span><span class="sxs-lookup"><span data-stu-id="41407-116">If you don't like having two sets of migrations, you can manually combine them into a single set that can be applied to both providers.</span></span>

<span data-ttu-id="41407-117">批注可以共存，因为提供程序会忽略它不了解任何批注。</span><span class="sxs-lookup"><span data-stu-id="41407-117">Annotations can coexist since a provider ignores any annotations that it doesn't understand.</span></span> <span data-ttu-id="41407-118">例如，适用于 Microsoft SQL Server 和 SQLite 的主键列可能如下所示。</span><span class="sxs-lookup"><span data-stu-id="41407-118">For example, a primary key column that works with both Microsoft SQL Server and SQLite might look like this.</span></span>

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

<span data-ttu-id="41407-119">如果操作只能应用一个提供程序上 （或它们以不同的方式提供程序之间），使用`ActiveProvider`属性来通知的提供程序处于活动状态。</span><span class="sxs-lookup"><span data-stu-id="41407-119">If operations can only be applied on one provider (or they're differently between providers), use the `ActiveProvider` property to tell which provider is active.</span></span>

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
