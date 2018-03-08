---
title: "查询类型的 EF 核心"
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: 19a371c65da33e8209cc1ab3423a67c34ddae61e
ms.sourcegitcommit: fc68321c211aca38f7b9dc3a75677c6ca1b2524b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
---
# <a name="query-types"></a>查询类型
> [!NOTE]
> 此功能是 EF 核心 2.1 中的新增功能

查询类型都是可以添加到 EF 核心模型的只读查询结果类型。 查询类型启用的即席查询 （如匿名类型），但不更灵活，因为它们可以具有指定的映射配置。

从概念上讲类似于实体类型中，它们是：

- 它们是 POCO C# 类型添加到模型，在```OnModelCreating```使用```ModelBuilder.Query```方法，或通过 DbContext"集"属性 (对于查询的类型这样的属性被类型化为```DbQuery<T>```而应假定```DbSet<T>```)。
- 它们支持很多与常规实体类型相同的映射功能。 示例、 继承映射、 导航 （请参阅下面的限制），然后在关系存储，能够配置目标数据库架构对象通过```ToTable```， ```HasColumn``` fluent api 方法 （或数据注释）。

查询类型都不同于实体类型中，它们：

- 不需要一个密钥，以定义。
- 永远不会跟踪由更改跟踪器。
- 永远不会发现的约定。
- 仅支持一部分导航映射功能-具体而言，它们可能永远不会充当一种关系的主体端。
- 可以将映射到_定义查询_-定义的查询是辅助查询可充当数据源的查询类型。

下面是一些的查询类型主要使用方案：

- 映射到数据库视图。
- 将映射到没有定义的主键的表。
- 即席充当的返回类型```FromSql()```查询。
- 将映射到模型中定义的查询。

> [!TIP]
> 将查询类型映射到数据库视图使用实现```ToTable```fluent API。

## <a name="example"></a>示例

下面的示例演示如何使用查询类型来查询数据库视图。

> [!TIP]
> 可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes)。

首先，我们定义一个简单的博客和 Post 模型：

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

接下来，我们定义一个简单的数据库视图，使我们能够查询数与每个博客文章：

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

接下来，我们定义一个类以保留的数据库视图的结果：

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#QueryType)]

接下来，我们配置中的查询类型_OnModelCreating_使用```modelBuilder.Query<T>```API。
我们可以使用标准 fluent 配置 Api 来配置查询类型的映射：

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

最后，我们可以查询数据库视图中的标准方式：

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> 请注意我们还定义了上下文级别查询属性 (DbQuery) 使其作为针对此类型的查询的根。
