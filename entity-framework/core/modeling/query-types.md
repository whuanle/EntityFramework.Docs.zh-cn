---
title: 查询类型的 EF Core
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: f16e3a130f3a4f92b2bf6014f2df0ca4eec56a25
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2018
ms.locfileid: "34163169"
---
# <a name="query-types"></a>查询类型
> [!NOTE]
> 此功能是 EF Core 2.1 中的新增功能

EF Core 模型可以包含实体类型，除了_查询类型_，这可以用于执行针对未映射到实体类型的数据的数据库查询。

查询类型的实体类型具有许多相似之处：

- 它们也可以添加到模型或者在`OnModelCreating`，或通过"set"上的属性，派生_DbContext_。
- 它们支持许多相同的映射功能，如继承映射，导航属性 （请参阅下面的限制），然后在关系存储，能够配置的目标数据库对象和通过 fluent API 方法或数据注释的列。

但是值不同于实体中的类型，它们：

- 不需要一个密钥，以定义。
- 永远不会跟踪的更改上_DbContext_并因此永远不会插入、 更新或删除数据库上。
- 永远不会发现的约定。
- 仅支持一部分导航映射功能-具体而言：
  - 它们可能永远不会充当一种关系的主体端。
  - 它们只能包含指向实体的引用导航属性。
  - 实体不能包含为查询类型的导航属性。
- 得到解决_ModelBuilder_使用`Query`方法而不是`Entity`方法。
- 在映射_DbContext_类型的属性通过`DbQuery<T>`而不是 `DbSet<T>`
- 映射到使用的数据库对象`ToView`方法，而不是`ToTable`。
- 可以将映射到_定义查询_-定义查询是在充当查询类型的数据源的模型中声明的辅助查询。

下面是一些的查询类型主要使用方案：

- 即席充当的返回类型`FromSql()`查询。
- 映射到数据库视图。
- 将映射到没有定义的主键的表。
- 将映射到模型中定义的查询。

> [!TIP]
> 将查询类型映射到的数据库对象使用实现`ToView`fluent API。 此方法中指定的数据库对象是从 EF Core 的角度来看，_视图_，这意味着它将被视为只读查询源和不能作为目标的更新、 插入或删除操作。 但是，这并不意味着该数据库对象是实际需要的数据库视图-还可以是以只读方式将被视为数据库表。 相反，实体类型，EF Core 假定数据库对象中指定`ToTable`方法可以视为_表_、 意味着它可以用作查询源，但是还针对由更新、 删除和插入操作。 事实上，可以指定的名称中的数据库视图`ToTable`和所有内容应该可以满足要求，只要视图配置为在数据库上可更新。

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

接下来，我们配置中的查询类型_OnModelCreating_使用`modelBuilder.Query<T>`API。
我们可以使用标准 fluent 配置 Api 来配置查询类型的映射：

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

最后，我们可以查询数据库视图中的标准方式：

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> 请注意我们还定义了上下文级别查询属性 (DbQuery) 使其作为针对此类型的查询的根。
