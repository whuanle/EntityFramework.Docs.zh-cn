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
* 无值生成
* 在生成的值添加
* 在添加或更新时生成值

### <a name="no-value-generation"></a>无值生成

没有值生成意味着，您将始终提供有效的值以被保存到数据库。 必须先将有效的值赋到新的实体，再将这些新的实体添加到上下文中。

### <a name="value-generated-on-add"></a>在添加时生成值

在添加时生成值，意思是为新实体生成值。

根据正在使用的数据库提供程序不同，值可能会通过 EF在客户端生成或者由数据库生成。 如果由数据库生成值，那么当你将实体添加到上下文时，EF可能会赋一个临时值。 而后，在数据库的`SaveChanges()`过程中所生成的值会替换掉这个临时值。

如果您将一个实体添加到已经给属性赋了一个值的上下文，这时 EF 将尝试插入该值而不是生成一个新值。 属性被认为是已赋值的，如果属性未被赋 CLR的默认值 (`string`默认值为`null`，`int`默认值为`0`，`Guid.Empty`默认值为`Guid`，等)。 有关详细信息，请参阅[设置已生成属性的显式值](../saving/explicit-values-generated-properties.md)。

> [!WARNING]  
> 如何为添加的实体生成的值取决于正在使用的数据库提供程序。 数据库提供程序可能会为某些属性类型自行设置值的生成，但其他的可能要求您手动设置如何生成值。
>
> 例如，使用 SQL Server 时，将自动生成的`GUID`属性的值（使用 SQL Server 的连续 GUID 算法）。 但是，如果您指定在添加时生成`DateTime`属性，则您必须设置生成值的方法。 此处例举一种方法，是配置`GETDATE()`的默认值，请参阅[默认值](relational/default-values.md)。

### <a name="value-generated-on-add-or-update"></a>在添加或更新时生成值

在添加或更新时生成值，意味着在每次保存该记录时 （插入或更新），生成新值。

就像`value generated on add`，如果您为一个实体的新增实例上的属性指定了一个值，这时 EF 将尝试插入该值而不是生成一个新值。 还可以在更新时设置显式值。 有关详细信息，请参阅[设置已生成属性的显式值](../saving/explicit-values-generated-properties.md)。

> [!WARNING]
> 如何在添加和更新实体时生成值取决于正在使用的数据库提供程序。 数据库提供程序可能会为某些属性类型自行设置值的生成，但其他的可能要求您手动设置如何生成值。
> 
> 例如，在使用 SQL Server时，`byte[]`属性设置为在添加和更新时生成值并标注为并发标记，将被设置`rowversion`数据类型，以便在数据库中生成值。 但是，如果你指定在添加或更新时生成`DateTime`属性，则您必须设置生成值的方法。 此处例举一种方法，是配置`GETDATE()`的默认值(请参阅[默认值](relational/default-values.md)) 以生成新行的值。 然后您可以使用数据库触发器在更新（如下面的示例触发器）过程中生成值。
> 
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="conventions"></a>约定

按照约定，非复合主键类型 short、 int、 long、 或 Guid 将会被设置在添加时生成值。 所有其他属性值，将被设置为无值生成。

## <a name="data-annotations"></a>数据注释

### <a name="no-value-generation-data-annotations"></a>没有值生成 （数据批注）

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-data-annotations"></a>在添加时生成值 （数据批注）

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> 这只是让 EF 知道将为已添加的实体生成值，它不保证EF 将设置实际机制来生成值。 请参阅[在添加时生成值](#value-generated-on-add)部分，了解更多详细信息。

### <a name="value-generated-on-add-or-update-data-annotations"></a>在添加或更新生成值 （数据批注）

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> 这只是让 EF 知道将为添加或更新的实体生成值，它不保证，EF 将设置实际机制来生成值。 请参阅[在添加或更新时生成值](#value-generated-on-add-or-update)部分，了解更多详细信息。

## <a name="fluent-api"></a>Fluent API

Fluent API 可用于更改某一给定属性的值生成模式。

### <a name="no-value-generation-fluent-api"></a>没有值生成 (Fluent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-fluent-api"></a>在添加时生成值 (Fluent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> `ValueGeneratedOnAdd()` 只是让 EF 知道为添加的实体生成值，它不保证，EF 将设置实际机制来生成值。  请参阅[在添加时生成值](#value-generated-on-add)部分，了解更多详细信息。

### <a name="value-generated-on-add-or-update-fluent-api"></a>在添加或更新时生成值 (Fluent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> 这只是让 EF 知道将为添加或更新的实体生成值，它不保证，EF 将设置实际机制来生成值。 请参阅[在添加和更新时生成值](#value-generated-on-add-or-update)部分，了解更多详细信息。
