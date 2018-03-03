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
# <a name="entity-types-with-constructors"></a><span data-ttu-id="44f6f-102">使用构造函数的实体类型</span><span class="sxs-lookup"><span data-stu-id="44f6f-102">Entity types with constructors</span></span>

> [!NOTE]  
> <span data-ttu-id="44f6f-103">此功能是在 EF 核心 2.1 的新增功能。</span><span class="sxs-lookup"><span data-stu-id="44f6f-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="44f6f-104">从开始 EF 核心 2.1，它则现在可以定义参数的构造函数，并让 EF 核心创建实体的实例时调用此构造函数。</span><span class="sxs-lookup"><span data-stu-id="44f6f-104">Starting with EF Core 2.1, it is now possible to define a constructor with parameters and have EF Core call this constructor when creating an instance of the entity.</span></span> <span data-ttu-id="44f6f-105">构造函数参数可以绑定到映射的属性，或到各种类型的服务，以便于行为喜欢延迟加载。</span><span class="sxs-lookup"><span data-stu-id="44f6f-105">The constructor parameters can be bound to mapped properties, or to various kinds of services to facilitate behaviors like lazy-loading.</span></span>

> [!NOTE]  
> <span data-ttu-id="44f6f-106">截至 EF 核心 2.1，按照约定将为所有构造函数绑定。</span><span class="sxs-lookup"><span data-stu-id="44f6f-106">As of EF Core 2.1, all constructor binding is by convention.</span></span> <span data-ttu-id="44f6f-107">若要使用的特定构造函数的配置被计划未来的版本。</span><span class="sxs-lookup"><span data-stu-id="44f6f-107">Configuration of specific constructors to use is planned for a future release.</span></span>

## <a name="binding-to-mapped-properties"></a><span data-ttu-id="44f6f-108">绑定到映射的属性</span><span class="sxs-lookup"><span data-stu-id="44f6f-108">Binding to mapped properties</span></span>

<span data-ttu-id="44f6f-109">典型的博客/Post 模型，请考虑：</span><span class="sxs-lookup"><span data-stu-id="44f6f-109">Consider a typical Blog/Post model:</span></span>

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

<span data-ttu-id="44f6f-110">当 EF 核心创建这些类型的实例时，如的结果的查询，它将首先调用默认的无参数构造函数，然后设置每个属性的值从数据库。</span><span class="sxs-lookup"><span data-stu-id="44f6f-110">When EF Core creates instances of these types, such as for the results of a query, it will first call the default parameterless constructor and then set each property to the value from the database.</span></span> <span data-ttu-id="44f6f-111">但是，如果 EF 核心查找的参数化构造函数参数名称和类型相匹配的映射属性，则它将改为调用这些属性具有值的参数化构造函数，然后将未显式设置每个属性。</span><span class="sxs-lookup"><span data-stu-id="44f6f-111">However, if EF Core finds a parameterized constructor with parameter names and types that match those of mapped properties, then it will instead call the parameterized constructor with values for those properties and will not set each property explicitly.</span></span> <span data-ttu-id="44f6f-112">例如:</span><span class="sxs-lookup"><span data-stu-id="44f6f-112">For example:</span></span>

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
<span data-ttu-id="44f6f-113">需要注意的一些事项：</span><span class="sxs-lookup"><span data-stu-id="44f6f-113">Some things to note:</span></span>
* <span data-ttu-id="44f6f-114">需要具有构造函数的参数不是所有属性。</span><span class="sxs-lookup"><span data-stu-id="44f6f-114">Not all properties need to have constructor parameters.</span></span> <span data-ttu-id="44f6f-115">例如，由任何构造函数参数，因此 EF 核心将以正常方式调用的构造函数后设置未设置 Post.Content 属性。</span><span class="sxs-lookup"><span data-stu-id="44f6f-115">For example, the Post.Content property is not set by any constructor parameter, so EF Core will set it after calling the constructor in the normal way.</span></span>
* <span data-ttu-id="44f6f-116">参数类型和名称必须与匹配的属性类型和名称，只不过属性可以是 Pascal 大小写形式时参数采用 camel 大小写形式。</span><span class="sxs-lookup"><span data-stu-id="44f6f-116">The parameter types and names must match property types and names, except that properties can be Pascal-cased while the parameters are camel-cased.</span></span>
* <span data-ttu-id="44f6f-117">无法设置 （如博客或更高版本的文章） 的导航属性，EF 核心使用构造函数。</span><span class="sxs-lookup"><span data-stu-id="44f6f-117">EF Core cannot set navigation properties (such as Blog or Posts above) using a constructor.</span></span>
* <span data-ttu-id="44f6f-118">构造函数可以是公共、 私有的或具有任何其他可访问性。</span><span class="sxs-lookup"><span data-stu-id="44f6f-118">The constructor can be public, private, or have any other accessibility.</span></span>

