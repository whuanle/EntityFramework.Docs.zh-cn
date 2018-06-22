---
title: 关系的 EF 核心
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
# <a name="relationships"></a><span data-ttu-id="814ca-102">关系</span><span class="sxs-lookup"><span data-stu-id="814ca-102">Relationships</span></span>

<span data-ttu-id="814ca-103">关系定义了两个实体将互相关联起来。</span><span class="sxs-lookup"><span data-stu-id="814ca-103">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="814ca-104">在关系数据库中，它被表示通过外键约束。</span><span class="sxs-lookup"><span data-stu-id="814ca-104">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="814ca-105">大部分这篇文章中的示例使用一个对多关系来演示概念。</span><span class="sxs-lookup"><span data-stu-id="814ca-105">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="814ca-106">有关一对一和多对多关系的示例，请参阅[其他关系模式](#other-relationship-patterns)文章结尾部分。</span><span class="sxs-lookup"><span data-stu-id="814ca-106">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="814ca-107">术语的定义</span><span class="sxs-lookup"><span data-stu-id="814ca-107">Definition of Terms</span></span>

<span data-ttu-id="814ca-108">有大量用于描述关系的术语</span><span class="sxs-lookup"><span data-stu-id="814ca-108">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="814ca-109">**依赖的实体：** 这是包含外键属性的实体。</span><span class="sxs-lookup"><span data-stu-id="814ca-109">**Dependent entity:** This is the entity that contains the foreign key property(s).</span></span> <span data-ttu-id="814ca-110">有时称为 child 的关系。</span><span class="sxs-lookup"><span data-stu-id="814ca-110">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="814ca-111">**主体实体：** 这是包含主/备用键属性的实体。</span><span class="sxs-lookup"><span data-stu-id="814ca-111">**Principal entity:** This is the entity that contains the primary/alternate key property(s).</span></span> <span data-ttu-id="814ca-112">有时称为 'parent' 的关系。</span><span class="sxs-lookup"><span data-stu-id="814ca-112">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="814ca-113">**外键：** 中用于存储与相关实体的主体键属性的值的依赖实体属性。</span><span class="sxs-lookup"><span data-stu-id="814ca-113">**Foreign key:** The property(s) in the dependent entity that is used to store the values of the principal key property that the entity is related to.</span></span>

* <span data-ttu-id="814ca-114">**主体的密钥：** 唯一标识的主体实体属性。</span><span class="sxs-lookup"><span data-stu-id="814ca-114">**Principal key:** The property(s) that uniquely identifies the principal entity.</span></span> <span data-ttu-id="814ca-115">这可能是 primary key 或备用密钥。</span><span class="sxs-lookup"><span data-stu-id="814ca-115">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="814ca-116">**导航属性：** 包含对相关实体引用的主体和/或依赖实体上定义的属性。</span><span class="sxs-lookup"><span data-stu-id="814ca-116">**Navigation property:** A property defined on the principal and/or dependent entity that contains a reference(s) to the related entity(s).</span></span>

  * <span data-ttu-id="814ca-117">**集合导航属性：** 一个导航属性，包含对多个相关实体的引用。</span><span class="sxs-lookup"><span data-stu-id="814ca-117">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="814ca-118">**引用导航属性：** 一个导航属性，保存对单个相关实体的引用。</span><span class="sxs-lookup"><span data-stu-id="814ca-118">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="814ca-119">**反向导航属性：** 在讨论特定导航属性时，此术语是指关系另一端的导航属性。</span><span class="sxs-lookup"><span data-stu-id="814ca-119">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>

<span data-ttu-id="814ca-120">下面的代码清单显示之间的一个对多关系`Blog`和`Post`</span><span class="sxs-lookup"><span data-stu-id="814ca-120">The following code listing shows a one-to-many relationship between `Blog` and `Post`</span></span>

* <span data-ttu-id="814ca-121">`Post`是依赖实体</span><span class="sxs-lookup"><span data-stu-id="814ca-121">`Post` is the dependent entity</span></span>

* <span data-ttu-id="814ca-122">`Blog`是主体实体</span><span class="sxs-lookup"><span data-stu-id="814ca-122">`Blog` is the principal entity</span></span>

* <span data-ttu-id="814ca-123">`Post.BlogId`是外键</span><span class="sxs-lookup"><span data-stu-id="814ca-123">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="814ca-124">`Blog.BlogId`是 （在这种情况下它是主键，而不是备用密钥） 的主体键</span><span class="sxs-lookup"><span data-stu-id="814ca-124">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="814ca-125">`Post.Blog`是引用导航属性</span><span class="sxs-lookup"><span data-stu-id="814ca-125">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="814ca-126">`Blog.Posts`是集合导航属性</span><span class="sxs-lookup"><span data-stu-id="814ca-126">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="814ca-127">`Post.Blog`是的反向导航属性`Blog.Posts`（反之亦然）</span><span class="sxs-lookup"><span data-stu-id="814ca-127">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a><span data-ttu-id="814ca-128">约定</span><span class="sxs-lookup"><span data-stu-id="814ca-128">Conventions</span></span>

<span data-ttu-id="814ca-129">按照约定，发现的类型上的导航属性时，将创建关系。</span><span class="sxs-lookup"><span data-stu-id="814ca-129">By convention, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="814ca-130">如果它指向的类型不能将映射为标量类型由当前的数据库提供程序，该属性被视为一个导航属性。</span><span class="sxs-lookup"><span data-stu-id="814ca-130">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="814ca-131">关系发现的约定将始终以目标主体实体的主键。</span><span class="sxs-lookup"><span data-stu-id="814ca-131">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="814ca-132">若要针对备用密钥，必须使用 Fluent API 执行其他配置。</span><span class="sxs-lookup"><span data-stu-id="814ca-132">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="814ca-133">完全定义的关系</span><span class="sxs-lookup"><span data-stu-id="814ca-133">Fully Defined Relationships</span></span>

<span data-ttu-id="814ca-134">关系的最常见模式是能够在两端的关系和依赖的实体类中定义的外键属性上定义的导航属性。</span><span class="sxs-lookup"><span data-stu-id="814ca-134">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="814ca-135">如果两个类型之间找到一对导航属性，则它们将可配置为位于同一关系的反向导航属性。</span><span class="sxs-lookup"><span data-stu-id="814ca-135">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="814ca-136">如果依赖实体包含名为的属性`<primary key property name>`， `<navigation property name><primary key property name>`，或`<principal entity name><primary key property name>`然后会将其配置为外键。</span><span class="sxs-lookup"><span data-stu-id="814ca-136">If the dependent entity contains a property named `<primary key property name>`, `<navigation property name><primary key property name>`, or `<principal entity name><primary key property name>` then it will be configured as the foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> <span data-ttu-id="814ca-137">如果有多个定义两个类型之间的导航属性 (即指向对方的导航的多个非重复对)、 约定将然后创建任何关系，并且将需要手动配置它们以确定如何导航属性对长达。</span><span class="sxs-lookup"><span data-stu-id="814ca-137">If there are multiple navigation properties defined between two types (i.e. more than one distinct pair of navigations that point to each other), then no relationships will be created by convention and you will need to manually configure them to identify how the navigation properties pair up.</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="814ca-138">没有外键属性</span><span class="sxs-lookup"><span data-stu-id="814ca-138">No Foreign Key Property</span></span>

<span data-ttu-id="814ca-139">尽管建议具有外键属性的依赖的实体类中定义，则不需要。</span><span class="sxs-lookup"><span data-stu-id="814ca-139">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="814ca-140">如果不找到任何外键属性，则卷影的外键属性，将名称引入`<navigation property name><principal key property name>`(请参阅[隐藏属性](shadow-properties.md)有关详细信息)。</span><span class="sxs-lookup"><span data-stu-id="814ca-140">If no foreign key property is found, a shadow foreign key property will be introduced with the name `<navigation property name><principal key property name>` (see [Shadow Properties](shadow-properties.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a><span data-ttu-id="814ca-141">一个导航属性</span><span class="sxs-lookup"><span data-stu-id="814ca-141">Single Navigation Property</span></span>

<span data-ttu-id="814ca-142">包括一个导航属性 （没有反向导航和没有外键属性） 已足够具有关系定义的约定。</span><span class="sxs-lookup"><span data-stu-id="814ca-142">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="814ca-143">你还可以单个导航属性和外键属性。</span><span class="sxs-lookup"><span data-stu-id="814ca-143">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a><span data-ttu-id="814ca-144">级联删除</span><span class="sxs-lookup"><span data-stu-id="814ca-144">Cascade Delete</span></span>

<span data-ttu-id="814ca-145">按照约定，级联删除将设置为*Cascade*的所需的关系和*ClientSetNull*可选关系。</span><span class="sxs-lookup"><span data-stu-id="814ca-145">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="814ca-146">*级联*意味着依赖实体也会被删除。</span><span class="sxs-lookup"><span data-stu-id="814ca-146">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="814ca-147">*ClientSetNull*意味着不会加载到内存的依赖实体仍保持不变并且必须是手动将其删除，或更新以指向有效的主体实体。</span><span class="sxs-lookup"><span data-stu-id="814ca-147">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="814ca-148">对于加载到内存的实体，EF 核心将尝试将外键属性设置为 null。</span><span class="sxs-lookup"><span data-stu-id="814ca-148">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="814ca-149">请参阅[必选和可选关系](#required-and-optional-relationships)部分以了解必需和可选关系之间的差异。</span><span class="sxs-lookup"><span data-stu-id="814ca-149">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="814ca-150">请参阅[级联删除](../saving/cascade-delete.md)更多详细信息的不同删除行为和约定所使用的默认值。</span><span class="sxs-lookup"><span data-stu-id="814ca-150">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="814ca-151">数据注释</span><span class="sxs-lookup"><span data-stu-id="814ca-151">Data Annotations</span></span>

<span data-ttu-id="814ca-152">有两个用于配置关系的数据注释`[ForeignKey]`和`[InverseProperty]`。</span><span class="sxs-lookup"><span data-stu-id="814ca-152">There are two data annotations that can be used to configure relationships, `[ForeignKey]` and `[InverseProperty]`.</span></span>

### <a name="foreignkey"></a><span data-ttu-id="814ca-153">[ForeignKey]</span><span class="sxs-lookup"><span data-stu-id="814ca-153">[ForeignKey]</span></span>

<span data-ttu-id="814ca-154">数据注释可用于配置哪些属性应用作给定关系的外键属性。</span><span class="sxs-lookup"><span data-stu-id="814ca-154">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="814ca-155">这通常是约定没有发现外键属性。</span><span class="sxs-lookup"><span data-stu-id="814ca-155">This is typically done when the foreign key property is not discovered by convention.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?name=Entities&highlight=17)]

> [!TIP]  
> <span data-ttu-id="814ca-156">`[ForeignKey]`批注可以放置在关系中这两个导航属性。</span><span class="sxs-lookup"><span data-stu-id="814ca-156">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="814ca-157">它不需要继续依赖的实体类中的导航属性。</span><span class="sxs-lookup"><span data-stu-id="814ca-157">It does not need to go on the navigation property in the dependent entity class.</span></span>

### <a name="inverseproperty"></a><span data-ttu-id="814ca-158">[InverseProperty]</span><span class="sxs-lookup"><span data-stu-id="814ca-158">[InverseProperty]</span></span>

<span data-ttu-id="814ca-159">数据注释可用于配置如何依赖和主体实体的导航属性配对。</span><span class="sxs-lookup"><span data-stu-id="814ca-159">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="814ca-160">这通常是多个对两个实体类型之间的导航属性时。</span><span class="sxs-lookup"><span data-stu-id="814ca-160">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?name=Entities&highlight=20,23)]

