---
title: 加载相关数据 - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: 05833055f4744940364da4fdea7ded9a90d67508
ms.sourcegitcommit: a3aec015e0ad7ee31e0f75f00bbf2d286a3ac5c1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/18/2018
ms.locfileid: "42447761"
---
# <a name="loading-related-data"></a>加载相关数据

Entity Framework Core 允许你在模型中使用导航属性来加载相关实体。 有三种常见的 O/RM 模式可用于加载相关数据。
* “预先加载”表示从数据库中加载相关数据，作为初始查询的一部分。
* “显式加载”表示稍后从数据库中显式加载相关数据。
* “延迟加载”表示在访问导航属性时，从数据库中以透明方式加载相关数据。

> [!TIP]  
> 可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying)。

## <a name="eager-loading"></a>预先加载

可以使用 `Include` 方法来指定要包含在查询结果中的相关数据。 在以下示例中，结果中返回的博客将使用相关文章填充其 `Posts` 属性。

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Entity Framework Core 会将导航属性自动修复为之前加载到上下文实例中的任何其他实体。 因此，即使不显式包含导航属性的数据，但如果先前加载了部分或所有相关实体，则仍可能填充该属性。


可以将来自多个关系的相关数据包含在单个查询中。

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>包括多个级别

可以深化到关系以使用 `ThenInclude` 方法包括相关数据的多个级别。 以下示例加载了所有博客、其相关文章及每篇文章的作者。

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> 当前版本的 Visual Studio 提供的代码完成选项不正确，因此在集合导航属性之后使用 `ThenInclude` 方法时，可能会导致正确的表达式标记有语法错误。 这是在 https://github.com/dotnet/roslyn/issues/8237 中跟踪的 IntelliSense bug 的症状。 只要代码正确无误且能够成功编译，就可以放心地忽略这些虚假语法错误。 

可以用链锁住对 `ThenInclude` 的多个调用以继续包括相关数据的更深级别。

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

可以全部合并以将来自多个级别和多个根的相关数据包含在同一查询中。

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

可能希望包括所包含的实体之一的多个相关实体。 例如，当查询 `Blog` 时，将包括 `Posts` ，然后希望同时包括 `Posts` 的 `Author` 和 `Tags`。 为此，需要在根目录开头指定每个包含路径。 例如，`Blog -> Posts -> Author` 和 `Blog -> Posts -> Tags`。 这并不意味着你将获得冗余联接，在大多数情况下，当生成 SQL 时，EF 将合并联接。

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a>在派生类型上包括

可以使用 `Include` 和 `ThenInclude` 包括来自仅在派生类型上定义的导航的相关数据。 

给定以下模型：

```csharp
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

所有人员（可以使用许多模式预先加载的学生）的 `School` 导航的内容：

- 使用强制转换
  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- 使用 `as` 运算符
  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- 使用采用类型 `string` 的参数的 `Include` 的重载
  ```csharp
  context.People.Include("Student").ToList()
  ```

### <a name="ignored-includes"></a>忽略包含

如果更改查询，从而使其不再返回查询以之为开头的实体类型的实例，则会忽略 include 运算符。

在以下示例中，include 运算符基于 `Blog`，但 `Select` 运算符用于更改查询以返回匿名类型。 在这种情况下，include 运算符没有任何效果。

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

默认情况下，当忽略 include 运算符时，EF Core 将记录警告。 有关查看日志记录输出的详细信息，请参阅[日志记录](../miscellaneous/logging.md)。 当忽略 include 运算符以引发异常或不执行任何操作时，可以更改行为。 这是在为上下文（如果使用的是 ASP.NET Core，则通常在 `DbContext.OnConfiguring` 或 `Startup.cs` 中）设置选项时完成的。

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a>显式加载

> [!NOTE]  
> EF Core 1.1 中已引入此功能。

可以通过 `DbContext.Entry(...)` API 显式加载导航属性。

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

还可以通过执行返回相关实体的单独查询来显式加载导航属性。 如果已启用更改跟踪，则在加载实体时，EF Core 将自动设置新加载实体的导航属性以引用已加载的任何实体，并设置已加载实体的导航属性以引用新加载的实体。

### <a name="querying-related-entities"></a>查询相关实体

还可以获得表示导航属性内容的 LINQ 查询。

这样可以执行诸如通过相关实体运行聚合运算符而无需将其加载到内存中等操作。

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

还可以筛选将哪些相关实体加载到内存中。

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>延迟加载

> [!NOTE]  
> EF Core 2.1 中已引入此功能。

使用延迟加载的最简单方式是通过安装 [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) 包，并通过调用 `UseLazyLoadingProxies` 来启用该包。 例如:
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
或者，使用 AddDbContext 时：
```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```
EF Core 将为可被重写的任何导航属性（即，必须是 `virtual` 且在可从中继承的类上）启用延迟加载。 例如，在以下实体中，`Post.Blog` 和 `Blog.Posts` 导航属性将被延迟加载。
```csharp
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
### <a name="lazy-loading-without-proxies"></a>不使用代理进行延迟加载

使用代理进行延迟加载的工作方式是将 `ILazyLoader` 注入到实体中，如[实体类型构造函数](../modeling/constructors.md)中所述。 例如:
```csharp
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
        get => LazyLoader.Load(this, ref _posts);
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
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
这不需要从中继承的实体类型或要虚拟化的导航属性，并允许使用 `new` 创建的实体实例在附加到上下文后进行延迟加载。 但是，它需要对 `ILazyLoader` 服务的引用，在 [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) 包中对此进行了定义。 此包包含一组最小的类型，因此依赖于此类型几乎不会有影响。 但是，若要完全避免依赖于实体类型中的任何 EF Core 包，可以注入 `ILazyLoader.Load` 方法作为委托。 例如:
```csharp
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
        get => LazyLoader.Load(this, ref _posts);
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
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
上述代码使用 `Load` 扩展方法，以便更干净地使用委托：
```csharp
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
> 延迟加载委托的构造函数参数必须称为“lazyLoader”。 为未来版本使用不同于此计划的名称的配置。

## <a name="related-data-and-serialization"></a>相关数据和序列化

由于 EF Core 将自动修复导航属性，因此可以在对象图中以循环结束。 例如，加载博客及其相关文章将生成引用文章集合的博客对象。 其中每篇文章将返回引用该博客。

某些序列化框架不允许使用此类循环。 例如，如果遇到循环，Json.NET 将引发以下异常。

> Newtonsoft.Json.JsonSerializationException：为“MyApplication.Models.Blog”类型的“Blog”属性检测到自引用循环。

如果正在使用 ASP.NET Core，则可以将 Json.NET 配置为忽略在对象图中找到的循环。 这是在 `Startup.cs` 中通过 `ConfigureServices(...)` 方法完成的。

```csharp
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
