---
title: 索引的 EF 核心
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
ms.technology: entity-framework-core
uid: core/modeling/indexes
ms.openlocfilehash: f57b545d53613cec6887734bf434958ee8fff4d8
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054881"
---
# <a name="indexes"></a><span data-ttu-id="e88f4-102">索引</span><span class="sxs-lookup"><span data-stu-id="e88f4-102">Indexes</span></span>

<span data-ttu-id="e88f4-103">索引是跨多个数据存储的一个公共概念。</span><span class="sxs-lookup"><span data-stu-id="e88f4-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="e88f4-104">尽管数据存储中的其实现可能会有所不同，它们用于使基于列 （或一组列） 的查找更加高效。</span><span class="sxs-lookup"><span data-stu-id="e88f4-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="e88f4-105">约定</span><span class="sxs-lookup"><span data-stu-id="e88f4-105">Conventions</span></span>

<span data-ttu-id="e88f4-106">按照约定，为外键使用每个属性 （或组的属性） 中创建索引。</span><span class="sxs-lookup"><span data-stu-id="e88f4-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="e88f4-107">数据注释</span><span class="sxs-lookup"><span data-stu-id="e88f4-107">Data Annotations</span></span>

<span data-ttu-id="e88f4-108">不使用数据注释创建索引。</span><span class="sxs-lookup"><span data-stu-id="e88f4-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="e88f4-109">Fluent API</span><span class="sxs-lookup"><span data-stu-id="e88f4-109">Fluent API</span></span>

<span data-ttu-id="e88f4-110">Fluent API 可用于的单个属性上指定索引。</span><span class="sxs-lookup"><span data-stu-id="e88f4-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="e88f4-111">默认情况下，索引将非唯一。</span><span class="sxs-lookup"><span data-stu-id="e88f4-111">By default, indexes are non-unique.</span></span>

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

<span data-ttu-id="e88f4-112">此外可以指定索引应是唯一的这意味着任何两个实体可以具有的给定属性的相同值。</span><span class="sxs-lookup"><span data-stu-id="e88f4-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

<span data-ttu-id="e88f4-113">你还可以通过多个列中指定索引。</span><span class="sxs-lookup"><span data-stu-id="e88f4-113">You can also specify an index over more than one column.</span></span>

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
> <span data-ttu-id="e88f4-114">没有每个非重复的属性集只有一个索引。</span><span class="sxs-lookup"><span data-stu-id="e88f4-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="e88f4-115">如果你使用 Fluent API 配置索引上的一组属性已索引定义，方法是约定或以前的配置，然后将会更改该索引的定义。</span><span class="sxs-lookup"><span data-stu-id="e88f4-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="e88f4-116">这是在你想进一步配置的索引，它由约定很有用。</span><span class="sxs-lookup"><span data-stu-id="e88f4-116">This is useful if you want to further configure an index that was created by convention.</span></span>
