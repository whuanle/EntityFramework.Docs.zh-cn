---
title: 卷影属性-EF 核心
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
ms.technology: entity-framework-core
uid: core/modeling/shadow-properties
ms.openlocfilehash: 8c7f70789ddc6ebd24f9bfae931069834db593c2
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2017
ms.locfileid: "26053547"
---
# <a name="shadow-properties"></a>隐藏属性

卷影属性是指你.NET 实体类中未定义但 EF 核心模型中该实体类型定义。 值和状态的这些属性将保留只是在更改跟踪器。

不应在映射的实体类型公开的数据库中没有数据时，卷影属性非常有用。 它们通常用于外键属性，其中两个实体之间的关系由在数据库中，某个外键值，但使用实体类型之间的导航属性的实体类型上托管关系。

卷影属性值可以获取和通过更改`ChangeTracker`API。

``` csharp
   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

可以通过 LINQ 查询中引用隐藏属性`EF.Property`静态方法。

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>约定

当发现一种关系，但没有外键属性找到依赖的实体类中，可以通过约定创建卷影属性。 在这种情况下，将引入了卷影的外键属性。 将名为卷影外键属性`<navigation property name><principal key property name>`（依赖的实体，用于指向主体实体，对导航用于命名）。 如果主体键属性名称中包含的名称的导航属性，则名称只需将`<principal key property name>`。 如果依赖实体中没有任何导航属性，则主体类型名称使用在其位置。

例如，下面的代码清单将导致`BlogId`引入到的卷影属性`Post`实体。

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/ShadowForeignKey.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
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

## <a name="data-annotations"></a>数据注释

使用数据注释，可以创建卷影属性。

## <a name="fluent-api"></a>Fluent API

Fluent API 可用于配置卷影属性。 一旦调用了字符串重载的`Property`你可以将任何其他属性将在配置调用的链接。

如果名称提供给`Property`方法与匹配的名称的现有属性 （卷影属性或其中一个实体类上定义），则代码将配置该现有属性，而不是引入一个新的卷影属性。

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/ShadowProperty.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property<DateTime>("LastUpdated");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
