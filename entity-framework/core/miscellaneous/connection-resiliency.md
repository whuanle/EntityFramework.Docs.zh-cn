---
title: 连接复原的 EF Core
author: rowanmiller
ms.author: divega
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: dae646e39b4dbd96b34f47582f9b2aa531cf88a7
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614332"
---
# <a name="connection-resiliency"></a><span data-ttu-id="b257c-102">连接弹性</span><span class="sxs-lookup"><span data-stu-id="b257c-102">Connection Resiliency</span></span>

<span data-ttu-id="b257c-103">连接复原自动重试失败的数据库命令。</span><span class="sxs-lookup"><span data-stu-id="b257c-103">Connection resiliency automatically retries failed database commands.</span></span> <span data-ttu-id="b257c-104">通过提供"执行策略"，它封装检测故障，然后重试命令所需的逻辑，该功能可以使用与任何数据库。</span><span class="sxs-lookup"><span data-stu-id="b257c-104">The feature can be used with any database by supplying an "execution strategy", which encapsulates the logic necessary to detect failures and retry commands.</span></span> <span data-ttu-id="b257c-105">EF Core提供程序可以提供定制为其特定数据库失败条件和最佳的重试策略的执行策略。</span><span class="sxs-lookup"><span data-stu-id="b257c-105">EF Core providers can supply execution strategies tailored to their specific database failure conditions and optimal retry policies.</span></span>

<span data-ttu-id="b257c-106">例如，SQL Server 提供程序包括专门针对 SQL Server （包括 SQL Azure） 的执行策略。</span><span class="sxs-lookup"><span data-stu-id="b257c-106">As an example, the SQL Server provider includes an execution strategy that is specifically tailored to SQL Server (including SQL Azure).</span></span> <span data-ttu-id="b257c-107">它知道可以重试的异常类型，并且具有合理的默认值的最大重试，重试次数等之间的延迟。</span><span class="sxs-lookup"><span data-stu-id="b257c-107">It is aware of the exception types that can be retried and has sensible defaults for maximum retries, delay between retries, etc.</span></span>

<span data-ttu-id="b257c-108">配置您的上下文的选项时指定的执行策略。</span><span class="sxs-lookup"><span data-stu-id="b257c-108">An execution strategy is specified when configuring the options for your context.</span></span> <span data-ttu-id="b257c-109">这是通常位于`OnConfiguring`方法的派生上下文中，或在`Startup.cs`ASP.NET Core 应用程序。</span><span class="sxs-lookup"><span data-stu-id="b257c-109">This is typically in the `OnConfiguring` method of your derived context, or in `Startup.cs` for an ASP.NET Core application.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a><span data-ttu-id="b257c-110">自定义执行策略</span><span class="sxs-lookup"><span data-stu-id="b257c-110">Custom execution strategy</span></span>

<span data-ttu-id="b257c-111">没有一种机制来注册您如果想要更改任何默认设置自己的自定义执行策略。</span><span class="sxs-lookup"><span data-stu-id="b257c-111">There is a mechanism to register a custom execution strategy of your own if you wish to change any of the defaults.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a><span data-ttu-id="b257c-112">执行策略和事务</span><span class="sxs-lookup"><span data-stu-id="b257c-112">Execution strategies and transactions</span></span>

<span data-ttu-id="b257c-113">会自动重试失败的执行策略需要能够播放失败的重试块中的每个操作。</span><span class="sxs-lookup"><span data-stu-id="b257c-113">An execution strategy that automatically retries on failures needs to be able to play back each operation in a retry block that fails.</span></span> <span data-ttu-id="b257c-114">启用重试次数后，通过 EF Core 执行每个操作将成为其自己的可重试操作。</span><span class="sxs-lookup"><span data-stu-id="b257c-114">When retries are enabled, each operation you perform via EF Core becomes its own retriable operation.</span></span> <span data-ttu-id="b257c-115">也就是说，每个查询和每次调用`SaveChanges()`如果发生暂时性故障重试作为一个单元。</span><span class="sxs-lookup"><span data-stu-id="b257c-115">That is, each query and each call to `SaveChanges()` will be retried as a unit if a transient failure occurs.</span></span>

<span data-ttu-id="b257c-116">但是，如果你的代码启动了事务使用`BeginTransaction()`要定义自己的需要作为一个单元来处理的操作的组和事务内的所有内容可能需要进行播放应发生故障时。</span><span class="sxs-lookup"><span data-stu-id="b257c-116">However, if your code initiates a transaction using `BeginTransaction()` you are defining your own group of operations that need to be treated as a unit, and everything inside the transaction would need to be played back shall a failure occur.</span></span> <span data-ttu-id="b257c-117">如果您尝试执行此操作使用执行策略时，将收到如下所示的异常：</span><span class="sxs-lookup"><span data-stu-id="b257c-117">You will receive an exception like the following if you attempt to do this when using an execution strategy:</span></span>

