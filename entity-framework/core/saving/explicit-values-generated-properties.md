---
title: "为生成的属性的 EF 核心设置显式值"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3f1993c2-cdf5-425b-bac2-a2665a20322b
ms.technology: entity-framework-core
uid: core/saving/explicit-values-generated-properties
ms.openlocfilehash: f34e92d9a3b10b6ff904257ccd047a8acdaad231
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
---
# <a name="setting-explicit-values-for-generated-properties"></a>设置生成的属性的显式值

生成的属性是的属性，其值生成的 （无论是 EF 或数据库） 时该实体是添加和/或更新。 请参阅[生成属性](../modeling/generated-properties.md)有关详细信息。

可能的情况下你想要设置为生成的属性，而不是让一个生成一个显式值。

> [!TIP]  
> 你可以查看这篇文章[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/ExplicitValuesGenerateProperties/)GitHub 上。

## <a name="the-model"></a>模型

在本文中使用的模型包含单个`Employee`实体。

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Employee.cs#Sample)]

## <a name="saving-an-explicit-value-during-add"></a>在添加过程中保存一个显式值

`Employee.EmploymentStarted`属性被配置为具有生成的新实体 （使用默认值） 的数据库值。

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#EmploymentStarted)]

下面的代码将两个员工插入到数据库。
* 首先，没有值分配给`Employee.EmploymentStarted`属性，使它保持设置的 CLR 默认值为`DateTime`。
* 对于第二个，我们设置的显式值的`1-Jan-2000`。

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmploymentStarted)]

输出显示该数据库为第一个员工生成一个值，并且我们显式值用于第二个。

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/1/2000 12:00:00 AM
```

### <a name="explicit-values-into-sql-server-identity-columns"></a>到 SQL Server 标识列的显式值

按照约定`Employee.EmployeeId`属性是存储生成`IDENTITY`列。

大多数情况下，如上所示的方法将适用于键属性。 但是，若要将显式值插入到 SQL Server`IDENTITY`列中，你需要手动启用`IDENTITY_INSERT`之前调用`SaveChanges()`。

> [!NOTE]  
> 我们有[功能请求](https://github.com/aspnet/EntityFramework/issues/703)在我们的积压工作来自动执行此操作的 SQL Server 提供程序内。

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmployeeId)]

输出显示提供的 id 已保存到数据库。

``` Console
100: John Doe
101: Jane Doe
```

## <a name="setting-an-explicit-value-during-update"></a>在更新过程中设置一个显式值

`Employee.LastPayRaise`属性配置为具有在更新过程中由数据库生成的值。

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#LastPayRaise)]

> [!NOTE]  
> 默认情况下，EF 核心将引发异常，如果你尝试保存已显式配置为在更新过程中生成属性的值。 若要避免此问题，你需要到较低级别的元数据 API 下拉列表中，设置`AfterSaveBehavior`（如上所示）。

> [!NOTE]  
> **EF 核心 2.0 中的更改：**通过控制后保存行为已在以前的版本`IsReadOnlyAfterSave`标志。 过时此标志和将其替换为`AfterSaveBehavior`。

要生成的值的数据库中还有触发器`LastPayRaise`期间的列`UPDATE`操作。

[!code-sql[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/employee_UPDATE.sql)]

下面的代码会增加数据库中的两个雇员的薪水。
* 首先，没有值分配给`Employee.LastPayRaise`属性，因此它将保留为 null。
* 对于第二个，我们设置的显式值的前一周 （后可以追溯支付引发）。

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#LastPayRaise)]

输出显示该数据库为第一个员工生成一个值，并且我们显式值用于第二个。

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/19/2017 12:00:00 AM
```
