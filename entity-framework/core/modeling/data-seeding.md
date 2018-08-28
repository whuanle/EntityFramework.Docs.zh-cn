---
title: 数据种子设定的 EF Core
author: AndriySvyryd
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 48ba2389de4b57dbe4c2b2124911c71440d45556
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994474"
---
# <a name="data-seeding"></a>数据种子设定

> [!NOTE]  
> 此功能是在 EF Core 2.1 的新增功能。

数据种子设定允许以提供初始数据来填充数据库。 与不同在 ef6 更高版本，在 EF Core 中设定数据种子都与关联模型配置的一部分作为实体类型。 然后 EF Core [迁移](xref:core/managing-schemas/migrations/index)什么插入、 更新或删除操作需要时将数据库升级到新版本的模型的应用可以自动计算。

例如，可用于此配置的种子数据`Blog`在`OnModelCreating`:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

若要添加实体具有关系的外键值的需要来指定。 频繁的外键属性是在卷影状态下，因此，若要能够设置匿名类的值应使用：

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

一旦添加了实体，则建议使用[迁移](xref:core/managing-schemas/migrations/index)以应用更改。 

或者，可以使用`context.Database.EnsureCreated()`若要创建新的数据库包含种子数据，例如对于测试数据库，或者在使用内存中提供程序。 请注意，如果数据库已存在，`EnsureCreated()`既不会更新该架构，也不在数据库中的种子数据。
