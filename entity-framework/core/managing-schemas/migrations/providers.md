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
<a name="migrations-with-multiple-providers"></a><span data-ttu-id="303a5-102">多个提供程序的迁移</span><span class="sxs-lookup"><span data-stu-id="303a5-102">Migrations with Multiple Providers</span></span>
==================================
<span data-ttu-id="303a5-103">[EF Core 工具][ 1]仅创建为活动提供程序的迁移的基架。</span><span class="sxs-lookup"><span data-stu-id="303a5-103">The [EF Core Tools][1] only scaffold migrations for the active provider.</span></span> <span data-ttu-id="303a5-104">有时，但是，你可能想要将多个提供程序 （例如 Microsoft SQL Server 和 SQLite） 用于 DbContext。</span><span class="sxs-lookup"><span data-stu-id="303a5-104">Sometimes, however, you may want to use more than one provider (for example Microsoft SQL Server and SQLite) with your DbContext.</span></span> <span data-ttu-id="303a5-105">有两种方法来处理这种情况与迁移。</span><span class="sxs-lookup"><span data-stu-id="303a5-105">There are two ways to handle this with Migrations.</span></span> <span data-ttu-id="303a5-106">你可以保留两个集的迁移-另一个用于每个提供程序--或合并到单个这些设置，可以处理两者。</span><span class="sxs-lookup"><span data-stu-id="303a5-106">You can maintain two sets of migrations--one for each provider--or merge them into a single set that can work on both.</span></span>

<a name="two-migration-sets"></a><span data-ttu-id="303a5-107">两个迁移设置</span><span class="sxs-lookup"><span data-stu-id="303a5-107">Two migration sets</span></span>
------------------
<span data-ttu-id="303a5-108">在第一种方法，生成两个迁移的每个模型更改。</span><span class="sxs-lookup"><span data-stu-id="303a5-108">In the first approach, you generate two migrations for each model change.</span></span>

<span data-ttu-id="303a5-109">一个方法可以解决这是为了放置每个迁移一组[在单独的程序集中][ 2]并手动添加两个迁移之间进行切换的活动提供程序 （和迁移程序集）。</span><span class="sxs-lookup"><span data-stu-id="303a5-109">One way to do this is to put each migration set [in a separate assembly][2] and manually switch the active provider (and migrations assembly) between adding the two migrations.</span></span>

<span data-ttu-id="303a5-110">使用这些工具更轻松的另一种方法是创建派生自 DbContext 和重写活动提供程序的新类型。</span><span class="sxs-lookup"><span data-stu-id="303a5-110">Another approach that makes working with the tools easier is to create a new type that derives from your DbContext and overrides the active provider.</span></span> <span data-ttu-id="303a5-111">此类型用于在设计时添加或应用迁移的时间。</span><span class="sxs-lookup"><span data-stu-id="303a5-111">This type is used at design time when adding or applying migrations.</span></span>

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> <span data-ttu-id="303a5-112">由于每个迁移集使用其自己的 DbContext 类型，此方法不需要使用单独迁移程序集。</span><span class="sxs-lookup"><span data-stu-id="303a5-112">Since each migration set uses its own DbContext types, this approach doesn't require using a separate migrations assembly.</span></span>

<span data-ttu-id="303a5-113">在添加新的迁移时，指定上下文类型。</span><span class="sxs-lookup"><span data-stu-id="303a5-113">When adding new migration, specify the context types.</span></span>

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> <span data-ttu-id="303a5-114">不需要指定以后的迁移的输出目录，因为它们创建为与最后一个同级。</span><span class="sxs-lookup"><span data-stu-id="303a5-114">You don't need to specify the output directory for subsequent migrations since they are created as siblings to the last one.</span></span>

<a name="one-migration-set"></a><span data-ttu-id="303a5-115">一个迁移组</span><span class="sxs-lookup"><span data-stu-id="303a5-115">One migration set</span></span>
-----------------
<span data-ttu-id="303a5-116">如果您不喜欢使用两个集的迁移，可以手动将它们合并成一组可以应用于两个提供程序。</span><span class="sxs-lookup"><span data-stu-id="303a5-116">If you don't like having two sets of migrations, you can manually combine them into a single set that can be applied to both providers.</span></span>

<span data-ttu-id="303a5-117">批注可以共存，因为提供程序将忽略任何不理解的注释。</span><span class="sxs-lookup"><span data-stu-id="303a5-117">Annotations can coexist since a provider ignores any annotations that it doesn't understand.</span></span> <span data-ttu-id="303a5-118">例如，适用于 Microsoft SQL Server 和 SQLite 的主键列可能如下所示。</span><span class="sxs-lookup"><span data-stu-id="303a5-118">For example, a primary key column that works with both Microsoft SQL Server and SQLite might look like this.</span></span>

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

<span data-ttu-id="303a5-119">如果操作只能应用一个提供程序上 （或它们以不同的方式之间提供程序），使用`ActiveProvider`属性告诉哪个提供程序处于活动状态。</span><span class="sxs-lookup"><span data-stu-id="303a5-119">If operations can only be applied on one provider (or they're differently between providers), use the `ActiveProvider` property to tell which provider is active.</span></span>

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
