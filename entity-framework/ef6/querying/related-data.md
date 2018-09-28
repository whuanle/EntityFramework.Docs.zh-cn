---
title: 加载相关实体的 EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c8417e18-a2ee-499c-9ce9-2a48cc5b468a
ms.openlocfilehash: 2d33d9db8acc61f7d556e3eca46b1ea90198723e
ms.sourcegitcommit: 15022dd06d919c29b1189c82611ea32f9fdc6617
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2018
ms.locfileid: "47415752"
---
# <a name="loading-related-entities"></a><span data-ttu-id="be914-102">加载相关的实体</span><span class="sxs-lookup"><span data-stu-id="be914-102">Loading Related Entities</span></span>
<span data-ttu-id="be914-103">Entity Framework 支持三种方法来加载相关的数据的预先加载、 延迟加载和显式加载。</span><span class="sxs-lookup"><span data-stu-id="be914-103">Entity Framework supports three ways to load related data - eager loading, lazy loading and explicit loading.</span></span> <span data-ttu-id="be914-104">本主题所介绍的方法同样适用于查询使用 Code First 和 EF 设计器创建的模型。</span><span class="sxs-lookup"><span data-stu-id="be914-104">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="eagerly-loading"></a><span data-ttu-id="be914-105">预先加载</span><span class="sxs-lookup"><span data-stu-id="be914-105">Eagerly Loading</span></span>  

<span data-ttu-id="be914-106">预先加载是实体的一种类型的查询在查询的一部分，并加载相关的实体的过程。</span><span class="sxs-lookup"><span data-stu-id="be914-106">Eager loading is the process whereby a query for one type of entity also loads related entities as part of the query.</span></span> <span data-ttu-id="be914-107">预先加载被通过使用 Include 方法。</span><span class="sxs-lookup"><span data-stu-id="be914-107">Eager loading is achieved by use of the Include method.</span></span> <span data-ttu-id="be914-108">例如，以下查询将加载博客和与每个博客相关的所有文章。</span><span class="sxs-lookup"><span data-stu-id="be914-108">For example, the queries below will load blogs and all the posts related to each blog.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs and related posts
    var blogs1 = context.Blogs
                        .Include(b => b.Posts)
                        .ToList();

    // Load one blogs and its related posts
    var blog1 = context.Blogs
                       .Where(b => b.Name == "ADO.NET Blog")
                       .Include(b => b.Posts)
                       .FirstOrDefault();

    // Load all blogs and related posts  
    // using a string to specify the relationship
    var blogs2 = context.Blogs
                        .Include("Posts")
                        .ToList();

    // Load one blog and its related posts  
    // using a string to specify the relationship
    var blog2 = context.Blogs
                       .Where(b => b.Name == "ADO.NET Blog")
                       .Include("Posts")
                       .FirstOrDefault();
}
```  

<span data-ttu-id="be914-109">请注意，Include system.data.entity 的引用命名空间中的扩展方法因此请确保使用该命名空间。</span><span class="sxs-lookup"><span data-stu-id="be914-109">Note that Include is an extension method in the System.Data.Entity namespace so make sure you are using that namespace.</span></span>  

### <a name="eagerly-loading-multiple-levels"></a><span data-ttu-id="be914-110">积极加载多个级别</span><span class="sxs-lookup"><span data-stu-id="be914-110">Eagerly loading multiple levels</span></span>  

<span data-ttu-id="be914-111">还有可能自愿加载相关实体的多个级别。</span><span class="sxs-lookup"><span data-stu-id="be914-111">It is also possible to eagerly load multiple levels of related entities.</span></span> <span data-ttu-id="be914-112">以下查询显示如何为集合和引用导航属性执行此操作的示例。</span><span class="sxs-lookup"><span data-stu-id="be914-112">The queries below show examples of how to do this for both collection and reference navigation properties.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs, all related posts, and all related comments
    var blogs1 = context.Blogs
                        .Include(b => b.Posts.Select(p => p.Comments))
                        .ToList();

    // Load all users, their related profiles, and related avatar
    var users1 = context.Users
                        .Include(u => u.Profile.Avatar)
                        .ToList();

    // Load all blogs, all related posts, and all related comments  
    // using a string to specify the relationships
    var blogs2 = context.Blogs
                        .Include("Posts.Comments")
                        .ToList();

    // Load all users, their related profiles, and related avatar  
    // using a string to specify the relationships
    var users2 = context.Users
                        .Include("Profile.Avatar")
                        .ToList();
}
```  

