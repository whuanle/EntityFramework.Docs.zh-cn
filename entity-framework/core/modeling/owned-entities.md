---
title: 拥有 EF Core 的实体类型-
author: julielerman
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: 1104a8a9a4540e33624fad69c47f2f950c6669bf
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489402"
---
# <a name="owned-entity-types"></a><span data-ttu-id="3a706-102">固有的实体类型</span><span class="sxs-lookup"><span data-stu-id="3a706-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="3a706-103">此功能是在 EF Core 2.0 中的新增功能。</span><span class="sxs-lookup"><span data-stu-id="3a706-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="3a706-104">EF Core 让你可以只显示对其他实体类型的导航属性的模型实体类型。</span><span class="sxs-lookup"><span data-stu-id="3a706-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="3a706-105">这些被称为_固有实体类型_。</span><span class="sxs-lookup"><span data-stu-id="3a706-105">These are called _owned entity types_.</span></span> <span data-ttu-id="3a706-106">包含固有的实体类型的实体是其_所有者_。</span><span class="sxs-lookup"><span data-stu-id="3a706-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="3a706-107">显式配置</span><span class="sxs-lookup"><span data-stu-id="3a706-107">Explicit configuration</span></span>

<span data-ttu-id="3a706-108">拥有永远不会包含类型由 EF Core 模型中按照约定的实体。</span><span class="sxs-lookup"><span data-stu-id="3a706-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="3a706-109">你可以使用`OwnsOne`中的方法`OnModelCreating`或批注与类型`OwnedAttribute`（新在 EF Core 2.1） 配置为拥有类型的类型。</span><span class="sxs-lookup"><span data-stu-id="3a706-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="3a706-110">在此示例中，StreetAddress 是没有标识属性的类型。</span><span class="sxs-lookup"><span data-stu-id="3a706-110">In this example, StreetAddress is a type with no identity property.</span></span> <span data-ttu-id="3a706-111">它用作 Order 类型的属性来指定特定订单的发货地址。</span><span class="sxs-lookup"><span data-stu-id="3a706-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span> <span data-ttu-id="3a706-112">在中`OnModelCreating`，我们使用`OwnsOne`方法，以指定 ShippingAddress 属性是拥有订单类型的实体。</span><span class="sxs-lookup"><span data-stu-id="3a706-112">In `OnModelCreating`, we use the `OwnsOne` method to specify that the ShippingAddress property is an Owned Entity of the Order type.</span></span>

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

<span data-ttu-id="3a706-113">如果 ShippingAddress 属性是私有的订单类型，则可以使用的字符串版本`OwnsOne`方法：</span><span class="sxs-lookup"><span data-stu-id="3a706-113">If the ShippingAddress property is private in the Order type, you can use the string version of the `OwnsOne` method:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

<span data-ttu-id="3a706-114">在此示例中，我们使用`OwnedAttribute`来实现相同的目的：</span><span class="sxs-lookup"><span data-stu-id="3a706-114">In this example, we use the `OwnedAttribute` to achieve the same goal:</span></span>

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

## <a name="implicit-keys"></a><span data-ttu-id="3a706-115">隐式键</span><span class="sxs-lookup"><span data-stu-id="3a706-115">Implicit keys</span></span>

<span data-ttu-id="3a706-116">在 EF Core 2.0 和 2.1 中，仅引用导航属性可以指向拥有的类型。</span><span class="sxs-lookup"><span data-stu-id="3a706-116">In EF Core 2.0 and 2.1, only reference navigation properties can point to owned types.</span></span> <span data-ttu-id="3a706-117">不支持的固有类型的集合。</span><span class="sxs-lookup"><span data-stu-id="3a706-117">Collections of owned types are not supported.</span></span> <span data-ttu-id="3a706-118">拥有这些引用的类型与所有者始终具有一对一的关系，因此他们并不需要其自己的密钥值。</span><span class="sxs-lookup"><span data-stu-id="3a706-118">These reference owned types always have a one-to-one relationship with the owner, therefore they don't need their own key values.</span></span> <span data-ttu-id="3a706-119">在上一示例中，不需要定义键属性的 StreetAddress 类型。</span><span class="sxs-lookup"><span data-stu-id="3a706-119">In the previous example, the StreetAddress type does not need to define a key property.</span></span>  

<span data-ttu-id="3a706-120">若要了解如何 EF Core 可跟踪这些对象，最好先认为，作为创建主键[阴影属性](xref:core/modeling/shadow-properties)对于固有类型。</span><span class="sxs-lookup"><span data-stu-id="3a706-120">In order to understand how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="3a706-121">固有类型的实例键的值将为所有者实例的键的值相同。</span><span class="sxs-lookup"><span data-stu-id="3a706-121">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>      

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="3a706-122">映射固有类型与表拆分</span><span class="sxs-lookup"><span data-stu-id="3a706-122">Mapping owned types with table splitting</span></span>

