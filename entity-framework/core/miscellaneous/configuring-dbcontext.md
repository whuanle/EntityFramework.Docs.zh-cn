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
# <a name="configuring-a-dbcontext"></a>配置创建的 DbContext

这篇文章演示模式配置`DbContext`与`DbContextOptions`。 选项主要用于选择并配置数据存储区。

## <a name="configuring-dbcontextoptions"></a>配置 DbContextOptions

`DbContext`必须具有的实例`DbContextOptions`才能执行。 这可以通过重写配置`OnConfiguring`，或提供外部通过构造函数自变量。

如果将使用它们，`OnConfiguring`上提供的选项，这意味着它累加性并且可以执行覆盖提供给构造函数自变量的选项。

### <a name="constructor-argument"></a>构造函数自变量

使用构造函数的上下文代码

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
> DbContext 的基构造函数还接受非泛型版本的`DbContextOptions`。 对于具有多个上下文类型应用程序，建议不要使用非泛型版本。

应用程序代码，以从构造函数参数初始化

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>OnConfiguring

> [!WARNING]  
> `OnConfiguring`上次发生，并且可以覆盖从 DI 或构造函数中获得的选项。 此方法不会将自身添加到测试 （除非你以完整的数据库为目标）。

使用上下文代码`OnConfiguring`:

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

应用程序代码以初始化与`OnConfiguring`:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

## <a name="using-dbcontext-with-dependency-injection"></a>使用依赖关系注入 DbContext

EF 支持使用`DbContext`与依赖关系注入容器。 DbContext 类型可通过使用添加到服务容器`AddDbContext<TContext>`。

`AddDbContext`将这两种你的 DbContext 类型， `TContext`，和`DbContextOptions<TContext>`可用于从服务容器的注入。

请参阅[详细阅读](#more-reading)下面有关依赖关系注入的信息。

将 dbcontext 添加到依赖关系注入

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

这要求添加[构造函数参数](#constructor-argument)为所接受的 DbContext 类型`DbContextOptions`。

上下文代码：

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

（在 ASP.NET Core) 的应用程序代码：

``` csharp
public MyController(BloggingContext context)
```

（通常不直接，使用服务提供商处） 的应用程序代码：

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="using-idesigntimedbcontextfactorytcontext"></a>使用`IDesignTimeDbContextFactory<TContext>`

上面的选项的替代方法，你可能会也提供的实现`IDesignTimeDbContextFactory<TContext>`。 EF 工具可使用此工厂创建的 DbContext 实例。 这可能是启用特定的设计时体验，如迁移所必需的。

实现此接口可启用没有公共默认构造函数的上下文类型的设计时服务。 设计时服务将自动发现的此接口中派生的上下文相同的程序集中的实现。

示例:

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

## <a name="more-reading"></a>详细阅读

* 读取[首先使用 ASP.NET Core](../get-started/aspnetcore/index.md)有关 ASP.NET 核心中使用 EF 的详细信息。
* 读取[依赖关系注入](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html)若要了解有关使用 DI 详细信息。
* 读取[测试](testing/index.md)有关详细信息。
