---
title: "如何查询工作的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
ms.technology: entity-framework-core
uid: core/querying/overview
ms.openlocfilehash: 7fd2940d559f82016d7a8fc3fdcf3af0d5b8bc8f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="how-queries-work"></a>查询的工作原理

实体框架核心使用语言集成查询 (LINQ) 查询数据从数据库。 LINQ，可使用 C# （或所用.NET 语言的） 来编写基于派生的上下文和实体类的强类型的查询。

## <a name="the-life-of-a-query"></a>查询的生命周期

以下是过程的每个查询所经历的高级别概述。

1. LINQ 查询处理由实体框架核心以生成已准备好处理数据库提供程序的表示形式
   1. 以便此处理不需要每次执行查询时进行缓存的结果
2. 结果传递给数据库提供程序
   1. 数据库提供程序标识查询的哪些部分可以计算数据库中
   2. 这些部分的查询将转换为数据库特定查询语言 (例如关系数据库的 SQL)
   3. 一个或多个查询发送到数据库和返回的结果集 （结果是从数据库中不实体实例的值）
3. 在结果集中的每个项
   1. 如果这是跟踪查询，EF 检查数据是否表示已在上下文实例的更改跟踪实体
      * 如果是这样，现有实体，并返回
      * 否则，创建新实体、 更改跟踪，则安装程序，并返回新的实体
   2. 如果这是无跟踪查询，EF 会检查数据是否表示已中的此查询的结果集的实体
      * 如果现有实体，因此，并返回<sup>(1)</sup>
      * 否则，创建并返回一个新实体

<sup>(1)</sup>没有跟踪的查询使用弱引用可跟踪已返回的实体。 如果前一个结果具有相同标识号超出范围，且垃圾回收运行，则可能获得的新实体实例。

## <a name="when-queries-are-executed"></a>当执行查询

在调用 LINQ 运算符时，您正在只需生成内存中表示的查询。 使用结果时，查询将只发送到数据库。

导致发送给数据库的查询的最常见操作如下：
* 循环中的结果`for`循环
* 使用运算符，如`ToList`， `ToArray`， `Single`，`Count`
* 数据绑定到 UI 查询的结果

> [!WARNING]  
> **始终验证用户输入：**时的 EF 提供保护以防止 SQL 注入攻击，它并不输入任何常规验证。 因此如果值传递到 Api，在 LINQ 查询中，分配给等实体属性，使用来自不受信任的源则相应的验证，每个应用程序的要求，应执行。 这包括用于动态构造查询的所有用户输入。 即使是在使用 LINQ 中，如果您接受用户输入用于生成的表达式需要确保只有预期表达式比可以用来构造。
