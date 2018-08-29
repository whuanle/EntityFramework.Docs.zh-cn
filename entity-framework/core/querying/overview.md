---
title: 查询的工作原理 - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
ms.technology: entity-framework-core
uid: core/querying/overview
ms.openlocfilehash: 1d28d215302625cf2b6788359527a93a77b7e9fd
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2018
ms.locfileid: "42447688"
---
# <a name="how-queries-work"></a>查询的工作原理

Entity Framework Core 使用语言集成查询 (LINQ) 来查询数据库中的数据。 通过 LINQ 可使用 C#（或你选择的 .NET 语言）基于派生上下文和实体类编写强类型查询。

## <a name="the-life-of-a-query"></a>查询的寿命

下面是每个查询所经历的过程的高级概述。

1. LINQ 查询由 Entity Framework Core 处理，用于生成已准备好由数据库提供程序处理的表示形式
   1. 结果会被缓存，以便每次执行查询时无需进行此处理
2. 结果会传递到数据库提供程序
   1. 数据库提供程序确定可以在数据库中评估查询的哪些部分
   2. 查询的这些部分会转换为数据库特定的查询语言（例如，关系数据库的 SQL）
   3. 一个或多个查询会发送到数据库和返回的结果集（结果是数据库中的值，而不是实体实例）
3. 对于结果集中的每一项
   1. 如果这是跟踪查询，EF 会检查数据是否表示已在上下文实例的更改跟踪器中的实体
      * 如果是，则会返回现有实体
      * 如果不是，则会创建新实体、设置更改跟踪并返回该新实体
   2. 如果这是非跟踪查询，EF 会检查数据是否表示已在此查询的结果集中的实体
      * 如果是，则会返回现有实体 <sup>(1)</sup>
      * 如果不是，则会创建新实体并返回该新实体

<sup>(1)</sup> 非跟踪查询使用弱引用跟踪已返回的实体。 如果具有相同标识的上一个结果超出范围，并运行垃圾回收，则可能会获得新的实体实例。

## <a name="when-queries-are-executed"></a>执行查询时

调用 LINQ 运算符时，只会生成查询的内存中表示形式。 使用结果时，查询只会发送到数据库。

导致查询发送到数据库的最常见操作如下：
* 在 `for` 循环中循环访问结果
* 使用 `ToList`、`ToArray`、`Single`、`Count` 等运算符
* 将查询结果数据绑定到 UI

> [!WARNING]  
> **始终验证用户输入：** 当 EF 抵御 SQL 注入攻击时，不会执行输入的任何常规验证。 因此，如果传递到 API、用于 LINQ 查询、分配给实体属性等的值来自不受信任的源，则应按照每个应用程序要求执行相应的验证。 这包括用于动态构造查询的所有用户输入。 即使在使用 LINQ 时，如果接受用于生成表达式的用户输入，则也会需要确保只能构造预期表达式。