<span data-ttu-id="be914-113">请注意，不当前可以对加载哪些相关的实体进行筛选。</span><span class="sxs-lookup"><span data-stu-id="be914-113">Note that it is not currently possible to filter which related entities are loaded.</span></span> <span data-ttu-id="be914-114">包括将始终会将所有相关实体中。</span><span class="sxs-lookup"><span data-stu-id="be914-114">Include will always bring in all related entities.</span></span>  

## <a name="lazy-loading"></a><span data-ttu-id="be914-115">延迟加载</span><span class="sxs-lookup"><span data-stu-id="be914-115">Lazy Loading</span></span>  

<span data-ttu-id="be914-116">延迟加载是凭此实体的集合自动从数据库加载第一次访问指的实体/实体属性的过程。</span><span class="sxs-lookup"><span data-stu-id="be914-116">Lazy loading is the process whereby an entity or collection of entities is automatically loaded from the database the first time that a property referring to the entity/entities is accessed.</span></span> <span data-ttu-id="be914-117">使用 POCO 实体类型时，延迟加载被通过创建派生的代理类型的实例，然后重写虚拟属性添加加载挂钩。</span><span class="sxs-lookup"><span data-stu-id="be914-117">When using POCO entity types, lazy loading is achieved by creating instances of derived proxy types and then overriding virtual properties to add the loading hook.</span></span> <span data-ttu-id="be914-118">例如，使用下面定义的网络日志实体类时，相关的帖子将会加载在首次访问文章导航属性：</span><span class="sxs-lookup"><span data-stu-id="be914-118">For example, when using the Blog entity class defined below, the related Posts will be loaded the first time the Posts navigation property is accessed:</span></span>  

``` csharp
public class Blog
{  
    public int BlogId { get; set; }  
    public string Name { get; set; }  
    public string Url { get; set; }  
    public string Tags { get; set; }  

    public virtual ICollection<Post> Posts { get; set; }  
}
```  

### <a name="turn-lazy-loading-off-for-serialization"></a><span data-ttu-id="be914-119">启用延迟加载关闭以进行序列化</span><span class="sxs-lookup"><span data-stu-id="be914-119">Turn lazy loading off for serialization</span></span>  

<span data-ttu-id="be914-120">延迟加载和序列化不要混合在一起，和如果您不小心最终查询为整个数据库，只是因为启用了延迟加载。</span><span class="sxs-lookup"><span data-stu-id="be914-120">Lazy loading and serialization don’t mix well, and if you aren’t careful you can end up querying for your entire database just because lazy loading is enabled.</span></span> <span data-ttu-id="be914-121">大多数序列化程序通过访问每个属性类型的实例上工作。</span><span class="sxs-lookup"><span data-stu-id="be914-121">Most serializers work by accessing each property on an instance of a type.</span></span> <span data-ttu-id="be914-122">属性访问触发延迟加载更多实体进行序列化。</span><span class="sxs-lookup"><span data-stu-id="be914-122">Property access triggers lazy loading, so more entities get serialized.</span></span> <span data-ttu-id="be914-123">对这些实体访问属性，并且加载更多实体。</span><span class="sxs-lookup"><span data-stu-id="be914-123">On those entities properties are accessed, and even more entities are loaded.</span></span> <span data-ttu-id="be914-124">它是启用延迟加载关闭之前序列化实体的好办法。</span><span class="sxs-lookup"><span data-stu-id="be914-124">It’s a good practice to turn lazy loading off before you serialize an entity.</span></span> <span data-ttu-id="be914-125">以下部分说明如何执行此操作。</span><span class="sxs-lookup"><span data-stu-id="be914-125">The following sections show how to do this.</span></span>  

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a><span data-ttu-id="be914-126">关闭延迟加载的特定导航属性</span><span class="sxs-lookup"><span data-stu-id="be914-126">Turning off lazy loading for specific navigation properties</span></span>  

