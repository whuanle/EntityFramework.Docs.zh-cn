---
title: 必需/可选属性-EF 核心
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
ms.technology: entity-framework-core
uid: core/modeling/required-optional
ms.openlocfilehash: 2af1d49e12ef980f81cb9c00556dee471673ccae
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052847"
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="04f4c-102">必选和可选属性</span><span class="sxs-lookup"><span data-stu-id="04f4c-102">Required and Optional Properties</span></span>

<span data-ttu-id="04f4c-103">该属性被视为可选如果有效它才能包含`null`。</span><span class="sxs-lookup"><span data-stu-id="04f4c-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="04f4c-104">如果`null`不是有效的值分配给属性中，则可以认为是必需的属性。</span><span class="sxs-lookup"><span data-stu-id="04f4c-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="04f4c-105">约定</span><span class="sxs-lookup"><span data-stu-id="04f4c-105">Conventions</span></span>

<span data-ttu-id="04f4c-106">按照约定，其 CLR 类型可以包含 null 的属性将被配置为可选 (`string`， `int?`，`byte[]`等。)。</span><span class="sxs-lookup"><span data-stu-id="04f4c-106">By convention, a property whose CLR type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="04f4c-107">将配置其 CLR 类型不能包含 null 的属性所需的方式 (`int`， `decimal`，`bool`等。)。</span><span class="sxs-lookup"><span data-stu-id="04f4c-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="04f4c-108">其 CLR 类型不能包含 null 的属性不能配置为可选。</span><span class="sxs-lookup"><span data-stu-id="04f4c-108">A property whose CLR type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="04f4c-109">始终将所需的实体框架视为属性。</span><span class="sxs-lookup"><span data-stu-id="04f4c-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="04f4c-110">数据注释</span><span class="sxs-lookup"><span data-stu-id="04f4c-110">Data Annotations</span></span>

<span data-ttu-id="04f4c-111">可以使用数据注释以指示需要一个属性。</span><span class="sxs-lookup"><span data-stu-id="04f4c-111">You can use Data Annotations to indicate that a property is required.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="04f4c-112">Fluent API</span><span class="sxs-lookup"><span data-stu-id="04f4c-112">Fluent API</span></span>

<span data-ttu-id="04f4c-113">可以使用 Fluent API 以指示需要一个属性。</span><span class="sxs-lookup"><span data-stu-id="04f4c-113">You can use the Fluent API to indicate that a property is required.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
