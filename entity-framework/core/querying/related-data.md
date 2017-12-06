---
title: "正在加载相关数据的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: cd26bd2e6f85083f73d97b1356d0ba38f53e0b8f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="loading-related-data"></a><span data-ttu-id="e6618-102">加载相关的数据</span><span class="sxs-lookup"><span data-stu-id="e6618-102">Loading Related Data</span></span>

<span data-ttu-id="e6618-103">实体框架核心，可在模型中使用的导航属性，来加载相关的实体。</span><span class="sxs-lookup"><span data-stu-id="e6618-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="e6618-104">有三种常见 O/RM 模式，用于加载相关的数据。</span><span class="sxs-lookup"><span data-stu-id="e6618-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="e6618-105">**预先加载**意味着相关的数据从数据库加载作为初始查询的一部分。</span><span class="sxs-lookup"><span data-stu-id="e6618-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="e6618-106">**显式加载**意味着相关的数据在更高版本时显式加载从数据库。</span><span class="sxs-lookup"><span data-stu-id="e6618-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="e6618-107">**延迟加载**意味着访问导航属性时，相关的数据以透明方式加载从数据库。</span><span class="sxs-lookup"><span data-stu-id="e6618-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span> <span data-ttu-id="e6618-108">延迟加载尚不可能与 EF 核心。</span><span class="sxs-lookup"><span data-stu-id="e6618-108">Lazy loading is not yet possible with EF Core.</span></span>

> [!TIP]  
> <span data-ttu-id="e6618-109">你可以查看这篇文章[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying)GitHub 上。</span><span class="sxs-lookup"><span data-stu-id="e6618-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="e6618-110">预先加载</span><span class="sxs-lookup"><span data-stu-id="e6618-110">Eager loading</span></span>

<span data-ttu-id="e6618-111">你可以使用`Include`方法，以便指定要包含在查询结果的相关的数据。</span><span class="sxs-lookup"><span data-stu-id="e6618-111">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="e6618-112">在下面的示例中，结果中返回博客将提供了其`Posts`填入相关文章的属性。</span><span class="sxs-lookup"><span data-stu-id="e6618-112">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="e6618-113">与以前已加载到上下文实例的任何其他实体的实体框架核心将自动修复向上导航属性。</span><span class="sxs-lookup"><span data-stu-id="e6618-113">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="e6618-114">因此，即使显式不包含导航属性的数据，可能仍填充属性，如果某些或以前加载所有相关实体。</span><span class="sxs-lookup"><span data-stu-id="e6618-114">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="e6618-115">您可以在单个查询中包含从多个关系的相关的数据。</span><span class="sxs-lookup"><span data-stu-id="e6618-115">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="e6618-116">包括多个级别</span><span class="sxs-lookup"><span data-stu-id="e6618-116">Including multiple levels</span></span>

<span data-ttu-id="e6618-117">你可以向下钻取通过关系以包含多个级别的相关的数据使用`ThenInclude`方法。</span><span class="sxs-lookup"><span data-stu-id="e6618-117">You can drill down thru relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="e6618-118">下面的示例加载所有博客文章、 其相关的文章和每次发布的作者。</span><span class="sxs-lookup"><span data-stu-id="e6618-118">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

<span data-ttu-id="e6618-119">你可以链接到多个调用`ThenInclude`继续进一步包括的相关数据的级别。</span><span class="sxs-lookup"><span data-stu-id="e6618-119">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="e6618-120">你可以将此特性以在同一查询中包含来自多个级别和多个根的相关的数据的所有组合。</span><span class="sxs-lookup"><span data-stu-id="e6618-120">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="e6618-121">你可能想要包括一个所包含的实体的多个相关的实体。</span><span class="sxs-lookup"><span data-stu-id="e6618-121">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="e6618-122">例如，当查询`Blog`s，包括`Posts`后想要同时包含这两者`Author`和`Tags`的`Posts`。</span><span class="sxs-lookup"><span data-stu-id="e6618-122">For example, when querying `Blog`s, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="e6618-123">若要执行此操作，你需要指定每个包含从根目录开始的路径。</span><span class="sxs-lookup"><span data-stu-id="e6618-123">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="e6618-124">例如， `Blog -> Posts -> Author` 和 `Blog -> Posts -> Tags`。</span><span class="sxs-lookup"><span data-stu-id="e6618-124">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="e6618-125">这并不意味着你将获得冗余联接，在大多数情况下，EF 将合并联接生成 SQL。</span><span class="sxs-lookup"><span data-stu-id="e6618-125">This does not mean you will get redundant joins, in most cases EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="ignored-includes"></a><span data-ttu-id="e6618-126">忽略包括</span><span class="sxs-lookup"><span data-stu-id="e6618-126">Ignored includes</span></span>

