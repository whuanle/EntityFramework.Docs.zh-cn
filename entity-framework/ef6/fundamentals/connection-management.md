---
title: 连接管理-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: ecaa5a27-b19e-4bf9-8142-a3fb00642270
ms.openlocfilehash: a6352bbbc38c38bd5f30536736ec969056df2c7d
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489331"
---
# <a name="connection-management"></a><span data-ttu-id="ba834-102">连接管理</span><span class="sxs-lookup"><span data-stu-id="ba834-102">Connection management</span></span>
<span data-ttu-id="ba834-103">此页介绍实体框架的行为将连接传递给上下文和功能方面**Database.Connection.Open()** API。</span><span class="sxs-lookup"><span data-stu-id="ba834-103">This page describes the behavior of Entity Framework with regard to passing connections to the context and the functionality of the **Database.Connection.Open()** API.</span></span>  

## <a name="passing-connections-to-the-context"></a><span data-ttu-id="ba834-104">传递到上下文的连接</span><span class="sxs-lookup"><span data-stu-id="ba834-104">Passing Connections to the Context</span></span>  

### <a name="behavior-for-ef5-and-earlier-versions"></a><span data-ttu-id="ba834-105">EF5 和较早版本的行为</span><span class="sxs-lookup"><span data-stu-id="ba834-105">Behavior for EF5 and earlier versions</span></span>  

<span data-ttu-id="ba834-106">有两个构造函数接受连接：</span><span class="sxs-lookup"><span data-stu-id="ba834-106">There are two constructors which accept connections:</span></span>  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

<span data-ttu-id="ba834-107">可以使用这些，但必须先解决了几个限制：</span><span class="sxs-lookup"><span data-stu-id="ba834-107">It is possible to use these but you have to work around a couple of limitations:</span></span>  

1. <span data-ttu-id="ba834-108">如果传递的打开连接到其中一种然后第一次该框架将尝试使用它引发 InvalidOperationException 说它无法重新打开已打开的连接。</span><span class="sxs-lookup"><span data-stu-id="ba834-108">If you pass an open connection to either of these then the first time the framework attempts to use it an InvalidOperationException is thrown saying it cannot re-open an already open connection.</span></span>  
2. <span data-ttu-id="ba834-109">ContextOwnsConnection 标志被解释为表示在释放上下文时应释放基础存储区连接。</span><span class="sxs-lookup"><span data-stu-id="ba834-109">The contextOwnsConnection flag is interpreted to mean whether or not the underlying store connection should be disposed when the context is disposed.</span></span> <span data-ttu-id="ba834-110">但是，无论启用该设置存储连接始终关闭时释放上下文。</span><span class="sxs-lookup"><span data-stu-id="ba834-110">But, regardless of that setting, the store connection is always closed when the context is disposed.</span></span> <span data-ttu-id="ba834-111">因此如果必须使用相同的连接的多个 DbContext 任何上下文被处置首先将关闭连接 （同样如果具有混合的现有的 ADO.NET 连接，使用 DbContext，DbContext 将始终关闭连接时它已释放）.</span><span class="sxs-lookup"><span data-stu-id="ba834-111">So if you have more than one DbContext with the same connection whichever context is disposed first will close the connection (similarly if you have mixed an existing ADO.NET connection with a DbContext, DbContext will always close the connection when it is disposed).</span></span>  

<span data-ttu-id="ba834-112">就可以通过传递已关闭的连接，并仅执行已创建的所有上下文后会打开它的代码解决上面的第一个限制：</span><span class="sxs-lookup"><span data-stu-id="ba834-112">It is possible to work around the first limitation above by passing a closed connection and only executing code that would open it once all contexts have been created:</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Common;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;
using System.Linq;

