---
title: "并发标记的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: 6574a9098d38c4aa525ffb4896adb01082420b5f
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2017
---
# <a name="concurrency-tokens"></a>并发标记

如果属性配置为并发标记 EF 将检查与该记录中保存更改时，没有其他用户已修改数据库中的该值。 EF 使用乐观并发模式，这意味着它将假定未更改值并尝试保存数据，但如果它找到的值已更改引发。

例如，我们可能需要配置`LastName`上`Person`是并发标记。 这意味着，如果一个用户尝试保存对某些更改`Person`，但另一个用户已更改`LastName`则将引发异常。 这可能需要，以便你的应用程序就会提示用户以确保此记录仍在保存其更改之前表示实际是同一个人。

> [!NOTE]
> 本页介绍如何配置并发标记。 请参阅[处理并发](../saving/concurrency.md)有关如何在你的应用程序中使用开放式并发的示例。

## <a name="how-concurrency-tokens-work-in-ef"></a>并发标记 EF 中的工作原理

数据存储区可以通过检查任何记录正在更新或删除仍具有相同的值已指派上下文最初从数据库加载数据时的并发标记强制并发标记。

例如，关系数据库来实现此包括中的并发标记`WHERE`子句任何`UPDATE`或`DELETE`命令和检查受影响的行数。 如果并发标记仍与匹配，则将更新一个行。 如果数据库中的值已更改，将不更新任何行。

```sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="conventions"></a>约定

按照约定，属性被永远不会配置为并发标记。

## <a name="data-annotations"></a>数据注释

你可以使用数据注释将属性配置为并发标记。

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>Fluent API

Fluent API 可用于将属性配置为并发标记。

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>时间戳/行版本

时间戳是每次插入或更新行时，由数据库生成一个新值是其中一个属性。 属性也将被视为并发标记。 这可确保如果其他人进行了修改你尝试更新由于数据查询的行，则将收到异常。

这如何实现由正在使用的数据库提供程序。 对于 SQL Server，时间戳通常使用上*byte []*属性，用于将设置为*ROWVERSION*数据库中的列。

### <a name="conventions"></a>约定

按照约定，属性被永远不会配置为时间戳。

### <a name="data-annotations"></a>数据注释

数据注释可用于将属性配置为时间戳。

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>Fluent API

Fluent API 可用于将属性配置为时间戳。

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
