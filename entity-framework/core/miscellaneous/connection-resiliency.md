---
title: 连接复原的 EF Core
author: rowanmiller
ms.author: divega
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: 34ca1908257ed5544f2e134fa7686c9802fcebea
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949291"
---
# <a name="connection-resiliency"></a>连接复原

连接复原自动重试失败的数据库命令。 通过提供"执行策略"，它封装检测故障，然后重试命令所需的逻辑，该功能可以使用与任何数据库。 EF Core提供程序可以提供定制为其特定数据库失败条件和最佳的重试策略的执行策略。

例如，SQL Server 提供程序包括专门针对 SQL Server （包括 SQL Azure） 的执行策略。 它知道可以重试的异常类型，并且具有合理的默认值的最大重试，重试次数等之间的延迟。

配置您的上下文的选项时指定的执行策略。 这是通常位于`OnConfiguring`方法的派生上下文中，或在`Startup.cs`ASP.NET Core 应用程序。

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a>自定义执行策略

没有一种机制来注册您如果想要更改任何默认设置自己的自定义执行策略。

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

会自动重试失败的执行策略需要能够播放失败重试块中的每个操作。 启用重试次数后，通过 EF Core 执行每个操作将成为其自己的可重试操作。 也就是说，每个查询和每次调用`SaveChanges()`如果发生暂时性故障重试作为一个单元。

但是，如果你的代码启动了事务使用`BeginTransaction()`要定义自己的需要作为一个单元来处理的操作的组和事务内的所有内容可能需要进行播放应发生故障时。 如果您尝试执行此操作使用执行策略时，将收到如下所示的异常：

> InvalidOperationException： 配置的执行策略 SqlServerRetryingExecutionStrategy 不支持用户启动的事务。 使用由“DbContext.Database.CreateExecutionStrategy()”返回的执行策略执行事务（作为一个可回溯单元）中的所有操作。

解决方案是需要执行的委托表示的所有内容，手动调用执行策略。 如果发生暂时性故障，执行策略会再次调用委托。

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a>事务提交失败和幂等性问题

一般情况下，连接失败时当前事务会回滚。 但是，如果在连接断开时在事务正在将提交所生成的事务状态为未知。 请参阅此[博客文章](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx)的更多详细信息。

默认情况下，执行策略将重试该操作像该事务已回滚，但如果它不是这种情况将如果这导致异常的新的数据库状态不兼容，或者可能会导致**数据损坏**如果操作不依赖于特定状态，例如在插入新行自动生成键值。

有几种方法来解决此问题。

### <a name="option-1---do-almost-nothing"></a>选项 1-执行操作 （几乎） 执行任何操作

因此，可能会使应用程序直接失败，如果实际上发生这种情况可接受的事务提交过程中的连接故障的可能性较低。

但是，您需要以避免使用应用商店生成的键，为了确保添加重复的行而不是引发异常。 请考虑使用客户端生成的 GUID 值或客户端的值生成器。

### <a name="option-2---rebuild-application-state"></a>选项 2-重新生成应用程序状态

1. 放弃当前`DbContext`。
2. 创建一个新`DbContext`和从数据库中还原应用程序的状态。
3. 通知用户，最后一次操作可能不具有已成功完成。

### <a name="option-3---add-state-verification"></a>选项 3-添加状态验证

对于大多数更改数据库状态的操作就可以添加代码来检查它是否成功。 EF 提供了一个扩展方法来简化此过程- `IExecutionStrategy.ExecuteInTransaction`。

此方法开始和提交事务，还接受中的函数`verifySucceeded`事务提交期间发生暂时性错误时所调用的参数。

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> 这里`SaveChanges`调用使用了`acceptAllChangesOnSuccess`设置为`false`为了避免更改的状态`Blog`到实体`Unchanged`如果`SaveChanges`成功。 这样就可以重试相同的操作，如果提交失败并回滚事务。

### <a name="option-4---manually-track-the-transaction"></a>选项 4-手动跟踪事务

如果你需要使用应用商店生成的键或需要对不依赖于执行的操作处理提交失败的常规方法可以将每个事务分配提交失败时，将检查的 ID。

1. 将表添加到用于跟踪事务的状态的数据库。
2. 插入到表中每个事务的开始处的行。
3. 如果连接失败在提交期间，检查存在的数据库中的相应行。
4. 如果提交成功，删除对应的行，以避免对表的增长。

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> 请确保用于验证的上下文具有定义为连接很可能会再次在验证期间失败，如果事务在提交期间失败的执行策略。