namespace ConnectionManagementExamples
{
    class ConnectionManagementExampleEF5
    {         
        public static void TwoDbContextsOneConnection()
        {
            using (var context1 = new BloggingContext())
            {
                var conn =
                    ((EntityConnection)  
                        ((IObjectContextAdapter)context1).ObjectContext.Connection)  
                            .StoreConnection;

                using (var context2 = new BloggingContext(conn, contextOwnsConnection: false))
                {
                    context2.Database.ExecuteSqlCommand(
                        @"UPDATE Blogs SET Rating = 5" +
                        " WHERE Name LIKE '%Entity Framework%'");

                    var query = context1.Posts.Where(p => p.Blog.Rating > 5);
                    foreach (var post in query)
                    {
                        post.Title += "[Cool Blog]";
                    }
                    context1.SaveChanges();
                }
            }
        }
    }
}
```  

<span data-ttu-id="ba834-113">第二个限制只是意味着您需要避免在您准备好要关闭的连接之前释放任何 DbContext 对象。</span><span class="sxs-lookup"><span data-stu-id="ba834-113">The second limitation just means you need to refrain from disposing any of your DbContext objects until you are ready for the connection to be closed.</span></span>  

### <a name="behavior-in-ef6-and-future-versions"></a><span data-ttu-id="ba834-114">EF6 和未来版本中的行为</span><span class="sxs-lookup"><span data-stu-id="ba834-114">Behavior in EF6 and future versions</span></span>  

<span data-ttu-id="ba834-115">在 EF6 和未来版本 DbContext 具有相同的两个构造函数，但不再需要时接收到关闭的连接传递给构造函数。</span><span class="sxs-lookup"><span data-stu-id="ba834-115">In EF6 and future versions the DbContext has the same two constructors but no longer requires that the connection passed to the constructor be closed when it is received.</span></span> <span data-ttu-id="ba834-116">因此，这是现在可以：</span><span class="sxs-lookup"><span data-stu-id="ba834-116">So this is now possible:</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace ConnectionManagementExamples
{
    class ConnectionManagementExample
    {
        public static void PassingAnOpenConnection()
        {
            using (var conn = new SqlConnection("{connectionString}"))
            {
                conn.Open();

                var sqlCommand = new SqlCommand();
                sqlCommand.Connection = conn;
                sqlCommand.CommandText =
                    @"UPDATE Blogs SET Rating = 5" +
                     " WHERE Name LIKE '%Entity Framework%'";
                sqlCommand.ExecuteNonQuery();

                using (var context = new BloggingContext(conn, contextOwnsConnection: false))
                {
                    var query = context.Posts.Where(p => p.Blog.Rating > 5);
                    foreach (var post in query)
                    {
                        post.Title += "[Cool Blog]";
                    }
                    context.SaveChanges();
                }

                var sqlCommand2 = new SqlCommand();
                sqlCommand2.Connection = conn;
                sqlCommand2.CommandText =
                    @"UPDATE Blogs SET Rating = 7" +
                     " WHERE Name LIKE '%Entity Framework Rocks%'";
                sqlCommand2.ExecuteNonQuery();
            }
        }
    }
}
```  

<span data-ttu-id="ba834-117">此外 contextOwnsConnection 标志现在可以控制连接是同时关闭和释放时释放 DbContext。</span><span class="sxs-lookup"><span data-stu-id="ba834-117">Also the contextOwnsConnection flag now controls whether or not the connection is both closed and disposed when the DbContext is disposed.</span></span> <span data-ttu-id="ba834-118">因此在上面的示例连接时不会关闭的上下文是重要的是在以前版本的 EF，但而不是连接本身被释放后释放 （第 32 行） （第 40 行）。</span><span class="sxs-lookup"><span data-stu-id="ba834-118">So in the above example the connection is not closed when the context is disposed (line 32) as it would have been in previous versions of EF, but rather when the connection itself is disposed (line 40).</span></span>  

<span data-ttu-id="ba834-119">当然，这是连接 (只需设置 contextOwnsConnection 为 true，或使用其他构造函数之一) 控制对 dbcontext 仍可以根据需要。</span><span class="sxs-lookup"><span data-stu-id="ba834-119">Of course it is still possible for the DbContext to take control of the connection (just set contextOwnsConnection to true or use one of the other constructors) if you so wish.</span></span>  

> [!NOTE]
> <span data-ttu-id="ba834-120">使用此新模型中使用事务时，有一些其他注意事项。</span><span class="sxs-lookup"><span data-stu-id="ba834-120">There are some additional considerations when using transactions with this new model.</span></span> <span data-ttu-id="ba834-121">有关详细信息，请参阅[使用事务](~/ef6/saving/transactions.md)。</span><span class="sxs-lookup"><span data-stu-id="ba834-121">For details see [Working with Transactions](~/ef6/saving/transactions.md).</span></span>  

## <a name="databaseconnectionopen"></a><span data-ttu-id="ba834-122">Database.Connection.Open()</span><span class="sxs-lookup"><span data-stu-id="ba834-122">Database.Connection.Open()</span></span>  

### <a name="behavior-for-ef5-and-earlier-versions"></a><span data-ttu-id="ba834-123">EF5 和较早版本的行为</span><span class="sxs-lookup"><span data-stu-id="ba834-123">Behavior for EF5 and earlier versions</span></span>  

