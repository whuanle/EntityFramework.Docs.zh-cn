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
# <a name="basic-save"></a>基本保存

了解如何添加、 修改和删除数据使用上下文和实体类。

> [!TIP]  
> 可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/)。

## <a name="adding-data"></a>添加数据

使用*DbSet.Add*方法将添加的实体类的新实例。 在调用时，将在数据库中插入数据*SaveChanges*。

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> 中所述的实体的完整图的所有工作的添加、 附加和更新方法传递给它们，[相关数据](related-data.md)部分。 或者，EntityEntry.State 属性可用来设置单个实体的状态。 例如 `context.Entry(blog).State = EntityState.Modified`。

## <a name="updating-data"></a>更新数据

EF 会自动检测到现有实体，由上下文跟踪所做的更改。 这包括你负载/查询从数据库中的实体和实体，之前添加并保存到数据库。

只需修改分配给属性的值，然后调用*SaveChanges*。

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a>删除数据

使用*DbSet.Remove*方法删除的实体类的实例。

如果该实体已存在数据库中，它将删除期间*SaveChanges*。 如果实体具有尚未保存到数据库 （即它将跟踪添加） 然后它将从上下文中移除，将无法再插入时*SaveChanges*调用。

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a>在单个 SaveChanges 的多个操作

你可以组合到一个调用到多个添加/更新/删除操作*SaveChanges*。

> [!NOTE]  
> 对于大多数数据库提供程序， *SaveChanges*是事务性的。 这意味着所有操作将成功或失败，将永远不会向左部分应用操作。

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
