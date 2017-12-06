---
title: "包括和排除属性-EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
ms.technology: entity-framework-core
uid: core/modeling/included-properties
ms.openlocfilehash: a6eaea4319f6a4d30c223265bf75a88731a38443
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="including--excluding-properties"></a><span data-ttu-id="7ecbc-102">包括和排除属性</span><span class="sxs-lookup"><span data-stu-id="7ecbc-102">Including & Excluding Properties</span></span>

<span data-ttu-id="7ecbc-103">在模型中的属性包括意味着 EF 具有该属性有关的元数据，并且将尝试读取和写入从/到数据库的值。</span><span class="sxs-lookup"><span data-stu-id="7ecbc-103">Including a property in the model means that EF has metadata about that property and will attempt to read and write values from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="7ecbc-104">约定</span><span class="sxs-lookup"><span data-stu-id="7ecbc-104">Conventions</span></span>

<span data-ttu-id="7ecbc-105">按照约定，将在模型中包括具有 getter 和 setter 的公共属性。</span><span class="sxs-lookup"><span data-stu-id="7ecbc-105">By convention, public properties with a getter and a setter will be included in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="7ecbc-106">数据注释</span><span class="sxs-lookup"><span data-stu-id="7ecbc-106">Data Annotations</span></span>

<span data-ttu-id="7ecbc-107">可以使用数据注释从模型中排除属性。</span><span class="sxs-lookup"><span data-stu-id="7ecbc-107">You can use Data Annotations to exclude a property from the model.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/IgnoreProperty.cs?highlight=6)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    [NotMapped]
    public DateTime LoadedFromDatabase { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="7ecbc-108">Fluent API</span><span class="sxs-lookup"><span data-stu-id="7ecbc-108">Fluent API</span></span>

<span data-ttu-id="7ecbc-109">可以使用 Fluent API 若要从模型中排除属性。</span><span class="sxs-lookup"><span data-stu-id="7ecbc-109">You can use the Fluent API to exclude a property from the model.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IgnoreProperty.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Ignore(b => b.LoadedFromDatabase);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public DateTime LoadedFromDatabase { get; set; }
}
```
