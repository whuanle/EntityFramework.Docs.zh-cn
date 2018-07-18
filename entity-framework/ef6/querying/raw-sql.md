---
title: 原始 SQL 查询的 EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 9e1ee76e-2499-408c-81e8-9b6c5d1945a0
caps.latest.revision: 3
ms.openlocfilehash: 1d968604cfa500784c4699b0923512572a06d773
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/08/2018
ms.locfileid: "39120081"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="ba5cc-102">原生 SQL 查询</span><span class="sxs-lookup"><span data-stu-id="ba5cc-102">Raw SQL Queries</span></span>
<span data-ttu-id="ba5cc-103">实体框架，可与实体类使用 LINQ 进行查询。</span><span class="sxs-lookup"><span data-stu-id="ba5cc-103">Entity Framework allows you to query using LINQ with your entity classes.</span></span> <span data-ttu-id="ba5cc-104">但是，可能有时你想要使用原始 SQL 直接针对数据库运行查询。</span><span class="sxs-lookup"><span data-stu-id="ba5cc-104">However, there may be times that you want to run queries using raw SQL directly against the database.</span></span> <span data-ttu-id="ba5cc-105">这包括调用存储的过程的当前不支持映射到存储过程的 Code First 模型会很有用。</span><span class="sxs-lookup"><span data-stu-id="ba5cc-105">This includes calling stored procedures, which can be helpful for Code First models that currently do not support mapping to stored procedures.</span></span> <span data-ttu-id="ba5cc-106">本主题所介绍的方法同样适用于查询使用 Code First 和 EF 设计器创建的模型。</span><span class="sxs-lookup"><span data-stu-id="ba5cc-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="writing-sql-queries-for-entities"></a><span data-ttu-id="ba5cc-107">编写实体 SQL 查询</span><span class="sxs-lookup"><span data-stu-id="ba5cc-107">Writing SQL queries for entities</span></span>  

<span data-ttu-id="ba5cc-108">DbSet 的 SqlQuery 方法允许原始 SQL 查询，将返回实体实例写入。</span><span class="sxs-lookup"><span data-stu-id="ba5cc-108">The SqlQuery method on DbSet allows a raw SQL query to be written that will return entity instances.</span></span> <span data-ttu-id="ba5cc-109">返回的对象将由上下文跟踪，就像它们是如果它们未返回 LINQ 查询。</span><span class="sxs-lookup"><span data-stu-id="ba5cc-109">The returned objects will be tracked by the context just as they would be if they were returned by a LINQ query.</span></span> <span data-ttu-id="ba5cc-110">例如：</span><span class="sxs-lookup"><span data-stu-id="ba5cc-110">For example:</span></span>  

``` csharp  
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("SELECT * FROM dbo.Blogs").ToList();
}
```  

<span data-ttu-id="ba5cc-111">请注意，就像 LINQ 查询，不执行查询之前枚举结果-这在上面的示例通过调用 ToList。</span><span class="sxs-lookup"><span data-stu-id="ba5cc-111">Note that, just as for LINQ queries, the query is not executed until the results are enumerated—in the example above this is done with the call to ToList.</span></span>  

<span data-ttu-id="ba5cc-112">每当有原始 SQL 查询写入以下两个原因，应格外小心。</span><span class="sxs-lookup"><span data-stu-id="ba5cc-112">Care should be taken whenever raw SQL queries are written for two reasons.</span></span> <span data-ttu-id="ba5cc-113">首先，应编写查询以确保它仅返回所请求类型的新技术确实的实体。</span><span class="sxs-lookup"><span data-stu-id="ba5cc-113">First, the query should be written to ensure that it only returns entities that are really of the requested type.</span></span> <span data-ttu-id="ba5cc-114">例如，使用功能，例如继承时很容易编写查询将创建的错误的 CLR 类型的实体。</span><span class="sxs-lookup"><span data-stu-id="ba5cc-114">For example, when using features such as inheritance it is easy to write a query that will create entities that are of the wrong CLR type.</span></span>  

<span data-ttu-id="ba5cc-115">第二，某些类型的原始 SQL 查询公开潜在的安全风险，尤其是在方面 SQL 注入式攻击。</span><span class="sxs-lookup"><span data-stu-id="ba5cc-115">Second, some types of raw SQL query expose potential security risks, especially around SQL injection attacks.</span></span> <span data-ttu-id="ba5cc-116">请确保正确的方式来防止此类攻击的参数在查询中使用。</span><span class="sxs-lookup"><span data-stu-id="ba5cc-116">Make sure that you use parameters in your query in the correct way to guard against such attacks.</span></span>  

