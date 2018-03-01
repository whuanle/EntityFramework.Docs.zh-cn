---
title: "事务的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
ms.technology: entity-framework-core
uid: core/saving/transactions
ms.openlocfilehash: 2dda7b7d58ae058fc2aa89fe16fbf46adc8c6bdc
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2018
---
# <a name="using-transactions"></a><span data-ttu-id="0b54a-102">使用事务</span><span class="sxs-lookup"><span data-stu-id="0b54a-102">Using Transactions</span></span>

<span data-ttu-id="0b54a-103">事务允许多个数据库操作以原子方式处理。</span><span class="sxs-lookup"><span data-stu-id="0b54a-103">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="0b54a-104">如果将提交事务，则表示的所有操作已成功应用于数据库。</span><span class="sxs-lookup"><span data-stu-id="0b54a-104">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="0b54a-105">如果事务回滚后，所有操作将应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="0b54a-105">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="0b54a-106">可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/)。</span><span class="sxs-lookup"><span data-stu-id="0b54a-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="0b54a-107">默认事务行为</span><span class="sxs-lookup"><span data-stu-id="0b54a-107">Default transaction behavior</span></span>

<span data-ttu-id="0b54a-108">默认情况下，如果数据库提供程序支持事务，所有更改对单个调用中`SaveChanges()`应用在事务中。</span><span class="sxs-lookup"><span data-stu-id="0b54a-108">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="0b54a-109">如果任何所做的更改失败，然后回滚事务，并没有更改应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="0b54a-109">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="0b54a-110">这意味着，`SaveChanges()`保证完全成功，还是保持数据库不做任何修改如果发生错误。</span><span class="sxs-lookup"><span data-stu-id="0b54a-110">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="0b54a-111">对于大多数应用程序，此默认行为已足够。</span><span class="sxs-lookup"><span data-stu-id="0b54a-111">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="0b54a-112">如果应用程序的要求，则认为有必要，您应仅手动控制事务。</span><span class="sxs-lookup"><span data-stu-id="0b54a-112">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="0b54a-113">控制事务</span><span class="sxs-lookup"><span data-stu-id="0b54a-113">Controlling transactions</span></span>

<span data-ttu-id="0b54a-114">你可以使用`DbContext.Database`API 来开始、 提交和回滚事务。</span><span class="sxs-lookup"><span data-stu-id="0b54a-114">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="0b54a-115">下面的示例显示了两个`SaveChanges()`操作和 LINQ 查询在单个事务中执行。</span><span class="sxs-lookup"><span data-stu-id="0b54a-115">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="0b54a-116">并非所有的数据库提供程序支持事务。</span><span class="sxs-lookup"><span data-stu-id="0b54a-116">Not all database providers support transactions.</span></span> <span data-ttu-id="0b54a-117">某些访问接口可能会引发或无操作事务调用 Api 时。</span><span class="sxs-lookup"><span data-stu-id="0b54a-117">Some providers may throw or no-op when transaction APIs are called.</span></span>

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/ControllingTransaction/Sample.cs?highlight=3,17,18,19)] -->
``` csharp
        using (var context = new BloggingContext())
        {
            using (var transaction = context.Database.BeginTransaction())
            {
                try
                {
                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context.SaveChanges();

                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/visualstudio" });
                    context.SaveChanges();

                    var blogs = context.Blogs
                        .OrderBy(b => b.Url)
                        .ToList();

                    // Commit transaction if all commands succeed, transaction will auto-rollback
                    // when disposed if either commands fails
                    transaction.Commit();
                }
                catch (Exception)
                {
                    // TODO: Handle failure
                }
            }
        }
```

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="0b54a-118">跨上下文事务 （仅限关系数据库）</span><span class="sxs-lookup"><span data-stu-id="0b54a-118">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="0b54a-119">你还可以跨多个上下文实例共享一个事务。</span><span class="sxs-lookup"><span data-stu-id="0b54a-119">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="0b54a-120">当使用关系数据库提供程序，因为它需要使用时，此功能才可用`DbTransaction`和`DbConnection`，哪些是特定于关系数据库。</span><span class="sxs-lookup"><span data-stu-id="0b54a-120">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="0b54a-121">若要共享一个事务，该上下文必须共享同时`DbConnection`和`DbTransaction`。</span><span class="sxs-lookup"><span data-stu-id="0b54a-121">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="0b54a-122">允许连接到外部提供</span><span class="sxs-lookup"><span data-stu-id="0b54a-122">Allow connection to be externally provided</span></span>

<span data-ttu-id="0b54a-123">共享`DbConnection`需要构造它时，将连接传递到上下文的能力。</span><span class="sxs-lookup"><span data-stu-id="0b54a-123">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="0b54a-124">允许的最简单办法`DbConnection`外部提供，即将停止使用`DbContext.OnConfiguring`方法来配置上下文和外部创建`DbContextOptions`并将它们传递给上下文构造函数。</span><span class="sxs-lookup"><span data-stu-id="0b54a-124">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="0b54a-125">`DbContextOptionsBuilder` 是你在中使用的 API`DbContext.OnConfiguring`若要配置的上下文，现在想要从外部使用它来创建`DbContextOptions`。</span><span class="sxs-lookup"><span data-stu-id="0b54a-125">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