<span data-ttu-id="ba834-124">在 EF5 和早期版本中是一个 bug，以便**ObjectContext.Connection.State**未更新以反映基础存储区连接的真实状态。</span><span class="sxs-lookup"><span data-stu-id="ba834-124">In EF5 and earlier versions there is a bug such that the **ObjectContext.Connection.State** was not updated to reflect the true state of the underlying store connection.</span></span> <span data-ttu-id="ba834-125">例如，如果执行下面的代码，可以返回状态**Closed**尽管事实上基础存储连接**打开**。</span><span class="sxs-lookup"><span data-stu-id="ba834-125">For example, if you executed the following code you can be returned the status **Closed** even though in fact the underlying store connection is **Open**.</span></span>  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

<span data-ttu-id="ba834-126">另外，如果通过调用 Database.Connection.Open() 打开数据库连接将打开直到下一次执行查询或调用任何内容需要数据库连接 (例如，但在 SaveChanges())，基础存储连接将关闭。</span><span class="sxs-lookup"><span data-stu-id="ba834-126">Separately, if you open the database connection by calling Database.Connection.Open() it will be open until the next time you execute a query or call anything which requires a database connection (for example, SaveChanges()) but after that the underlying store connection will be closed.</span></span> <span data-ttu-id="ba834-127">上下文将然后重新打开并重新关闭连接另一个数据库操作是必需的任何时间：</span><span class="sxs-lookup"><span data-stu-id="ba834-127">The context will then re-open and re-close the connection any time another database operation is required:</span></span>  

``` csharp
using System;
using System.Data;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;

namespace ConnectionManagementExamples
{
    public class DatabaseOpenConnectionBehaviorEF5
    {
        public static void DatabaseOpenConnectionBehavior()
        {
            using (var context = new BloggingContext())
            {
                // At this point the underlying store connection is closed

                context.Database.Connection.Open();

                // Now the underlying store connection is open  
                // (though ObjectContext.Connection.State will report closed)

                var blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);

                // The underlying store connection is still open  

                context.SaveChanges();

                // After SaveChanges() the underlying store connection is closed  
                // Each SaveChanges() / query etc now opens and immediately closes
                // the underlying store connection

                blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();
            }
        }
    }
}
```  

### <a name="behavior-in-ef6-and-future-versions"></a><span data-ttu-id="ba834-128">EF6 和未来版本中的行为</span><span class="sxs-lookup"><span data-stu-id="ba834-128">Behavior in EF6 and future versions</span></span>  

<span data-ttu-id="ba834-129">EF6 和未来版本我们已采取的方法，如果调用代码选择通过调用上下文打开连接。Database.Connection.Open()，则具有充分的理由这样做，框架将假定它想要控制打开和关闭的连接，并将不再自动关闭连接。</span><span class="sxs-lookup"><span data-stu-id="ba834-129">For EF6 and future versions we have taken the approach that if the calling code chooses to open the connection by calling context.Database.Connection.Open() then it has a good reason for doing so and the framework will assume that it wants control over opening and closing of the connection and will no longer close the connection automatically.</span></span>  

> [!NOTE]
> <span data-ttu-id="ba834-130">这会导致连接这种情况对于长时间，因此使用谨慎处于打开状态。</span><span class="sxs-lookup"><span data-stu-id="ba834-130">This can potentially lead to connections which are open for a long time so use with care.</span></span>  

<span data-ttu-id="ba834-131">我们还更新了代码，以便 ObjectContext.Connection.State 现在跟踪的基础连接的状态正确。</span><span class="sxs-lookup"><span data-stu-id="ba834-131">We also updated the code so that ObjectContext.Connection.State now keeps track of the state of the underlying connection correctly.</span></span>  

``` csharp
using System;
using System.Data;
using System.Data.Entity;
using System.Data.Entity.Core.EntityClient;
using System.Data.Entity.Infrastructure;

namespace ConnectionManagementExamples
{
    internal class DatabaseOpenConnectionBehaviorEF6
    {
        public static void DatabaseOpenConnectionBehavior()
        {
            using (var context = new BloggingContext())
            {
                // At this point the underlying store connection is closed

                context.Database.Connection.Open();

                // Now the underlying store connection is open and the
                // ObjectContext.Connection.State correctly reports open too

                var blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();

                // The underlying store connection remains open for the next operation  

                blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();

                // The underlying store connection is still open

           } // The context is disposed – so now the underlying store connection is closed
        }
    }
}
```  
