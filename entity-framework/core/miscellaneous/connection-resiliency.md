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
# <a name="connection-resiliency"></a><span data-ttu-id="0a1ab-102">连接复原</span><span class="sxs-lookup"><span data-stu-id="0a1ab-102">Connection Resiliency</span></span>

<span data-ttu-id="0a1ab-103">连接复原自动重试失败的数据库命令。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-103">Connection resiliency automatically retries failed database commands.</span></span> <span data-ttu-id="0a1ab-104">功能可以提供的"执行策略，"封装检测故障，然后重试命令所需的逻辑与任何数据库一起使用。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-104">The feature can be used with any database by supplying an "execution strategy", which encapsulates the logic necessary to detect failures and retry commands.</span></span> <span data-ttu-id="0a1ab-105">EF Core提供程序可以提供定制为其特定数据库失败条件和最佳的重试策略的执行策略。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-105">EF Core providers can supply execution strategies tailored to their specific database failure conditions and optimal retry policies.</span></span>

<span data-ttu-id="0a1ab-106">例如，SQL Server 提供程序包括专门定制到 SQL Server （包括 SQL Azure） 的执行策略。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-106">As an example, the SQL Server provider includes an execution strategy that is specifically tailored to SQL Server (including SQL Azure).</span></span> <span data-ttu-id="0a1ab-107">它是感知的可以重试异常类型，并且具有最大重试，重试等之间的延迟合理的默认值。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-107">It is aware of the exception types that can be retried and has sensible defaults for maximum retries, delay between retries, etc.</span></span>

<span data-ttu-id="0a1ab-108">配置你的上下文的选项时指定的执行策略。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-108">An execution strategy is specified when configuring the options for your context.</span></span> <span data-ttu-id="0a1ab-109">这是通常位于`OnConfiguring`方法将派生的上下文，或在`Startup.cs`ASP.NET Core 应用程序。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-109">This is typically in the `OnConfiguring` method of your derived context, or in `Startup.cs` for an ASP.NET Core application.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a><span data-ttu-id="0a1ab-110">自定义执行策略</span><span class="sxs-lookup"><span data-stu-id="0a1ab-110">Custom execution strategy</span></span>

<span data-ttu-id="0a1ab-111">没有一种机制来注册你自己，如果你想要更改任何默认值的自定义执行策略。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-111">There is a mechanism to register a custom execution strategy of your own if you wish to change any of the defaults.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a><span data-ttu-id="0a1ab-112">执行策略和事务</span><span class="sxs-lookup"><span data-stu-id="0a1ab-112">Execution strategies and transactions</span></span>

<span data-ttu-id="0a1ab-113">需要能够播放失败重试块中的每个操作失败将自动重试执行策略。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-113">An execution strategy that automatically retries on failures needs to be able to play back each operation in an retry block that fails.</span></span> <span data-ttu-id="0a1ab-114">启用重试后，通过 EF Core执行每个操作将成为其自己的可重试操作，即每个查询和每次调用`SaveChanges()`将重试作为一个单元如果发生暂时性故障。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-114">When retries are enabled, each operation you perform via EF Core becomes its own retriable operation, i.e. each query and each call to `SaveChanges()` will be retried as a unit if a transient failure occurs.</span></span>

<span data-ttu-id="0a1ab-115">但是，如果你的代码启动了事务使用`BeginTransaction()`要定义你自己的一组操作需要被视为一个单元，即在事务内的所有内容都将需要播放应发生故障时。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-115">However, if your code initiates a transaction using `BeginTransaction()` you are defining your own group of operations that need to be treated as a unit, i.e. everything inside the transaction would need to be played back shall a failure occur.</span></span> <span data-ttu-id="0a1ab-116">如果你尝试执行此操作使用的执行策略时，将收到如下所示的异常。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-116">You will receive an exception like the following if you attempt to do this when using an execution strategy.</span></span>

> <span data-ttu-id="0a1ab-117">InvalidOperationException： 配置的执行策略 SqlServerRetryingExecutionStrategy 不支持用户启动事务。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-117">InvalidOperationException: The configured execution strategy 'SqlServerRetryingExecutionStrategy' does not support user initiated transactions.</span></span> <span data-ttu-id="0a1ab-118">使用 DbContext.Database.CreateExecutionStrategy() 返回的执行策略在作为可重试单元事务中执行所有操作。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-118">Use the execution strategy returned by 'DbContext.Database.CreateExecutionStrategy()' to execute all the operations in the transaction as a retriable unit.</span></span>

<span data-ttu-id="0a1ab-119">解决方案是使用委托表示的所有内容，需要执行手动调用执行策略。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-119">The solution is to manually invoke the execution strategy with a delegate representing everything that needs to be executed.</span></span> <span data-ttu-id="0a1ab-120">如果发生暂时性故障，则执行策略将再次调用委托。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-120">If a transient failure occurs, the execution strategy will invoke the delegate again.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a><span data-ttu-id="0a1ab-121">事务提交失败和幂等性问题</span><span class="sxs-lookup"><span data-stu-id="0a1ab-121">Transaction commit failure and the idempotency issue</span></span>

<span data-ttu-id="0a1ab-122">一般情况下，连接失败时当前事务将回滚。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-122">In general, when there is a connection failure the current transaction is rolled back.</span></span> <span data-ttu-id="0a1ab-123">但是，如果在事务处于断开连接是否将提交所生成的事务的状态为未知。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-123">However, if the connection is dropped while the transaction is being committed the resulting state of the transaction is unknown.</span></span> <span data-ttu-id="0a1ab-124">请参阅此[博客文章](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx)有关详细信息。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-124">See this [blog post](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) for more details.</span></span>