<span data-ttu-id="0b54a-126">替代方法是继续使用`DbContext.OnConfiguring`，但接受`DbConnection`，保存并随后将用于`DbContext.OnConfiguring`。</span><span class="sxs-lookup"><span data-stu-id="0b54a-126">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    private DbConnection _connection;

    public BloggingContext(DbConnection connection)
    {
      _connection = connection;
    }

    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer(_connection);
    }
}
```

### <a name="share-connection-and-transaction"></a><span data-ttu-id="0b54a-127">共享连接和事务</span><span class="sxs-lookup"><span data-stu-id="0b54a-127">Share connection and transaction</span></span>

<span data-ttu-id="0b54a-128">你现在可以创建多个共享相同的连接的上下文实例。</span><span class="sxs-lookup"><span data-stu-id="0b54a-128">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="0b54a-129">然后使用`DbContext.Database.UseTransaction(DbTransaction)`API 以同一事务中的两个上下文中登记。</span><span class="sxs-lookup"><span data-stu-id="0b54a-129">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="0b54a-130">使用外部 DbTransactions （仅限关系数据库）</span><span class="sxs-lookup"><span data-stu-id="0b54a-130">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="0b54a-131">如果使用多个数据访问技术来访问关系数据库，你可能想要共享这些不同技术所执行的操作之间的事务。</span><span class="sxs-lookup"><span data-stu-id="0b54a-131">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="0b54a-132">下面的示例，演示如何在同一事务中执行运算 ADO.NET SqlClient 和 Entity Framework 核心操作。</span><span class="sxs-lookup"><span data-stu-id="0b54a-132">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a><span data-ttu-id="0b54a-133">使用 System.Transactions</span><span class="sxs-lookup"><span data-stu-id="0b54a-133">Using System.Transactions</span></span>

> [!NOTE]  
> <span data-ttu-id="0b54a-134">此功能是在 EF 核心 2.1 的新增功能。</span><span class="sxs-lookup"><span data-stu-id="0b54a-134">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="0b54a-135">可使用环境事务，如果你需要进行协调跨较大作用域。</span><span class="sxs-lookup"><span data-stu-id="0b54a-135">It is possible to use ambient transactions if you need to coordinate across a larger scope.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,24,25,26)]

<span data-ttu-id="0b54a-136">还有可能在显式事务中登记。</span><span class="sxs-lookup"><span data-stu-id="0b54a-136">It is also possible to enlist in an explicit transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,13,26,27,28)]

### <a name="limitations-of-systemtransactions"></a><span data-ttu-id="0b54a-137">System.Transactions 的限制</span><span class="sxs-lookup"><span data-stu-id="0b54a-137">Limitations of System.Transactions</span></span>  

1. <span data-ttu-id="0b54a-138">EF 核心依赖于的数据库提供程序以实现对 System.Transactions 支持。</span><span class="sxs-lookup"><span data-stu-id="0b54a-138">EF Core relies on database providers to implement support for System.Transactions.</span></span> <span data-ttu-id="0b54a-139">尽管对于.NET Framework，API 仅已最近添加到.NET 核心并因此支持，支持是十分常见 ADO.NET 提供程序之间是不可为大范围。</span><span class="sxs-lookup"><span data-stu-id="0b54a-139">Although support is quite common among ADO.NET providers for .NET Framework, the API has only been recently added to .NET Core and hence support is not be as widespread.</span></span> <span data-ttu-id="0b54a-140">如果提供程序未实现对 System.Transactions 的支持，则可能对这些 Api 的调用将被完全忽略。</span><span class="sxs-lookup"><span data-stu-id="0b54a-140">If a provider does not implement support for System.Transactions, it is possible that calls to these APIs will be completely ignored.</span></span> <span data-ttu-id="0b54a-141">用于.NET 核心的 SqlClient 支持它从 2.1 及以上版本。</span><span class="sxs-lookup"><span data-stu-id="0b54a-141">SqlClient for .NET Core does support it from 2.1 onwards.</span></span> <span data-ttu-id="0b54a-142">用于.NET 核心 2.0 的 SqlClient 将引发异常的尝试使用该功能。</span><span class="sxs-lookup"><span data-stu-id="0b54a-142">SqlClient for .NET Core 2.0 will throw an exception of you attempt to use the feature.</span></span> 

   > [!IMPORTANT]  
   > <span data-ttu-id="0b54a-143">建议你测试，API 的行为与正确与你的提供程序之前用于管理事务依赖于该功能。</span><span class="sxs-lookup"><span data-stu-id="0b54a-143">It is recommended that you test that the API behaves correctly with your provider before you rely on it for managing transactions.</span></span> <span data-ttu-id="0b54a-144">建议如果它不联系数据库提供程序 maintainer 的。</span><span class="sxs-lookup"><span data-stu-id="0b54a-144">You are encouraged to contact the maintainer of the database provider if it does not.</span></span> 

2. <span data-ttu-id="0b54a-145">从版本 2.1，开始在.NET 核心的 System.Transactions 实现不包括对分布式事务的支持，因此不能使用`TransactionScope`或`CommitableTransaction`来协调跨多个资源管理器的事务。</span><span class="sxs-lookup"><span data-stu-id="0b54a-145">As of version 2.1, the System.Transactions implementation in .NET Core does not include support for distributed transactions, therefore you cannot use `TransactionScope` or `CommitableTransaction`to coordinate transactions across multiple resource managers.</span></span> 
