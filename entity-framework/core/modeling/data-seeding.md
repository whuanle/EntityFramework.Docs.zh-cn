---
title: "数据种子设定的 EF 核心"
author: AndriySvyryd
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/data-seeding
ms.openlocfilehash: 693ffe44e247a79e01ac7c98a36472bf2c68d37f
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2018
---
# <a name="data-seeding"></a>数据种子设定

> [!NOTE]  
> 此功能是在 EF 核心 2.1 的新增功能。

数据种子设定允许若要提供初始数据填充数据库。 与不同在 ef6 更高版本，在 EF 核心中设定数据种子都与关联模型配置的一部分作为实体类型。 然后 EF 核心迁移可以自动计算什么插入、 更新或删除操作需要要在将数据库升级到新版本的模型时应用。

例如，可用于此配置的种子数据`Blog`中`OnModelCreating`:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

若要添加实体的关系的外键值需要指定。 经常外键属性是在卷影状态下，因此，若要能够设置匿名类的值应使用：

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]
