---
title: 正在加载相关数据的 EF 核心
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: 5f1fb9376300739ab0e306d9d60e7ec71aa2d2e7
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2018
---
# <a name="loading-related-data"></a><span data-ttu-id="f3e75-102">加载相关的数据</span><span class="sxs-lookup"><span data-stu-id="f3e75-102">Loading Related Data</span></span>

<span data-ttu-id="f3e75-103">实体框架核心，可在模型中使用的导航属性，来加载相关的实体。</span><span class="sxs-lookup"><span data-stu-id="f3e75-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="f3e75-104">有三种常见 O/RM 模式，用于加载相关的数据。</span><span class="sxs-lookup"><span data-stu-id="f3e75-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="f3e75-105">**预先加载**意味着相关的数据从数据库加载作为初始查询的一部分。</span><span class="sxs-lookup"><span data-stu-id="f3e75-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="f3e75-106">**显式加载**意味着相关的数据在更高版本时显式加载从数据库。</span><span class="sxs-lookup"><span data-stu-id="f3e75-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="f3e75-107">**延迟加载**意味着访问导航属性时，相关的数据以透明方式加载从数据库。</span><span class="sxs-lookup"><span data-stu-id="f3e75-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="f3e75-108">可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying)。</span><span class="sxs-lookup"><span data-stu-id="f3e75-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="f3e75-109">预先加载</span><span class="sxs-lookup"><span data-stu-id="f3e75-109">Eager loading</span></span>

<span data-ttu-id="f3e75-110">你可以使用`Include`方法，以便指定要包含在查询结果的相关的数据。</span><span class="sxs-lookup"><span data-stu-id="f3e75-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="f3e75-111">在下面的示例中，结果中返回博客将提供了其`Posts`填入相关文章的属性。</span><span class="sxs-lookup"><span data-stu-id="f3e75-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="f3e75-112">与以前已加载到上下文实例的任何其他实体的实体框架核心将自动修复向上导航属性。</span><span class="sxs-lookup"><span data-stu-id="f3e75-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="f3e75-113">因此，即使显式不包含导航属性的数据，可能仍填充属性，如果某些或以前加载所有相关实体。</span><span class="sxs-lookup"><span data-stu-id="f3e75-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="f3e75-114">您可以在单个查询中包含从多个关系的相关的数据。</span><span class="sxs-lookup"><span data-stu-id="f3e75-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="f3e75-115">包括多个级别</span><span class="sxs-lookup"><span data-stu-id="f3e75-115">Including multiple levels</span></span>

<span data-ttu-id="f3e75-116">你可以向下钻取通过关系以包含多个级别的相关的数据使用`ThenInclude`方法。</span><span class="sxs-lookup"><span data-stu-id="f3e75-116">You can drill down thru relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="f3e75-117">下面的示例加载所有博客文章、 其相关的文章和每次发布的作者。</span><span class="sxs-lookup"><span data-stu-id="f3e75-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> <span data-ttu-id="f3e75-118">当前版本的 Visual Studio 提供完成选项的代码不正确，因此会正确的表达式被使用时，标记有语法错误`ThenInclude`方法之后使用的集合导航属性。</span><span class="sxs-lookup"><span data-stu-id="f3e75-118">Current versions of Visual Studio offer incorrect code completion options and can cause correct expressions to be flagged with syntax errors when using the `ThenInclude` method after a collection navigation property.</span></span> <span data-ttu-id="f3e75-119">这是在跟踪 IntelliSense bug 的症状https://github.com/dotnet/roslyn/issues/8237。</span><span class="sxs-lookup"><span data-stu-id="f3e75-119">This is a symptom of an IntelliSense bug tracked at https://github.com/dotnet/roslyn/issues/8237.</span></span> <span data-ttu-id="f3e75-120">则可以安全地忽略这些虚假语法错误，只要代码正确无误，并且可以成功编译。</span><span class="sxs-lookup"><span data-stu-id="f3e75-120">It is safe to ignore these spurious syntax errors as long as the code is correct and can be compiled successfully.</span></span> 