<span data-ttu-id="e6618-127">如果您更改查询，从而使其不再返回查询开始与实体类型的实例，则会忽略 include 运算符。</span><span class="sxs-lookup"><span data-stu-id="e6618-127">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="e6618-128">在下面的示例中，基于包含运算符`Blog`，但然后`Select`运算符用于更改查询返回一个匿名类型。</span><span class="sxs-lookup"><span data-stu-id="e6618-128">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="e6618-129">在这种情况下，包括运算符产生任何影响。</span><span class="sxs-lookup"><span data-stu-id="e6618-129">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="e6618-130">默认情况下，EF 核心将记录一个警告时包括运算符将被忽略。</span><span class="sxs-lookup"><span data-stu-id="e6618-130">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="e6618-131">请参阅[日志记录](../miscellaneous/logging.md)有关查看日志记录输出的详细信息。</span><span class="sxs-lookup"><span data-stu-id="e6618-131">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="e6618-132">包括运算符忽略引发或不执行任何操作时，你可以更改的行为。</span><span class="sxs-lookup"><span data-stu-id="e6618-132">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="e6618-133">这是通常在设置您的上下文中的选项时`DbContext.OnConfiguring`，或在`Startup.cs`如果正在使用 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="e6618-133">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="e6618-134">显式加载</span><span class="sxs-lookup"><span data-stu-id="e6618-134">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="e6618-135">EF 核心 1.1 中已引入此功能。</span><span class="sxs-lookup"><span data-stu-id="e6618-135">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="e6618-136">你可以显式加载通过一个导航属性`DbContext.Entry(...)`API。</span><span class="sxs-lookup"><span data-stu-id="e6618-136">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="e6618-137">你还可以显式可以通过执行单独的查询返回相关的实体加载导航属性。</span><span class="sxs-lookup"><span data-stu-id="e6618-137">You can also explicitly load a navigation property by executing a seperate query that returns the related entities.</span></span> <span data-ttu-id="e6618-138">如果已启用更改跟踪，则在加载时实体，EF 核心将自动设置新加载的实体，以引用已加载，任何实体的导航属性并设置要引用的已加载实体的导航属性新加载的实体。</span><span class="sxs-lookup"><span data-stu-id="e6618-138">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="e6618-139">查询相关的实体</span><span class="sxs-lookup"><span data-stu-id="e6618-139">Querying related entities</span></span>

<span data-ttu-id="e6618-140">你还可以获得表示导航属性的内容的 LINQ 查询。</span><span class="sxs-lookup"><span data-stu-id="e6618-140">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="e6618-141">这允许你执行如运行通过相关的实体的聚合运算符，而无需加载到内存的操作。</span><span class="sxs-lookup"><span data-stu-id="e6618-141">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="e6618-142">你还可以筛选哪些相关的实体加载到内存。</span><span class="sxs-lookup"><span data-stu-id="e6618-142">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="e6618-143">延迟加载</span><span class="sxs-lookup"><span data-stu-id="e6618-143">Lazy loading</span></span>

<span data-ttu-id="e6618-144">EF 核心尚不支持延迟加载。</span><span class="sxs-lookup"><span data-stu-id="e6618-144">Lazy loading is not yet supported by EF Core.</span></span> <span data-ttu-id="e6618-145">你可以查看[积压工作上的延迟加载项](https://github.com/aspnet/EntityFramework/issues/3797)以跟踪此功能。</span><span class="sxs-lookup"><span data-stu-id="e6618-145">You can view the [lazy loading item on our backlog](https://github.com/aspnet/EntityFramework/issues/3797) to track this feature.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="e6618-146">相关的数据和序列化</span><span class="sxs-lookup"><span data-stu-id="e6618-146">Related data and serialization</span></span>

<span data-ttu-id="e6618-147">因为 EF 核心将自动修复向上导航属性，你可以得到周期对象关系图中。</span><span class="sxs-lookup"><span data-stu-id="e6618-147">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="e6618-148">例如，正在加载博客和它被相关文章将导致博客对象，它引用的文章的集合。</span><span class="sxs-lookup"><span data-stu-id="e6618-148">For example, Loading a blog and it's related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="e6618-149">每个这些文章将具有返回到博客的引用。</span><span class="sxs-lookup"><span data-stu-id="e6618-149">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="e6618-150">某些序列化框架不允许此类周期。</span><span class="sxs-lookup"><span data-stu-id="e6618-150">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="e6618-151">例如，如果循环是遇到 Json.NET 将引发以下异常。</span><span class="sxs-lookup"><span data-stu-id="e6618-151">For example, Json.NET will throw the following exception if a cycle is encoutered.</span></span>

> <span data-ttu-id="e6618-152">Newtonsoft.Json.JsonSerializationException： 自助引用与类型 MyApplication.Models.Blog 属性博客检测到循环。</span><span class="sxs-lookup"><span data-stu-id="e6618-152">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="e6618-153">如果你使用的 ASP.NET 核心，你可以配置 Json.NET 若要忽略的对象图中找到的周期。</span><span class="sxs-lookup"><span data-stu-id="e6618-153">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="e6618-154">将执行此操作`ConfigureServices(...)`中的方法`Startup.cs`。</span><span class="sxs-lookup"><span data-stu-id="e6618-154">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```
