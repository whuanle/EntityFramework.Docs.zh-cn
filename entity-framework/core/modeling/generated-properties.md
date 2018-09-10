---
title: 生成的值的 EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: eb082011-11a1-41b4-a108-15daafa03e80
uid: core/modeling/generated-properties
ms.openlocfilehash: 9ecfa924a0614f327f0bd202cb7dda95bea810af
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250694"
---
# <a name="generated-values"></a>生成的值

## <a name="value-generation-patterns"></a>值生成模式

有三种可用于属性的值生成模式：
* 值，未生成
* 在生成的值添加
* 生成的值上添加或更新

### <a name="no-value-generation"></a>值，未生成

没有值生成意味着，将始终提供有效的值保存到数据库。 它们被添加到上下文之前，必须将此有效的值分配到新的实体。

### <a name="value-generated-on-add"></a>在生成的值添加

在生成的值将添加方法，为新实体生成的值。

根据正在使用的数据库提供程序，值可能会生成客户端通过 EF 或数据库中。 如果由数据库生成的值，然后 EF 时，会分配临时值将实体添加到上下文。 然后通过在过程的数据库生成值替换为此临时值`SaveChanges()`。

如果将实体添加到分配给属性的值的上下文，然后 EF 将尝试插入该值，而不是无需生成一个新。 属性将被视为已分配未分配的 CLR 默认值的值 (`null`有关`string`，`0`有关`int`，`Guid.Empty`为`Guid`，等等。)。 有关详细信息，请参阅[生成属性的显式值](../saving/explicit-values-generated-properties.md)。

> [!WARNING]  
> 如何为添加的实体生成的值取决于正在使用的数据库提供程序。 数据库提供程序可能会在安装程序自动值生成对于某些属性类型，但其他人可能要求你手动设置生成的值如何。
>
> 例如，使用 SQL Server 时，值将自动生成的`GUID`属性 （使用 SQL Server 的连续 GUID 算法）。 但是，如果你指定`DateTime`生成属性添加，则必须安装程序生成的值的方法。 一种方法来执行此操作，是配置了默认值`GETDATE()`，请参阅[默认值](relational/default-values.md)。

### <a name="value-generated-on-add-or-update"></a>生成的值上添加或更新

生成的值上添加或更新意味着每次保存该记录时 （插入或更新），生成新值。

如`value generated on add`，如果新添加的值将会插入而不是所生成的值的实体实例上指定属性的值。 还有可能更新时将显式值。 有关详细信息，请参阅[生成属性的显式值](../saving/explicit-values-generated-properties.md)。

> [!WARNING]
> 如何添加和已更新的实体生成的值取决于正在使用的数据库提供程序。 数据库提供程序可能会在安装程序自动值生成对于某些属性类型，而其他人将需要你手动设置生成的值如何。
> 
> 例如，在使用 SQL Server`byte[]`生成上设置的属性添加或更新并标记为并发标记，将使用设置`rowversion`数据类型的以便将在数据库中生成值。 但是，如果你指定`DateTime`生成属性上添加或更新，则必须安装程序生成的值的方法。 一种方法来执行此操作，是配置了默认值`GETDATE()`(请参阅[默认值](relational/default-values.md)) 以生成新行的值。 然后可以使用数据库触发器在更新 （如下面的示例触发器） 过程中生成值。
> 
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="conventions"></a>约定

按照约定，非复合主键类型 short、 int、 long、 或 Guid 就是安装程序已添加对生成的值。 所有其他属性值，未生成与将安装程序。

## <a name="data-annotations"></a>数据注释

### <a name="no-value-generation-data-annotations"></a>没有值生成 （数据批注）

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-data-annotations"></a>在生成的值添加 （数据批注）

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> 这只是让 EF 知道为添加的实体生成值，它不保证，EF 将在安装程序的实际机制来生成值。 请参阅[上生成的值添加](#value-generated-on-add)部分，了解更多详细信息。

### <a name="value-generated-on-add-or-update-data-annotations"></a>在生成的值将添加或更新 （数据批注）

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> 这只是让 EF 知道值生成的添加或更新实体，它不保证，EF 将在安装程序的实际机制来生成值。 请参阅[上生成的值将添加或更新](#value-generated-on-add-or-update)部分，了解更多详细信息。

## <a name="fluent-api"></a>Fluent API

Fluent API 可用于更改某一给定属性的值生成模式。

### <a name="no-value-generation-fluent-api"></a>没有值生成 (Fluent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-fluent-api"></a>在生成的值添加 (Fluent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> `ValueGeneratedOnAdd()` 只是让 EF 知道为添加的实体生成值，它不保证，EF 将在安装程序的实际机制来生成值。  请参阅[上生成的值添加](#value-generated-on-add)部分，了解更多详细信息。

### <a name="value-generated-on-add-or-update-fluent-api"></a>在生成的值将添加或更新 (Fluent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> 这只是让 EF 知道值生成的添加或更新实体，它不保证，EF 将在安装程序的实际机制来生成值。 请参阅[上生成的值将添加或更新](#value-generated-on-add-or-update)部分，了解更多详细信息。
