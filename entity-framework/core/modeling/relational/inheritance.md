---
title: "继承 （关系数据库） 的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
ms.technology: entity-framework-core
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 55286adf08a6a1c3286b7059d747a62e1feffd22
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/22/2017
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="c66d9-102">继承 （关系数据库）</span><span class="sxs-lookup"><span data-stu-id="c66d9-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="c66d9-103">一般而言，本部分中的配置适用于关系数据库。</span><span class="sxs-lookup"><span data-stu-id="c66d9-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="c66d9-104">安装关系数据库提供程序时，此处显示的扩展方法将变为可用（原因在于共享的 Microsoft.EntityFrameworkCore.Relational 包）。</span><span class="sxs-lookup"><span data-stu-id="c66d9-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="c66d9-105">EF 模型中的继承用于控制如何在数据库中表示的实体类中的继承。</span><span class="sxs-lookup"><span data-stu-id="c66d9-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="c66d9-106">目前，每个层次结构一个表的 (TPH) 模式在 EF 核心实现。</span><span class="sxs-lookup"><span data-stu-id="c66d9-106">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="c66d9-107">其他常见模式如每种类型一个表 (TPT) 和表的每种具体的类型 (TPC) 尚不可用。</span><span class="sxs-lookup"><span data-stu-id="c66d9-107">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="c66d9-108">约定</span><span class="sxs-lookup"><span data-stu-id="c66d9-108">Conventions</span></span>

<span data-ttu-id="c66d9-109">按照约定，将使用的每个层次结构一张表 (TPH) 模式映射继承。</span><span class="sxs-lookup"><span data-stu-id="c66d9-109">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="c66d9-110">TPH 使用单个表来存储层次结构中的所有类型的数据。</span><span class="sxs-lookup"><span data-stu-id="c66d9-110">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="c66d9-111">鉴别器列用于标识的每一行代表的类型。</span><span class="sxs-lookup"><span data-stu-id="c66d9-111">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="c66d9-112">在模型中显式包含两个或多个继承的类型时，EF 核心将仅安装程序继承 (请参阅[继承](../inheritance.md)有关详细信息)。</span><span class="sxs-lookup"><span data-stu-id="c66d9-112">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="c66d9-113">下面是一个示例，演示一个简单的继承方案和使用 TPH 模式的关系数据库表中存储的数据。</span><span class="sxs-lookup"><span data-stu-id="c66d9-113">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="c66d9-114">*鉴别器*列标识哪种类型的*博客*存储在每个行。</span><span class="sxs-lookup"><span data-stu-id="c66d9-114">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/Conventions/Samples/InheritanceDbSets.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<RssBlog> RssBlogs { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

![图像](_static/inheritance-tph-data.png)

## <a name="data-annotations"></a><span data-ttu-id="c66d9-116">数据注释</span><span class="sxs-lookup"><span data-stu-id="c66d9-116">Data Annotations</span></span>

<span data-ttu-id="c66d9-117">数据注释不能用于配置继承。</span><span class="sxs-lookup"><span data-stu-id="c66d9-117">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="c66d9-118">Fluent API</span><span class="sxs-lookup"><span data-stu-id="c66d9-118">Fluent API</span></span>

<span data-ttu-id="c66d9-119">Fluent API 可用于配置的名称和类型鉴别器列和用于标识层次结构中的每种类型的值。</span><span class="sxs-lookup"><span data-stu-id="c66d9-119">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/InheritanceTPHDiscriminator.cs?highlight=7,8,9,10)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("blog_type")
            .HasValue<Blog>("blog_base")
            .HasValue<RssBlog>("blog_rss");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```