## <a name="fluent-api"></a><span data-ttu-id="814ca-161">Fluent API</span><span class="sxs-lookup"><span data-stu-id="814ca-161">Fluent API</span></span>

<span data-ttu-id="814ca-162">若要配置关系 Fluent API 中，启动时通过标识构成关系的导航属性。</span><span class="sxs-lookup"><span data-stu-id="814ca-162">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="814ca-163">`HasOne`或`HasMany`标识你正在开始配置的实体类型上的导航属性。</span><span class="sxs-lookup"><span data-stu-id="814ca-163">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="814ca-164">然后链接到调用`WithOne`或`WithMany`来标识反向导航。</span><span class="sxs-lookup"><span data-stu-id="814ca-164">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="814ca-165">`HasOne`/`WithOne`用于引用导航属性和`HasMany` / `WithMany`用于集合导航属性。</span><span class="sxs-lookup"><span data-stu-id="814ca-165">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?name=Model&highlight=8,9,10)]

### <a name="single-navigation-property"></a><span data-ttu-id="814ca-166">一个导航属性</span><span class="sxs-lookup"><span data-stu-id="814ca-166">Single Navigation Property</span></span>

<span data-ttu-id="814ca-167">如果你只有一个导航属性，则存在一些无参数重载`WithOne`和`WithMany`。</span><span class="sxs-lookup"><span data-stu-id="814ca-167">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="814ca-168">这表示没有从概念上讲引用或集合上的另一端的关系，但不未包含在实体类中任何导航属性。</span><span class="sxs-lookup"><span data-stu-id="814ca-168">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?name=Model&highlight=10)]

