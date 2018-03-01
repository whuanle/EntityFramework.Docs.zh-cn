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
# <a name="data-seeding"></a><span data-ttu-id="40dc6-102">数据种子设定</span><span class="sxs-lookup"><span data-stu-id="40dc6-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="40dc6-103">此功能是在 EF 核心 2.1 的新增功能。</span><span class="sxs-lookup"><span data-stu-id="40dc6-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="40dc6-104">数据种子设定允许若要提供初始数据填充数据库。</span><span class="sxs-lookup"><span data-stu-id="40dc6-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="40dc6-105">与不同在 ef6 更高版本，在 EF 核心中设定数据种子都与关联模型配置的一部分作为实体类型。</span><span class="sxs-lookup"><span data-stu-id="40dc6-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="40dc6-106">然后 EF 核心迁移可以自动计算什么插入、 更新或删除操作需要要在将数据库升级到新版本的模型时应用。</span><span class="sxs-lookup"><span data-stu-id="40dc6-106">Then EF Core migrations can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="40dc6-107">例如，可用于此配置的种子数据`Blog`中`OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="40dc6-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="40dc6-108">若要添加实体的关系的外键值需要指定。</span><span class="sxs-lookup"><span data-stu-id="40dc6-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="40dc6-109">经常外键属性是在卷影状态下，因此，若要能够设置匿名类的值应使用：</span><span class="sxs-lookup"><span data-stu-id="40dc6-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]
