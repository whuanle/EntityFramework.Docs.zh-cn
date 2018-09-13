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
# <a name="owned-entity-types"></a>固有的实体类型

>[!NOTE]
> 此功能是在 EF Core 2.0 中的新增功能。

EF Core 让你可以只显示对其他实体类型的导航属性的模型实体类型。 这些被称为_固有实体类型_。 包含固有的实体类型的实体是其_所有者_。

## <a name="explicit-configuration"></a>显式配置

拥有永远不会包含类型由 EF Core 模型中按照约定的实体。 你可以使用`OwnsOne`中的方法`OnModelCreating`或批注与类型`OwnedAttribute`（新在 EF Core 2.1） 配置为拥有类型的类型。

在此示例中，StreetAddress 是没有标识属性的类型。 它用作 Order 类型的属性来指定特定订单的发货地址。 在中`OnModelCreating`，我们使用`OwnsOne`方法，以指定 ShippingAddress 属性是拥有订单类型的实体。

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

如果 ShippingAddress 属性是私有的订单类型，则可以使用的字符串版本`OwnsOne`方法：

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

在此示例中，我们使用`OwnedAttribute`来实现相同的目的：

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

## <a name="implicit-keys"></a>隐式键

在 EF Core 2.0 和 2.1 中，仅引用导航属性可以指向拥有的类型。 不支持的固有类型的集合。 拥有这些引用的类型与所有者始终具有一对一的关系，因此他们并不需要其自己的密钥值。 在上一示例中，不需要定义键属性的 StreetAddress 类型。  

若要了解如何 EF Core 可跟踪这些对象，最好先认为，作为创建主键[阴影属性](xref:core/modeling/shadow-properties)对于固有类型。 固有类型的实例键的值将为所有者实例的键的值相同。      

## <a name="mapping-owned-types-with-table-splitting"></a>映射固有类型与表拆分

使用关系数据库时，按照约定，固有类型映射到与所有者相同的表。 这要求将在两个表拆分： 某些列将用于存储数据的所有者，并且某些列将用于存储数据的固有实体。 这是一项称为表拆分的常见功能。

> [!TIP]
> 拥有表拆分与存储在一起的类型可以是到复杂类型中使用 EF6 的使用非常类似。

按照约定，EF Core 将遵循模式在拥有的实体类型的属性的名称的数据库列_EntityProperty_OwnedEntityProperty_。 因此 StreetAddress 属性会在具有名称 ShippingAddress_Street 和 ShippingAddress_City 订单表中显示。

您可以将附加`HasColumnName`方法来重命名这些列。 在其中 StreetAddress 是公共属性的情况下，映射将

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>共享多个固有类型之间的相同.NET 类型

固有的实体类型可以是相同的.NET 类型，作为另一个固有的实体类型，因此.NET 类型可能都不足以识别固有的类型。

在这些情况下，所有者从指向固有实体的属性将成为_定义导航_的固有的实体类型。 从 EF Core 的角度来看，定义导航是标识的旁边的.NET 类型的类型的一部分。   

例如，在下面的类： ShippingAddress 和 BillingAddress 为这两个相同的.NET 类型，StreetAddress:

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

若要了解如何 EF Core 将区分跟踪这些对象的实例，可能会有用考虑定义导航已旁边的值的键的所有者的实例键的一部分，拥有类型的.NET 类型。

## <a name="nested-owned-types"></a>嵌套固有的类型

在此示例中 OrderDetails 拥有 BillingAddress 和 ShippingAddress，均为 StreetAddress 类型。 然后 OrderDetails 属于 Order 类型。

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

它是链接到`OwnsOne`若要配置此模型的 fluent 映射中的方法：

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

可以实现相同的操作使用`OwnedAttribute`OrderDetails 和 StreetAdress 上。

嵌套固有类型，除了固有的类型可以引用常规实体。 在以下示例中，国家/地区是常规的非固有实体：

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

此功能在 EF6 中设置除了复杂类型的固有的实体类型。

## <a name="storing-owned-types-in-separate-tables"></a>存储拥有单独的表中的类型

此外固有的类型不同于 EF6 的复杂类型，它可以存储在单独的表所有者。 若要重写将固有的类型映射到与所有者相同的表的约定，您可以简单地调用`ToTable`并提供一个不同的表名称。 下面的示例将 OrderDetails 和其两个地址映射到一个单独的表从订单：

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
        od.ToTable("OrderDetails");
    });
```

## <a name="querying-owned-types"></a>查询固有的类型

查询所有者时，固有类型将默认包含在内。 不需要使用`Include`方法，即使固有的类型存储在一个单独的表中。 根据之前所述的模型，以下查询将拉取顺序、 OrderDetails 和从数据库的所有挂起订单的两个拥有的 StreetAddresses:

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a>限制

其中某些限制是基础如何固有实体类型的工作，但另一些限制，我们可能无法将其删除在将来的版本：

### <a name="shortcomings-in-previous-versions"></a>在早期版本中的不足之处
- 在 EF Core 2.0 中，导航到拥有不能在派生的实体类型中声明实体类型，除非自有的实体被显式映射到一个单独的表所有者层次结构中。 EF Core 2.1 中已删除此限制

### <a name="current-shortcomings"></a>当前的不足之处
- 包含的继承层次结构拥有不支持实体类型
- 固有的实体类型不能指向集合导航属性 （目前支持导航的唯一参考）
- 导航到所拥有的实体类型不能为 null，除非它们显式映射到单独的表中的所有者
- 固有的实体类型的实例无法共享的多个所有者 （这是一个已知的值对象不能使用固有的实体类型实现的方案）

### <a name="by-design-restrictions"></a>按设计限制
- 无法创建 `DbSet<T>`
- 不能调用`Entity<T>()`固有类型上使用 `ModelBuilder`
