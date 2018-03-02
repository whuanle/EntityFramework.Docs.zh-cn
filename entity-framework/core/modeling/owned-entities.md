---
title: "拥有 EF 核心的实体类型-"
author: julielerman
ms.author: divega
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
ms.technology: entity-framework-core
uid: core/modeling/owned-entities
ms.openlocfilehash: a6823377eb626ca92263c31351e1aef61db5a787
ms.sourcegitcommit: 4b7d3d3e258b0d9cb778bb45a9f4a33c0792e38e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2018
---
# <a name="owned-entity-types"></a><span data-ttu-id="3ed99-102">拥有的实体类型</span><span class="sxs-lookup"><span data-stu-id="3ed99-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="3ed99-103">此功能是在 EF 核心 2.0 中的新增功能。</span><span class="sxs-lookup"><span data-stu-id="3ed99-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="3ed99-104">EF 核心让你可以只显示对其他实体类型的导航属性的模型实体类型。</span><span class="sxs-lookup"><span data-stu-id="3ed99-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="3ed99-105">这些应用程序称为_拥有实体类型_。</span><span class="sxs-lookup"><span data-stu-id="3ed99-105">These are called _owned entity types_.</span></span> <span data-ttu-id="3ed99-106">包含拥有的实体类型的实体是其_所有者_。</span><span class="sxs-lookup"><span data-stu-id="3ed99-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="3ed99-107">显式配置</span><span class="sxs-lookup"><span data-stu-id="3ed99-107">Explicit configuration</span></span>

<span data-ttu-id="3ed99-108">拥有永远不会包含类型由 EF 核心模型中按照约定的实体。</span><span class="sxs-lookup"><span data-stu-id="3ed99-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="3ed99-109">你可以使用`OwnsOne`中的方法`OnModelCreating`或批注与类型`OwnedAttrbibute`（新在 EF 核心 2.1） 配置为拥有类型的类型。</span><span class="sxs-lookup"><span data-stu-id="3ed99-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttrbibute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="3ed99-110">在此示例中，StreetAddress 是没有标识属性的类型。</span><span class="sxs-lookup"><span data-stu-id="3ed99-110">In this example, StreetAddress is a type with no identity property.</span></span> <span data-ttu-id="3ed99-111">它用作 Order 类型的属性来指定特定订单的发货地址。</span><span class="sxs-lookup"><span data-stu-id="3ed99-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span> <span data-ttu-id="3ed99-112">在`OnModelCreating`，我们使用`OwnsOne`方法，以指定 ShippingAddress 属性是拥有顺序类型的实体。</span><span class="sxs-lookup"><span data-stu-id="3ed99-112">In `OnModelCreating`, we use the `OwnsOne` method to specify that the ShippingAddress property is an Owned Entity of the Order type.</span></span>

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

