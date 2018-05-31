---
title: 加载相关数据 - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: 5f1fb9376300739ab0e306d9d60e7ec71aa2d2e7
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812646"
---
# <a name="loading-related-data"></a><span data-ttu-id="1b604-102">加载相关数据</span><span class="sxs-lookup"><span data-stu-id="1b604-102">Loading Related Data</span></span>

<span data-ttu-id="1b604-103">Entity Framework Core 允许你在模型中使用导航属性来加载相关实体。</span><span class="sxs-lookup"><span data-stu-id="1b604-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="1b604-104">有三种常见的 O/RM 模式可用于加载相关数据。</span><span class="sxs-lookup"><span data-stu-id="1b604-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="1b604-105">“预先加载”表示从数据库中加载相关数据，作为初始查询的一部分。</span><span class="sxs-lookup"><span data-stu-id="1b604-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="1b604-106">“显式加载”表示稍后从数据库中显式加载相关数据。</span><span class="sxs-lookup"><span data-stu-id="1b604-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="1b604-107">“延迟加载”表示在访问导航属性时，从数据库中以透明方式加载相关数据。</span><span class="sxs-lookup"><span data-stu-id="1b604-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="1b604-108">可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying)。</span><span class="sxs-lookup"><span data-stu-id="1b604-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="1b604-109">预先加载</span><span class="sxs-lookup"><span data-stu-id="1b604-109">Eager loading</span></span>

<span data-ttu-id="1b604-110">可以使用 `Include` 方法来指定要包含在查询结果中的相关数据。</span><span class="sxs-lookup"><span data-stu-id="1b604-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="1b604-111">在以下示例中，结果中返回的博客将使用相关文章填充其 `Posts` 属性。</span><span class="sxs-lookup"><span data-stu-id="1b604-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="1b604-112">Entity Framework Core 会将导航属性自动修复为之前加载到上下文实例中的任何其他实体。</span><span class="sxs-lookup"><span data-stu-id="1b604-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="1b604-113">因此，即使不显式包含导航属性的数据，但如果先前加载了部分或所有相关实体，则仍可能填充该属性。</span><span class="sxs-lookup"><span data-stu-id="1b604-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="1b604-114">可以将来自多个关系的相关数据包含在单个查询中。</span><span class="sxs-lookup"><span data-stu-id="1b604-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="1b604-115">包括多个级别</span><span class="sxs-lookup"><span data-stu-id="1b604-115">Including multiple levels</span></span>

<span data-ttu-id="1b604-116">可以深化到关系以使用 `ThenInclude` 方法包括相关数据的多个级别。</span><span class="sxs-lookup"><span data-stu-id="1b604-116">You can drill down thru relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="1b604-117">以下示例加载了所有博客、其相关文章及每篇文章的作者。</span><span class="sxs-lookup"><span data-stu-id="1b604-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> <span data-ttu-id="1b604-118">当前版本的 Visual Studio 提供的代码完成选项不正确，因此在集合导航属性之后使用 `ThenInclude` 方法时，可能会导致正确的表达式标记有语法错误。</span><span class="sxs-lookup"><span data-stu-id="1b604-118">Current versions of Visual Studio offer incorrect code completion options and can cause correct expressions to be flagged with syntax errors when using the `ThenInclude` method after a collection navigation property.</span></span> <span data-ttu-id="1b604-119">这是在 https://github.com/dotnet/roslyn/issues/8237 中跟踪的 IntelliSense bug 的症状。</span><span class="sxs-lookup"><span data-stu-id="1b604-119">This is a symptom of an IntelliSense bug tracked at https://github.com/dotnet/roslyn/issues/8237.</span></span> <span data-ttu-id="1b604-120">只要代码正确无误且能够成功编译，就可以放心地忽略这些虚假语法错误。</span><span class="sxs-lookup"><span data-stu-id="1b604-120">It is safe to ignore these spurious syntax errors as long as the code is correct and can be compiled successfully.</span></span> 

<span data-ttu-id="1b604-121">可以用链锁住对 `ThenInclude` 的多个调用以继续包括相关数据的更深级别。</span><span class="sxs-lookup"><span data-stu-id="1b604-121">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="1b604-122">可以全部合并以将来自多个级别和多个根的相关数据包含在同一查询中。</span><span class="sxs-lookup"><span data-stu-id="1b604-122">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="1b604-123">可能希望包括所包含的实体之一的多个相关实体。</span><span class="sxs-lookup"><span data-stu-id="1b604-123">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="1b604-124">例如，当查询 `Blog` 时，将包括 `Posts` ，然后希望同时包括 `Posts` 的 `Author` 和 `Tags`。</span><span class="sxs-lookup"><span data-stu-id="1b604-124">For example, when querying `Blog`s, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="1b604-125">为此，需要在根目录开头指定每个包含路径。</span><span class="sxs-lookup"><span data-stu-id="1b604-125">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="1b604-126">例如，`Blog -> Posts -> Author` 和 `Blog -> Posts -> Tags`。</span><span class="sxs-lookup"><span data-stu-id="1b604-126">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="1b604-127">这并不意味着你将获得冗余联接，在大多数情况下，当生成 SQL 时，EF 将合并联接。</span><span class="sxs-lookup"><span data-stu-id="1b604-127">This does not mean you will get redundant joins, in most cases EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a><span data-ttu-id="1b604-128">在派生类型上包括</span><span class="sxs-lookup"><span data-stu-id="1b604-128">Include on derived types</span></span>

