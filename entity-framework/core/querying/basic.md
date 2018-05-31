---
title: 基本查询 - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ab6e35f1-397f-41c0-9ef4-85aec5466377
ms.technology: entity-framework-core
uid: core/querying/basic
ms.openlocfilehash: 5070faf2aeeffad680e24e7de5a0ffa03a8f0064
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052547"
---
# <a name="basic-queries"></a><span data-ttu-id="d3eb9-102">基本查询</span><span class="sxs-lookup"><span data-stu-id="d3eb9-102">Basic Queries</span></span>

<span data-ttu-id="d3eb9-103">了解如何使用语言集成查询 (LINQ) 从数据库加载实体。</span><span class="sxs-lookup"><span data-stu-id="d3eb9-103">Learn how to load entities from the database using Language Integrate Query (LINQ).</span></span>

> [!TIP]  
> <span data-ttu-id="d3eb9-104">可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying)。</span><span class="sxs-lookup"><span data-stu-id="d3eb9-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="101-linq-samples"></a><span data-ttu-id="d3eb9-105">101 个 LINQ 示例</span><span class="sxs-lookup"><span data-stu-id="d3eb9-105">101 LINQ samples</span></span>

<span data-ttu-id="d3eb9-106">此页面显示了多个使用 Entity Framework Core 完成常见任务的示例。</span><span class="sxs-lookup"><span data-stu-id="d3eb9-106">This page shows a few examples to achieve common tasks with Entity Framework Core.</span></span> <span data-ttu-id="d3eb9-107">有关显示 LINQ 可完成的任务的大量示例，请参阅 [101 个 LINQ 示例](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b)。</span><span class="sxs-lookup"><span data-stu-id="d3eb9-107">For an extensive set of samples showing what is possible with LINQ, see [101 LINQ Samples](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).</span></span>

## <a name="loading-all-data"></a><span data-ttu-id="d3eb9-108">加载所有数据</span><span class="sxs-lookup"><span data-stu-id="d3eb9-108">Loading all data</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.ToList();
}
```

## <a name="loading-a-single-entity"></a><span data-ttu-id="d3eb9-109">加载单个实体</span><span class="sxs-lookup"><span data-stu-id="d3eb9-109">Loading a single entity</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Single(b => b.BlogId == 1);
}
```

## <a name="filtering"></a><span data-ttu-id="d3eb9-110">筛选</span><span class="sxs-lookup"><span data-stu-id="d3eb9-110">Filtering</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .Where(b => b.Url.Contains("dotnet"))
        .ToList();
}
```
