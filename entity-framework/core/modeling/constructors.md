---
title: "使用构造函数的 EF 核心的实体类型"
author: ajcvickers
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
ms.technology: entity-framework-core
uid: core/modeling/constructors
ms.openlocfilehash: 38ab0c1c3cd8c490875abf30b8478c99bc58630f
ms.sourcegitcommit: 60b831318c4f5ec99061e8af6a7c9e7c03b3469c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2018
---
# <a name="entity-types-with-constructors"></a>使用构造函数的实体类型

> [!NOTE]  
> 此功能是在 EF 核心 2.1 的新增功能。

从开始 EF 核心 2.1，它则现在可以定义参数的构造函数，并让 EF 核心创建实体的实例时调用此构造函数。 构造函数参数可以绑定到映射的属性，或到各种类型的服务，以便于行为喜欢延迟加载。

> [!NOTE]  
> 截至 EF 核心 2.1，按照约定将为所有构造函数绑定。 若要使用的特定构造函数的配置被计划未来的版本。

## <a name="binding-to-mapped-properties"></a>绑定到映射的属性

典型的博客/Post 模型，请考虑：

```Csharp
public class Blog
{
    public int Id { get; set; }

    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public int Id { get; set; }
    
    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```

当 EF 核心创建这些类型的实例时，如的结果的查询，它将首先调用默认的无参数构造函数，然后设置每个属性的值从数据库。 但是，如果 EF 核心查找的参数化构造函数参数名称和类型相匹配的映射属性，则它将改为调用这些属性具有值的参数化构造函数，然后将未显式设置每个属性。 例如:

```Csharp
public class Blog
{
    public Blog(int id, string name, string author)
    {
        Id = id;
        Name = name;
        Author = author;
    }

    public int Id { get; set; }

    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public Post(int id, string title, DateTime postedOn)
    {
        Id = id;
        Title = title;
        PostedOn = postedOn;
    }

    public int Id { get; set; }

    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```
需要注意的一些事项：
* 需要具有构造函数的参数不是所有属性。 例如，由任何构造函数参数，因此 EF 核心将以正常方式调用的构造函数后设置未设置 Post.Content 属性。
* 参数类型和名称必须与匹配的属性类型和名称，只不过属性可以是 Pascal 大小写形式时参数采用 camel 大小写形式。
* 无法设置 （如博客或更高版本的文章） 的导航属性，EF 核心使用构造函数。
* 构造函数可以是公共、 私有的或具有任何其他可访问性。

### <a name="read-only-properties"></a>只读属性

通过构造函数中设置属性之后它很有意义，以使其中一些只读的。 EF 核心支持此功能，但有一些需要注意的事项：
* 按照约定未映射而无需 setter 的属性。 （这样倾向于映射不应将映射，如计算属性的属性。）
* 使用自动生成的密钥值需要是可读写，因为密钥的值需要插入新实体时，密钥生成器进行设置的密钥属性。

若要避免这些内容的简单方法是使用专用 setter。 例如:
```Csharp
public class Blog
{
    public Blog(int id, string name, string author)
    {
        Id = id;
        Name = name;
        Author = author;
    }

    public int Id { get; private set; }

    public string Name { get; private set; }
    public string Author { get; private set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public Post(int id, string title, DateTime postedOn)
    {
        Id = id;
        Title = title;
        PostedOn = postedOn;
    }

    public int Id { get; private set; }

    public string Title { get; private set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; private set; }

    public Blog Blog { get; set; }
}

```
EF 核心看到为读写模式，这意味着，像以前那样映射所有属性并密钥可能仍会由存储生成的具有专用 setter 的属性。

使用专用 setter 的替代方法是使属性实际上是只读的并在 OnModelCreating 中添加更多显式映射。 同样，可以完全删除某些属性并将其替换为只有字段。 例如，考虑这些实体类型：

```Csharp
public class Blog
{
    private int _id;

    public Blog(string name, string author)
    {
        Name = name;
        Author = author;
    }

    public string Name { get; }
    public string Author { get; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    private int _id;

    public Post(string title, DateTime postedOn)
    {
        Title = title;
        PostedOn = postedOn;
    }

    public string Title { get; }
    public string Content { get; set; }
    public DateTime PostedOn { get; }

    public Blog Blog { get; set; }
}
```
和中 OnModelCreating 此配置：
```Csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>(
        b =>
        {
            b.HasKey("_id");
            b.Property(e => e.Author);
            b.Property(e => e.Name);
        });

    modelBuilder.Entity<Post>(
        b =>
        {
            b.HasKey("_id");
            b.Property(e => e.Title);
            b.Property(e => e.PostedOn);
        });
}
```
需要注意的事项：
* "Property"的密钥现在是一个字段。 它不是`readonly`字段，以便可以使用由存储生成的键。 
* 其他属性是只读属性只能在构造函数中设置。
* 如果主键值是永远只有由 EF 设置，或从数据库读取，则无需将其包含在构造函数。 这为简单字段离开键"属性"，并使其清除，它不应设置显式创建新的博客或文章时。

> [!NOTE]  
> 此代码将导致编译器警告"169"，该字段永远不会使用该值。 由于在现实中 EF 核心 extralinguistic 的方式使用该字段，这可予以忽视。

## <a name="injecting-services"></a>将注入服务

EF 核心还可以将"服务"注入到实体类型的构造函数。 例如，以下可插入：
* `DbContext` -还为您派生的 DbContext 类型类型化的当前上下文实例
* `ILazyLoader` 的延迟加载服务-请参阅[延迟加载文档](../querying/related-data.md)有关详细信息
* `Action<object, string>` 的延迟加载委托-请参阅[延迟加载文档](../querying/related-data.md)有关详细信息
* `IEntityType` -与此实体类型关联的 EF 核心元数据

> [!NOTE]  
> 截至 EF 核心 2.1，可插入仅通过 EF 核心已知的服务。 支持将注入应用程序服务正在考虑针对将来的版本。

例如，插入的 DbContext 可用来有选择地访问数据库以获取有关相关的实体的信息不加载所有这些情况下。 在以下示例中这用于不加载文章情况下获取的博客中的文章数：

```Csharp
public class Blog
{
    public Blog()
    {
    }

    private Blog(BloggingContext context)
    {
        Context = context;
    }

    private BloggingContext Context { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; set; }

    public int PostsCount
        => Posts?.Count
           ?? Context?.Set<Post>().Count(p => Id == EF.Property<int?>(p, "BlogId"))
           ?? 0;
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```
有关此请注意以下内容：
* 构造函数是专用容器，因为它仅为曾经调用由 EF 核心，并且没有用于常规用途的另一个公共构造函数。
* 使用插入的服务的代码 （即上下文） 是对其防御性正在`null`以便处理在 EF 核心不创建实例的情况。
* 由于服务存储在读/写属性，则将重置时该实体附加到新的上下文实例。

> [!WARNING]  
> 因为它将直接与 EF 核心的实体类型，将注入如下 DbContext 通常被视为反模式。 使用如下服务注入之前请仔细考虑所有选项。
