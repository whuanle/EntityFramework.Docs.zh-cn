---
title: 关系的 EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
ms.technology: entity-framework-core
uid: core/modeling/relationships
ms.openlocfilehash: 1732d32643effb0f12111191ed4ba3abb4182d93
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
ms.locfileid: "26053027"
---
# <a name="relationships"></a>关系

关系定义了两个实体将互相关联起来。 在关系数据库中，它被表示通过外键约束。

> [!NOTE]  
> 大部分这篇文章中的示例使用一个对多关系来演示概念。 有关一对一和多对多关系的示例，请参阅[其他关系模式](#other-relationship-patterns)文章结尾部分。

## <a name="definition-of-terms"></a>术语的定义

有大量用于描述关系的术语

* **依赖的实体：** 这是包含外键属性的实体。 有时称为 child 的关系。

* **主体实体：** 这是包含主/备用键属性的实体。 有时称为 'parent' 的关系。

* **外键：** 中用于存储与相关实体的主体键属性的值的依赖实体属性。

* **主体的密钥：** 唯一标识的主体实体属性。 这可能是 primary key 或备用密钥。

* **导航属性：** 包含对相关实体引用的主体和/或依赖实体上定义的属性。

  * **集合导航属性：** 一个导航属性，包含对多个相关实体的引用。

  * **引用导航属性：** 一个导航属性，保存对单个相关实体的引用。

  * **反向导航属性：** 在讨论特定导航属性时，此术语是指关系另一端的导航属性。

下面的代码清单显示之间的一个对多关系`Blog`和`Post`

* `Post`是依赖实体

* `Blog`是主体实体

* `Post.BlogId`是外键

* `Blog.BlogId`是 （在这种情况下它是主键，而不是备用密钥） 的主体键

* `Post.Blog`是引用导航属性

* `Blog.Posts`是集合导航属性

* `Post.Blog`是的反向导航属性`Blog.Posts`（反之亦然）

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a>约定

按照约定，发现的类型上的导航属性时，将创建关系。 如果它指向的类型不能将映射为标量类型由当前的数据库提供程序，该属性被视为一个导航属性。

> [!NOTE]  
> 关系发现的约定将始终以目标主体实体的主键。 若要针对备用密钥，必须使用 Fluent API 执行其他配置。

### <a name="fully-defined-relationships"></a>完全定义的关系

关系的最常见模式是能够在两端的关系和依赖的实体类中定义的外键属性上定义的导航属性。

* 如果两个类型之间找到一对导航属性，则它们将可配置为位于同一关系的反向导航属性。

* 如果依赖实体包含名为的属性`<primary key property name>`， `<navigation property name><primary key property name>`，或`<principal entity name><primary key property name>`然后会将其配置为外键。

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> 如果有多个定义两个类型之间的导航属性 (即指向对方的导航的多个非重复对)、 约定将然后创建任何关系，并且将需要手动配置它们以确定如何导航属性对长达。

### <a name="no-foreign-key-property"></a>没有外键属性

尽管建议具有外键属性的依赖的实体类中定义，则不需要。 如果不找到任何外键属性，则卷影的外键属性，将名称引入`<navigation property name><principal key property name>`(请参阅[隐藏属性](shadow-properties.md)有关详细信息)。

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a>一个导航属性

包括一个导航属性 （没有反向导航和没有外键属性） 已足够具有关系定义的约定。 你还可以单个导航属性和外键属性。

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a>级联删除

按照约定，级联删除将设置为*Cascade*的所需的关系和*ClientSetNull*可选关系。 *级联*意味着依赖实体也会被删除。 *ClientSetNull*意味着不会加载到内存的依赖实体仍保持不变并且必须是手动将其删除，或更新以指向有效的主体实体。 对于加载到内存的实体，EF Core 将尝试将外键属性设置为 null。

请参阅[必选和可选关系](#required-and-optional-relationships)部分以了解必需和可选关系之间的差异。

请参阅[级联删除](../saving/cascade-delete.md)更多详细信息的不同删除行为和约定所使用的默认值。

## <a name="data-annotations"></a>数据注释

有两个用于配置关系的数据注释`[ForeignKey]`和`[InverseProperty]`。

### <a name="foreignkey"></a>[ForeignKey]

数据注释可用于配置哪些属性应用作给定关系的外键属性。 这通常是约定没有发现外键属性。

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?name=Entities&highlight=17)]

> [!TIP]  
> `[ForeignKey]`批注可以放置在关系中这两个导航属性。 它不需要继续依赖的实体类中的导航属性。

### <a name="inverseproperty"></a>[InverseProperty]

数据注释可用于配置如何依赖和主体实体的导航属性配对。 这通常是多个对两个实体类型之间的导航属性时。

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?name=Entities&highlight=20,23)]

