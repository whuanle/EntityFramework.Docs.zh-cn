---
title: 配置 DbContext 的 EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 6980acd53b0a74055af7a1e04b476f4625c327c9
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2018
ms.locfileid: "29152385"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="ec4ea-102">配置创建的 DbContext</span><span class="sxs-lookup"><span data-stu-id="ec4ea-102">Configuring a DbContext</span></span>

<span data-ttu-id="ec4ea-103">这篇文章演示用于配置基本模式`DbContext`通过`DbContextOptions`连接到使用特定的 EF Core 提供程序和可选行为的数据库。</span><span class="sxs-lookup"><span data-stu-id="ec4ea-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="ec4ea-104">设计时 DbContext 配置</span><span class="sxs-lookup"><span data-stu-id="ec4ea-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="ec4ea-105">EF Core 设计时工具如[迁移](xref:core/managing-schemas/migrations/index)需要能够发现和创建的工作实例`DbContext`以收集有关应用程序的实体类型以及它们如何映射到数据库架构的详细信息的类型。</span><span class="sxs-lookup"><span data-stu-id="ec4ea-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="ec4ea-106">此过程可能会自动，只要该工具可以轻松地创建`DbContext`，它将在配置同样到它如何将配置在运行时的方式。</span><span class="sxs-lookup"><span data-stu-id="ec4ea-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="ec4ea-107">尽管提供必要的配置信息到任何模式`DbContext`可在运行时，需要使用的工具`DbContext`在设计时只能处理有限数量的模式。</span><span class="sxs-lookup"><span data-stu-id="ec4ea-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="ec4ea-108">这些内容中的更详细地介绍[设计时上下文创建](xref:core/miscellaneous/cli/dbcontext-creation)部分。</span><span class="sxs-lookup"><span data-stu-id="ec4ea-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="ec4ea-109">配置 DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="ec4ea-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="ec4ea-110">`DbContext` 必须具有的实例`DbContextOptions`才能执行任何工作。</span><span class="sxs-lookup"><span data-stu-id="ec4ea-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="ec4ea-111">`DbContextOptions`实例传送配置信息，如：</span><span class="sxs-lookup"><span data-stu-id="ec4ea-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="ec4ea-112">数据库提供程序，若要使用，通常选择通过调用方法，如`UseSqlServer`或 `UseSqlite`</span><span class="sxs-lookup"><span data-stu-id="ec4ea-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`</span></span>
- <span data-ttu-id="ec4ea-113">任何必要的连接字符串或数据库实例中，标识符通常作为参数传递给上述提供程序选择方法</span><span class="sxs-lookup"><span data-stu-id="ec4ea-113">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="ec4ea-114">任何提供程序级别的可选行为的选择器，通常还链接到提供程序选择方法的调用中</span><span class="sxs-lookup"><span data-stu-id="ec4ea-114">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="ec4ea-115">任何常规 EF Core 行为选择器，通常链接之后或之前提供程序选择器方法</span><span class="sxs-lookup"><span data-stu-id="ec4ea-115">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="ec4ea-116">下面的示例将配置`DbContextOptions`若要使用 SQL Server 提供程序，在连接包含`connectionString`变量、 提供程序级别的命令超时，以及可使在中执行的所有查询 EF Core 行为选择器`DbContext`[否跟踪](xref:core/querying/tracking#no-tracking-queries)默认情况下：</span><span class="sxs-lookup"><span data-stu-id="ec4ea-116">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="ec4ea-117">提供程序选择器方法和上述其他行为选择器方法是扩展方法在`DbContextOptions`或提供程序特定的选项类别。</span><span class="sxs-lookup"><span data-stu-id="ec4ea-117">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="ec4ea-118">才能访问这些扩展方法，你可能需要具有的命名空间 (通常`Microsoft.EntityFrameworkCore`) 中的作用域以及在项目中包含更多文件包依赖关系。</span><span class="sxs-lookup"><span data-stu-id="ec4ea-118">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="ec4ea-119">`DbContextOptions`可以提供给`DbContext`通过重写`OnConfiguring`方法或外部通过构造函数自变量。</span><span class="sxs-lookup"><span data-stu-id="ec4ea-119">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="ec4ea-120">如果将使用它们，`OnConfiguring`最后应用，并且可以覆盖提供给构造函数自变量的选项。</span><span class="sxs-lookup"><span data-stu-id="ec4ea-120">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="ec4ea-121">构造函数自变量</span><span class="sxs-lookup"><span data-stu-id="ec4ea-121">Constructor argument</span></span>

<span data-ttu-id="ec4ea-122">使用构造函数的上下文代码：</span><span class="sxs-lookup"><span data-stu-id="ec4ea-122">Context code with constructor:</span></span>

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
> <span data-ttu-id="ec4ea-123">DbContext 的基构造函数还接受非泛型版本的`DbContextOptions`，但对于具有多个上下文类型应用程序不建议使用非泛型版本。</span><span class="sxs-lookup"><span data-stu-id="ec4ea-123">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="ec4ea-124">应用程序代码以初始化从构造函数自变量：</span><span class="sxs-lookup"><span data-stu-id="ec4ea-124">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="ec4ea-125">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="ec4ea-125">OnConfiguring</span></span>

<span data-ttu-id="ec4ea-126">使用上下文代码`OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="ec4ea-126">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="ec4ea-127">应用程序代码以初始化`DbContext`使用`OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="ec4ea-127">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="ec4ea-128">此方法不会将自身添加到测试，除非测试以完整的数据库为目标。</span><span class="sxs-lookup"><span data-stu-id="ec4ea-128">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="ec4ea-129">使用依赖关系注入 DbContext</span><span class="sxs-lookup"><span data-stu-id="ec4ea-129">Using DbContext with dependency injection</span></span>

<span data-ttu-id="ec4ea-130">EF Core 支持使用`DbContext`与依赖关系注入容器。</span><span class="sxs-lookup"><span data-stu-id="ec4ea-130">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="ec4ea-131">DbContext 类型可通过使用添加到服务容器`AddDbContext<TContext>`方法。</span><span class="sxs-lookup"><span data-stu-id="ec4ea-131">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="ec4ea-132">`AddDbContext<TContext>` 将这两种你的 DbContext 类型， `TContext`，和相应`DbContextOptions<TContext>`可用于从服务容器的注入。</span><span class="sxs-lookup"><span data-stu-id="ec4ea-132">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="ec4ea-133">请参阅[详细阅读](#more-reading)下面有关依赖关系注入的其他信息。</span><span class="sxs-lookup"><span data-stu-id="ec4ea-133">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="ec4ea-134">添加`Dbcontext`依赖关系注入到：</span><span class="sxs-lookup"><span data-stu-id="ec4ea-134">Adding the `Dbcontext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="ec4ea-135">这要求添加[构造函数参数](#constructor-argument)为所接受的 DbContext 类型`DbContextOptions<TContext>`。</span><span class="sxs-lookup"><span data-stu-id="ec4ea-135">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="ec4ea-136">上下文代码：</span><span class="sxs-lookup"><span data-stu-id="ec4ea-136">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="ec4ea-137">（在 ASP.NET Core) 的应用程序代码：</span><span class="sxs-lookup"><span data-stu-id="ec4ea-137">Application code (in ASP.NET Core):</span></span>

``` csharp
public class MyController
{
    private readonly BloggingContext _context;

    public MyController(BloggingContext context)
    {
      _context = context;
    }

    ...
}
```

<span data-ttu-id="ec4ea-138">（通常不直接，使用服务提供商处） 的应用程序代码：</span><span class="sxs-lookup"><span data-stu-id="ec4ea-138">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a><span data-ttu-id="ec4ea-139">详细阅读</span><span class="sxs-lookup"><span data-stu-id="ec4ea-139">More reading</span></span>

* <span data-ttu-id="ec4ea-140">读取[首先使用 ASP.NET Core](../get-started/aspnetcore/index.md)有关 ASP.NET Core 中使用 EF 的详细信息。</span><span class="sxs-lookup"><span data-stu-id="ec4ea-140">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="ec4ea-141">读取[依赖关系注入](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection)若要了解有关使用 DI 详细信息。</span><span class="sxs-lookup"><span data-stu-id="ec4ea-141">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="ec4ea-142">读取[测试](testing/index.md)有关详细信息。</span><span class="sxs-lookup"><span data-stu-id="ec4ea-142">Read [Testing](testing/index.md) for more information.</span></span>
