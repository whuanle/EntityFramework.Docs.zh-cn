---
title: 卷影属性-EF Core
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
# <a name="shadow-properties"></a><span data-ttu-id="34769-102">隐藏属性</span><span class="sxs-lookup"><span data-stu-id="34769-102">Shadow Properties</span></span>

<span data-ttu-id="34769-103">卷影属性是指你.NET 实体类中未定义但 EF Core 模型中该实体类型定义。</span><span class="sxs-lookup"><span data-stu-id="34769-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="34769-104">值和状态的这些属性将保留只是在更改跟踪器。</span><span class="sxs-lookup"><span data-stu-id="34769-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="34769-105">不应在映射的实体类型公开的数据库中没有数据时，卷影属性非常有用。</span><span class="sxs-lookup"><span data-stu-id="34769-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="34769-106">它们通常用于外键属性，其中两个实体之间的关系由在数据库中，某个外键值，但使用实体类型之间的导航属性的实体类型上托管关系。</span><span class="sxs-lookup"><span data-stu-id="34769-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="34769-107">卷影属性值可以获取和通过更改`ChangeTracker`API。</span><span class="sxs-lookup"><span data-stu-id="34769-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="34769-108">可以通过 LINQ 查询中引用隐藏属性`EF.Property`静态方法。</span><span class="sxs-lookup"><span data-stu-id="34769-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="34769-109">约定</span><span class="sxs-lookup"><span data-stu-id="34769-109">Conventions</span></span>

<span data-ttu-id="34769-110">当发现一种关系，但没有外键属性找到依赖的实体类中，可以通过约定创建卷影属性。</span><span class="sxs-lookup"><span data-stu-id="34769-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="34769-111">在这种情况下，将引入了卷影的外键属性。</span><span class="sxs-lookup"><span data-stu-id="34769-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="34769-112">将名为卷影外键属性`<navigation property name><principal key property name>`（依赖的实体，用于指向主体实体，对导航用于命名）。</span><span class="sxs-lookup"><span data-stu-id="34769-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="34769-113">如果主体键属性名称中包含的名称的导航属性，则名称只需将`<principal key property name>`。</span><span class="sxs-lookup"><span data-stu-id="34769-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="34769-114">如果依赖实体中没有任何导航属性，则主体类型名称使用在其位置。</span><span class="sxs-lookup"><span data-stu-id="34769-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="34769-115">例如，下面的代码清单将导致`BlogId`引入到的卷影属性`Post`实体。</span><span class="sxs-lookup"><span data-stu-id="34769-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="34769-116">数据注释</span><span class="sxs-lookup"><span data-stu-id="34769-116">Data Annotations</span></span>

<span data-ttu-id="34769-117">使用数据注释，可以创建卷影属性。</span><span class="sxs-lookup"><span data-stu-id="34769-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="34769-118">Fluent API</span><span class="sxs-lookup"><span data-stu-id="34769-118">Fluent API</span></span>

<span data-ttu-id="34769-119">Fluent API 可用于配置卷影属性。</span><span class="sxs-lookup"><span data-stu-id="34769-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="34769-120">一旦调用了字符串重载的`Property`你可以将任何其他属性将在配置调用的链接。</span><span class="sxs-lookup"><span data-stu-id="34769-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="34769-121">如果名称提供给`Property`方法与匹配的名称的现有属性 （卷影属性或其中一个实体类上定义），则代码将配置该现有属性，而不是引入一个新的卷影属性。</span><span class="sxs-lookup"><span data-stu-id="34769-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

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