<span data-ttu-id="f3e75-121">你可以链接到多个调用`ThenInclude`继续进一步包括的相关数据的级别。</span><span class="sxs-lookup"><span data-stu-id="f3e75-121">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="f3e75-122">你可以将此特性以在同一查询中包含来自多个级别和多个根的相关的数据的所有组合。</span><span class="sxs-lookup"><span data-stu-id="f3e75-122">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="f3e75-123">你可能想要包括一个所包含的实体的多个相关的实体。</span><span class="sxs-lookup"><span data-stu-id="f3e75-123">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="f3e75-124">例如，当查询`Blog`s，包括`Posts`后想要同时包含这两者`Author`和`Tags`的`Posts`。</span><span class="sxs-lookup"><span data-stu-id="f3e75-124">For example, when querying `Blog`s, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="f3e75-125">若要执行此操作，你需要指定每个包含从根目录开始的路径。</span><span class="sxs-lookup"><span data-stu-id="f3e75-125">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="f3e75-126">例如，`Blog -> Posts -> Author`和`Blog -> Posts -> Tags`。</span><span class="sxs-lookup"><span data-stu-id="f3e75-126">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="f3e75-127">这并不意味着你将获得冗余联接，在大多数情况下，EF 将合并联接生成 SQL。</span><span class="sxs-lookup"><span data-stu-id="f3e75-127">This does not mean you will get redundant joins, in most cases EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a><span data-ttu-id="f3e75-128">包含派生类型上</span><span class="sxs-lookup"><span data-stu-id="f3e75-128">Include on derived types</span></span>

<span data-ttu-id="f3e75-129">您可以包含相关的数据从仅在使用的派生的类型上定义的导航`Include`和`ThenInclude`。</span><span class="sxs-lookup"><span data-stu-id="f3e75-129">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span> 

<span data-ttu-id="f3e75-130">给定以下模型：</span><span class="sxs-lookup"><span data-stu-id="f3e75-130">Given the following model:</span></span>

```Csharp
    public class SchoolContext : DbContext
    {
        public DbSet<Person> People { get; set; }
        public DbSet<School> Schools { get; set; }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<School>().HasMany(s => s.Students).WithOne(s => s.School);
        }
    }

    public class Person
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }

    public class Student : Person
    {
        public School School { get; set; }
    }

    public class School
    {
        public int Id { get; set; }
        public string Name { get; set; }

        public List<Student> Students { get; set; }
    }
```

<span data-ttu-id="f3e75-131">内容`School`导航所有人士提供学生可以积极加载使用大量的模式：</span><span class="sxs-lookup"><span data-stu-id="f3e75-131">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

- <span data-ttu-id="f3e75-132">使用强制转换</span><span class="sxs-lookup"><span data-stu-id="f3e75-132">using cast</span></span>
  ```Csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- <span data-ttu-id="f3e75-133">使用`as`运算符</span><span class="sxs-lookup"><span data-stu-id="f3e75-133">using `as` operator</span></span>
  ```Csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- <span data-ttu-id="f3e75-134">使用重载的`Include`它采用的类型参数 `string`</span><span class="sxs-lookup"><span data-stu-id="f3e75-134">using overload of `Include` that takes parameter of type `string`</span></span>
  ```Csharp
  context.People.Include("Student").ToList()
  ```

### <a name="ignored-includes"></a><span data-ttu-id="f3e75-135">忽略包括</span><span class="sxs-lookup"><span data-stu-id="f3e75-135">Ignored includes</span></span>

