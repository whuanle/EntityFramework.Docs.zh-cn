---
title: 包括和排除类型的 EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: a5a14f62524754fed179e9a41fac5e29faf185ca
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996145"
---
# <a name="including--excluding-types"></a><span data-ttu-id="fe49f-102">包括和排除类型</span><span class="sxs-lookup"><span data-stu-id="fe49f-102">Including & Excluding Types</span></span>

<span data-ttu-id="fe49f-103">模型表示 EF 有关具有元数据类型，将尝试读取和写入从/向数据库实例中包含一个类型。</span><span class="sxs-lookup"><span data-stu-id="fe49f-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="fe49f-104">约定</span><span class="sxs-lookup"><span data-stu-id="fe49f-104">Conventions</span></span>

<span data-ttu-id="fe49f-105">按照约定，在中公开的类型`DbSet`上您的上下文属性都包含在您的模型。</span><span class="sxs-lookup"><span data-stu-id="fe49f-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="fe49f-106">此外，类型中提及的`OnModelCreating`，还提供了方法。</span><span class="sxs-lookup"><span data-stu-id="fe49f-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="fe49f-107">最后，通过以递归方式浏览已发现类型的导航属性找到的任何类型也包括在模型中。</span><span class="sxs-lookup"><span data-stu-id="fe49f-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="fe49f-108">**例如，以下代码列表中发现所有三种类型：**</span><span class="sxs-lookup"><span data-stu-id="fe49f-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="fe49f-109">`Blog` 因为在公开`DbSet`上下文属性</span><span class="sxs-lookup"><span data-stu-id="fe49f-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="fe49f-110">`Post` 因为发现通过`Blog.Posts`导航属性</span><span class="sxs-lookup"><span data-stu-id="fe49f-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="fe49f-111">`AuditEntry` 因为中提及 `OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="fe49f-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="fe49f-112">数据注释</span><span class="sxs-lookup"><span data-stu-id="fe49f-112">Data Annotations</span></span>

<span data-ttu-id="fe49f-113">可以使用数据注释来从模型中排除类型。</span><span class="sxs-lookup"><span data-stu-id="fe49f-113">You can use Data Annotations to exclude a type from the model.</span></span>

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

## <a name="fluent-api"></a><span data-ttu-id="fe49f-114">Fluent API</span><span class="sxs-lookup"><span data-stu-id="fe49f-114">Fluent API</span></span>

<span data-ttu-id="fe49f-115">Fluent API 可用于从模型中排除类型。</span><span class="sxs-lookup"><span data-stu-id="fe49f-115">You can use the Fluent API to exclude a type from the model.</span></span>

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