<span data-ttu-id="0a1ab-125">默认情况下，执行策略将重试该操作就像该事务已回滚，但如果它不是这样这将导致异常如果新数据库的状态不兼容，或者可能会导致**数据损坏**如果操作不依赖于特定的状态，例如，在插入具有自动生成的密钥值的新行时。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-125">By default, the execution strategy will retry the operation as if the transaction was rolled back, but if it's not the case this will result in an exception if the new database state is incompatible or could lead to **data corruption** if the operation does not rely on a particular state, for example when inserting a new row with auto-generated key values.</span></span>

<span data-ttu-id="0a1ab-126">有多种与此处理。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-126">There are several ways to deal with this.</span></span>

### <a name="option-1---do-almost-nothing"></a><span data-ttu-id="0a1ab-127">选项 1-是否执行任何操作 （几乎）</span><span class="sxs-lookup"><span data-stu-id="0a1ab-127">Option 1 - Do (almost) nothing</span></span>

<span data-ttu-id="0a1ab-128">事务提交过程中的连接故障的可能性较低，因此它可能是可接受的应用程序以直接失败，如果实际发生这种情况。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-128">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>

<span data-ttu-id="0a1ab-129">但是，你需要避免使用由存储生成的键，以确保添加重复的行而不是引发异常。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-129">However, you need to avoid using store-generated keys in order to ensure that an exception is thrown instead of adding a duplicate row.</span></span> <span data-ttu-id="0a1ab-130">请考虑使用由客户端生成的 GUID 值或客户端值生成器。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-130">Consider using a client-generated GUID value or a client-side value generator.</span></span>

### <a name="option-2---rebuild-application-state"></a><span data-ttu-id="0a1ab-131">选项 2-重新生成应用程序状态</span><span class="sxs-lookup"><span data-stu-id="0a1ab-131">Option 2 - Rebuild application state</span></span>

1. <span data-ttu-id="0a1ab-132">放弃当前`DbContext`。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-132">Discard the current `DbContext`.</span></span>
2. <span data-ttu-id="0a1ab-133">创建一个新`DbContext`和从数据库中还原你的应用程序的状态。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-133">Create a new `DbContext` and restore the state of your application from the database.</span></span>
3. <span data-ttu-id="0a1ab-134">通知用户，最后一个操作可能不具有已成功完成。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-134">Inform the user that the last operation might not have been completed successfully.</span></span>

### <a name="option-3---add-state-verification"></a><span data-ttu-id="0a1ab-135">选项 3-添加状态验证</span><span class="sxs-lookup"><span data-stu-id="0a1ab-135">Option 3 - Add state verification</span></span>

<span data-ttu-id="0a1ab-136">对于大多数更改数据库状态的操作则可以添加检查它是否成功的代码。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-136">For most of the operations that change the database state it is possible to add code that checks whether it succeeded.</span></span> <span data-ttu-id="0a1ab-137">EF 提供一个扩展方法来简化此过程- `IExecutionStrategy.ExecuteInTransaction`。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-137">EF provides an extension method to make this easier - `IExecutionStrategy.ExecuteInTransaction`.</span></span>

<span data-ttu-id="0a1ab-138">此方法开始和提交事务，还接受中的函数`verifySucceeded`在事务提交期间发生暂时性错误时调用的参数。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-138">This method begins and commits a transaction and also accepts a function in the `verifySucceeded` parameter that is invoked when a transient error occurs during the transaction commit.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> <span data-ttu-id="0a1ab-139">此处`SaveChanges`使用调用`acceptAllChangesOnSuccess`设置为`false`为了避免更改的状态`Blog`实体到`Unchanged`如果`SaveChanges`成功。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-139">Here `SaveChanges` is invoked with `acceptAllChangesOnSuccess` set to `false` to avoid changing the state of the `Blog` entity to `Unchanged` if `SaveChanges` succeeds.</span></span> <span data-ttu-id="0a1ab-140">这样就可以重试相同的操作如果提交失败并回滚事务。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-140">This allows to retry the same operation if the commit fails and the transaction is rolled back.</span></span>

### <a name="option-4---manually-track-the-transaction"></a><span data-ttu-id="0a1ab-141">选项 4-手动跟踪的事务</span><span class="sxs-lookup"><span data-stu-id="0a1ab-141">Option 4 - Manually track the transaction</span></span>

<span data-ttu-id="0a1ab-142">如果你需要使用由存储生成的键或需要不依赖于执行的操作中泛型的方式来处理提交故障的每个事务无法分配提交失败时检查的 ID。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-142">If you need to use store-generated keys or need a generic way of handling commit failures that doesn't depend on the operation performed each transaction could be assigned an ID that is checked when the commit fails.</span></span>

1. <span data-ttu-id="0a1ab-143">将表添加到用于跟踪事务的状态的数据库。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-143">Add a table to the database used to track the status of the transactions.</span></span>
2. <span data-ttu-id="0a1ab-144">将行插入表中的每个事务的开头。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-144">Insert a row into the table at the beginning of each transaction.</span></span>
3. <span data-ttu-id="0a1ab-145">如果连接失败的提交，检查数据库中的相应行存在。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-145">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>
4. <span data-ttu-id="0a1ab-146">如果提交成功，删除相应的行，以避免表的增长。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-146">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> <span data-ttu-id="0a1ab-147">请确保用于验证的上下文具有定义为连接是可能会在验证期间再次失败，如果在事务提交过程失败的执行策略。</span><span class="sxs-lookup"><span data-stu-id="0a1ab-147">Make sure that the context used for the verification has an execution strategy defined as the connection is likely to fail again during verification if it failed during transaction commit.</span></span>