// OnModelCreating
modelBuilder.Entity<Order>().OwnsOne(p => p.ShippingAddress);
```

<span data-ttu-id="3ed99-113">如果该 ShippingAddress 属性是在顺序类型中为私有，则可以使用的字符串版本`OwnsOne`方法：</span><span class="sxs-lookup"><span data-stu-id="3ed99-113">If the ShippingAddress property is private in the Order type, you can use the string version of the `OwnsOne` method:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

<span data-ttu-id="3ed99-114">在此示例中，我们使用`OwnedAttribute`来实现相同的目的：</span><span class="sxs-lookup"><span data-stu-id="3ed99-114">In this example, we use the `OwnedAttribute` to achieve the same goal:</span></span>

``` csharp
[Owned]
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}
```

## <a name="implicit-keys"></a><span data-ttu-id="3ed99-115">隐式键</span><span class="sxs-lookup"><span data-stu-id="3ed99-115">Implicit keys</span></span>

<span data-ttu-id="3ed99-116">在 EF 核心 2.0 和 2.1 中，仅引用导航属性可以指向拥有的类型。</span><span class="sxs-lookup"><span data-stu-id="3ed99-116">In EF Core 2.0 and 2.1, only reference navigation properties can point to owned types.</span></span> <span data-ttu-id="3ed99-117">不支持拥有类型的集合。</span><span class="sxs-lookup"><span data-stu-id="3ed99-117">Collections of owned types are not supported.</span></span> <span data-ttu-id="3ed99-118">这些引用拥有类型始终与所有者具有一对一的关系，因此它们不需要其自己的密钥值。</span><span class="sxs-lookup"><span data-stu-id="3ed99-118">These reference owned types always have a one-to-one relationship with the owner, therefore they don't need their own key values.</span></span> <span data-ttu-id="3ed99-119">在前面的示例中，不需要定义键属性 StreetAddress 类型。</span><span class="sxs-lookup"><span data-stu-id="3ed99-119">In the previous example, the StreetAddress type does not need to define a key property.</span></span>  

<span data-ttu-id="3ed99-120">在了解如何 EF 核心跟踪这些对象的顺序，有必要考虑主密钥被创建为[隐藏属性](xref:core/modeling/shadow-properties)拥有的类型。</span><span class="sxs-lookup"><span data-stu-id="3ed99-120">In order to understanding how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="3ed99-121">拥有类型的实例键的值将作为所有者实例的密钥的值相同。</span><span class="sxs-lookup"><span data-stu-id="3ed99-121">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>      

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="3ed99-122">映射拥有与表拆分的类型</span><span class="sxs-lookup"><span data-stu-id="3ed99-122">Mapping owned types with table splitting</span></span>

<span data-ttu-id="3ed99-123">在使用关系数据库时，按照约定拥有类型映射到与所有者相同的表。</span><span class="sxs-lookup"><span data-stu-id="3ed99-123">When using relational databases, by convention owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="3ed99-124">这要求拆分成两个表： 某些列将用于存储的所有者，数据和某些列将用于存储数据的拥有的实体。</span><span class="sxs-lookup"><span data-stu-id="3ed99-124">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="3ed99-125">这是一个称作表拆分的常见功能。</span><span class="sxs-lookup"><span data-stu-id="3ed99-125">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="3ed99-126">拥有表拆分与存储的类型可以是使用非常类似于复杂类型中使用 ef6 更高版本。</span><span class="sxs-lookup"><span data-stu-id="3ed99-126">Owned types stored with table splitting can be used very similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="3ed99-127">按照约定，EF 核心将遵循模式在拥有的实体类型的属性的名称的数据库列_EntityProperty_OwnedEntityProperty_。</span><span class="sxs-lookup"><span data-stu-id="3ed99-127">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _EntityProperty_OwnedEntityProperty_.</span></span> <span data-ttu-id="3ed99-128">因此 StreetAddress 属性将显示在具有名 ShippingAddress_Street 和 ShippingAddress_City Orders 表中。</span><span class="sxs-lookup"><span data-stu-id="3ed99-128">Therefore the StreetAddress properties will appear in the Orders table with the names ShippingAddress_Street and ShippingAddress_City.</span></span>

<span data-ttu-id="3ed99-129">你可以将附加`HasColumnName`方法来重命名这些列。</span><span class="sxs-lookup"><span data-stu-id="3ed99-129">You can append the `HasColumnName` method to rename those columns.</span></span> <span data-ttu-id="3ed99-130">在其中 StreetAddress 是公共属性的情况下，映射将是</span><span class="sxs-lookup"><span data-stu-id="3ed99-130">In the case where StreetAddress is a public property, the mappings would be</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="3ed99-131">共享多个类型中拥有相同的.NET 类型</span><span class="sxs-lookup"><span data-stu-id="3ed99-131">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="3ed99-132">拥有的实体类型可以是相同的.NET 类型作为另一个拥有的实体类型，因此.NET 类型可能不够来标识拥有的类型。</span><span class="sxs-lookup"><span data-stu-id="3ed99-132">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="3ed99-133">在这些情况下，从所有者指向拥有的实体的属性将成为_定义导航_拥有的实体类型。</span><span class="sxs-lookup"><span data-stu-id="3ed99-133">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="3ed99-134">从 EF 核心的角度来看，定义导航是标识的旁边的.NET 类型的类型的一部分。</span><span class="sxs-lookup"><span data-stu-id="3ed99-134">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="3ed99-135">例如，在以下类中，ShippingAddress 和 BillingAddress 都相同的.NET 类型，StreetAddress:</span><span class="sxs-lookup"><span data-stu-id="3ed99-135">For example, in the following class, ShippingAddress and BillingAddress are both of the same .NET type, StreetAddress:</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

<span data-ttu-id="3ed99-136">若要了解如何 EF 核心将区分跟踪这些对象的实例，可能会有用考虑定义导航已旁边的值的键的所有者的实例键的一部分，拥有类型的.NET 类型。</span><span class="sxs-lookup"><span data-stu-id="3ed99-136">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="3ed99-137">嵌套拥有的类型</span><span class="sxs-lookup"><span data-stu-id="3ed99-137">Nested owned types</span></span>

<span data-ttu-id="3ed99-138">在此示例中 OrderDetails 拥有 BillingAddress 和 ShippingAddress，它们是两个 StreetAddress 类型。</span><span class="sxs-lookup"><span data-stu-id="3ed99-138">In this example OrderDetails owns BillingAddress and ShippingAddress, which are both StreetAddress types.</span></span> <span data-ttu-id="3ed99-139">然后 OrderDetails 归顺序类型。</span><span class="sxs-lookup"><span data-stu-id="3ed99-139">Then OrderDetails is owned by the Order type.</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public OrderDetails OrderDetails { get; set; }
    public OrderStatus Status { get; set; }
}

public class OrderDetails
{
    public StreetAddress BillingAddress { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```

<span data-ttu-id="3ed99-140">可以链接到`OwnsOne`fluent 映射以配置此模型中的方法：</span><span class="sxs-lookup"><span data-stu-id="3ed99-140">It is possible to chain the `OwnsOne` method in a fluent mapping to configure this model:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

<span data-ttu-id="3ed99-141">它是可能实现相同的操作使用`OwnedAttribute`OrderDetails 和 StreetAdress 上。</span><span class="sxs-lookup"><span data-stu-id="3ed99-141">It is possible to achieve the same thing using `OwnedAttribute` on both OrderDetails and StreetAdress.</span></span>