<span data-ttu-id="1b604-129">可以使用 `Include` 和 `ThenInclude` 包括来自仅在派生类型上定义的导航的相关数据。</span><span class="sxs-lookup"><span data-stu-id="1b604-129">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span> 

<span data-ttu-id="1b604-130">给定以下模型：</span><span class="sxs-lookup"><span data-stu-id="1b604-130">Given the following model:</span></span>

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

<span data-ttu-id="1b604-131">所有人员（可以使用许多模式预先加载的学生）的 `School` 导航的内容：</span><span class="sxs-lookup"><span data-stu-id="1b604-131">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

- <span data-ttu-id="1b604-132">使用强制转换</span><span class="sxs-lookup"><span data-stu-id="1b604-132">using cast</span></span>
  ```Csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- <span data-ttu-id="1b604-133">使用 `as` 运算符</span><span class="sxs-lookup"><span data-stu-id="1b604-133">using `as` operator</span></span>
  ```Csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- <span data-ttu-id="1b604-134">使用采用类型 `string` 的参数的 `Include` 的重载</span><span class="sxs-lookup"><span data-stu-id="1b604-134">using overload of `Include` that takes parameter of type `string`</span></span>
  ```Csharp
  context.People.Include("Student").ToList()
  ```

### <a name="ignored-includes"></a><span data-ttu-id="1b604-135">忽略包含</span><span class="sxs-lookup"><span data-stu-id="1b604-135">Ignored includes</span></span>

<span data-ttu-id="1b604-136">如果更改查询，从而使其不再返回查询以之为开头的实体类型的实例，则会忽略 include 运算符。</span><span class="sxs-lookup"><span data-stu-id="1b604-136">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="1b604-137">在以下示例中，include 运算符基于 `Blog`，但 `Select` 运算符用于更改查询以返回匿名类型。</span><span class="sxs-lookup"><span data-stu-id="1b604-137">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="1b604-138">在这种情况下，include 运算符没有任何效果。</span><span class="sxs-lookup"><span data-stu-id="1b604-138">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="1b604-139">默认情况下，当忽略 include 运算符时，EF Core 将记录警告。</span><span class="sxs-lookup"><span data-stu-id="1b604-139">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="1b604-140">有关查看日志记录输出的详细信息，请参阅[日志记录](../miscellaneous/logging.md)。</span><span class="sxs-lookup"><span data-stu-id="1b604-140">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="1b604-141">当忽略 include 运算符以引发异常或不执行任何操作时，可以更改行为。</span><span class="sxs-lookup"><span data-stu-id="1b604-141">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="1b604-142">这是在为上下文（如果使用的是 ASP.NET Core，则通常在 `DbContext.OnConfiguring` 或 `Startup.cs` 中）设置选项时完成的。</span><span class="sxs-lookup"><span data-stu-id="1b604-142">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="1b604-143">显式加载</span><span class="sxs-lookup"><span data-stu-id="1b604-143">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="1b604-144">EF Core 1.1 中已引入此功能。</span><span class="sxs-lookup"><span data-stu-id="1b604-144">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="1b604-145">可以通过 `DbContext.Entry(...)` API 显式加载导航属性。</span><span class="sxs-lookup"><span data-stu-id="1b604-145">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="1b604-146">还可以通过执行返回相关实体的单独查询来显式加载导航属性。</span><span class="sxs-lookup"><span data-stu-id="1b604-146">You can also explicitly load a navigation property by executing a seperate query that returns the related entities.</span></span> <span data-ttu-id="1b604-147">如果已启用更改跟踪，则在加载实体时，EF Core 将自动设置新加载实体的导航属性以引用已加载的任何实体，并设置已加载实体的导航属性以引用新加载的实体。</span><span class="sxs-lookup"><span data-stu-id="1b604-147">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="1b604-148">查询相关实体</span><span class="sxs-lookup"><span data-stu-id="1b604-148">Querying related entities</span></span>

<span data-ttu-id="1b604-149">还可以获得表示导航属性内容的 LINQ 查询。</span><span class="sxs-lookup"><span data-stu-id="1b604-149">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="1b604-150">这样可以执行诸如通过相关实体运行聚合运算符而无需将其加载到内存中等操作。</span><span class="sxs-lookup"><span data-stu-id="1b604-150">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="1b604-151">还可以筛选将哪些相关实体加载到内存中。</span><span class="sxs-lookup"><span data-stu-id="1b604-151">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="1b604-152">延迟加载</span><span class="sxs-lookup"><span data-stu-id="1b604-152">Lazy loading</span></span>

> [!NOTE]  
> <span data-ttu-id="1b604-153">EF Core 2.1 中已引入此功能。</span><span class="sxs-lookup"><span data-stu-id="1b604-153">This feature was introduced in EF Core 2.1.</span></span>

