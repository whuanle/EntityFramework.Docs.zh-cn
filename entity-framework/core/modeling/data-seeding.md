---
title: 数据种子设定的 EF Core
author: AndriySvyryd
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/data-seeding
ms.openlocfilehash: 7028e1923152b27f56721dab75aae8b9c2f5ad75
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2018
ms.locfileid: "34163195"
---
# <a name="data-seeding"></a><span data-ttu-id="aa822-102">数据种子设定</span><span class="sxs-lookup"><span data-stu-id="aa822-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="aa822-103">此功能是在 EF Core 2.1 的新增功能。</span><span class="sxs-lookup"><span data-stu-id="aa822-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="aa822-104">数据种子设定允许若要提供初始数据填充数据库。</span><span class="sxs-lookup"><span data-stu-id="aa822-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="aa822-105">与不同在 ef6 更高版本，在 EF Core中设定数据种子都与关联模型配置的一部分作为实体类型。</span><span class="sxs-lookup"><span data-stu-id="aa822-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="aa822-106">然后 EF Core[迁移](xref:core/managing-schemas/migrations/index)什么插入、 更新或删除操作需要时将数据库升级到新版本的模型的应用可以自动计算。</span><span class="sxs-lookup"><span data-stu-id="aa822-106">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="aa822-107">例如，可用于此配置的种子数据`Blog`中`OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="aa822-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="aa822-108">若要添加实体的关系的外键值需要指定。</span><span class="sxs-lookup"><span data-stu-id="aa822-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="aa822-109">经常外键属性是在卷影状态下，因此，若要能够设置匿名类的值应使用：</span><span class="sxs-lookup"><span data-stu-id="aa822-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="aa822-110">一旦已添加实体，则建议使用[迁移](xref:core/managing-schemas/migrations/index)以应用更改。</span><span class="sxs-lookup"><span data-stu-id="aa822-110">Once entities have been added, it is recommended to use [migrations](xref:core/managing-schemas/migrations/index) to apply changes.</span></span> 

<span data-ttu-id="aa822-111">或者，可以使用`context.Database.EnsureCreated()`以创建新的数据库包含种子数据，例如对于一个测试数据库，或者在使用内存中提供程序。</span><span class="sxs-lookup"><span data-stu-id="aa822-111">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider.</span></span> <span data-ttu-id="aa822-112">请注意，如果数据库已存在，`EnsureCreated()`都将更新架构或数据库中的种子数据。</span><span class="sxs-lookup"><span data-stu-id="aa822-112">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor the seed data in the database.</span></span>