## <a name="fluent-api"></a>Fluent API

若要配置关系 Fluent API 中，启动时通过标识构成关系的导航属性。 `HasOne`或`HasMany`标识你正在开始配置的实体类型上的导航属性。 然后链接到调用`WithOne`或`WithMany`来标识反向导航。 `HasOne`/`WithOne`用于引用导航属性和`HasMany` / `WithMany`用于集合导航属性。

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?name=Model&highlight=8,9,10)]

### <a name="single-navigation-property"></a>一个导航属性

如果你只有一个导航属性，则存在一些无参数重载`WithOne`和`WithMany`。 这表示没有从概念上讲引用或集合上的另一端的关系，但不未包含在实体类中任何导航属性。

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?name=Model&highlight=10)]

### <a name="foreign-key"></a>外键

Fluent API 可用于配置哪些属性应用作给定关系的外键属性。

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?name=Model&highlight=11)]

以下代码列表演示如何配置的组合外键。

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?name=Model&highlight=13)]

你可以使用的字符串重载`HasForeignKey(...)`卷影属性配置为外键 (请参阅[隐藏属性](shadow-properties.md)有关详细信息)。 建议显式将卷影属性添加到模型，然后再使用它作为外键 （如下所示）。

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="principal-key"></a>主体密钥

如果你想要引用以外的主键属性的外键，可以使用 Fluent API 来配置该关系的主体键属性。 自动配置为该主体的密钥将该属性是作为备用键的安装程序 (请参阅[备用键](alternate-keys.md)有关详细信息)。

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/PrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => s.CarLicensePlate)
            .HasPrincipalKey(c => c.LicensePlate);
    }
}

public class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

以下代码列表演示如何配置主体组合键。

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CompositePrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => new { s.CarState, s.CarLicensePlate })
            .HasPrincipalKey(c => new { c.State, c.LicensePlate });
    }
}

public class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarState { get; set; }
    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

> [!WARNING]  
> 指定主体键属性的顺序必须与匹配外键指定的顺序。

### <a name="required-and-optional-relationships"></a>必选和可选的关系

Fluent API 可用于配置关系为必需或可选。 最终此参数控制外键属性是必需还是可选。 使用卷影状态外键时，这是最有用。 如果你有在实体类中的外键属性，则关系的 requiredness 确定基于外键属性是必需还是可选 (请参阅[必选和可选属性](required-optional.md)的详细信息信息）。

<!-- [!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/Required.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}
```

### <a name="cascade-delete"></a>级联删除

Fluent API 可用于显式配置为某个给定的关系的级联删除行为。

请参阅[级联删除](../saving/cascade-delete.md)上保存的数据部分中有关每个选项的详细讨论。

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CascadeDelete.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .OnDelete(DeleteBehavior.Cascade);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int? BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="other-relationship-patterns"></a>其他关系模式

### <a name="one-to-one"></a>一对一

一对一关系两端具有引用导航属性。 它们遵循相同的约定一个对多关系，但在外键属性，以确保只有一个依赖于与每个主体引入唯一索引。

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/Relationships/OneToOne.cs?highlight=6,15,16)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

> [!NOTE]  
> EF 将选择一个要为基于其能够检测到外键属性的依赖项的实体。 如果错误实体被选作依赖项，可以使用 Fluent API 要更正此问题。

如果配置使用 Fluent API 的关系，则使用`HasOne`和`WithOne`方法。

配置你需要指定相关的实体类型中的外键时请注意到提供的泛型参数`HasForeignKey`下面的列表中。 一个对多关系中很明显的实体引用导航是依赖项和一个具有集合是的主体。 但这不是这样一对一关系-因此中显式定义的需要。

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/OneToOne.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<BlogImage> BlogImages { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasOne(p => p.BlogImage)
            .WithOne(i => i.Blog)
            .HasForeignKey<BlogImage>(b => b.BlogForeignKey);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogForeignKey { get; set; }
    public Blog Blog { get; set; }
}
```

### <a name="many-to-many"></a>多对多

尚不支持而不需要的实体类来表示联接表的多对多关系。 但是，您可以通过包括联接表和映射两个单独的一对多关系的实体类表示多对多关系。

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/ManyToMany.cs?highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Tag> Tags { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<PostTag>()
            .HasKey(t => new { t.PostId, t.TagId });

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Post)
            .WithMany(p => p.PostTags)
            .HasForeignKey(pt => pt.PostId);

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Tag)
            .WithMany(t => t.PostTags)
            .HasForeignKey(pt => pt.TagId);
    }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class Tag
{
    public string TagId { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class PostTag
{
    public int PostId { get; set; }
    public Post Post { get; set; }

    public string TagId { get; set; }
    public Tag Tag { get; set; }
}
```