<span data-ttu-id="f3e75-136">如果您更改查询，从而使其不再返回查询开始与实体类型的实例，则会忽略 include 运算符。</span><span class="sxs-lookup"><span data-stu-id="f3e75-136">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="f3e75-137">在下面的示例中，基于包含运算符`Blog`，但然后`Select`运算符用于更改查询返回一个匿名类型。</span><span class="sxs-lookup"><span data-stu-id="f3e75-137">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="f3e75-138">在这种情况下，包括运算符产生任何影响。</span><span class="sxs-lookup"><span data-stu-id="f3e75-138">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="f3e75-139">默认情况下，EF 核心将记录一个警告时包括运算符将被忽略。</span><span class="sxs-lookup"><span data-stu-id="f3e75-139">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="f3e75-140">请参阅[日志记录](../miscellaneous/logging.md)有关查看日志记录输出的详细信息。</span><span class="sxs-lookup"><span data-stu-id="f3e75-140">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="f3e75-141">包括运算符忽略引发或不执行任何操作时，你可以更改的行为。</span><span class="sxs-lookup"><span data-stu-id="f3e75-141">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="f3e75-142">这是通常在设置您的上下文中的选项时`DbContext.OnConfiguring`，或在`Startup.cs`如果正在使用 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="f3e75-142">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="f3e75-143">显式加载</span><span class="sxs-lookup"><span data-stu-id="f3e75-143">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="f3e75-144">EF 核心 1.1 中已引入此功能。</span><span class="sxs-lookup"><span data-stu-id="f3e75-144">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="f3e75-145">你可以显式加载通过一个导航属性`DbContext.Entry(...)`API。</span><span class="sxs-lookup"><span data-stu-id="f3e75-145">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="f3e75-146">你还可以显式可以通过执行单独的查询返回相关的实体加载导航属性。</span><span class="sxs-lookup"><span data-stu-id="f3e75-146">You can also explicitly load a navigation property by executing a seperate query that returns the related entities.</span></span> <span data-ttu-id="f3e75-147">如果已启用更改跟踪，则在加载时实体，EF 核心将自动设置新加载的实体，以引用已加载，任何实体的导航属性并设置要引用的已加载实体的导航属性新加载的实体。</span><span class="sxs-lookup"><span data-stu-id="f3e75-147">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="f3e75-148">查询相关的实体</span><span class="sxs-lookup"><span data-stu-id="f3e75-148">Querying related entities</span></span>

<span data-ttu-id="f3e75-149">你还可以获得表示导航属性的内容的 LINQ 查询。</span><span class="sxs-lookup"><span data-stu-id="f3e75-149">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="f3e75-150">这允许你执行如运行通过相关的实体的聚合运算符，而无需加载到内存的操作。</span><span class="sxs-lookup"><span data-stu-id="f3e75-150">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="f3e75-151">你还可以筛选哪些相关的实体加载到内存。</span><span class="sxs-lookup"><span data-stu-id="f3e75-151">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="f3e75-152">延迟加载</span><span class="sxs-lookup"><span data-stu-id="f3e75-152">Lazy loading</span></span>

> [!NOTE]  
> <span data-ttu-id="f3e75-153">EF 核心 2.1 中引入了此功能。</span><span class="sxs-lookup"><span data-stu-id="f3e75-153">This feature was introduced in EF Core 2.1.</span></span>

<span data-ttu-id="f3e75-154">使用延迟加载的最简单方法是通过安装[Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/)包，并使其通过调用`UseLazyLoadingProxies`。</span><span class="sxs-lookup"><span data-stu-id="f3e75-154">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="f3e75-155">例如：</span><span class="sxs-lookup"><span data-stu-id="f3e75-155">For example:</span></span>
```Csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
<span data-ttu-id="f3e75-156">或者，使用 AddDbContext 时：</span><span class="sxs-lookup"><span data-stu-id="f3e75-156">Or when using AddDbContext:</span></span>
```Csharp
    .AddDbContext<BloggingContext>(
        b => b.UseLazyLoadingProxies()
              .UseSqlServer(myConnectionString));
