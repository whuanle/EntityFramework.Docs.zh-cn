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
# <a name="using-transactions"></a>使用事务

事务允许多个数据库操作以原子方式处理。 如果将提交事务，则表示的所有操作已成功应用于数据库。 如果事务回滚后，所有操作将应用到数据库。

> [!TIP]  
> 你可以查看这篇文章[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/)GitHub 上。

## <a name="default-transaction-behavior"></a>默认事务行为

默认情况下，如果数据库提供程序支持事务，所有更改对单个调用中`SaveChanges()`应用在事务中。 如果任何所做的更改失败，然后回滚事务，并没有更改应用到数据库。 这意味着，`SaveChanges()`保证完全成功，还是保持数据库不做任何修改如果发生错误。

对于大多数应用程序，此默认行为已足够。 如果应用程序的要求，则认为有必要，您应仅手动控制事务。

## <a name="controlling-transactions"></a>控制事务

你可以使用`DbContext.Database`API 来开始、 提交和回滚事务。 下面的示例显示了两个`SaveChanges()`操作和 LINQ 查询在单个事务中执行。

并非所有的数据库提供程序支持事务。 某些访问接口可能会引发或无操作事务调用 Api 时。

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

## <a name="cross-context-transaction-relational-databases-only"></a>跨上下文事务 （仅限关系数据库）

你还可以跨多个上下文实例共享一个事务。 当使用关系数据库提供程序，因为它需要使用时，此功能才可用`DbTransaction`和`DbConnection`，哪些是特定于关系数据库。

若要共享一个事务，该上下文必须共享同时`DbConnection`和`DbTransaction`。

### <a name="allow-connection-to-be-externally-provided"></a>允许连接到外部提供

共享`DbConnection`需要构造它时，将连接传递到上下文的能力。

允许的最简单办法`DbConnection`外部提供，即将停止使用`DbContext.OnConfiguring`方法来配置上下文和外部创建`DbContextOptions`并将它们传递给上下文构造函数。

> [!TIP]  
> `DbContextOptionsBuilder`是你在中使用的 API`DbContext.OnConfiguring`若要配置的上下文，现在想要从外部使用它来创建`DbContextOptions`。

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

替代方法是继续使用`DbContext.OnConfiguring`，但接受`DbConnection`，保存并随后将用于`DbContext.OnConfiguring`。

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

### <a name="share-connection-and-transaction"></a>共享连接和事务

你现在可以创建多个共享相同的连接的上下文实例。 然后使用`DbContext.Database.UseTransaction(DbTransaction)`API 以同一事务中的两个上下文中登记。

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

## <a name="using-external-dbtransactions-relational-databases-only"></a>使用外部 DbTransactions （仅限关系数据库）

如果使用多个数据访问技术来访问关系数据库，你可能想要共享这些不同技术所执行的操作之间的事务。

下面的示例，演示如何在同一事务中执行运算 ADO.NET SqlClient 和 Entity Framework 核心操作。

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
