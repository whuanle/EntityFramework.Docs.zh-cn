---
title: "基本保存的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: e99d755b8f7c42b15a73cfdb6a2f8999a405a739
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="basic-save"></a><span data-ttu-id="9ff8d-102">基本保存</span><span class="sxs-lookup"><span data-stu-id="9ff8d-102">Basic Save</span></span>

<span data-ttu-id="9ff8d-103">了解如何添加、 修改和删除数据使用上下文和实体类。</span><span class="sxs-lookup"><span data-stu-id="9ff8d-103">Learn how to add, modify, and remove data using your context and entity classes.</span></span>

> [!TIP]  
> <span data-ttu-id="9ff8d-104">你可以查看这篇文章[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/)GitHub 上。</span><span class="sxs-lookup"><span data-stu-id="9ff8d-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) on GitHub.</span></span>

## <a name="adding-data"></a><span data-ttu-id="9ff8d-105">添加数据</span><span class="sxs-lookup"><span data-stu-id="9ff8d-105">Adding Data</span></span>

<span data-ttu-id="9ff8d-106">使用*DbSet.Add*方法将添加的实体类的新实例。</span><span class="sxs-lookup"><span data-stu-id="9ff8d-106">Use the *DbSet.Add* method to add new instances of your entity classes.</span></span> <span data-ttu-id="9ff8d-107">在调用时，将在数据库中插入数据*SaveChanges*。</span><span class="sxs-lookup"><span data-stu-id="9ff8d-107">The data will be inserted in the database when you call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

## <a name="updating-data"></a><span data-ttu-id="9ff8d-108">更新数据</span><span class="sxs-lookup"><span data-stu-id="9ff8d-108">Updating Data</span></span>

<span data-ttu-id="9ff8d-109">EF 会自动检测到现有实体，由上下文跟踪所做的更改。</span><span class="sxs-lookup"><span data-stu-id="9ff8d-109">EF will automatically detect changes made to an existing entity that is tracked by the context.</span></span> <span data-ttu-id="9ff8d-110">这包括你负载/查询从数据库中的实体和实体，之前添加并保存到数据库。</span><span class="sxs-lookup"><span data-stu-id="9ff8d-110">This includes entities that you load/query from the database, and entities that were previously added and saved to the database.</span></span>

<span data-ttu-id="9ff8d-111">只需修改分配给属性的值，然后调用*SaveChanges*。</span><span class="sxs-lookup"><span data-stu-id="9ff8d-111">Simply modify the values assigned to properties and then call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a><span data-ttu-id="9ff8d-112">删除数据</span><span class="sxs-lookup"><span data-stu-id="9ff8d-112">Deleting Data</span></span>

<span data-ttu-id="9ff8d-113">使用*DbSet.Remove*方法删除的实体类的实例。</span><span class="sxs-lookup"><span data-stu-id="9ff8d-113">Use the *DbSet.Remove* method to delete instances of you entity classes.</span></span>

<span data-ttu-id="9ff8d-114">如果该实体已存在数据库中，它将删除期间*SaveChanges*。</span><span class="sxs-lookup"><span data-stu-id="9ff8d-114">If the entity already exists in the database, it will be deleted during *SaveChanges*.</span></span> <span data-ttu-id="9ff8d-115">如果实体具有尚未保存到数据库 （即它将跟踪添加） 然后它将从上下文中移除，将无法再插入时*SaveChanges*调用。</span><span class="sxs-lookup"><span data-stu-id="9ff8d-115">If the entity has not yet been saved to the database (i.e. it is tracked as added) then it will be removed from the context and will no longer be inserted when *SaveChanges* is called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a><span data-ttu-id="9ff8d-116">在单个 SaveChanges 的多个操作</span><span class="sxs-lookup"><span data-stu-id="9ff8d-116">Multiple Operations in a single SaveChanges</span></span>

<span data-ttu-id="9ff8d-117">你可以组合到一个调用到多个添加/更新/删除操作*SaveChanges*。</span><span class="sxs-lookup"><span data-stu-id="9ff8d-117">You can combine multiple Add/Update/Remove operations into a single call to *SaveChanges*.</span></span>

> [!NOTE]  
> <span data-ttu-id="9ff8d-118">对于大多数数据库提供程序， *SaveChanges*是事务性的。</span><span class="sxs-lookup"><span data-stu-id="9ff8d-118">For most database providers, *SaveChanges* is transactional.</span></span> <span data-ttu-id="9ff8d-119">这意味着所有操作将成功或失败，将永远不会向左部分应用操作。</span><span class="sxs-lookup"><span data-stu-id="9ff8d-119">This means  all the operations will either succeed or fail and the operations will never be left partially applied.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
