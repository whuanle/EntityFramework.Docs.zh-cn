---
title: "配置 DbContext 的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 96abf3b94be3e1d19f833644f1c2f6f13fe0e730
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2017
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="354ae-102">配置创建的 DbContext</span><span class="sxs-lookup"><span data-stu-id="354ae-102">Configuring a DbContext</span></span>

<span data-ttu-id="354ae-103">这篇文章演示模式配置`DbContext`与`DbContextOptions`。</span><span class="sxs-lookup"><span data-stu-id="354ae-103">This article shows patterns for configuring a `DbContext` with `DbContextOptions`.</span></span> <span data-ttu-id="354ae-104">选项主要用于选择并配置数据存储区。</span><span class="sxs-lookup"><span data-stu-id="354ae-104">Options are primarily used to select and configure the data store.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="354ae-105">配置 DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="354ae-105">Configuring DbContextOptions</span></span>

<span data-ttu-id="354ae-106">`DbContext`必须具有的实例`DbContextOptions`才能执行。</span><span class="sxs-lookup"><span data-stu-id="354ae-106">`DbContext` must have an instance of `DbContextOptions` in order to execute.</span></span> <span data-ttu-id="354ae-107">这可以通过重写配置`OnConfiguring`，或提供外部通过构造函数自变量。</span><span class="sxs-lookup"><span data-stu-id="354ae-107">This can be configured by overriding `OnConfiguring`, or supplied externally via a constructor argument.</span></span>

<span data-ttu-id="354ae-108">如果将使用它们，`OnConfiguring`上提供的选项，这意味着它累加性并且可以执行覆盖提供给构造函数自变量的选项。</span><span class="sxs-lookup"><span data-stu-id="354ae-108">If both are used, `OnConfiguring` is executed on the supplied options, meaning it is additive and can overwrite  options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="354ae-109">构造函数自变量</span><span class="sxs-lookup"><span data-stu-id="354ae-109">Constructor argument</span></span>

<span data-ttu-id="354ae-110">使用构造函数的上下文代码</span><span class="sxs-lookup"><span data-stu-id="354ae-110">Context code with constructor</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

