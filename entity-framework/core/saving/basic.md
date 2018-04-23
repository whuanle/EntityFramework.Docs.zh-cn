---
title: 基本保存的 EF 核心
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: deead323301dc4a0ee0748b4536ddff4596b99e6
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/16/2018
---
# <a name="basic-save"></a><span data-ttu-id="40bef-102">基本保存</span><span class="sxs-lookup"><span data-stu-id="40bef-102">Basic Save</span></span>

<span data-ttu-id="40bef-103">了解如何添加、 修改和删除数据使用上下文和实体类。</span><span class="sxs-lookup"><span data-stu-id="40bef-103">Learn how to add, modify, and remove data using your context and entity classes.</span></span>

> [!TIP]  
> <span data-ttu-id="40bef-104">可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/)。</span><span class="sxs-lookup"><span data-stu-id="40bef-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) on GitHub.</span></span>

## <a name="adding-data"></a><span data-ttu-id="40bef-105">添加数据</span><span class="sxs-lookup"><span data-stu-id="40bef-105">Adding Data</span></span>

<span data-ttu-id="40bef-106">使用*DbSet.Add*方法将添加的实体类的新实例。</span><span class="sxs-lookup"><span data-stu-id="40bef-106">Use the *DbSet.Add* method to add new instances of your entity classes.</span></span> <span data-ttu-id="40bef-107">在调用时，将在数据库中插入数据*SaveChanges*。</span><span class="sxs-lookup"><span data-stu-id="40bef-107">The data will be inserted in the database when you call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> <span data-ttu-id="40bef-108">中所述的实体的完整图的所有工作的添加、 附加和更新方法传递给它们，[相关数据](related-data.md)部分。</span><span class="sxs-lookup"><span data-stu-id="40bef-108">The Add, Attach, and Update methods all work on the full graph of entities passed to them, as described in the [Related Data](related-data.md) section.</span></span> <span data-ttu-id="40bef-109">或者，EntityEntry.State 属性可用来设置单个实体的状态。</span><span class="sxs-lookup"><span data-stu-id="40bef-109">Alternately, the EntityEntry.State property can be used to set the state of just a single entity.</span></span> <span data-ttu-id="40bef-110">例如 `context.Entry(blog).State = EntityState.Modified`。</span><span class="sxs-lookup"><span data-stu-id="40bef-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="updating-data"></a><span data-ttu-id="40bef-111">更新数据</span><span class="sxs-lookup"><span data-stu-id="40bef-111">Updating Data</span></span>

<span data-ttu-id="40bef-112">EF 会自动检测到现有实体，由上下文跟踪所做的更改。</span><span class="sxs-lookup"><span data-stu-id="40bef-112">EF will automatically detect changes made to an existing entity that is tracked by the context.</span></span> <span data-ttu-id="40bef-113">这包括你负载/查询从数据库中的实体和实体，之前添加并保存到数据库。</span><span class="sxs-lookup"><span data-stu-id="40bef-113">This includes entities that you load/query from the database, and entities that were previously added and saved to the database.</span></span>

<span data-ttu-id="40bef-114">只需修改分配给属性的值，然后调用*SaveChanges*。</span><span class="sxs-lookup"><span data-stu-id="40bef-114">Simply modify the values assigned to properties and then call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a><span data-ttu-id="40bef-115">删除数据</span><span class="sxs-lookup"><span data-stu-id="40bef-115">Deleting Data</span></span>

<span data-ttu-id="40bef-116">使用*DbSet.Remove*方法删除的实体类的实例。</span><span class="sxs-lookup"><span data-stu-id="40bef-116">Use the *DbSet.Remove* method to delete instances of you entity classes.</span></span>

<span data-ttu-id="40bef-117">如果该实体已存在数据库中，它将删除期间*SaveChanges*。</span><span class="sxs-lookup"><span data-stu-id="40bef-117">If the entity already exists in the database, it will be deleted during *SaveChanges*.</span></span> <span data-ttu-id="40bef-118">如果实体具有尚未保存到数据库 （即它将跟踪添加） 然后它将从上下文中移除，将无法再插入时*SaveChanges*调用。</span><span class="sxs-lookup"><span data-stu-id="40bef-118">If the entity has not yet been saved to the database (i.e. it is tracked as added) then it will be removed from the context and will no longer be inserted when *SaveChanges* is called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a><span data-ttu-id="40bef-119">在单个 SaveChanges 的多个操作</span><span class="sxs-lookup"><span data-stu-id="40bef-119">Multiple Operations in a single SaveChanges</span></span>

<span data-ttu-id="40bef-120">你可以组合到一个调用到多个添加/更新/删除操作*SaveChanges*。</span><span class="sxs-lookup"><span data-stu-id="40bef-120">You can combine multiple Add/Update/Remove operations into a single call to *SaveChanges*.</span></span>

> [!NOTE]  
> <span data-ttu-id="40bef-121">对于大多数数据库提供程序， *SaveChanges*是事务性的。</span><span class="sxs-lookup"><span data-stu-id="40bef-121">For most database providers, *SaveChanges* is transactional.</span></span> <span data-ttu-id="40bef-122">这意味着所有操作将成功或失败，将永远不会向左部分应用操作。</span><span class="sxs-lookup"><span data-stu-id="40bef-122">This means  all the operations will either succeed or fail and the operations will never be left partially applied.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
