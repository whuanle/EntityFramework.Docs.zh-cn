---
title: "并发标记的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: f3cf28d5c54e63aa76058e9fe1d9f3de5b37d579
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2018
---
# <a name="concurrency-tokens"></a>并发标记

> [!NOTE]
> 本页介绍如何配置并发标记。 请参阅[处理并发冲突](../saving/concurrency.md)并发控制 EF 核心以及如何处理应用程序中的并发冲突的示例中的工作原理的详细说明。

属性配置为并发标记用于实现乐观并发控制。

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
