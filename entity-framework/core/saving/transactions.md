---
title: "事务的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
ms.technology: entity-framework-core
uid: core/saving/transactions
ms.openlocfilehash: a2f890c0af1e83cbcc1d40d68540ff7132a9bafd
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="using-transactions"></a><span data-ttu-id="89195-102">使用事务</span><span class="sxs-lookup"><span data-stu-id="89195-102">Using Transactions</span></span>

<span data-ttu-id="89195-103">事务允许多个数据库操作以原子方式处理。</span><span class="sxs-lookup"><span data-stu-id="89195-103">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="89195-104">如果将提交事务，则表示的所有操作已成功应用于数据库。</span><span class="sxs-lookup"><span data-stu-id="89195-104">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="89195-105">如果事务回滚后，所有操作将应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="89195-105">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="89195-106">你可以查看这篇文章[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/)GitHub 上。</span><span class="sxs-lookup"><span data-stu-id="89195-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="89195-107">默认事务行为</span><span class="sxs-lookup"><span data-stu-id="89195-107">Default transaction behavior</span></span>

<span data-ttu-id="89195-108">默认情况下，如果数据库提供程序支持事务，所有更改对单个调用中`SaveChanges()`应用在事务中。</span><span class="sxs-lookup"><span data-stu-id="89195-108">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="89195-109">如果任何所做的更改失败，然后回滚事务，并没有更改应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="89195-109">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="89195-110">这意味着，`SaveChanges()`保证完全成功，还是保持数据库不做任何修改如果发生错误。</span><span class="sxs-lookup"><span data-stu-id="89195-110">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="89195-111">对于大多数应用程序，此默认行为已足够。</span><span class="sxs-lookup"><span data-stu-id="89195-111">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="89195-112">如果应用程序的要求，则认为有必要，您应仅手动控制事务。</span><span class="sxs-lookup"><span data-stu-id="89195-112">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="89195-113">控制事务</span><span class="sxs-lookup"><span data-stu-id="89195-113">Controlling transactions</span></span>

<span data-ttu-id="89195-114">你可以使用`DbContext.Database`API 来开始、 提交和回滚事务。</span><span class="sxs-lookup"><span data-stu-id="89195-114">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="89195-115">下面的示例显示了两个`SaveChanges()`操作和 LINQ 查询在单个事务中执行。</span><span class="sxs-lookup"><span data-stu-id="89195-115">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="89195-116">并非所有的数据库提供程序支持事务。</span><span class="sxs-lookup"><span data-stu-id="89195-116">Not all database providers support transactions.</span></span> <span data-ttu-id="89195-117">某些访问接口可能会引发或无操作事务调用 Api 时。</span><span class="sxs-lookup"><span data-stu-id="89195-117">Some providers may throw or no-op when transaction APIs are called.</span></span>

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

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="89195-118">跨上下文事务 （仅限关系数据库）</span><span class="sxs-lookup"><span data-stu-id="89195-118">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="89195-119">你还可以跨多个上下文实例共享一个事务。</span><span class="sxs-lookup"><span data-stu-id="89195-119">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="89195-120">当使用关系数据库提供程序，因为它需要使用时，此功能才可用`DbTransaction`和`DbConnection`，哪些是特定于关系数据库。</span><span class="sxs-lookup"><span data-stu-id="89195-120">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="89195-121">若要共享一个事务，该上下文必须共享同时`DbConnection`和`DbTransaction`。</span><span class="sxs-lookup"><span data-stu-id="89195-121">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="89195-122">允许连接到外部提供</span><span class="sxs-lookup"><span data-stu-id="89195-122">Allow connection to be externally provided</span></span>

<span data-ttu-id="89195-123">共享`DbConnection`需要构造它时，将连接传递到上下文的能力。</span><span class="sxs-lookup"><span data-stu-id="89195-123">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="89195-124">允许的最简单办法`DbConnection`外部提供，即将停止使用`DbContext.OnConfiguring`方法来配置上下文和外部创建`DbContextOptions`并将它们传递给上下文构造函数。</span><span class="sxs-lookup"><span data-stu-id="89195-124">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="89195-125">`DbContextOptionsBuilder`是你在中使用的 API`DbContext.OnConfiguring`若要配置的上下文，现在想要从外部使用它来创建`DbContextOptions`。</span><span class="sxs-lookup"><span data-stu-id="89195-125">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?highlight=3,4,5)] -->
``` csharp
    public class BloggingContext : DbContext
    {
        public BloggingContext(DbContextOptions<BloggingContext> options)
            : base(options)
        { }

        public DbSet<Blog> Blogs { get; set; }
    }
```

<span data-ttu-id="89195-126">替代方法是继续使用`DbContext.OnConfiguring`，但接受`DbConnection`，保存并随后将用于`DbContext.OnConfiguring`。</span><span class="sxs-lookup"><span data-stu-id="89195-126">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

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

### <a name="share-connection-and-transaction"></a><span data-ttu-id="89195-127">共享连接和事务</span><span class="sxs-lookup"><span data-stu-id="89195-127">Share connection and transaction</span></span>

<span data-ttu-id="89195-128">你现在可以创建多个共享相同的连接的上下文实例。</span><span class="sxs-lookup"><span data-stu-id="89195-128">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="89195-129">然后使用`DbContext.Database.UseTransaction(DbTransaction)`API 以同一事务中的两个上下文中登记。</span><span class="sxs-lookup"><span data-stu-id="89195-129">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?highlight=1,2,3,7,16,23,24,25)] -->
``` csharp
        var options = new DbContextOptionsBuilder<BloggingContext>()
            .UseSqlServer(new SqlConnection(connectionString))
            .Options;

        using (var context1 = new BloggingContext(options))
        {
            using (var transaction = context1.Database.BeginTransaction())
            {
                try
                {
                    context1.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context1.SaveChanges();

                    using (var context2 = new BloggingContext(options))
                    {
                        context2.Database.UseTransaction(transaction.GetDbTransaction());

                        var blogs = context2.Blogs
                            .OrderBy(b => b.Url)
                            .ToList();
                    }

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

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="89195-130">使用外部 DbTransactions （仅限关系数据库）</span><span class="sxs-lookup"><span data-stu-id="89195-130">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="89195-131">如果使用多个数据访问技术来访问关系数据库，你可能想要共享这些不同技术所执行的操作之间的事务。</span><span class="sxs-lookup"><span data-stu-id="89195-131">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="89195-132">下面的示例，演示如何在同一事务中执行运算 ADO.NET SqlClient 和 Entity Framework 核心操作。</span><span class="sxs-lookup"><span data-stu-id="89195-132">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?highlight=4,10,21,26,27,28)] -->
``` csharp
        var connection = new SqlConnection(connectionString);
        connection.Open();

        using (var transaction = connection.BeginTransaction())
        {
            try
            {
                // Run raw ADO.NET command in the transaction
                var command = connection.CreateCommand();
                command.Transaction = transaction;
                command.CommandText = "DELETE FROM dbo.Blogs";
                command.ExecuteNonQuery();

                // Run an EF Core command in the transaction
                var options = new DbContextOptionsBuilder<BloggingContext>()
                    .UseSqlServer(connection)
                    .Options;

                using (var context = new BloggingContext(options))
                {
                    context.Database.UseTransaction(transaction);
                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context.SaveChanges();
                }

                // Commit transaction if all commands succeed, transaction will auto-rollback
                // when disposed if either commands fails
                transaction.Commit();
            }
            catch (System.Exception)
            {
                // TODO: Handle failure
            }
```