### <a name="read-only-properties"></a><span data-ttu-id="44f6f-119">只读属性</span><span class="sxs-lookup"><span data-stu-id="44f6f-119">Read-only properties</span></span>

<span data-ttu-id="44f6f-120">通过构造函数中设置属性之后它很有意义，以使其中一些只读的。</span><span class="sxs-lookup"><span data-stu-id="44f6f-120">Once properties are being set via the constructor it can make sense to make some of them read-only.</span></span> <span data-ttu-id="44f6f-121">EF 核心支持此功能，但有一些需要注意的事项：</span><span class="sxs-lookup"><span data-stu-id="44f6f-121">EF Core supports this, but there are some things to look out for:</span></span>
* <span data-ttu-id="44f6f-122">按照约定未映射而无需 setter 的属性。</span><span class="sxs-lookup"><span data-stu-id="44f6f-122">Properties without setters are not mapped by convention.</span></span> <span data-ttu-id="44f6f-123">（这样倾向于映射不应将映射，如计算属性的属性。）</span><span class="sxs-lookup"><span data-stu-id="44f6f-123">(Doing so tends to map properties that should not be mapped, such as computed properties.)</span></span>
* <span data-ttu-id="44f6f-124">使用自动生成的密钥值需要是可读写，因为密钥的值需要插入新实体时，密钥生成器进行设置的密钥属性。</span><span class="sxs-lookup"><span data-stu-id="44f6f-124">Using automatically generated key values requires a key property that is read-write, since the key value needs to be set by the key generator when inserting new entities.</span></span>

<span data-ttu-id="44f6f-125">若要避免这些内容的简单方法是使用专用 setter。</span><span class="sxs-lookup"><span data-stu-id="44f6f-125">An easy way to avoid these things is to use private setters.</span></span> <span data-ttu-id="44f6f-126">例如:</span><span class="sxs-lookup"><span data-stu-id="44f6f-126">For example:</span></span>
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
<span data-ttu-id="44f6f-127">EF 核心看到为读写模式，这意味着，像以前那样映射所有属性并密钥可能仍会由存储生成的具有专用 setter 的属性。</span><span class="sxs-lookup"><span data-stu-id="44f6f-127">EF Core sees a property with a private setter as read-write, which means that all properties are mapped as before and the key can still be store-generated.</span></span>

<span data-ttu-id="44f6f-128">使用专用 setter 的替代方法是使属性实际上是只读的并在 OnModelCreating 中添加更多显式映射。</span><span class="sxs-lookup"><span data-stu-id="44f6f-128">An alternative to using private setters is to make properties really read-only and add more explicit mapping in OnModelCreating.</span></span> <span data-ttu-id="44f6f-129">同样，可以完全删除某些属性并将其替换为只有字段。</span><span class="sxs-lookup"><span data-stu-id="44f6f-129">Likewise, some properties can be removed completely and replaced with only fields.</span></span> <span data-ttu-id="44f6f-130">例如，考虑这些实体类型：</span><span class="sxs-lookup"><span data-stu-id="44f6f-130">For example, consider these entity types:</span></span>

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
<span data-ttu-id="44f6f-131">和中 OnModelCreating 此配置：</span><span class="sxs-lookup"><span data-stu-id="44f6f-131">And this configuration in OnModelCreating:</span></span>
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
<span data-ttu-id="44f6f-132">需要注意的事项：</span><span class="sxs-lookup"><span data-stu-id="44f6f-132">Things to note:</span></span>
* <span data-ttu-id="44f6f-133">"Property"的密钥现在是一个字段。</span><span class="sxs-lookup"><span data-stu-id="44f6f-133">The key "property" is now a field.</span></span> <span data-ttu-id="44f6f-134">它不是`readonly`字段，以便可以使用由存储生成的键。</span><span class="sxs-lookup"><span data-stu-id="44f6f-134">It is not a `readonly` field so that store-generated keys can be used.</span></span> 
* <span data-ttu-id="44f6f-135">其他属性是只读属性只能在构造函数中设置。</span><span class="sxs-lookup"><span data-stu-id="44f6f-135">The other properties are read-only properties set only in the constructor.</span></span>
* <span data-ttu-id="44f6f-136">如果主键值是永远只有由 EF 设置，或从数据库读取，则无需将其包含在构造函数。</span><span class="sxs-lookup"><span data-stu-id="44f6f-136">If the primary key value is only ever set by EF or read from the database, then there is no need to include it in the constructor.</span></span> <span data-ttu-id="44f6f-137">这为简单字段离开键"属性"，并使其清除，它不应设置显式创建新的博客或文章时。</span><span class="sxs-lookup"><span data-stu-id="44f6f-137">This leaves the key "property" as a simple field and makes it clear that it should not be set explicitly when creating new blogs or posts.</span></span>

