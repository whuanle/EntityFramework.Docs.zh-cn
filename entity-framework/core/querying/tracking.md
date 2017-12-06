---
title: "跟踪 vs。否跟踪查询的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
ms.technology: entity-framework-core
uid: core/querying/tracking
ms.openlocfilehash: 9a22c893f3b1e9991560e25e0252287a2844b39e
ms.sourcegitcommit: 3b6159db8a6c0653f13c7b528367b4e69ac3d51e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2017
---
# <a name="tracking-vs-no-tracking-queries"></a><span data-ttu-id="0014f-102">跟踪 vs。否跟踪查询</span><span class="sxs-lookup"><span data-stu-id="0014f-102">Tracking vs. No-Tracking Queries</span></span>

<span data-ttu-id="0014f-103">跟踪行为控件，无论实体 Framework 内核将其更改跟踪器中保留有关实体实例信息。</span><span class="sxs-lookup"><span data-stu-id="0014f-103">Tracking behavior controls whether or not Entity Framework Core will keep information about an entity instance in its change tracker.</span></span> <span data-ttu-id="0014f-104">如果跟踪的实体，实体中检测到任何更改将永久保存到数据库期间`SaveChanges()`。</span><span class="sxs-lookup"><span data-stu-id="0014f-104">If an entity is tracked, any changes detected in the entity will be persisted to the database during `SaveChanges()`.</span></span> <span data-ttu-id="0014f-105">获取跟踪查询的实体与以前已加载到 DbContext 实例的实体之间，实体框架核心将还修复向上导航属性。</span><span class="sxs-lookup"><span data-stu-id="0014f-105">Entity Framework Core will also fix-up navigation properties between entities that are obtained from a tracking query and entities that were previously loaded into the DbContext instance.</span></span>

> [!TIP]  
> <span data-ttu-id="0014f-106">你可以查看这篇文章[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying)GitHub 上。</span><span class="sxs-lookup"><span data-stu-id="0014f-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="tracking-queries"></a><span data-ttu-id="0014f-107">跟踪查询</span><span class="sxs-lookup"><span data-stu-id="0014f-107">Tracking queries</span></span>

<span data-ttu-id="0014f-108">默认情况下，跟踪返回实体类型的查询。</span><span class="sxs-lookup"><span data-stu-id="0014f-108">By default, queries that return entity types are tracking.</span></span> <span data-ttu-id="0014f-109">这意味着你可以对这些实体实例进行更改，并且这些更改保存的`SaveChanges()`。</span><span class="sxs-lookup"><span data-stu-id="0014f-109">This means you can make changes to those entity instances and have those changes persisted by `SaveChanges()`.</span></span>

<span data-ttu-id="0014f-110">在下面的示例中，检测到并持久化到数据库中，在过程更改为博客分级`SaveChanges()`。</span><span class="sxs-lookup"><span data-stu-id="0014f-110">In the following example, the change to the blogs rating will be detected and persisted to the database during `SaveChanges()`.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a><span data-ttu-id="0014f-111">否跟踪查询</span><span class="sxs-lookup"><span data-stu-id="0014f-111">No-tracking queries</span></span>

<span data-ttu-id="0014f-112">在只读方案中使用结果时，没有跟踪查询很有用。</span><span class="sxs-lookup"><span data-stu-id="0014f-112">No tracking queries are useful when the results are used in a read-only scenario.</span></span> <span data-ttu-id="0014f-113">它们可以更加快速地执行，因为无需安装更改跟踪信息。</span><span class="sxs-lookup"><span data-stu-id="0014f-113">They are quicker to execute because there is no need to setup change tracking information.</span></span>

<span data-ttu-id="0014f-114">你可以交换是否-跟踪的单个查询：</span><span class="sxs-lookup"><span data-stu-id="0014f-114">You can swap an individual query to be no-tracking:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

<span data-ttu-id="0014f-115">你还可以更改默认跟踪上下文实例级别的行为：</span><span class="sxs-lookup"><span data-stu-id="0014f-115">You can also change the default tracking behavior at the context instance level:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> <span data-ttu-id="0014f-116">没有跟踪的查询仍然执行标识解析在执行查询。</span><span class="sxs-lookup"><span data-stu-id="0014f-116">No tracking queries still perform identity resolution within the excuting query.</span></span> <span data-ttu-id="0014f-117">如果结果集包含相同的实体多次，将返回结果集中的每个匹配项的实体类的同一个实例。</span><span class="sxs-lookup"><span data-stu-id="0014f-117">If the result set contains the same entity multiple times, the same instance of the entity class will be returned for each occurrence in the result set.</span></span> <span data-ttu-id="0014f-118">但是，弱引用用于跟踪的已返回的实体。</span><span class="sxs-lookup"><span data-stu-id="0014f-118">However, weak references are used to keep track of entities that have already been returned.</span></span> <span data-ttu-id="0014f-119">如果前一个结果具有相同标识号超出范围，且垃圾回收运行，则可能获得的新实体实例。</span><span class="sxs-lookup"><span data-stu-id="0014f-119">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span> <span data-ttu-id="0014f-120">有关详细信息，请参阅[查询的工作原理](overview.md)。</span><span class="sxs-lookup"><span data-stu-id="0014f-120">For more information, see [How Query Works](overview.md).</span></span>

## <a name="tracking-and-projections"></a><span data-ttu-id="0014f-121">跟踪和投影</span><span class="sxs-lookup"><span data-stu-id="0014f-121">Tracking and projections</span></span>

<span data-ttu-id="0014f-122">即使查询的结果类型不是实体类型，如果结果包含实体类型它们将仍在默认情况下跟踪。</span><span class="sxs-lookup"><span data-stu-id="0014f-122">Even if the result type of the query isn't an entity type, if the result contains entity types they will still be tracked by default.</span></span> <span data-ttu-id="0014f-123">在下面的查询，它返回匿名类型的实例`Blog`集将跟踪结果中。</span><span class="sxs-lookup"><span data-stu-id="0014f-123">In the following query, which returns an anonymous type, the instances of `Blog` in the result set will be tracked.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=7)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Blog = b,
                Posts = b.Posts.Count()
            });
}
```

<span data-ttu-id="0014f-124">如果结果集不包含任何实体类型，会不执行任何跟踪。</span><span class="sxs-lookup"><span data-stu-id="0014f-124">If the result set does not contain any entity types, then no tracking is performed.</span></span> <span data-ttu-id="0014f-125">在下面的查询，这将返回一个匿名类型使用某些实体 （但不实际实体类型的实例） 中的值时，没有任何跟踪执行。</span><span class="sxs-lookup"><span data-stu-id="0014f-125">In the following query, which returns an anonymous type with some of the values from the entity (but no instances of the actual entity type), there is no tracking performed.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Id = b.BlogId,
                Url = b.Url
            });
}
```
