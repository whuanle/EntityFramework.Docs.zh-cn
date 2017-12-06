---
title: "全局查询筛选器-EF 核心"
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 4e3c3c99d155f69e00fed99c415f519808ea1a68
ms.sourcegitcommit: 6e379265e4f087fb7cf180c824722c81750554dc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
---
# <a name="global-query-filters"></a>全局查询筛选器

全局查询筛选器是 LINQ 查询谓词 (布尔表达式通常传递到 LINQ*其中*查询运算符) 在元数据模型中应用于实体类型 (通常在*OnModelCreating*)。 此类筛选器将自动应用于涉及这些实体类型，包括实体类型引用间接，例如通过使用包含或直接导航属性引用任何 LINQ 查询。 此功能的一些常见的应用程序是：

* **软删除**的实体类型定义*IsDeleted*属性。
* **多租户**的实体类型定义*TenantId*属性。

## <a name="example"></a>示例

下面的示例演示如何使用全局查询筛选器来实现简单的博客模型中的软删除和多租户功能的查询行为。

> [!TIP]
> 你可以查看这篇文章[示例](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters)GitHub 上。

首先，定义实体：

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Entities)]

请注意为 _ 声明_tenantId_字段上_博客_实体。 这将用于将每个博客实例与特定租户相关联。 此外定义是_IsDeleted_属性_Post_实体类型。 这使用此选项可跟踪是否_Post_实例已被"软删除"。 即实例标记为已删除而不进行以物理方式删除基础数据。

接下来，配置中的查询筛选器_OnModelCreating_使用```HasQueryFilter```API。

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Configuration)]

谓词表达式传递给_HasQueryFilter_调用将立即自动应用于这些类型的任何 LINQ 查询。

> [!TIP]
> 请注意，使用 DbContext 实例级别字段：```_tenantId```用于设置当前的租户。 模型级别筛选器将使用从正确的上下文实例的值。 即执行查询实例。

## <a name="disabling-filters"></a>禁用筛选器

可能使用的单个 LINQ 查询禁用筛选器```IgnoreQueryFilters()```运算符。

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>限制

全局查询筛选器具有以下限制：

* 筛选器不能包含对导航属性的引用。
* 根实体类型的继承层次结构可以仅定义筛选器。
