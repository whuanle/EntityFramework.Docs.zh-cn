---
title: 序列的 EF 核心
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
ms.technology: entity-framework-core
uid: core/modeling/relational/sequences
ms.openlocfilehash: 98a40aeecbec0fd9fb9cc108d6b5f98178dea403
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052707"
---
# <a name="sequences"></a><span data-ttu-id="c8bed-102">序列</span><span class="sxs-lookup"><span data-stu-id="c8bed-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="c8bed-103">一般情况下，此部分中的配置是适用于关系数据库。</span><span class="sxs-lookup"><span data-stu-id="c8bed-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="c8bed-104">此处所示的扩展方法将变为可用时安装关系数据库提供程序 (由于共享*Microsoft.EntityFrameworkCore.Relational*包)。</span><span class="sxs-lookup"><span data-stu-id="c8bed-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="c8bed-105">序列生成顺序的数字值在数据库中。</span><span class="sxs-lookup"><span data-stu-id="c8bed-105">A sequence generates a sequential numeric values in the database.</span></span> <span data-ttu-id="c8bed-106">序列不与特定表相关联。</span><span class="sxs-lookup"><span data-stu-id="c8bed-106">Sequences are not associated with a specific table.</span></span>

## <a name="conventions"></a><span data-ttu-id="c8bed-107">约定</span><span class="sxs-lookup"><span data-stu-id="c8bed-107">Conventions</span></span>

<span data-ttu-id="c8bed-108">按照约定，序列不会引入中对模型。</span><span class="sxs-lookup"><span data-stu-id="c8bed-108">By convention, sequences are not introduced in to the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="c8bed-109">数据注释</span><span class="sxs-lookup"><span data-stu-id="c8bed-109">Data Annotations</span></span>

<span data-ttu-id="c8bed-110">你可以配置使用数据注释的序列。</span><span class="sxs-lookup"><span data-stu-id="c8bed-110">You can not configure a sequence using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="c8bed-111">Fluent API</span><span class="sxs-lookup"><span data-stu-id="c8bed-111">Fluent API</span></span>

<span data-ttu-id="c8bed-112">Fluent API 可用于在模型中创建一个序列。</span><span class="sxs-lookup"><span data-stu-id="c8bed-112">You can use the Fluent API to create a sequence in the model.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/Sequence.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers");
    }
}

public class Order
{
    public int OrderId { get; set; }
    public int OrderNo { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="c8bed-113">你还可以配置序列，如其架构、 起始值和增量的其他的方面。</span><span class="sxs-lookup"><span data-stu-id="c8bed-113">You can also configure additional aspect of the sequence, such as its schema, start value, and increment.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/SequenceConfigured.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers", schema: "shared")
            .StartsAt(1000)
            .IncrementsBy(5);
    }
}
```

<span data-ttu-id="c8bed-114">后一个序列中引入，可用于在您的模型中生成属性的值。</span><span class="sxs-lookup"><span data-stu-id="c8bed-114">Once a sequence is introduced, you can use it to generate values for properties in your model.</span></span> <span data-ttu-id="c8bed-115">例如，你可以使用[默认值](default-values.md)从序列中插入的下一步的值。</span><span class="sxs-lookup"><span data-stu-id="c8bed-115">For example, you can use [Default Values](default-values.md) to insert the next value from the sequence.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/SequenceUsed.cs?highlight=11,12,13)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers", schema: "shared")
            .StartsAt(1000)
            .IncrementsBy(5);

        modelBuilder.Entity<Order>()
            .Property(o => o.OrderNo)
            .HasDefaultValueSql("NEXT VALUE FOR shared.OrderNumbers");
    }
}

public class Order
{
    public int OrderId { get; set; }
    public int OrderNo { get; set; }
    public string Url { get; set; }
}
```
