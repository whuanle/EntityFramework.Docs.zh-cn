---
title: "备份 EF 核心的字段-"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
ms.technology: entity-framework-core
uid: core/modeling/backing-field
ms.openlocfilehash: e95417b3368d09a64f9ec02ae40c38dc770c2e6b
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2017
---
# <a name="backing-fields"></a>支持字段

> [!NOTE]  
> 此功能是 EF 核心 1.1 中的新增功能。

支持字段允许 EF 读取和/或写入一个字段，而不是一个属性。 这一点可能很有用，当在类中的封装用于限制的使用和/或增强围绕访问数据的语义由应用程序代码，但值应进行读取和/或写入到数据库而不使用这些限制时 /增强功能。

## <a name="conventions"></a>约定

按照约定，将作为备份为给定属性 （按优先顺序列出） 的字段发现以下字段。 字段只会发现的模型中包含的属性。 在模型中包含属性的详细信息，请参阅[包括和排除属性](included-properties.md)。

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

当配置的支持字段时，EF 将直接写入该字段具体化数据库 （而不是使用属性 setter） 中的实体实例时。 如果 EF 需要读取或写入在其他时间值，它将尽可能使用该属性。 例如，如果 EF 需要更新属性的值，它将使用属性 setter，如果已定义。 如果属性是只读的然后它将写入到字段。

## <a name="data-annotations"></a>数据注释

支持字段时，无法使用数据注释进行配置。

## <a name="fluent-api"></a>Fluent API

Fluent API 可用于配置属性的支持字段。

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a>控制当使用此字段

你可以配置当 EF 使用的字段或属性。 请参阅[PropertyAccessMode 枚举](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode)有关支持的选项。

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a>不带属性的字段

你还可以在实体类中，没有相应的 CLR 属性，但改为使用字段来存储实体中的数据在模型中创建概念的属性。 这是不同于[隐藏属性](shadow-properties.md)、 数据更改跟踪器中的存储位置。 此值通常将在实体类使用方法来获取/设置值使用。

可让 EF 中的字段的名称`Property(...)`API。 如果不没有具有给定名称的任何属性，将查找字段 EF。

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

你还可以选择为之外的字段名称的名称的属性。 创建模型，然后使用此名称，最值得注意的是它将用于映射到数据库中的列名称。

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

当实体类中有任何属性时，你可以使用`EF.Property(...)`LINQ 查询来指代从概念上讲是模型的一部分的属性中的方法。

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