<span data-ttu-id="be914-127">可以通过使文章属性非虚拟关闭延迟加载的 Posts 集合：</span><span class="sxs-lookup"><span data-stu-id="be914-127">Lazy loading of the Posts collection can be turned off by making the Posts property non-virtual:</span></span>  

``` csharp
public class Blog
{  
    public int BlogId { get; set; }  
    public string Name { get; set; }  
    public string Url { get; set; }  
    public string Tags { get; set; }  

    public ICollection<Post> Posts { get; set; }  
}
```  

<span data-ttu-id="be914-128">加载的帖子集合仍可以使用预先加载 (请参阅*积极加载*上方) 或 Load 方法 (请参阅*显式加载*下面)。</span><span class="sxs-lookup"><span data-stu-id="be914-128">Loading of the Posts collection can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>  

### <a name="turn-off-lazy-loading-for-all-entities"></a><span data-ttu-id="be914-129">关闭延迟加载的所有实体</span><span class="sxs-lookup"><span data-stu-id="be914-129">Turn off lazy loading for all entities</span></span>  

<span data-ttu-id="be914-130">延迟加载可以关闭的所有实体的上下文中设置配置属性的标志。</span><span class="sxs-lookup"><span data-stu-id="be914-130">Lazy loading can be turned off for all entities in the context by setting a flag on the Configuration property.</span></span> <span data-ttu-id="be914-131">例如：</span><span class="sxs-lookup"><span data-stu-id="be914-131">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```  

<span data-ttu-id="be914-132">仍可以使用预先加载实现加载相关实体 (请参阅*积极加载*上方) 或 Load 方法 (请参阅*显式加载*下面)。</span><span class="sxs-lookup"><span data-stu-id="be914-132">Loading of related entities can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>  

## <a name="explicitly-loading"></a><span data-ttu-id="be914-133">显式加载</span><span class="sxs-lookup"><span data-stu-id="be914-133">Explicitly Loading</span></span>  

<span data-ttu-id="be914-134">即使使用禁用延迟加载就仍然可以延迟加载相关的实体，但它必须使用显式调用完成。</span><span class="sxs-lookup"><span data-stu-id="be914-134">Even with lazy loading disabled it is still possible to lazily load related entities, but it must be done with an explicit call.</span></span> <span data-ttu-id="be914-135">若要执行此操作的相关的实体的条目使用 Load 方法。</span><span class="sxs-lookup"><span data-stu-id="be914-135">To do so you use the Load method on the related entity’s entry.</span></span> <span data-ttu-id="be914-136">例如：</span><span class="sxs-lookup"><span data-stu-id="be914-136">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var post = context.Posts.Find(2);

    // Load the blog related to a given post
    context.Entry(post).Reference(p => p.Blog).Load();

    // Load the blog related to a given post using a string  
    context.Entry(post).Reference("Blog").Load();

    var blog = context.Blogs.Find(1);

    // Load the posts related to a given blog
    context.Entry(blog).Collection(p => p.Posts).Load();

    // Load the posts related to a given blog  
    // using a string to specify the relationship
    context.Entry(blog).Collection("Posts").Load();
}
```  

<span data-ttu-id="be914-137">请注意当实体具有与另一个单一实体的导航属性时，应使用的引用方法。</span><span class="sxs-lookup"><span data-stu-id="be914-137">Note that the Reference method should be used when an entity has a navigation property to another single entity.</span></span> <span data-ttu-id="be914-138">另一方面，当实体具有的一系列其他实体的导航属性时，应使用的收集方法。</span><span class="sxs-lookup"><span data-stu-id="be914-138">On the other hand, the Collection method should be used when an entity has a navigation property to a collection of other entities.</span></span>  

