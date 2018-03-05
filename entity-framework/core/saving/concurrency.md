---
title: "处理并发冲突的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 03/03/2018
ms.technology: entity-framework-core
uid: core/saving/concurrency
ms.openlocfilehash: 288d9c6fced5ebbaa2c366248c68547502c3698e
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2018
---
# <a name="handling-concurrency-conflicts"></a>处理并发冲突

> [!NOTE]
> 此页文档在 EF 核心中的并发工作原理以及如何处理应用程序中的并发冲突。 请参阅[并发标记](xref:core/modeling/concurrency)有关如何在您的模型中配置并发标记的详细信息。

> [!TIP]
> 可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/)。

_数据库并发_指在其中多个进程或用户访问或在同一时间更改数据库中的相同数据的情况下。 _并发控制_引用特定的机制用于在场的并发更改的情况下确保数据一致性。

EF 核心实现_乐观并发控制_，这意味着它将允许多个进程或用户进行独立而无需同步开销的更改或锁定。 在理想情况下，这些更改将不会相互影响，因此能够成功。 在最差的情况下，两个或多个进程将尝试进行更改之间发生冲突，其中只有一应成功。

## <a name="how-concurrency-control-works-in-ef-core"></a>在 EF 核心中的并发控制工作原理

配置为并发标记用于实现乐观并发控制的属性： 每当期间执行 update 或 delete 操作`SaveChanges`，与原始数据库上的并发标记的值进行比较通过 EF 核心读取的值。

- 如果值匹配，则可以完成该操作。
- 如果值不匹配，EF 核心假设另一个用户已执行冲突的操作，并中止当前事务。

另一个用户已执行与当前操作冲突的操作时这种情况称为_并发冲突_。

数据库提供程序负责实现的并发标记值进行比较。

在关系数据库上 EF 核心包括中的并发标记的值的检查`WHERE`子句任何`UPDATE`或`DELETE`语句。 执行语句后, EF 核心读取受影响的行数。

如果未影响任何行，将检测并发冲突，并且 EF 核心`DbUpdateConcurrencyException`。

例如，我们可能想要配置`LastName`上`Person`是并发标记。 则针对用户的任何更新操作会包含在并发检查`WHERE`子句：

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a>解决并发冲突

继续前面的示例中，如果一个用户尝试保存对某些更改`Person`，但另一个用户已更改`LastName`将引发异常。

此时，应用程序可能只需通知用户更新未成功由于更改之间发生冲突，并移动上。 但它可能需要提示用户以确保此记录仍表示同一实际个人并重试该操作。

此过程是一种_解决并发冲突_。

解决并发冲突的方法包括合并从当前挂起的更改`DbContext`数据库中的值。 合并值取决于应用程序，并且可能将引导用户输入。

**有三个组可用于帮助解决并发冲突的值：**

* **当前值**是应用程序尝试写入数据库的值。

* **原始值**是最初检索到从数据库中执行的任何编辑之前的值。

* **数据库值**是当前数据库中存储的值。

处理并发冲突的常规方法是：

1. 捕获`DbUpdateConcurrencyException`期间`SaveChanges`。
2. 使用`DbUpdateConcurrencyException.Entries`准备一组新的更改受影响的实体。
3. 刷新并发标记，以反映数据库中的当前值的原始值。
4. 重试过程，直到不发生任何冲突。

在下面的示例中，`Person.FirstName`和`Person.LastName`并发令牌的形式安装程序。 没有`// TODO:`其中包括应用程序特定逻辑，以选择要保存的值的位置中的注释。

[!code-csharp[Main](../../../samples/core/Saving/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
