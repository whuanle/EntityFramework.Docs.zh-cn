---
title: 处理事务提交失败的 EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 5b1f7a7d-1b24-4645-95ec-5608a31ef577
ms.openlocfilehash: 71d5649dd993bb95e24165a55d812c71a37f03f3
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489383"
---
# <a name="handling-transaction-commit-failures"></a><span data-ttu-id="9764c-102">处理事务提交失败</span><span class="sxs-lookup"><span data-stu-id="9764c-102">Handling transaction commit failures</span></span>
> [!NOTE]
> <span data-ttu-id="9764c-103">**EF6.1 及更高版本仅**的功能，Api，Entity Framework 6.1 中引入了此页所述的等。</span><span class="sxs-lookup"><span data-stu-id="9764c-103">**EF6.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="9764c-104">如果使用的是早期版本，则部分或全部信息不适用。</span><span class="sxs-lookup"><span data-stu-id="9764c-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="9764c-105">6.1 的一部分，我们引入新的连接复原功能 ef： 检测和暂时性连接故障影响的确认的事务提交时自动恢复的能力。</span><span class="sxs-lookup"><span data-stu-id="9764c-105">As part of 6.1 we are introducing a new connection resiliency feature for EF: the ability to detect and recover automatically when transient connection failures affect the acknowledgement of transaction commits.</span></span> <span data-ttu-id="9764c-106">该方案的完整详细信息是最适合的博客文章中所述[SQL 数据库连接和幂等性问题](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx)。</span><span class="sxs-lookup"><span data-stu-id="9764c-106">The full details of the scenario are best described in the blog post [SQL Database Connectivity and the Idempotency Issue](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).</span></span>  <span data-ttu-id="9764c-107">总之，该方案是在事务提交过程中引发异常时有两个可能的原因：</span><span class="sxs-lookup"><span data-stu-id="9764c-107">In summary, the scenario is that when an exception is raised during a transaction commit there are two possible causes:</span></span>  

1. <span data-ttu-id="9764c-108">在服务器上的事务提交失败</span><span class="sxs-lookup"><span data-stu-id="9764c-108">The transaction commit failed on the server</span></span>
2. <span data-ttu-id="9764c-109">在服务器上的事务提交成功，但连接问题导致无法从到达客户端的成功通知</span><span class="sxs-lookup"><span data-stu-id="9764c-109">The transaction commit succeeded on the server but a connectivity issue prevented the success notification from reaching the client</span></span>  

<span data-ttu-id="9764c-110">第一种情况发生在应用程序或用户可以重试该操作，但第二种情况发生时应避免使用重试次数和应用程序无法自动恢复。</span><span class="sxs-lookup"><span data-stu-id="9764c-110">When the first situation happens the application or the user can retry the operation, but when the second situation occurs retries should be avoided and the application could recover automatically.</span></span> <span data-ttu-id="9764c-111">面临的挑战是操作的，如果不能检测异常报告，在提交期间，应用程序不能选择正确过程的实际原因是操作的什么。</span><span class="sxs-lookup"><span data-stu-id="9764c-111">The challenge is that without the ability to detect what was the actual reason an exception was reported during commit, the application cannot choose the right course of action.</span></span> <span data-ttu-id="9764c-112">EF 6.1 中的新功能使得 EF 可仔细检查与数据库，如果事务成功，并以透明方式采取适当的操作过程。</span><span class="sxs-lookup"><span data-stu-id="9764c-112">The new feature in EF 6.1 allows EF to double-check with the database if the transaction succeeded and take the right course of action transparently.</span></span>  

## <a name="using-the-feature"></a><span data-ttu-id="9764c-113">使用的功能</span><span class="sxs-lookup"><span data-stu-id="9764c-113">Using the feature</span></span>  

<span data-ttu-id="9764c-114">若要启用该功能需要包括对[SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx)的构造函数中你**DbConfiguration**。</span><span class="sxs-lookup"><span data-stu-id="9764c-114">In order to enable the feature you need include a call to [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) in the constructor of your **DbConfiguration**.</span></span> <span data-ttu-id="9764c-115">如果您不熟悉**DbConfiguration**，请参阅[代码基于配置](~/ef6/fundamentals/configuring/code-based.md)。</span><span class="sxs-lookup"><span data-stu-id="9764c-115">If you are unfamiliar with **DbConfiguration**, see [Code Based Configuration](~/ef6/fundamentals/configuring/code-based.md).</span></span> <span data-ttu-id="9764c-116">可以结合使用自动重试机制在 EF6 中，我们引入了可帮助在中，事务实际上无法提交对服务器造成暂时性故障的情况下使用此功能：</span><span class="sxs-lookup"><span data-stu-id="9764c-116">This feature can be used in combination with the automatic retries we introduced in EF6, which help in the situation in which the transaction actually failed to commit on the server due to a transient failure:</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

