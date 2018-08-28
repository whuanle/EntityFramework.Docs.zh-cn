---
title: 索引的 EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: 87fe893243377e3ab83d419ae9bedf813ca50c3f
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995475"
---
# <a name="indexes"></a><span data-ttu-id="16b12-102">索引</span><span class="sxs-lookup"><span data-stu-id="16b12-102">Indexes</span></span>

<span data-ttu-id="16b12-103">索引是在许多数据存储之间的一个常见概念。</span><span class="sxs-lookup"><span data-stu-id="16b12-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="16b12-104">而其数据存储区中的实现可能会有所不同，它们用于使基于列 （或一组列） 上查找更高效。</span><span class="sxs-lookup"><span data-stu-id="16b12-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="16b12-105">约定</span><span class="sxs-lookup"><span data-stu-id="16b12-105">Conventions</span></span>

<span data-ttu-id="16b12-106">按照约定，用作外键每个属性 （或组的属性） 中创建的索引。</span><span class="sxs-lookup"><span data-stu-id="16b12-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="16b12-107">数据注释</span><span class="sxs-lookup"><span data-stu-id="16b12-107">Data Annotations</span></span>

<span data-ttu-id="16b12-108">不使用数据批注创建索引。</span><span class="sxs-lookup"><span data-stu-id="16b12-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="16b12-109">Fluent API</span><span class="sxs-lookup"><span data-stu-id="16b12-109">Fluent API</span></span>

<span data-ttu-id="16b12-110">Fluent API 可用于根据单个属性中指定的索引。</span><span class="sxs-lookup"><span data-stu-id="16b12-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="16b12-111">默认情况下，索引是非唯一的。</span><span class="sxs-lookup"><span data-stu-id="16b12-111">By default, indexes are non-unique.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Index.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="16b12-112">此外可以指定索引应是唯一的这意味着任何两个实体可以具有给定属性的相同值。</span><span class="sxs-lookup"><span data-stu-id="16b12-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

<span data-ttu-id="16b12-113">此外可以通过多个列指定一个索引。</span><span class="sxs-lookup"><span data-stu-id="16b12-113">You can also specify an index over more than one column.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .HasIndex(p => new { p.FirstName, p.LastName });
    }
}

public class Person
{
    public int PersonId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
}
```

> [!TIP]  
> <span data-ttu-id="16b12-114">没有每个非重复的属性集只有一个索引。</span><span class="sxs-lookup"><span data-stu-id="16b12-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="16b12-115">如果 Fluent API 用于在一组已定义了索引，不论是通过约定或以前配置的属性上配置索引然后你将会更改该索引定义。</span><span class="sxs-lookup"><span data-stu-id="16b12-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="16b12-116">这是很有用，如果你想要进一步配置按约定创建的索引。</span><span class="sxs-lookup"><span data-stu-id="16b12-116">This is useful if you want to further configure an index that was created by convention.</span></span>