```
<span data-ttu-id="f3e75-157">EF 核心然后启用延迟加载的任何导航属性，可以重写-即，它必须是`virtual`和可以从继承的类上。</span><span class="sxs-lookup"><span data-stu-id="f3e75-157">EF Core will then enable lazy-loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="f3e75-158">例如，在以下实体，`Post.Blog`和`Blog.Posts`导航属性将延迟加载。</span><span class="sxs-lookup"><span data-stu-id="f3e75-158">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>
```Csharp
public class Blog
{
    public int Id { get; set; }
    public string Name { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public virtual Blog Blog { get; set; }
}
```
### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="f3e75-159">Lazy 加载没有代理</span><span class="sxs-lookup"><span data-stu-id="f3e75-159">Lazy-loading without proxies</span></span>

<span data-ttu-id="f3e75-160">Lazy 加载代理工作通过将注入`ILazyLoader`中所述，服务实体，到[实体类型构造函数](../modeling/constructors.md)。</span><span class="sxs-lookup"><span data-stu-id="f3e75-160">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="f3e75-161">例如：</span><span class="sxs-lookup"><span data-stu-id="f3e75-161">For example:</span></span>
```Csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader?.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader?.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="f3e75-162">这不需要从中继承的实体类型或导航属性以是虚拟机，并允许使用创建的实体实例`new`若要延迟加载一次附加到上下文。</span><span class="sxs-lookup"><span data-stu-id="f3e75-162">This doesn't require entity types to be inherited from or navigation properties to be virtual and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="f3e75-163">但是，它需要对引用`ILazyLoader`服务，其标为 EF 核心程序集的实体类型。</span><span class="sxs-lookup"><span data-stu-id="f3e75-163">However, it requires a reference to the `ILazyLoader` service, which couples entity types to the EF Core assembly.</span></span> <span data-ttu-id="f3e75-164">为了避免这种 EF 核心才允许`ILazyLoader.Load`方法来插入视为的委托。</span><span class="sxs-lookup"><span data-stu-id="f3e75-164">To avoid this EF Core allows the `ILazyLoader.Load` method to be injected as a delegate.</span></span> <span data-ttu-id="f3e75-165">例如：</span><span class="sxs-lookup"><span data-stu-id="f3e75-165">For example:</span></span>
```Csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader?.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader?.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="f3e75-166">使用上面的代码`Load`扩展方法来执行有点清除器使用委托：</span><span class="sxs-lookup"><span data-stu-id="f3e75-166">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>
```Csharp
public static class PocoLoadingExtensions
{
    public static TRelated Load<TRelated>(
        this Action<object, string> loader,
        object entity,
        ref TRelated navigationField,
        [CallerMemberName] string navigationName = null)
        where TRelated : class
    {
        loader?.Invoke(entity, navigationName);

        return navigationField;
    }
}
```
> [!NOTE]  
> <span data-ttu-id="f3e75-167">延迟加载委托构造函数参数必须调用"lazyLoader"。</span><span class="sxs-lookup"><span data-stu-id="f3e75-167">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="f3e75-168">要为未来版本中使用计划这一功能的不同名称的配置。</span><span class="sxs-lookup"><span data-stu-id="f3e75-168">Configuration to use a different name this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="f3e75-169">相关的数据和序列化</span><span class="sxs-lookup"><span data-stu-id="f3e75-169">Related data and serialization</span></span>

<span data-ttu-id="f3e75-170">因为 EF 核心将自动修复向上导航属性，你可以得到周期对象关系图中。</span><span class="sxs-lookup"><span data-stu-id="f3e75-170">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="f3e75-171">例如，正在加载博客和它被相关文章将导致博客对象，它引用的文章的集合。</span><span class="sxs-lookup"><span data-stu-id="f3e75-171">For example, Loading a blog and it's related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="f3e75-172">每个这些文章将具有返回到博客的引用。</span><span class="sxs-lookup"><span data-stu-id="f3e75-172">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="f3e75-173">某些序列化框架不允许此类周期。</span><span class="sxs-lookup"><span data-stu-id="f3e75-173">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="f3e75-174">例如，如果遇到循环 Json.NET 将引发以下异常。</span><span class="sxs-lookup"><span data-stu-id="f3e75-174">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="f3e75-175">Newtonsoft.Json.JsonSerializationException： 自助引用与类型 MyApplication.Models.Blog 属性博客检测到循环。</span><span class="sxs-lookup"><span data-stu-id="f3e75-175">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="f3e75-176">如果你使用的 ASP.NET 核心，你可以配置 Json.NET 若要忽略的对象图中找到的周期。</span><span class="sxs-lookup"><span data-stu-id="f3e75-176">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="f3e75-177">将执行此操作`ConfigureServices(...)`中的方法`Startup.cs`。</span><span class="sxs-lookup"><span data-stu-id="f3e75-177">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

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
