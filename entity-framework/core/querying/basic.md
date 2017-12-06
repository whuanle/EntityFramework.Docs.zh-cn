---
title: "基本查询的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ab6e35f1-397f-41c0-9ef4-85aec5466377
ms.technology: entity-framework-core
uid: core/querying/basic
ms.openlocfilehash: 5070faf2aeeffad680e24e7de5a0ffa03a8f0064
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="basic-queries"></a><span data-ttu-id="6ecf6-102">基本查询</span><span class="sxs-lookup"><span data-stu-id="6ecf6-102">Basic Queries</span></span>

<span data-ttu-id="6ecf6-103">了解如何使用语言集成查询 (LINQ) 从数据库加载实体。</span><span class="sxs-lookup"><span data-stu-id="6ecf6-103">Learn how to load entities from the database using Language Integrate Query (LINQ).</span></span>

> [!TIP]  
> <span data-ttu-id="6ecf6-104">你可以查看这篇文章[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying)GitHub 上。</span><span class="sxs-lookup"><span data-stu-id="6ecf6-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="101-linq-samples"></a><span data-ttu-id="6ecf6-105">101 LINQ 示例</span><span class="sxs-lookup"><span data-stu-id="6ecf6-105">101 LINQ samples</span></span>

<span data-ttu-id="6ecf6-106">此页显示的一些示例实现使用实体框架核心的常见任务。</span><span class="sxs-lookup"><span data-stu-id="6ecf6-106">This page shows a few examples to achieve common tasks with Entity Framework Core.</span></span> <span data-ttu-id="6ecf6-107">一套全面的示例演示了可能使用 LINQ，请参阅[101 LINQ 示例](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b)。</span><span class="sxs-lookup"><span data-stu-id="6ecf6-107">For an extensive set of samples showing what is possible with LINQ, see [101 LINQ Samples](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).</span></span>

## <a name="loading-all-data"></a><span data-ttu-id="6ecf6-108">加载的所有数据</span><span class="sxs-lookup"><span data-stu-id="6ecf6-108">Loading all data</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.ToList();
}
```

## <a name="loading-a-single-entity"></a><span data-ttu-id="6ecf6-109">加载单个实体</span><span class="sxs-lookup"><span data-stu-id="6ecf6-109">Loading a single entity</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Single(b => b.BlogId == 1);
}
```

## <a name="filtering"></a><span data-ttu-id="6ecf6-110">筛选</span><span class="sxs-lookup"><span data-stu-id="6ecf6-110">Filtering</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .Where(b => b.Url.Contains("dotnet"))
        .ToList();
}
```
