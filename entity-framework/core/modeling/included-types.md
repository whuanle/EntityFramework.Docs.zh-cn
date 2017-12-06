---
title: "包括和排除类型的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
ms.technology: entity-framework-core
uid: core/modeling/included-types
ms.openlocfilehash: a8d7293a144968d2506bdcc76e55a1a0b1e3fd4b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="including--excluding-types"></a><span data-ttu-id="4a7e6-102">包括和排除类型</span><span class="sxs-lookup"><span data-stu-id="4a7e6-102">Including & Excluding Types</span></span>

<span data-ttu-id="4a7e6-103">在模型意味着，EF 的元数据，该类型并且将尝试读取和写入从/到数据库的实例中包含一种类型。</span><span class="sxs-lookup"><span data-stu-id="4a7e6-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="4a7e6-104">约定</span><span class="sxs-lookup"><span data-stu-id="4a7e6-104">Conventions</span></span>

<span data-ttu-id="4a7e6-105">按照约定，在中公开的类型`DbSet`上您的上下文属性都包含在您的模型。</span><span class="sxs-lookup"><span data-stu-id="4a7e6-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="4a7e6-106">此外，会在中提到的类型`OnModelCreating`，还提供了方法。</span><span class="sxs-lookup"><span data-stu-id="4a7e6-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="4a7e6-107">最后，通过以递归方式浏览发现的类型的导航属性找到的任何类型也会包括在模型中。</span><span class="sxs-lookup"><span data-stu-id="4a7e6-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="4a7e6-108">**例如，下面的代码清单中发现所有三种类型：**</span><span class="sxs-lookup"><span data-stu-id="4a7e6-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="4a7e6-109">`Blog`因为在公开`DbSet`上下文属性</span><span class="sxs-lookup"><span data-stu-id="4a7e6-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="4a7e6-110">`Post`因为发现通过`Blog.Posts`导航属性</span><span class="sxs-lookup"><span data-stu-id="4a7e6-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="4a7e6-111">`AuditEntry`因为中提到`OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="4a7e6-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/IncludedTypes.cs?highlight=3,7,16)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<AuditEntry>();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}

public class AuditEntry
{
    public int AuditEntryId { get; set; }
    public string Username { get; set; }
    public string Action { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="4a7e6-112">数据注释</span><span class="sxs-lookup"><span data-stu-id="4a7e6-112">Data Annotations</span></span>

<span data-ttu-id="4a7e6-113">你可以使用数据注释从模型排除类型。</span><span class="sxs-lookup"><span data-stu-id="4a7e6-113">You can use Data Annotations to exclude a type from the model.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/IgnoreType.cs?highlight=9)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogMetadata Metadata { get; set; }
}

[NotMapped]
public class BlogMetadata
{
    public DateTime LoadedFromDatabase { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="4a7e6-114">Fluent API</span><span class="sxs-lookup"><span data-stu-id="4a7e6-114">Fluent API</span></span>

<span data-ttu-id="4a7e6-115">Fluent API 可用于从模型中排除类型。</span><span class="sxs-lookup"><span data-stu-id="4a7e6-115">You can use the Fluent API to exclude a type from the model.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IgnoreType.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Ignore<BlogMetadata>();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogMetadata Metadata { get; set; }
}

public class BlogMetadata
{
    public DateTime LoadedFromDatabase { get; set; }
}
```