### <a name="foreign-key"></a><span data-ttu-id="814ca-169">外键</span><span class="sxs-lookup"><span data-stu-id="814ca-169">Foreign Key</span></span>

<span data-ttu-id="814ca-170">Fluent API 可用于配置哪些属性应用作给定关系的外键属性。</span><span class="sxs-lookup"><span data-stu-id="814ca-170">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?name=Model&highlight=11)]

<span data-ttu-id="814ca-171">以下代码列表演示如何配置的组合外键。</span><span class="sxs-lookup"><span data-stu-id="814ca-171">The following code listing shows how to configure a composite foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?name=Model&highlight=13)]

<span data-ttu-id="814ca-172">你可以使用的字符串重载`HasForeignKey(...)`卷影属性配置为外键 (请参阅[隐藏属性](shadow-properties.md)有关详细信息)。</span><span class="sxs-lookup"><span data-stu-id="814ca-172">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="814ca-173">建议显式将卷影属性添加到模型，然后再使用它作为外键 （如下所示）。</span><span class="sxs-lookup"><span data-stu-id="814ca-173">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="principal-key"></a><span data-ttu-id="814ca-174">主体密钥</span><span class="sxs-lookup"><span data-stu-id="814ca-174">Principal Key</span></span>

<span data-ttu-id="814ca-175">如果你想要引用以外的主键属性的外键，可以使用 Fluent API 来配置该关系的主体键属性。</span><span class="sxs-lookup"><span data-stu-id="814ca-175">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="814ca-176">自动配置为该主体的密钥将该属性是作为备用键的安装程序 (请参阅[备用键](alternate-keys.md)有关详细信息)。</span><span class="sxs-lookup"><span data-stu-id="814ca-176">The property that you configure as the principal key will automatically be setup as an alternate key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

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

