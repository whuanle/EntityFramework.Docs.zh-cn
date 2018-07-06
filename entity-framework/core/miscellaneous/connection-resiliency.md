---
title: 连接复原的 EF Core
author: rowanmiller
ms.author: divega
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: aec69577cd4b19fdebedb33ed6fd8f2665b0a032
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2017
ms.locfileid: "26053527"
---
# <a name="connection-resiliency"></a>连接复原

连接复原自动重试失败的数据库命令。 功能可以提供的"执行策略，"封装检测故障，然后重试命令所需的逻辑与任何数据库一起使用。 EF Core 提供程序可以提供定制为其特定数据库失败条件和最佳的重试策略的执行策略。

例如，SQL Server 提供程序包括专门定制到 SQL Server （包括 SQL Azure） 的执行策略。 它是感知的可以重试异常类型，并且具有最大重试，重试等之间的延迟合理的默认值。

配置你的上下文的选项时指定的执行策略。 这是通常位于`OnConfiguring`方法将派生的上下文，或在`Startup.cs`ASP.NET Core 应用程序。

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a>自定义执行策略

没有一种机制来注册你自己，如果你想要更改任何默认值的自定义执行策略。

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a>执行策略和事务

需要能够播放失败重试块中的每个操作失败将自动重试执行策略。 启用重试后，通过 EF Core 执行每个操作将成为其自己的可重试操作，即每个查询和每次调用`SaveChanges()`将重试作为一个单元如果发生暂时性故障。

但是，如果你的代码启动了事务使用`BeginTransaction()`要定义你自己的一组操作需要被视为一个单元，即在事务内的所有内容都将需要播放应发生故障时。 如果你尝试执行此操作使用的执行策略时，将收到如下所示的异常。

> InvalidOperationException： 配置的执行策略 SqlServerRetryingExecutionStrategy 不支持用户启动事务。 使用 DbContext.Database.CreateExecutionStrategy() 返回的执行策略在作为可重试单元事务中执行所有操作。

解决方案是使用委托表示的所有内容，需要执行手动调用执行策略。 如果发生暂时性故障，则执行策略将再次调用委托。

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a>事务提交失败和幂等性问题

一般情况下，连接失败时当前事务将回滚。 但是，如果在事务处于断开连接是否将提交所生成的事务的状态为未知。 请参阅此[博客文章](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx)有关详细信息。

默认情况下，执行策略将重试该操作就像该事务已回滚，但如果它不是这样这将导致异常如果新数据库的状态不兼容，或者可能会导致**数据损坏**如果操作不依赖于特定的状态，例如，在插入具有自动生成的密钥值的新行时。

有多种与此处理。

### <a name="option-1---do-almost-nothing"></a>选项 1-是否执行任何操作 （几乎）

事务提交过程中的连接故障的可能性较低，因此它可能是可接受的应用程序以直接失败，如果实际发生这种情况。

但是，你需要避免使用由存储生成的键，以确保添加重复的行而不是引发异常。 请考虑使用由客户端生成的 GUID 值或客户端值生成器。

### <a name="option-2---rebuild-application-state"></a>选项 2-重新生成应用程序状态

1. 放弃当前`DbContext`。
2. 创建一个新`DbContext`和从数据库中还原你的应用程序的状态。
3. 通知用户，最后一个操作可能不具有已成功完成。

### <a name="option-3---add-state-verification"></a>选项 3-添加状态验证

对于大多数更改数据库状态的操作则可以添加检查它是否成功的代码。 EF 提供一个扩展方法来简化此过程- `IExecutionStrategy.ExecuteInTransaction`。

此方法开始和提交事务，还接受中的函数`verifySucceeded`在事务提交期间发生暂时性错误时调用的参数。

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> 此处`SaveChanges`使用调用`acceptAllChangesOnSuccess`设置为`false`为了避免更改的状态`Blog`实体到`Unchanged`如果`SaveChanges`成功。 这样就可以重试相同的操作如果提交失败并回滚事务。

### <a name="option-4---manually-track-the-transaction"></a>选项 4-手动跟踪的事务

如果你需要使用由存储生成的键或需要不依赖于执行的操作中泛型的方式来处理提交故障的每个事务无法分配提交失败时检查的 ID。

1. 将表添加到用于跟踪事务的状态的数据库。
2. 将行插入表中的每个事务的开头。
3. 如果连接失败的提交，检查数据库中的相应行存在。
4. 如果提交成功，删除相应的行，以避免表的增长。

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> 请确保用于验证的上下文具有定义为连接是可能会在验证期间再次失败，如果在事务提交过程失败的执行策略。
