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
# <a name="owned-entity-types"></a>拥有的实体类型

>[!NOTE]
> 此功能是在 EF 核心 2.0 中的新增功能。

EF 核心让你可以只显示对其他实体类型的导航属性的模型实体类型。 这些应用程序称为_拥有实体类型_。 包含拥有的实体类型的实体是其_所有者_。

## <a name="explicit-configuration"></a>显式配置

拥有永远不会包含类型由 EF 核心模型中按照约定的实体。 你可以使用`OwnsOne`中的方法`OnModelCreating`或批注与类型`OwnedAttrbibute`（新在 EF 核心 2.1） 配置为拥有类型的类型。

在此示例中，StreetAddress 是没有标识属性的类型。 它用作 Order 类型的属性来指定特定订单的发货地址。 在`OnModelCreating`，我们使用`OwnsOne`方法，以指定 ShippingAddress 属性是拥有顺序类型的实体。

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

如果该 ShippingAddress 属性是在顺序类型中为私有，则可以使用的字符串版本`OwnsOne`方法：

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

在 EF 核心 2.0 和 2.1 中，仅引用导航属性可以指向拥有的类型。 不支持拥有类型的集合。 这些引用拥有类型始终与所有者具有一对一的关系，因此它们不需要其自己的密钥值。 在前面的示例中，不需要定义键属性 StreetAddress 类型。  

在了解如何 EF 核心跟踪这些对象的顺序，有必要考虑主密钥被创建为[隐藏属性](xref:core/modeling/shadow-properties)拥有的类型。 拥有类型的实例键的值将作为所有者实例的密钥的值相同。      

## <a name="mapping-owned-types-with-table-splitting"></a>映射拥有与表拆分的类型

在使用关系数据库时，按照约定拥有类型映射到与所有者相同的表。 这要求拆分成两个表： 某些列将用于存储的所有者，数据和某些列将用于存储数据的拥有的实体。 这是一个称作表拆分的常见功能。

> [!TIP]
> 拥有表拆分与存储的类型可以是使用非常类似于复杂类型中使用 ef6 更高版本。

按照约定，EF 核心将遵循模式在拥有的实体类型的属性的名称的数据库列_EntityProperty_OwnedEntityProperty_。 因此 StreetAddress 属性将显示在具有名 ShippingAddress_Street 和 ShippingAddress_City Orders 表中。

你可以将附加`HasColumnName`方法来重命名这些列。 在其中 StreetAddress 是公共属性的情况下，映射将是

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>共享多个类型中拥有相同的.NET 类型

拥有的实体类型可以是相同的.NET 类型作为另一个拥有的实体类型，因此.NET 类型可能不够来标识拥有的类型。

在这些情况下，从所有者指向拥有的实体的属性将成为_定义导航_拥有的实体类型。 从 EF 核心的角度来看，定义导航是标识的旁边的.NET 类型的类型的一部分。   

例如，在以下类中，ShippingAddress 和 BillingAddress 都相同的.NET 类型，StreetAddress:

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

若要了解如何 EF 核心将区分跟踪这些对象的实例，可能会有用考虑定义导航已旁边的值的键的所有者的实例键的一部分，拥有类型的.NET 类型。

## <a name="nested-owned-types"></a>嵌套拥有的类型

在此示例中 OrderDetails 拥有 BillingAddress 和 ShippingAddress，它们是两个 StreetAddress 类型。 然后 OrderDetails 归顺序类型。

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

可以链接到`OwnsOne`fluent 映射以配置此模型中的方法：

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

它是可能实现相同的操作使用`OwnedAttribute`OrderDetails 和 StreetAdress 上。

嵌套拥有除类型之外，拥有的类型可以引用常规实体。 在下面的示例中，国家/地区是常规的 （即非自有） 实体：

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

此功能设置以从 EF6 脱离复杂类型拥有的实体类型。

## <a name="storing-owned-types-in-separate-tables"></a>存储拥有单独的表中的类型

也拥有的类型从 EF6 复杂与类型不同，它可以存储在所有者从单独的表。 若要重写将拥有的类型映射到与所有者相同的表的约定，你可以轻松调用`ToTable`并提供不同的表名。 下面的示例将 OrderDetails 和其两个地址映射到单独的表中的顺序：

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a>查询拥有的类型

查询所有者时，固有类型将默认包含在内。 不需要使用`Include`方法，即使拥有的类型存储在单独的表。 根据之前所述的模型，以下查询将拉取顺序、 OrderDetails 和从数据库的所有挂起订单两个拥有的 StreeAddresses:

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a>限制

以下是拥有的实体类型的一些限制。 这些限制一些基本到如何拥有类型的工作，但某些其他属性我们想要删除在将来版本的时间点限制：

### <a name="current-shortcomings"></a>当前不足之处
- 不支持的所有类型的继承
- 不能由集合导航属性指向拥有的类型
- 因为它们使用表分割默认拥有类型还具有以下限制，除非显式映射到另一个表：
   - 它们不能拥有由派生类型
   - 定义导航属性不能设置为 null （即拥有对同一个表的类型并总是必需的）

### <a name="by-design-restrictions"></a>按设计限制
- 无法创建 `DbSet<T>`
- 不能调用`Entity<T>()`拥有类型 `ModelBuilder`