<span data-ttu-id="814ca-177">以下代码列表演示如何配置主体组合键。</span><span class="sxs-lookup"><span data-stu-id="814ca-177">The following code listing shows how to configure a composite principal key.</span></span>

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
> <span data-ttu-id="814ca-178">指定主体键属性的顺序必须与匹配外键指定的顺序。</span><span class="sxs-lookup"><span data-stu-id="814ca-178">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

### <a name="required-and-optional-relationships"></a><span data-ttu-id="814ca-179">必选和可选的关系</span><span class="sxs-lookup"><span data-stu-id="814ca-179">Required and Optional Relationships</span></span>

<span data-ttu-id="814ca-180">Fluent API 可用于配置关系为必需或可选。</span><span class="sxs-lookup"><span data-stu-id="814ca-180">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="814ca-181">最终此参数控制外键属性是必需还是可选。</span><span class="sxs-lookup"><span data-stu-id="814ca-181">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="814ca-182">使用卷影状态外键时，这是最有用。</span><span class="sxs-lookup"><span data-stu-id="814ca-182">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="814ca-183">如果你有在实体类中的外键属性，则关系的 requiredness 确定基于外键属性是必需还是可选 (请参阅[必选和可选属性](required-optional.md)的详细信息信息）。</span><span class="sxs-lookup"><span data-stu-id="814ca-183">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

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

### <a name="cascade-delete"></a><span data-ttu-id="814ca-184">级联删除</span><span class="sxs-lookup"><span data-stu-id="814ca-184">Cascade Delete</span></span>

<span data-ttu-id="814ca-185">Fluent API 可用于显式配置为某个给定的关系的级联删除行为。</span><span class="sxs-lookup"><span data-stu-id="814ca-185">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="814ca-186">请参阅[级联删除](../saving/cascade-delete.md)上保存的数据部分中有关每个选项的详细讨论。</span><span class="sxs-lookup"><span data-stu-id="814ca-186">See [Cascade Delete](../saving/cascade-delete.md) on the Saving Data section for a detailed discussion of each option.</span></span>

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

## <a name="other-relationship-patterns"></a><span data-ttu-id="814ca-187">其他关系模式</span><span class="sxs-lookup"><span data-stu-id="814ca-187">Other Relationship Patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="814ca-188">一对一</span><span class="sxs-lookup"><span data-stu-id="814ca-188">One-to-one</span></span>

<span data-ttu-id="814ca-189">一对一关系两端具有引用导航属性。</span><span class="sxs-lookup"><span data-stu-id="814ca-189">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="814ca-190">它们遵循相同的约定一个对多关系，但在外键属性，以确保只有一个依赖于与每个主体引入唯一索引。</span><span class="sxs-lookup"><span data-stu-id="814ca-190">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

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
> <span data-ttu-id="814ca-191">EF 将选择一个要为基于其能够检测到外键属性的依赖项的实体。</span><span class="sxs-lookup"><span data-stu-id="814ca-191">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="814ca-192">如果错误实体被选作依赖项，可以使用 Fluent API 要更正此问题。</span><span class="sxs-lookup"><span data-stu-id="814ca-192">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="814ca-193">如果配置使用 Fluent API 的关系，则使用`HasOne`和`WithOne`方法。</span><span class="sxs-lookup"><span data-stu-id="814ca-193">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="814ca-194">配置你需要指定相关的实体类型中的外键时请注意到提供的泛型参数`HasForeignKey`下面的列表中。</span><span class="sxs-lookup"><span data-stu-id="814ca-194">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="814ca-195">一个对多关系中很明显的实体引用导航是依赖项和一个具有集合是的主体。</span><span class="sxs-lookup"><span data-stu-id="814ca-195">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="814ca-196">但这不是这样一对一关系-因此中显式定义的需要。</span><span class="sxs-lookup"><span data-stu-id="814ca-196">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

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

### <a name="many-to-many"></a><span data-ttu-id="814ca-197">多对多</span><span class="sxs-lookup"><span data-stu-id="814ca-197">Many-to-many</span></span>

<span data-ttu-id="814ca-198">尚不支持而不需要的实体类来表示联接表的多对多关系。</span><span class="sxs-lookup"><span data-stu-id="814ca-198">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="814ca-199">但是，您可以通过包括联接表和映射两个单独的一对多关系的实体类表示多对多关系。</span><span class="sxs-lookup"><span data-stu-id="814ca-199">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

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