### <a name="loading-entities-from-stored-procedures"></a><span data-ttu-id="ba5cc-117">从存储过程加载实体</span><span class="sxs-lookup"><span data-stu-id="ba5cc-117">Loading entities from stored procedures</span></span>  

<span data-ttu-id="ba5cc-118">DbSet.SqlQuery 可用于从存储过程的结果加载实体。</span><span class="sxs-lookup"><span data-stu-id="ba5cc-118">You can use DbSet.SqlQuery to load entities from the results of a stored procedure.</span></span> <span data-ttu-id="ba5cc-119">例如，以下代码调用 dbo。GetBlogs 数据库中的过程：</span><span class="sxs-lookup"><span data-stu-id="ba5cc-119">For example, the following code calls the dbo.GetBlogs procedure in the database:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("dbo.GetBlogs").ToList();
}
```  

<span data-ttu-id="ba5cc-120">此外可以将参数传递到存储过程，请使用以下语法：</span><span class="sxs-lookup"><span data-stu-id="ba5cc-120">You can also pass parameters to a stored procedure using the following syntax:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogId = 1;

    var blogs = context.Blogs.SqlQuery("dbo.GetBlogById @p0", blogId).Single();
}
```  

## <a name="writing-sql-queries-for-non-entity-types"></a><span data-ttu-id="ba5cc-121">为非实体类型编写 SQL 查询</span><span class="sxs-lookup"><span data-stu-id="ba5cc-121">Writing SQL queries for non-entity types</span></span>  

<span data-ttu-id="ba5cc-122">可以使用数据库类中的 SqlQuery 方法创建返回任意类型，包括基元类型的实例的 SQL 查询。</span><span class="sxs-lookup"><span data-stu-id="ba5cc-122">A SQL query returning instances of any type, including primitive types, can be created using the SqlQuery method on the Database class.</span></span> <span data-ttu-id="ba5cc-123">例如：</span><span class="sxs-lookup"><span data-stu-id="ba5cc-123">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogNames = context.Database.SqlQuery<string>(
                       "SELECT Name FROM dbo.Blogs").ToList();
}
```  

<span data-ttu-id="ba5cc-124">即使对象是实体类型的实例，则永远不会将由上下文跟踪从 SqlQuery 返回在数据库上的结果。</span><span class="sxs-lookup"><span data-stu-id="ba5cc-124">The results returned from SqlQuery on Database will never be tracked by the context even if the objects are instances of an entity type.</span></span>  

## <a name="sending-raw-commands-to-the-database"></a><span data-ttu-id="ba5cc-125">将原始命令发送到数据库</span><span class="sxs-lookup"><span data-stu-id="ba5cc-125">Sending raw commands to the database</span></span>  

<span data-ttu-id="ba5cc-126">可以在数据库上使用 ExecuteSqlCommand 方法向数据库发送非查询命令。</span><span class="sxs-lookup"><span data-stu-id="ba5cc-126">Non-query commands can be sent to the database using the ExecuteSqlCommand method on Database.</span></span> <span data-ttu-id="ba5cc-127">例如：</span><span class="sxs-lookup"><span data-stu-id="ba5cc-127">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    context.Database.ExecuteSqlCommand(
        "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
}
```  

<span data-ttu-id="ba5cc-128">请注意使用 ExecuteSqlCommand 在数据库中的数据所做的任何更改都不透明的上下文，直到加载或从数据库重新加载实体。</span><span class="sxs-lookup"><span data-stu-id="ba5cc-128">Note that any changes made to data in the database using ExecuteSqlCommand are opaque to the context until entities are loaded or reloaded from the database.</span></span>  

### <a name="output-parameters"></a><span data-ttu-id="ba5cc-129">输出参数</span><span class="sxs-lookup"><span data-stu-id="ba5cc-129">Output Parameters</span></span>  

<span data-ttu-id="ba5cc-130">如果使用输出参数，其值将不可用，直到已完全读取结果。</span><span class="sxs-lookup"><span data-stu-id="ba5cc-130">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="ba5cc-131">这是由于 DbDataReader 的基础行为，请参阅[使用 DataReader 检索数据](http://go.microsoft.com/fwlink/?LinkID=398589)的更多详细信息。</span><span class="sxs-lookup"><span data-stu-id="ba5cc-131">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>  