> [!TIP]  
> <span data-ttu-id="354ae-111">DbContext 的基构造函数还接受非泛型版本的`DbContextOptions`。</span><span class="sxs-lookup"><span data-stu-id="354ae-111">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`.</span></span> <span data-ttu-id="354ae-112">对于具有多个上下文类型应用程序，建议不要使用非泛型版本。</span><span class="sxs-lookup"><span data-stu-id="354ae-112">Using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="354ae-113">应用程序代码，以从构造函数参数初始化</span><span class="sxs-lookup"><span data-stu-id="354ae-113">Application code to initialize from constructor argument</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="354ae-114">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="354ae-114">OnConfiguring</span></span>

> [!WARNING]  
> <span data-ttu-id="354ae-115">`OnConfiguring`上次发生，并且可以覆盖从 DI 或构造函数中获得的选项。</span><span class="sxs-lookup"><span data-stu-id="354ae-115">`OnConfiguring` occurs last and can overwrite options obtained from DI or the constructor.</span></span> <span data-ttu-id="354ae-116">此方法不会将自身添加到测试 （除非你以完整的数据库为目标）。</span><span class="sxs-lookup"><span data-stu-id="354ae-116">This approach does not lend itself to testing (unless you target the full database).</span></span>

<span data-ttu-id="354ae-117">使用上下文代码`OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="354ae-117">Context code with `OnConfiguring`:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlite("Data Source=blog.db");
    }
}
```

<span data-ttu-id="354ae-118">应用程序代码以初始化与`OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="354ae-118">Application code to initialize with `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

## <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="354ae-119">使用依赖关系注入 DbContext</span><span class="sxs-lookup"><span data-stu-id="354ae-119">Using DbContext with dependency injection</span></span>

<span data-ttu-id="354ae-120">EF 支持使用`DbContext`与依赖关系注入容器。</span><span class="sxs-lookup"><span data-stu-id="354ae-120">EF supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="354ae-121">DbContext 类型可通过使用添加到服务容器`AddDbContext<TContext>`。</span><span class="sxs-lookup"><span data-stu-id="354ae-121">Your DbContext type can be added to the service container by using `AddDbContext<TContext>`.</span></span>

<span data-ttu-id="354ae-122">`AddDbContext`将这两种你的 DbContext 类型， `TContext`，和`DbContextOptions<TContext>`可用于从服务容器的注入。</span><span class="sxs-lookup"><span data-stu-id="354ae-122">`AddDbContext` will make both your DbContext type, `TContext`, and `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="354ae-123">请参阅[详细阅读](#more-reading)下面有关依赖关系注入的信息。</span><span class="sxs-lookup"><span data-stu-id="354ae-123">See [more reading](#more-reading) below for information on dependency injection.</span></span>

<span data-ttu-id="354ae-124">将 dbcontext 添加到依赖关系注入</span><span class="sxs-lookup"><span data-stu-id="354ae-124">Adding dbcontext to dependency injection</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="354ae-125">这要求添加[构造函数参数](#constructor-argument)为所接受的 DbContext 类型`DbContextOptions`。</span><span class="sxs-lookup"><span data-stu-id="354ae-125">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions`.</span></span>

<span data-ttu-id="354ae-126">上下文代码：</span><span class="sxs-lookup"><span data-stu-id="354ae-126">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="354ae-127">（在 ASP.NET Core) 的应用程序代码：</span><span class="sxs-lookup"><span data-stu-id="354ae-127">Application code (in ASP.NET Core):</span></span>

``` csharp
public MyController(BloggingContext context)
```

<span data-ttu-id="354ae-128">（通常不直接，使用服务提供商处） 的应用程序代码：</span><span class="sxs-lookup"><span data-stu-id="354ae-128">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="using-idesigntimedbcontextfactorytcontext"></a><span data-ttu-id="354ae-129">使用`IDesignTimeDbContextFactory<TContext>`</span><span class="sxs-lookup"><span data-stu-id="354ae-129">Using `IDesignTimeDbContextFactory<TContext>`</span></span>

<span data-ttu-id="354ae-130">上面的选项的替代方法，你可能会也提供的实现`IDesignTimeDbContextFactory<TContext>`。</span><span class="sxs-lookup"><span data-stu-id="354ae-130">As an alternative to the options above, you may also provide an implementation of `IDesignTimeDbContextFactory<TContext>`.</span></span> <span data-ttu-id="354ae-131">EF 工具可使用此工厂创建的 DbContext 实例。</span><span class="sxs-lookup"><span data-stu-id="354ae-131">EF tools can use this factory to create an instance of your DbContext.</span></span> <span data-ttu-id="354ae-132">这可能是启用特定的设计时体验，如迁移所必需的。</span><span class="sxs-lookup"><span data-stu-id="354ae-132">This may be required in order to enable specific design-time experiences such as migrations.</span></span>

<span data-ttu-id="354ae-133">实现此接口可启用没有公共默认构造函数的上下文类型的设计时服务。</span><span class="sxs-lookup"><span data-stu-id="354ae-133">Implement this interface to enable design-time services for context types that do not have a public default constructor.</span></span> <span data-ttu-id="354ae-134">设计时服务将自动发现的此接口中派生的上下文相同的程序集中的实现。</span><span class="sxs-lookup"><span data-stu-id="354ae-134">Design-time services will automatically discover implementations of this interface that are in the same assembly as the derived context.</span></span>

<span data-ttu-id="354ae-135">示例:</span><span class="sxs-lookup"><span data-stu-id="354ae-135">Example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Infrastructure;

namespace MyProject
{
    public class BloggingContextFactory : IDesignTimeDbContextFactory<BloggingContext>
    {
        public BloggingContext CreateDbContext(string[] args)
        {
            var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
            optionsBuilder.UseSqlite("Data Source=blog.db");

            return new BloggingContext(optionsBuilder.Options);
        }
    }
}
```

## <a name="more-reading"></a><span data-ttu-id="354ae-136">详细阅读</span><span class="sxs-lookup"><span data-stu-id="354ae-136">More reading</span></span>

* <span data-ttu-id="354ae-137">读取[首先使用 ASP.NET Core](../get-started/aspnetcore/index.md)有关 ASP.NET 核心中使用 EF 的详细信息。</span><span class="sxs-lookup"><span data-stu-id="354ae-137">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="354ae-138">读取[依赖关系注入](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html)若要了解有关使用 DI 详细信息。</span><span class="sxs-lookup"><span data-stu-id="354ae-138">Read [Dependency Injection](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) to learn more about using DI.</span></span>
* <span data-ttu-id="354ae-139">读取[测试](testing/index.md)有关详细信息。</span><span class="sxs-lookup"><span data-stu-id="354ae-139">Read [Testing](testing/index.md) for more information.</span></span>
