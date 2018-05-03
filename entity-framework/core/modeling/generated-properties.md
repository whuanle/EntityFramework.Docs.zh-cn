---
title: 生成的值的 EF 核心
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: eb082011-11a1-41b4-a108-15daafa03e80
ms.technology: entity-framework-core
uid: core/modeling/generated-properties
ms.openlocfilehash: 88ccc2da3c2b6cbba8920d7113c82e769b459897
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2018
---
# <a name="generated-values"></a>生成的值

## <a name="value-generation-patterns"></a>值生成模式

有三种可用于属性的值生成模式。

### <a name="no-value-generation"></a>没有值生成

没有值生成意味着，你将始终提供有效的值保存到数据库。 在它们添加到上下文之前，必须将此有效的值分配到新的实体。

### <a name="value-generated-on-add"></a>在生成的值添加

在生成的值添加表示为新的实体生成一个值。

根据正在使用的数据库的提供程序的情况下，值可能生成由 EF 或数据库中的客户端。 如果值由数据库生成的 EF 可能分配一个临时值，当将实体添加到上下文。 然后在过程的数据库生成值替换为此临时值`SaveChanges()`。

如果向已分配给属性的值的上下文中添加实体，然后 EF 将尝试插入该值，而不是生成一个新。 属性将被视为已分配如果未分配的 CLR 的默认值的值 (`null`为`string`，`0`为`int`，`Guid.Empty`为`Guid`等。)。 有关详细信息，请参阅[生成的属性的显式值](../saving/explicit-values-generated-properties.md)。

> [!WARNING]  
> 如何为添加的实体生成值将取决于正在使用的数据库提供程序。 数据库提供程序可能在安装程序自动值生成对于某些属性类型，但其他可能需要你进行手动设置值的生成方式。
>
> 例如，在使用 SQL Server 时，值将为自动生成`GUID`（使用 SQL Server 顺序 GUID 算法） 的属性。 但是，如果你指定`DateTime`生成属性上添加，则您必须先设置要生成的值的方法。 执行此操作，一种方法是配置的默认值为`GETDATE()`，请参阅[默认值](relational/default-values.md)。

### <a name="value-generated-on-add-or-update"></a>生成的值在添加或更新

生成的值在添加或更新意味着每次 （插入或更新），将保存该记录生成了的新值。

如`value generated on add`，如果在新添加的实体，将而不是所生成的值插入值实例上指定属性的值。 还有可能要设置一个显式值更新时。 有关详细信息，请参阅[生成的属性的显式值](../saving/explicit-values-generated-properties.md)。

> [!WARNING]
> 如何添加和已更新的实体生成值将取决于正在使用的数据库提供程序。 数据库提供程序可能在安装程序自动值生成对于某些属性类型，而其他人将需要你手动设置值的生成方式。
> 
> 例如，在使用 SQL Server，`byte[]`生成上设置的属性添加或更新和标记为并发标记将与设置`rowversion`数据类型-以便将数据库中生成值。 但是，如果你指定`DateTime`生成属性上添加或更新，则您必须先设置要生成的值的方法。 执行此操作，一种方法是配置的默认值为`GETDATE()`(请参阅[默认值](relational/default-values.md)) 来生成新行的值。 然后可以使用数据库触发器在 （如下面的示例触发器） 的更新过程中生成值。
> 
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="conventions"></a>约定

按照约定，非复合主键的类型 short、 int、 long、 或 Guid 将安装程序能够生成上添加的值。 所有其他属性将与不值生成的安装程序。

## <a name="data-annotations"></a>数据注释

### <a name="no-value-generation-data-annotations"></a>没有值生成 （数据注释）

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-data-annotations"></a>在生成的值添加 （数据注释）

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> 这只是让 EF 知道为添加的实体生成值，它不保证 EF 将设置生成值的实际机制。 请参阅[上生成的值添加](#value-generated-on-add)有关详细信息部分。

### <a name="value-generated-on-add-or-update-data-annotations"></a>在生成的值将添加或更新 （数据注释）

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> 这只是让 EF 知道值生成的添加或更新实体，它不保证 EF 将设置生成值的实际机制。 请参阅[上生成的值添加或更新](#value-generated-on-add-or-update)有关详细信息部分。

## <a name="fluent-api"></a>Fluent API

Fluent API 可用于更改给定属性的值生成模式。

### <a name="no-value-generation-fluent-api"></a>没有值生成 (Fluent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-fluent-api"></a>在生成的值添加 (Fluent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> `ValueGeneratedOnAdd()` 只是让 EF 知道为添加的实体生成值，它不保证 EF 将设置生成值的实际机制。  请参阅[上生成的值添加](#value-generated-on-add)有关详细信息部分。

### <a name="value-generated-on-add-or-update-fluent-api"></a>在生成的值将添加或更新 (Fluent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> 这只是让 EF 知道值生成的添加或更新实体，它不保证 EF 将设置生成值的实际机制。 请参阅[上生成的值添加或更新](#value-generated-on-add-or-update)有关详细信息部分。