> <span data-ttu-id="b257c-118">InvalidOperationException： 配置的执行策略 SqlServerRetryingExecutionStrategy 不支持用户启动的事务。</span><span class="sxs-lookup"><span data-stu-id="b257c-118">InvalidOperationException: The configured execution strategy 'SqlServerRetryingExecutionStrategy' does not support user initiated transactions.</span></span> <span data-ttu-id="b257c-119">使用由“DbContext.Database.CreateExecutionStrategy()”返回的执行策略执行事务（作为一个可回溯单元）中的所有操作。</span><span class="sxs-lookup"><span data-stu-id="b257c-119">Use the execution strategy returned by 'DbContext.Database.CreateExecutionStrategy()' to execute all the operations in the transaction as a retriable unit.</span></span>

<span data-ttu-id="b257c-120">解决方案是需要执行的委托表示的所有内容，手动调用执行策略。</span><span class="sxs-lookup"><span data-stu-id="b257c-120">The solution is to manually invoke the execution strategy with a delegate representing everything that needs to be executed.</span></span> <span data-ttu-id="b257c-121">如果发生暂时性故障，执行策略会再次调用委托。</span><span class="sxs-lookup"><span data-stu-id="b257c-121">If a transient failure occurs, the execution strategy will invoke the delegate again.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a><span data-ttu-id="b257c-122">事务提交失败和幂等性问题</span><span class="sxs-lookup"><span data-stu-id="b257c-122">Transaction commit failure and the idempotency issue</span></span>

<span data-ttu-id="b257c-123">一般情况下，连接失败时当前事务会回滚。</span><span class="sxs-lookup"><span data-stu-id="b257c-123">In general, when there is a connection failure the current transaction is rolled back.</span></span> <span data-ttu-id="b257c-124">但是，如果在连接断开时在事务正在将提交所生成的事务状态为未知。</span><span class="sxs-lookup"><span data-stu-id="b257c-124">However, if the connection is dropped while the transaction is being committed the resulting state of the transaction is unknown.</span></span> <span data-ttu-id="b257c-125">请参阅此[博客文章](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx)的更多详细信息。</span><span class="sxs-lookup"><span data-stu-id="b257c-125">See this [blog post](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) for more details.</span></span>

<span data-ttu-id="b257c-126">默认情况下，执行策略将重试该操作像该事务已回滚，但如果它不是这种情况将如果这导致异常的新的数据库状态不兼容，或者可能会导致**数据损坏**如果操作不依赖于特定状态，例如在插入新行自动生成键值。</span><span class="sxs-lookup"><span data-stu-id="b257c-126">By default, the execution strategy will retry the operation as if the transaction was rolled back, but if it's not the case this will result in an exception if the new database state is incompatible or could lead to **data corruption** if the operation does not rely on a particular state, for example when inserting a new row with auto-generated key values.</span></span>

<span data-ttu-id="b257c-127">有几种方法来解决此问题。</span><span class="sxs-lookup"><span data-stu-id="b257c-127">There are several ways to deal with this.</span></span>

### <a name="option-1---do-almost-nothing"></a><span data-ttu-id="b257c-128">选项 1-执行操作 （几乎） 执行任何操作</span><span class="sxs-lookup"><span data-stu-id="b257c-128">Option 1 - Do (almost) nothing</span></span>

<span data-ttu-id="b257c-129">因此，可能会使应用程序直接失败，如果实际上发生这种情况可接受的事务提交过程中的连接故障的可能性较低。</span><span class="sxs-lookup"><span data-stu-id="b257c-129">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>

<span data-ttu-id="b257c-130">但是，您需要以避免使用应用商店生成的键，为了确保添加重复的行而不是引发异常。</span><span class="sxs-lookup"><span data-stu-id="b257c-130">However, you need to avoid using store-generated keys in order to ensure that an exception is thrown instead of adding a duplicate row.</span></span> <span data-ttu-id="b257c-131">请考虑使用客户端生成的 GUID 值或客户端的值生成器。</span><span class="sxs-lookup"><span data-stu-id="b257c-131">Consider using a client-generated GUID value or a client-side value generator.</span></span>

### <a name="option-2---rebuild-application-state"></a><span data-ttu-id="b257c-132">选项 2-重新生成应用程序状态</span><span class="sxs-lookup"><span data-stu-id="b257c-132">Option 2 - Rebuild application state</span></span>