<span data-ttu-id="3ed99-142">嵌套拥有除类型之外，拥有的类型可以引用常规实体。</span><span class="sxs-lookup"><span data-stu-id="3ed99-142">In addition to nested owned types, an owned type can reference a regular entity.</span></span> <span data-ttu-id="3ed99-143">在下面的示例中，国家/地区是常规的 （即非自有） 实体：</span><span class="sxs-lookup"><span data-stu-id="3ed99-143">In the following example, Country is a regular (i.e. non-owned) entity:</span></span>

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

<span data-ttu-id="3ed99-144">此功能设置以从 EF6 脱离复杂类型拥有的实体类型。</span><span class="sxs-lookup"><span data-stu-id="3ed99-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="3ed99-145">存储拥有单独的表中的类型</span><span class="sxs-lookup"><span data-stu-id="3ed99-145">Storing owned types in separate tables</span></span>

<span data-ttu-id="3ed99-146">也拥有的类型从 EF6 复杂与类型不同，它可以存储在所有者从单独的表。</span><span class="sxs-lookup"><span data-stu-id="3ed99-146">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="3ed99-147">若要重写将拥有的类型映射到与所有者相同的表的约定，你可以轻松调用`ToTable`并提供不同的表名。</span><span class="sxs-lookup"><span data-stu-id="3ed99-147">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="3ed99-148">下面的示例将 OrderDetails 和其两个地址映射到单独的表中的顺序：</span><span class="sxs-lookup"><span data-stu-id="3ed99-148">The following example will map OrderDetails and its two addresses to a separate table from Order:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a><span data-ttu-id="3ed99-149">查询拥有的类型</span><span class="sxs-lookup"><span data-stu-id="3ed99-149">Querying owned types</span></span>

<span data-ttu-id="3ed99-150">查询所有者时，固有类型将默认包含在内。</span><span class="sxs-lookup"><span data-stu-id="3ed99-150">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="3ed99-151">不需要使用`Include`方法，即使拥有的类型存储在单独的表。</span><span class="sxs-lookup"><span data-stu-id="3ed99-151">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="3ed99-152">根据之前所述的模型，以下查询将拉取顺序、 OrderDetails 和从数据库的所有挂起订单两个拥有的 StreeAddresses:</span><span class="sxs-lookup"><span data-stu-id="3ed99-152">Based on the model described before, the following query will pull Order, OrderDetails and the two owned StreeAddresses for all pending orders from the database:</span></span>

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a><span data-ttu-id="3ed99-153">限制</span><span class="sxs-lookup"><span data-stu-id="3ed99-153">Limitations</span></span>

<span data-ttu-id="3ed99-154">以下是拥有的实体类型的一些限制。</span><span class="sxs-lookup"><span data-stu-id="3ed99-154">Here are some limitations of owned entity types.</span></span> <span data-ttu-id="3ed99-155">这些限制一些基本到如何拥有类型的工作，但某些其他属性我们想要删除在将来版本的时间点限制：</span><span class="sxs-lookup"><span data-stu-id="3ed99-155">Some of these limitations are fundamental to how owned types work, but some others are point-in-time restrictions that we would like to remove in future releases:</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="3ed99-156">当前不足之处</span><span class="sxs-lookup"><span data-stu-id="3ed99-156">Current shortcomings</span></span>
- <span data-ttu-id="3ed99-157">不支持的所有类型的继承</span><span class="sxs-lookup"><span data-stu-id="3ed99-157">Inheritance of owned types is not supported</span></span>
- <span data-ttu-id="3ed99-158">不能由集合导航属性指向拥有的类型</span><span class="sxs-lookup"><span data-stu-id="3ed99-158">Owned types cannot be pointed at by a collection navigation property</span></span>
- <span data-ttu-id="3ed99-159">因为它们使用表分割默认拥有类型还具有以下限制，除非显式映射到另一个表：</span><span class="sxs-lookup"><span data-stu-id="3ed99-159">Since they use table splitting by default owned types also have the following restrictions unless explicitly mapped to a different table:</span></span>
   - <span data-ttu-id="3ed99-160">它们不能拥有由派生类型</span><span class="sxs-lookup"><span data-stu-id="3ed99-160">They cannot be owned by a derived type</span></span>
   - <span data-ttu-id="3ed99-161">定义导航属性不能设置为 null （即拥有对同一个表的类型并总是必需的）</span><span class="sxs-lookup"><span data-stu-id="3ed99-161">The defining navigation property cannot be set to null (i.e. owned types on the same table are always required)</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="3ed99-162">按设计限制</span><span class="sxs-lookup"><span data-stu-id="3ed99-162">By-design restrictions</span></span>
- <span data-ttu-id="3ed99-163">无法创建 `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="3ed99-163">You cannot create a `DbSet<T>`</span></span>
- <span data-ttu-id="3ed99-164">不能调用`Entity<T>()`拥有类型 `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="3ed99-164">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>