> [!NOTE]  
> <span data-ttu-id="44f6f-138">此代码将导致编译器警告"169"，该字段永远不会使用该值。</span><span class="sxs-lookup"><span data-stu-id="44f6f-138">This code will result in compiler warning '169' indicating that the field is never used.</span></span> <span data-ttu-id="44f6f-139">由于在现实中 EF 核心 extralinguistic 的方式使用该字段，这可予以忽视。</span><span class="sxs-lookup"><span data-stu-id="44f6f-139">This can be ignored since in reality EF Core is using the field in an extralinguistic manner.</span></span>

## <a name="injecting-services"></a><span data-ttu-id="44f6f-140">将注入服务</span><span class="sxs-lookup"><span data-stu-id="44f6f-140">Injecting services</span></span>

<span data-ttu-id="44f6f-141">EF 核心还可以将"服务"注入到实体类型的构造函数。</span><span class="sxs-lookup"><span data-stu-id="44f6f-141">EF Core can also inject "services" into an entity type's constructor.</span></span> <span data-ttu-id="44f6f-142">例如，以下可插入：</span><span class="sxs-lookup"><span data-stu-id="44f6f-142">For example, the following can be injected:</span></span>
* <span data-ttu-id="44f6f-143">`DbContext` -还为您派生的 DbContext 类型类型化的当前上下文实例</span><span class="sxs-lookup"><span data-stu-id="44f6f-143">`DbContext` - the current context instance, which can also be typed as your derived DbContext type</span></span>
* <span data-ttu-id="44f6f-144">`ILazyLoader` 的延迟加载服务-请参阅[延迟加载文档](../querying/related-data.md)有关详细信息</span><span class="sxs-lookup"><span data-stu-id="44f6f-144">`ILazyLoader` - the lazy-loading service--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="44f6f-145">`Action<object, string>` 的延迟加载委托-请参阅[延迟加载文档](../querying/related-data.md)有关详细信息</span><span class="sxs-lookup"><span data-stu-id="44f6f-145">`Action<object, string>` - a lazy-loading delegate--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="44f6f-146">`IEntityType` -与此实体类型关联的 EF 核心元数据</span><span class="sxs-lookup"><span data-stu-id="44f6f-146">`IEntityType` - the EF Core metadata associated with this entity type</span></span>

> [!NOTE]  
> <span data-ttu-id="44f6f-147">截至 EF 核心 2.1，可插入仅通过 EF 核心已知的服务。</span><span class="sxs-lookup"><span data-stu-id="44f6f-147">As of EF Core 2.1, only services known by EF Core can be injected.</span></span> <span data-ttu-id="44f6f-148">支持将注入应用程序服务正在考虑针对将来的版本。</span><span class="sxs-lookup"><span data-stu-id="44f6f-148">Support for injecting application services is being considered for a future release.</span></span>

<span data-ttu-id="44f6f-149">例如，插入的 DbContext 可用来有选择地访问数据库以获取有关相关的实体的信息不加载所有这些情况下。</span><span class="sxs-lookup"><span data-stu-id="44f6f-149">For example, an injected DbContext can be used to selectively access the database to obtain information about related entities without loading them all.</span></span> <span data-ttu-id="44f6f-150">在以下示例中这用于不加载文章情况下获取的博客中的文章数：</span><span class="sxs-lookup"><span data-stu-id="44f6f-150">In the example below this is used to obtain the number of posts in a blog without loading the posts:</span></span>

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
<span data-ttu-id="44f6f-151">有关此请注意以下内容：</span><span class="sxs-lookup"><span data-stu-id="44f6f-151">A few things to notice about this:</span></span>
* <span data-ttu-id="44f6f-152">构造函数是专用容器，因为它仅为曾经调用由 EF 核心，并且没有用于常规用途的另一个公共构造函数。</span><span class="sxs-lookup"><span data-stu-id="44f6f-152">The constructor is private, since it is only ever call by EF Core, and there is another public constructor for general use.</span></span>
* <span data-ttu-id="44f6f-153">使用插入的服务的代码 （即上下文） 是对其防御性正在`null`以便处理在 EF 核心不创建实例的情况。</span><span class="sxs-lookup"><span data-stu-id="44f6f-153">The code using the injected service (i.e. the context) is defensive against it being `null` to handle cases where EF Core is not creating the instance.</span></span>
* <span data-ttu-id="44f6f-154">由于服务存储在读/写属性，则将重置时该实体附加到新的上下文实例。</span><span class="sxs-lookup"><span data-stu-id="44f6f-154">Because service is stored in a read/write property it will be reset when the entity is attached to a new context instance.</span></span>

> [!WARNING]  
> <span data-ttu-id="44f6f-155">因为它将直接与 EF 核心的实体类型，将注入如下 DbContext 通常被视为反模式。</span><span class="sxs-lookup"><span data-stu-id="44f6f-155">Injecting the DbContext like this is often considered an anti-pattern since it couples your entity types directly to EF Core.</span></span> <span data-ttu-id="44f6f-156">使用如下服务注入之前请仔细考虑所有选项。</span><span class="sxs-lookup"><span data-stu-id="44f6f-156">Carefully consider all options before using service injection like this.</span></span>