1. <span data-ttu-id="b257c-133">放弃当前`DbContext`。</span><span class="sxs-lookup"><span data-stu-id="b257c-133">Discard the current `DbContext`.</span></span>
2. <span data-ttu-id="b257c-134">创建一个新`DbContext`和从数据库中还原应用程序的状态。</span><span class="sxs-lookup"><span data-stu-id="b257c-134">Create a new `DbContext` and restore the state of your application from the database.</span></span>
3. <span data-ttu-id="b257c-135">通知用户，最后一次操作可能不具有已成功完成。</span><span class="sxs-lookup"><span data-stu-id="b257c-135">Inform the user that the last operation might not have been completed successfully.</span></span>

### <a name="option-3---add-state-verification"></a><span data-ttu-id="b257c-136">选项 3-添加状态验证</span><span class="sxs-lookup"><span data-stu-id="b257c-136">Option 3 - Add state verification</span></span>

<span data-ttu-id="b257c-137">对于大多数更改数据库状态的操作就可以添加代码来检查它是否成功。</span><span class="sxs-lookup"><span data-stu-id="b257c-137">For most of the operations that change the database state it is possible to add code that checks whether it succeeded.</span></span> <span data-ttu-id="b257c-138">EF 提供了一个扩展方法来简化此过程- `IExecutionStrategy.ExecuteInTransaction`。</span><span class="sxs-lookup"><span data-stu-id="b257c-138">EF provides an extension method to make this easier - `IExecutionStrategy.ExecuteInTransaction`.</span></span>

<span data-ttu-id="b257c-139">此方法开始和提交事务，还接受中的函数`verifySucceeded`事务提交期间发生暂时性错误时所调用的参数。</span><span class="sxs-lookup"><span data-stu-id="b257c-139">This method begins and commits a transaction and also accepts a function in the `verifySucceeded` parameter that is invoked when a transient error occurs during the transaction commit.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> <span data-ttu-id="b257c-140">这里`SaveChanges`调用使用了`acceptAllChangesOnSuccess`设置为`false`为了避免更改的状态`Blog`到实体`Unchanged`如果`SaveChanges`成功。</span><span class="sxs-lookup"><span data-stu-id="b257c-140">Here `SaveChanges` is invoked with `acceptAllChangesOnSuccess` set to `false` to avoid changing the state of the `Blog` entity to `Unchanged` if `SaveChanges` succeeds.</span></span> <span data-ttu-id="b257c-141">这样就可以重试相同的操作，如果提交失败并回滚事务。</span><span class="sxs-lookup"><span data-stu-id="b257c-141">This allows to retry the same operation if the commit fails and the transaction is rolled back.</span></span>

### <a name="option-4---manually-track-the-transaction"></a><span data-ttu-id="b257c-142">选项 4-手动跟踪事务</span><span class="sxs-lookup"><span data-stu-id="b257c-142">Option 4 - Manually track the transaction</span></span>

<span data-ttu-id="b257c-143">如果你需要使用应用商店生成的键或需要对不依赖于执行的操作处理提交失败的常规方法可以将每个事务分配提交失败时，将检查的 ID。</span><span class="sxs-lookup"><span data-stu-id="b257c-143">If you need to use store-generated keys or need a generic way of handling commit failures that doesn't depend on the operation performed each transaction could be assigned an ID that is checked when the commit fails.</span></span>

1. <span data-ttu-id="b257c-144">将表添加到用于跟踪事务的状态的数据库。</span><span class="sxs-lookup"><span data-stu-id="b257c-144">Add a table to the database used to track the status of the transactions.</span></span>
2. <span data-ttu-id="b257c-145">插入到表中每个事务的开始处的行。</span><span class="sxs-lookup"><span data-stu-id="b257c-145">Insert a row into the table at the beginning of each transaction.</span></span>
3. <span data-ttu-id="b257c-146">如果连接失败在提交期间，检查存在的数据库中的相应行。</span><span class="sxs-lookup"><span data-stu-id="b257c-146">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>
4. <span data-ttu-id="b257c-147">如果提交成功，删除对应的行，以避免对表的增长。</span><span class="sxs-lookup"><span data-stu-id="b257c-147">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> <span data-ttu-id="b257c-148">请确保用于验证的上下文具有定义为连接很可能会再次在验证期间失败，如果事务在提交期间失败的执行策略。</span><span class="sxs-lookup"><span data-stu-id="b257c-148">Make sure that the context used for the verification has an execution strategy defined as the connection is likely to fail again during verification if it failed during transaction commit.</span></span>
