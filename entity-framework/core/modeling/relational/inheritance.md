---
title: 继承 （关系数据库） 的 EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
ms.technology: entity-framework-core
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 22eed0002b5903d3cfd18a7e4af0fcd2d46a5c4c
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2018
ms.locfileid: "29152348"
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="6e76b-102">继承 （关系数据库）</span><span class="sxs-lookup"><span data-stu-id="6e76b-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="6e76b-103">一般而言，本部分中的配置适用于关系数据库。</span><span class="sxs-lookup"><span data-stu-id="6e76b-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="6e76b-104">安装关系数据库提供程序时，此处显示的扩展方法将变为可用（原因在于共享的 Microsoft.EntityFrameworkCore.Relational 包）。</span><span class="sxs-lookup"><span data-stu-id="6e76b-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="6e76b-105">EF 模型中的继承用于控制如何在数据库中表示的实体类中的继承。</span><span class="sxs-lookup"><span data-stu-id="6e76b-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="6e76b-106">目前，每个层次结构一个表的 (TPH) 模式在 EF Core实现。</span><span class="sxs-lookup"><span data-stu-id="6e76b-106">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="6e76b-107">其他常见模式如每种类型一个表 (TPT) 和表的每种具体的类型 (TPC) 尚不可用。</span><span class="sxs-lookup"><span data-stu-id="6e76b-107">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="6e76b-108">约定</span><span class="sxs-lookup"><span data-stu-id="6e76b-108">Conventions</span></span>

<span data-ttu-id="6e76b-109">按照约定，将使用的每个层次结构一张表 (TPH) 模式映射继承。</span><span class="sxs-lookup"><span data-stu-id="6e76b-109">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="6e76b-110">TPH 使用单个表来存储层次结构中的所有类型的数据。</span><span class="sxs-lookup"><span data-stu-id="6e76b-110">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="6e76b-111">鉴别器列用于标识的每一行代表的类型。</span><span class="sxs-lookup"><span data-stu-id="6e76b-111">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="6e76b-112">在模型中显式包含两个或多个继承的类型时，EF Core将仅安装程序继承 (请参阅[继承](../inheritance.md)有关详细信息)。</span><span class="sxs-lookup"><span data-stu-id="6e76b-112">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="6e76b-113">下面是一个示例，演示一个简单的继承方案和使用 TPH 模式的关系数据库表中存储的数据。</span><span class="sxs-lookup"><span data-stu-id="6e76b-113">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="6e76b-114">*鉴别器*列标识哪种类型的*博客*存储在每个行。</span><span class="sxs-lookup"><span data-stu-id="6e76b-114">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="6e76b-116">数据注释</span><span class="sxs-lookup"><span data-stu-id="6e76b-116">Data Annotations</span></span>

<span data-ttu-id="6e76b-117">数据注释不能用于配置继承。</span><span class="sxs-lookup"><span data-stu-id="6e76b-117">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="6e76b-118">Fluent API</span><span class="sxs-lookup"><span data-stu-id="6e76b-118">Fluent API</span></span>

<span data-ttu-id="6e76b-119">Fluent API 可用于配置的名称和类型鉴别器列和用于标识层次结构中的每种类型的值。</span><span class="sxs-lookup"><span data-stu-id="6e76b-119">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

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

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="6e76b-120">配置鉴别器属性</span><span class="sxs-lookup"><span data-stu-id="6e76b-120">Configuring the discriminator property</span></span>

<span data-ttu-id="6e76b-121">在上面的示例中，鉴别器创建作为[隐藏属性](xref:core/modeling/shadow-properties)的基实体的层次结构。</span><span class="sxs-lookup"><span data-stu-id="6e76b-121">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="6e76b-122">由于这是在模型中的属性，可以将它配置其他属性一样。</span><span class="sxs-lookup"><span data-stu-id="6e76b-122">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="6e76b-123">例如，若要在使用默认值，通过约定鉴别器时设置的最大长度：</span><span class="sxs-lookup"><span data-stu-id="6e76b-123">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

<span data-ttu-id="6e76b-124">鉴别器还可以映射到你的实体中的实际 CLR 属性中。</span><span class="sxs-lookup"><span data-stu-id="6e76b-124">The discriminator can also be mapped to an actual CLR property in your entity.</span></span> <span data-ttu-id="6e76b-125">例如:</span><span class="sxs-lookup"><span data-stu-id="6e76b-125">For example:</span></span>
```C#
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("BlogType");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public string BlogType { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

<span data-ttu-id="6e76b-126">组合这两项操作就可以同时将鉴别器映射到某个实际属性并将其配置：</span><span class="sxs-lookup"><span data-stu-id="6e76b-126">Combining these two things together it is possible to both map the discriminator to a real property and configure it:</span></span>
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