<span data-ttu-id="3a706-123">使用关系数据库时，按照约定，固有类型映射到与所有者相同的表。</span><span class="sxs-lookup"><span data-stu-id="3a706-123">When using relational databases, by convention owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="3a706-124">这要求将在两个表拆分： 某些列将用于存储数据的所有者，并且某些列将用于存储数据的固有实体。</span><span class="sxs-lookup"><span data-stu-id="3a706-124">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="3a706-125">这是一项称为表拆分的常见功能。</span><span class="sxs-lookup"><span data-stu-id="3a706-125">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="3a706-126">拥有表拆分与存储在一起的类型可以是到复杂类型中使用 EF6 的使用非常类似。</span><span class="sxs-lookup"><span data-stu-id="3a706-126">Owned types stored with table splitting can be used very similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="3a706-127">按照约定，EF Core 将遵循模式在拥有的实体类型的属性的名称的数据库列_EntityProperty_OwnedEntityProperty_。</span><span class="sxs-lookup"><span data-stu-id="3a706-127">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _EntityProperty_OwnedEntityProperty_.</span></span> <span data-ttu-id="3a706-128">因此 StreetAddress 属性会在具有名称 ShippingAddress_Street 和 ShippingAddress_City 订单表中显示。</span><span class="sxs-lookup"><span data-stu-id="3a706-128">Therefore the StreetAddress properties will appear in the Orders table with the names ShippingAddress_Street and ShippingAddress_City.</span></span>

<span data-ttu-id="3a706-129">您可以将附加`HasColumnName`方法来重命名这些列。</span><span class="sxs-lookup"><span data-stu-id="3a706-129">You can append the `HasColumnName` method to rename those columns.</span></span> <span data-ttu-id="3a706-130">在其中 StreetAddress 是公共属性的情况下，映射将</span><span class="sxs-lookup"><span data-stu-id="3a706-130">In the case where StreetAddress is a public property, the mappings would be</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="3a706-131">共享多个固有类型之间的相同.NET 类型</span><span class="sxs-lookup"><span data-stu-id="3a706-131">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="3a706-132">固有的实体类型可以是相同的.NET 类型，作为另一个固有的实体类型，因此.NET 类型可能都不足以识别固有的类型。</span><span class="sxs-lookup"><span data-stu-id="3a706-132">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="3a706-133">在这些情况下，所有者从指向固有实体的属性将成为_定义导航_的固有的实体类型。</span><span class="sxs-lookup"><span data-stu-id="3a706-133">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="3a706-134">从 EF Core 的角度来看，定义导航是标识的旁边的.NET 类型的类型的一部分。</span><span class="sxs-lookup"><span data-stu-id="3a706-134">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="3a706-135">例如，在下面的类： ShippingAddress 和 BillingAddress 为这两个相同的.NET 类型，StreetAddress:</span><span class="sxs-lookup"><span data-stu-id="3a706-135">For example, in the following class, ShippingAddress and BillingAddress are both of the same .NET type, StreetAddress:</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

<span data-ttu-id="3a706-136">若要了解如何 EF Core 将区分跟踪这些对象的实例，可能会有用考虑定义导航已旁边的值的键的所有者的实例键的一部分，拥有类型的.NET 类型。</span><span class="sxs-lookup"><span data-stu-id="3a706-136">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="3a706-137">嵌套固有的类型</span><span class="sxs-lookup"><span data-stu-id="3a706-137">Nested owned types</span></span>

<span data-ttu-id="3a706-138">在此示例中 OrderDetails 拥有 BillingAddress 和 ShippingAddress，均为 StreetAddress 类型。</span><span class="sxs-lookup"><span data-stu-id="3a706-138">In this example OrderDetails owns BillingAddress and ShippingAddress, which are both StreetAddress types.</span></span> <span data-ttu-id="3a706-139">然后 OrderDetails 属于 Order 类型。</span><span class="sxs-lookup"><span data-stu-id="3a706-139">Then OrderDetails is owned by the Order type.</span></span>

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