public class MyConfiguration : DbConfiguration  
{
  public MyConfiguration()  
  {  
    SetTransactionHandler(SqlProviderServices.ProviderInvariantName, () => new CommitFailureHandler());  
    SetExecutionStrategy(SqlProviderServices.ProviderInvariantName, () => new SqlAzureExecutionStrategy());  
  }  
}
```  

## <a name="how-transactions-are-tracked"></a><span data-ttu-id="9764c-117">如何跟踪事务</span><span class="sxs-lookup"><span data-stu-id="9764c-117">How transactions are tracked</span></span>  

<span data-ttu-id="9764c-118">启用该功能后，EF 将自动将新表添加到名为数据库 **__Transactions**。</span><span class="sxs-lookup"><span data-stu-id="9764c-118">When the feature is enabled, EF will automatically add a new table to the database called **__Transactions**.</span></span> <span data-ttu-id="9764c-119">每次事务创建的 EF，如果在提交期间发生事务失败，检查该行存在时，此表中插入新行。</span><span class="sxs-lookup"><span data-stu-id="9764c-119">A new row is inserted in this table every time a transaction is created by EF and that row is checked for existence if a transaction failure occurs during commit.</span></span>  

<span data-ttu-id="9764c-120">尽管 EF 将执行这一最大努力来修剪表中的行，当不再需要时，应用程序在退出过早，因此可能需要手动在某些情况下表中清除表可能增大。</span><span class="sxs-lookup"><span data-stu-id="9764c-120">Although EF will do a best effort to prune rows from the table when they aren’t needed anymore, the table can grow if the application exits prematurely and for that reason you may need to purge the table manually in some cases.</span></span>  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a><span data-ttu-id="9764c-121">如何处理与早期版本的提交失败</span><span class="sxs-lookup"><span data-stu-id="9764c-121">How to handle commit failures with previous Versions</span></span>

<span data-ttu-id="9764c-122">在 EF 6.1 之前没有机制来处理 EF 产品中的提交失败。</span><span class="sxs-lookup"><span data-stu-id="9764c-122">Before EF 6.1 there was not mechanism to handle commit failures in the EF product.</span></span> <span data-ttu-id="9764c-123">有几种方法来处理这种情况可与以前版本的 EF6 应用：</span><span class="sxs-lookup"><span data-stu-id="9764c-123">There are several ways to dealing with this situation that can be applied to previous versions of EF6:</span></span>  

* <span data-ttu-id="9764c-124">选项 1-不执行任何操作</span><span class="sxs-lookup"><span data-stu-id="9764c-124">Option 1 - Do nothing</span></span>  

  <span data-ttu-id="9764c-125">因此，可能会使应用程序直接失败，如果实际上发生这种情况可接受的事务提交过程中的连接故障的可能性较低。</span><span class="sxs-lookup"><span data-stu-id="9764c-125">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>  

* <span data-ttu-id="9764c-126">选项 2-使用数据库来重置状态</span><span class="sxs-lookup"><span data-stu-id="9764c-126">Option 2 - Use the database to reset state</span></span>  

  1. <span data-ttu-id="9764c-127">放弃当前的 DbContext</span><span class="sxs-lookup"><span data-stu-id="9764c-127">Discard the current DbContext</span></span>  
  2. <span data-ttu-id="9764c-128">创建新的 DbContext 和从数据库中还原应用程序的状态</span><span class="sxs-lookup"><span data-stu-id="9764c-128">Create a new DbContext and restore the state of your application from the database</span></span>  
  3. <span data-ttu-id="9764c-129">通知用户，最后一次操作可能不具有已成功完成</span><span class="sxs-lookup"><span data-stu-id="9764c-129">Inform the user that the last operation might not have been completed successfully</span></span>  

* <span data-ttu-id="9764c-130">选项 3-手动跟踪事务</span><span class="sxs-lookup"><span data-stu-id="9764c-130">Option 3 - Manually track the transaction</span></span>  

  1. <span data-ttu-id="9764c-131">将非跟踪的表添加到用于跟踪事务的状态的数据库。</span><span class="sxs-lookup"><span data-stu-id="9764c-131">Add a non-tracked table to the database used to track the status of the transactions.</span></span>  
  2. <span data-ttu-id="9764c-132">插入到表中每个事务的开始处的行。</span><span class="sxs-lookup"><span data-stu-id="9764c-132">Insert a row into the table at the beginning of each transaction.</span></span>  
  3. <span data-ttu-id="9764c-133">如果连接失败在提交期间，检查存在的数据库中的相应行。</span><span class="sxs-lookup"><span data-stu-id="9764c-133">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>  
     - <span data-ttu-id="9764c-134">如果存在该行，则继续正常运行，因为该事务已提交成功</span><span class="sxs-lookup"><span data-stu-id="9764c-134">If the row is present, continue normally, as the transaction was committed successfully</span></span>  
     - <span data-ttu-id="9764c-135">如果该行不存在，使用执行策略以重试当前操作。</span><span class="sxs-lookup"><span data-stu-id="9764c-135">If the row is absent, use an execution strategy to retry the current operation.</span></span>  
  4. <span data-ttu-id="9764c-136">如果提交成功，删除对应的行，以避免对表的增长。</span><span class="sxs-lookup"><span data-stu-id="9764c-136">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>  

<span data-ttu-id="9764c-137">[这篇博客文章](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx)包含在 SQL Azure 上实现此目的的示例代码。</span><span class="sxs-lookup"><span data-stu-id="9764c-137">[This blog post](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) contains sample code for accomplishing this on SQL Azure.</span></span>  
