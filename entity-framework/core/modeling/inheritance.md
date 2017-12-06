---
title: "继承的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
ms.technology: entity-framework-core
uid: core/modeling/inheritance
ms.openlocfilehash: f0394bc55dfbfea8277b1ddf898361165dd1fe51
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="inheritance"></a>继承

EF 模型中的继承用于控制如何在数据库中表示的实体类中的继承。

## <a name="conventions"></a>约定

按照约定，由数据库提供程序，以确定继承在数据库中的表示方式。 请参阅[继承 （关系数据库）](relational/inheritance.md)这是如何处理与关系数据库提供程序。

如果模型中显式包含两个或多个继承的类型，EF 将仅设置继承。 EF 将不会扫描基表或派生类型，否则不包括在模型中。 你可以在模型中包含类型，通过公开*DbSet<TEntity>* 继承层次结构中的每个类型。

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

如果你不想要公开*DbSet<TEntity>* 对于层次结构中的一个或多个实体，你可以使用 Fluent API 以确保它们包括在模型中。
如果您不依赖于约定可以指定使用显式的基类型和`HasBaseType`。

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> 你可以使用`.HasBaseType((Type)null)`以层次结构中删除的实体类型。

## <a name="data-annotations"></a>数据注释

数据注释不能用于配置继承。

## <a name="fluent-api"></a>Fluent API

用于继承的 Fluent API 取决于你使用的数据库提供程序。 请参阅[继承 （关系数据库）](relational/inheritance.md)您可以对关系数据库提供程序执行的配置。
