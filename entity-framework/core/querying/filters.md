---
title: 全局查询筛选器 - EF Core
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 4e3c3c99d155f69e00fed99c415f519808ea1a68
ms.sourcegitcommit: 6e379265e4f087fb7cf180c824722c81750554dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
ms.locfileid: "26053897"
---
# <a name="global-query-filters"></a>全局查询筛选器

全局查询筛选器是应用于元数据模型（通常为 *OnModelCreating*）中的实体类型的 LINQ 查询谓词（通常传递给 LINQ *Where* 查询运算符的布尔表达式）。 此类筛选器自动应用于涉及这些实体类型（包括通过使用 Include 或直接导航属性引用等方式间接引用的实体类型）的所有 LINQ 查询。 此功能的一些常见应用如下：

* **软删除** - 实体类型定义“IsDeleted”属性。
* **多租户** - 实体类型定义“TenantId”属性。

## <a name="example"></a>示例

下面的示例显示了如何使用全局查询筛选器在简单的博客模型中实现软删除和多租户查询行为。

> [!TIP]
> 可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters)。

首先，定义实体：

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Entities)]

请记住_博客_实体上 __tenantId_ 字段的声明。 这会用于将每个博客实例与特定租户相关联。 还会定义_文章_实体类型上的 _IsDeleted_ 属性。 这会用于跟踪_文章_实例是否已“软删除”。 即实例标记为已删除，而实际上不会删除基础数据。

接下来，使用 ```HasQueryFilter``` API 在 _OnModelCreating_ 中配置查询筛选器。

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Configuration)]

传递给 _HasQueryFilter_ 调用的谓词表达式将立即自动应用于这些类型的任何 LINQ 查询。

> [!TIP]
> 请注意 DbContext 实例级别字段的使用：```_tenantId``` 用于设置当前租户。 模型级别筛选器将使用来自正确上下文实例的值。 即执行查询的实例。

## <a name="disabling-filters"></a>禁用筛选器

可使用 ```IgnoreQueryFilters()``` 运算符对各个 LINQ 查询禁用筛选器。

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>限制

全局查询筛选器具有以下限制：

* 筛选器不能包含对导航属性的引用。
* 仅可为继承层次结构的根实体类型定义筛选器。
