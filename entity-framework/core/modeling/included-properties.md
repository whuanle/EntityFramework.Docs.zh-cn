---
title: 包括和排除属性-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: 07b70e4517b67490e04a9ec9fa22b9b5d5217681
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998250"
---
# <a name="including--excluding-properties"></a><span data-ttu-id="a1217-102">包括和排除属性</span><span class="sxs-lookup"><span data-stu-id="a1217-102">Including & Excluding Properties</span></span>

<span data-ttu-id="a1217-103">模型中的属性包括意味着 EF 对有关该属性的元数据并将读取和写入从/向数据库的值。</span><span class="sxs-lookup"><span data-stu-id="a1217-103">Including a property in the model means that EF has metadata about that property and will attempt to read and write values from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="a1217-104">约定</span><span class="sxs-lookup"><span data-stu-id="a1217-104">Conventions</span></span>

<span data-ttu-id="a1217-105">按照约定，将在模型中包含有 getter 和 setter 的公共属性。</span><span class="sxs-lookup"><span data-stu-id="a1217-105">By convention, public properties with a getter and a setter will be included in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="a1217-106">数据注释</span><span class="sxs-lookup"><span data-stu-id="a1217-106">Data Annotations</span></span>

<span data-ttu-id="a1217-107">可以使用数据注释来从模型中排除某个属性。</span><span class="sxs-lookup"><span data-stu-id="a1217-107">You can use Data Annotations to exclude a property from the model.</span></span>

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

## <a name="fluent-api"></a><span data-ttu-id="a1217-108">Fluent API</span><span class="sxs-lookup"><span data-stu-id="a1217-108">Fluent API</span></span>

<span data-ttu-id="a1217-109">Fluent API 可用于从模型中排除某个属性。</span><span class="sxs-lookup"><span data-stu-id="a1217-109">You can use the Fluent API to exclude a property from the model.</span></span>

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