public enum OrderStatus
{
    Pending,
    Shipped
}

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```

<span data-ttu-id="3a706-140">它是链接到`OwnsOne`若要配置此模型的 fluent 映射中的方法：</span><span class="sxs-lookup"><span data-stu-id="3a706-140">It is possible to chain the `OwnsOne` method in a fluent mapping to configure this model:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

<span data-ttu-id="3a706-141">可以实现相同的操作使用`OwnedAttribute`OrderDetails 和 StreetAdress 上。</span><span class="sxs-lookup"><span data-stu-id="3a706-141">It is possible to achieve the same thing using `OwnedAttribute` on both OrderDetails and StreetAdress.</span></span>

<span data-ttu-id="3a706-142">嵌套固有类型，除了固有的类型可以引用常规实体。</span><span class="sxs-lookup"><span data-stu-id="3a706-142">In addition to nested owned types, an owned type can reference a regular entity.</span></span> <span data-ttu-id="3a706-143">在以下示例中，国家/地区是常规的非固有实体：</span><span class="sxs-lookup"><span data-stu-id="3a706-143">In the following example, Country is a regular non-owned entity:</span></span>

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

<span data-ttu-id="3a706-144">此功能在 EF6 中设置除了复杂类型的固有的实体类型。</span><span class="sxs-lookup"><span data-stu-id="3a706-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="3a706-145">存储拥有单独的表中的类型</span><span class="sxs-lookup"><span data-stu-id="3a706-145">Storing owned types in separate tables</span></span>

<span data-ttu-id="3a706-146">此外固有的类型不同于 EF6 的复杂类型，它可以存储在单独的表所有者。</span><span class="sxs-lookup"><span data-stu-id="3a706-146">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="3a706-147">若要重写将固有的类型映射到与所有者相同的表的约定，您可以简单地调用`ToTable`并提供一个不同的表名称。</span><span class="sxs-lookup"><span data-stu-id="3a706-147">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="3a706-148">下面的示例将 OrderDetails 和其两个地址映射到一个单独的表从订单：</span><span class="sxs-lookup"><span data-stu-id="3a706-148">The following example will map OrderDetails and its two addresses to a separate table from Order:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
        od.ToTable("OrderDetails");
    });
```

## <a name="querying-owned-types"></a><span data-ttu-id="3a706-149">查询固有的类型</span><span class="sxs-lookup"><span data-stu-id="3a706-149">Querying owned types</span></span>

<span data-ttu-id="3a706-150">查询所有者时，固有类型将默认包含在内。</span><span class="sxs-lookup"><span data-stu-id="3a706-150">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="3a706-151">不需要使用`Include`方法，即使固有的类型存储在一个单独的表中。</span><span class="sxs-lookup"><span data-stu-id="3a706-151">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="3a706-152">根据之前所述的模型，以下查询将拉取顺序、 OrderDetails 和从数据库的所有挂起订单的两个拥有的 StreetAddresses:</span><span class="sxs-lookup"><span data-stu-id="3a706-152">Based on the model described before, the following query will pull Order, OrderDetails and the two owned StreetAddresses for all pending orders from the database:</span></span>

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a><span data-ttu-id="3a706-153">限制</span><span class="sxs-lookup"><span data-stu-id="3a706-153">Limitations</span></span>

<span data-ttu-id="3a706-154">其中某些限制是基础如何固有实体类型的工作，但另一些限制，我们可能无法将其删除在将来的版本：</span><span class="sxs-lookup"><span data-stu-id="3a706-154">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="3a706-155">在早期版本中的不足之处</span><span class="sxs-lookup"><span data-stu-id="3a706-155">Shortcomings in previous versions</span></span>
- <span data-ttu-id="3a706-156">在 EF Core 2.0 中，导航到拥有不能在派生的实体类型中声明实体类型，除非自有的实体被显式映射到一个单独的表所有者层次结构中。</span><span class="sxs-lookup"><span data-stu-id="3a706-156">In EF Core 2.0, navigations to owned entity types cannot be declared in derived entity types unless the owned entities are explicitly mapped to a separate table from the owner hierarchy.</span></span> <span data-ttu-id="3a706-157">EF Core 2.1 中已删除此限制</span><span class="sxs-lookup"><span data-stu-id="3a706-157">This limitation has been removed in EF Core 2.1</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="3a706-158">当前的不足之处</span><span class="sxs-lookup"><span data-stu-id="3a706-158">Current shortcomings</span></span>
- <span data-ttu-id="3a706-159">包含的继承层次结构拥有不支持实体类型</span><span class="sxs-lookup"><span data-stu-id="3a706-159">Inheritance hierarchies that include owned entity types are not supported</span></span>
- <span data-ttu-id="3a706-160">固有的实体类型不能指向集合导航属性 （目前支持导航的唯一参考）</span><span class="sxs-lookup"><span data-stu-id="3a706-160">Owned entity types cannot be pointed at by a collection navigation property (only reference navigations are currently supported)</span></span>
- <span data-ttu-id="3a706-161">导航到所拥有的实体类型不能为 null，除非它们显式映射到单独的表中的所有者</span><span class="sxs-lookup"><span data-stu-id="3a706-161">Navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner</span></span>
- <span data-ttu-id="3a706-162">固有的实体类型的实例无法共享的多个所有者 （这是一个已知的值对象不能使用固有的实体类型实现的方案）</span><span class="sxs-lookup"><span data-stu-id="3a706-162">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types)</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="3a706-163">按设计限制</span><span class="sxs-lookup"><span data-stu-id="3a706-163">By-design restrictions</span></span>
- <span data-ttu-id="3a706-164">无法创建 `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="3a706-164">You cannot create a `DbSet<T>`</span></span>
- <span data-ttu-id="3a706-165">不能调用`Entity<T>()`固有类型上使用 `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="3a706-165">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>