<span data-ttu-id="1b604-154">使用延迟加载的最简单方式是通过安装 [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) 包，并通过调用 `UseLazyLoadingProxies` 来启用该包。</span><span class="sxs-lookup"><span data-stu-id="1b604-154">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="1b604-155">例如:</span><span class="sxs-lookup"><span data-stu-id="1b604-155">For example:</span></span>
```Csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
<span data-ttu-id="1b604-156">或者，使用 AddDbContext 时：</span><span class="sxs-lookup"><span data-stu-id="1b604-156">Or when using AddDbContext:</span></span>
```Csharp
    .AddDbContext<BloggingContext>(
        b => b.UseLazyLoadingProxies()
              .UseSqlServer(myConnectionString));
```
<span data-ttu-id="1b604-157">EF Core 将为可被重写的任何导航属性（即，必须是 `virtual` 且在可从中继承的类上）启用延迟加载。</span><span class="sxs-lookup"><span data-stu-id="1b604-157">EF Core will then enable lazy-loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="1b604-158">例如，在以下实体中，`Post.Blog` 和 `Blog.Posts` 导航属性将被延迟加载。</span><span class="sxs-lookup"><span data-stu-id="1b604-158">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>
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
### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="1b604-159">不使用代理进行延迟加载</span><span class="sxs-lookup"><span data-stu-id="1b604-159">Lazy-loading without proxies</span></span>

<span data-ttu-id="1b604-160">使用代理进行延迟加载的工作方式是将 `ILazyLoader` 注入到实体中，如[实体类型构造函数](../modeling/constructors.md)中所述。</span><span class="sxs-lookup"><span data-stu-id="1b604-160">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="1b604-161">例如:</span><span class="sxs-lookup"><span data-stu-id="1b604-161">For example:</span></span>
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
<span data-ttu-id="1b604-162">这不需要从中继承的实体类型或要虚拟化的导航属性，并允许使用 `new` 创建的实体实例在附加到上下文后进行延迟加载。</span><span class="sxs-lookup"><span data-stu-id="1b604-162">This doesn't require entity types to be inherited from or navigation properties to be virtual and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="1b604-163">但需要对 `ILazyLoader` 服务的引用，从而将实体类型结合到 EF Core 程序集。</span><span class="sxs-lookup"><span data-stu-id="1b604-163">However, it requires a reference to the `ILazyLoader` service, which couples entity types to the EF Core assembly.</span></span> <span data-ttu-id="1b604-164">为了避免这种情况，EF Core 允许注入 `ILazyLoader.Load` 方法作为委托。</span><span class="sxs-lookup"><span data-stu-id="1b604-164">To avoid this EF Core allows the `ILazyLoader.Load` method to be injected as a delegate.</span></span> <span data-ttu-id="1b604-165">例如:</span><span class="sxs-lookup"><span data-stu-id="1b604-165">For example:</span></span>
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
<span data-ttu-id="1b604-166">上述代码使用 `Load` 扩展方法，以便更干净地使用委托：</span><span class="sxs-lookup"><span data-stu-id="1b604-166">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>
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
> <span data-ttu-id="1b604-167">延迟加载委托的构造函数参数必须称为“lazyLoader”。</span><span class="sxs-lookup"><span data-stu-id="1b604-167">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="1b604-168">为未来版本计划使用不同名称的配置。</span><span class="sxs-lookup"><span data-stu-id="1b604-168">Configuration to use a different name this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="1b604-169">相关数据和序列化</span><span class="sxs-lookup"><span data-stu-id="1b604-169">Related data and serialization</span></span>

<span data-ttu-id="1b604-170">由于 EF Core 将自动修复导航属性，因此可以在对象图中以循环结束。</span><span class="sxs-lookup"><span data-stu-id="1b604-170">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="1b604-171">例如，加载博客及其相关文章将生成引用文章集合的博客对象。</span><span class="sxs-lookup"><span data-stu-id="1b604-171">For example, Loading a blog and it's related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="1b604-172">其中每篇文章将返回引用该博客。</span><span class="sxs-lookup"><span data-stu-id="1b604-172">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="1b604-173">某些序列化框架不允许使用此类循环。</span><span class="sxs-lookup"><span data-stu-id="1b604-173">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="1b604-174">例如，如果遇到循环，Json.NET 将引发以下异常。</span><span class="sxs-lookup"><span data-stu-id="1b604-174">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="1b604-175">Newtonsoft.Json.JsonSerializationException：为“MyApplication.Models.Blog”类型的“Blog”属性检测到自引用循环。</span><span class="sxs-lookup"><span data-stu-id="1b604-175">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="1b604-176">如果正在使用 ASP.NET Core，则可以将 Json.NET 配置为忽略在对象图中找到的循环。</span><span class="sxs-lookup"><span data-stu-id="1b604-176">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="1b604-177">这是在 `Startup.cs` 中通过 `ConfigureServices(...)` 方法完成的。</span><span class="sxs-lookup"><span data-stu-id="1b604-177">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

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