### <a name="applying-filters-when-explicitly-loading-related-entities"></a><span data-ttu-id="be914-139">显式加载相关的实体时应用筛选器</span><span class="sxs-lookup"><span data-stu-id="be914-139">Applying filters when explicitly loading related entities</span></span>  

<span data-ttu-id="be914-140">查询方法提供对加载相关的实体时，将使用实体框架的基础查询访问。</span><span class="sxs-lookup"><span data-stu-id="be914-140">The Query method provides access to the underlying query that Entity Framework will use when loading related entities.</span></span> <span data-ttu-id="be914-141">然后可以使用 LINQ 来执行通过 ToList、 负载等之类的 LINQ 扩展方法调用前对查询应用筛选器。查询方法可用于引用和集合导航属性，但是最有用的集合，其中使用它来加载仅集合的一部分。</span><span class="sxs-lookup"><span data-stu-id="be914-141">You can then use LINQ to apply filters to the query before executing it with a call to a LINQ extension method such as ToList, Load, etc. The Query method can be used with both reference and collection navigation properties but is most useful for collections where it can be used to load only part of the collection.</span></span> <span data-ttu-id="be914-142">例如：</span><span class="sxs-lookup"><span data-stu-id="be914-142">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
           .Collection(b => b.Posts)
           .Query()
           .Where(p => p.Tags.Contains("entity-framework"))
           .Load();

    // Load the posts with the 'entity-framework' tag related to a given blog  
    // using a string to specify the relationship  
    context.Entry(blog)
           .Collection("Posts")
           .Query()
           .Where(p => p.Tags.Contains("entity-framework"))
           .Load();
}
```  

<span data-ttu-id="be914-143">使用查询方法时，通常最好关闭延迟加载的导航属性。</span><span class="sxs-lookup"><span data-stu-id="be914-143">When using the Query method it is usually best to turn off lazy loading for the navigation property.</span></span> <span data-ttu-id="be914-144">这是因为否则整个集合可能会获取自动加载被延迟加载机制之前或之后执行筛选的查询。</span><span class="sxs-lookup"><span data-stu-id="be914-144">This is because otherwise the entire collection may get loaded automatically by the lazy loading mechanism either before or after the filtered query has been executed.</span></span>  

<span data-ttu-id="be914-145">请注意，虽然可以为字符串而不是 lambda 表达式指定的关系，返回的 IQueryable 不泛型时使用了字符串，因此可通过它完成任何有用的操作之前，通常需要强制转换方法。</span><span class="sxs-lookup"><span data-stu-id="be914-145">Note that while the relationship can be specified as a string instead of a lambda expression, the returned IQueryable is not generic when a string is used and so the Cast method is usually needed before anything useful can be done with it.</span></span>  

## <a name="using-query-to-count-related-entities-without-loading-them"></a><span data-ttu-id="be914-146">使用查询进行计数而不加载这些相关的实体</span><span class="sxs-lookup"><span data-stu-id="be914-146">Using Query to count related entities without loading them</span></span>  

<span data-ttu-id="be914-147">有时最好要知道多少个实体与数据库中的另一个实体而不实际产生加载这些实体的费用。</span><span class="sxs-lookup"><span data-stu-id="be914-147">Sometimes it is useful to know how many entities are related to another entity in the database without actually incurring the cost of loading all those entities.</span></span> <span data-ttu-id="be914-148">使用 LINQ 计数方法的查询方法可用来执行此操作。</span><span class="sxs-lookup"><span data-stu-id="be914-148">The Query method with the LINQ Count method can be used to do this.</span></span> <span data-ttu-id="be914-149">例如：</span><span class="sxs-lookup"><span data-stu-id="be914-149">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Count how many posts the blog has  
    var postCount = context.Entry(blog)
                           .Collection(b => b.Posts)
                           .Query()
                           .Count();
}
```  
