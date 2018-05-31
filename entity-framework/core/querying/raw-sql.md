---
title: 原生 SQL 查询 - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
ms.technology: entity-framework-core
uid: core/querying/raw-sql
ms.openlocfilehash: 29b7e20e875bf791a88a92636c1df4bc4e31656b
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2018
ms.locfileid: "34163208"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="6560a-102">原生 SQL 查询</span><span class="sxs-lookup"><span data-stu-id="6560a-102">Raw SQL Queries</span></span>

<span data-ttu-id="6560a-103">通过 Entity Framework Core 可以在使用关系数据库时下拉到原生 SQL 查询。</span><span class="sxs-lookup"><span data-stu-id="6560a-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="6560a-104">这在无法使用 LINQ 表示要执行的查询或在使用 LINQ 查询导致 SQL 发送到数据库的效率低时非常有用。</span><span class="sxs-lookup"><span data-stu-id="6560a-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in inefficient SQL being sent to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="6560a-105">可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying)。</span><span class="sxs-lookup"><span data-stu-id="6560a-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="limitations"></a><span data-ttu-id="6560a-106">限制</span><span class="sxs-lookup"><span data-stu-id="6560a-106">Limitations</span></span>

<span data-ttu-id="6560a-107">使用原生 SQL 查询时需注意以下几个限制：</span><span class="sxs-lookup"><span data-stu-id="6560a-107">There are a few limitations to be aware of when using raw SQL queries:</span></span>
* <span data-ttu-id="6560a-108">SQL 查询只能用于返回属于你的模型的实体类型。</span><span class="sxs-lookup"><span data-stu-id="6560a-108">SQL queries can only be used to return entity types that are part of your model.</span></span> <span data-ttu-id="6560a-109">积压工作有一个增强功能，即[可从原生 SQL 查询中返回临时类型](https://github.com/aspnet/EntityFramework/issues/1862)。</span><span class="sxs-lookup"><span data-stu-id="6560a-109">There is an enhancement on our backlog to [enable returning ad-hoc types from raw SQL queries](https://github.com/aspnet/EntityFramework/issues/1862).</span></span>

* <span data-ttu-id="6560a-110">SQL 查询必须返回实体或查询类型的所有属性的数据。</span><span class="sxs-lookup"><span data-stu-id="6560a-110">The SQL query must return data for all properties of the entity or query type.</span></span>

* <span data-ttu-id="6560a-111">结果集中的列名必须与属性映射到的列名称匹配。</span><span class="sxs-lookup"><span data-stu-id="6560a-111">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="6560a-112">请注意，这与 EF6 不同，其中已忽略原生 SQL 查询的属性/列映射，且结果集列名称必须与属性名称匹配。</span><span class="sxs-lookup"><span data-stu-id="6560a-112">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="6560a-113">SQL 查询不能包含相关数据。</span><span class="sxs-lookup"><span data-stu-id="6560a-113">The SQL query cannot contain related data.</span></span> <span data-ttu-id="6560a-114">但是，在许多情况下可以使用 `Include` 运算符在查询顶部进行编写以返回相关数据（请参阅[包括相关数据](#including-related-data)）。</span><span class="sxs-lookup"><span data-stu-id="6560a-114">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="6560a-115">传递到此方法的 `SELECT` 语句通常应该可以进行编写：如果 EF Core 需要计算服务器上的其他查询运算符（例如，转换 `FromSql` 后应用的 LINQ 运算符），则提供的 SQL 将被视为子查询。</span><span class="sxs-lookup"><span data-stu-id="6560a-115">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (e.g. to translate LINQ operators applied after `FromSql`), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="6560a-116">这意味着传递的 SQL 不应包含子查询上无效的任何字符或选项，如：</span><span class="sxs-lookup"><span data-stu-id="6560a-116">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="6560a-117">尾随分号</span><span class="sxs-lookup"><span data-stu-id="6560a-117">a trailing semicolon</span></span>
  * <span data-ttu-id="6560a-118">在 SQL Server 上，尾随的查询级提示，如 `OPTION (HASH JOIN)`</span><span class="sxs-lookup"><span data-stu-id="6560a-118">On SQL Server, a trailing query-level hint, e.g. `OPTION (HASH JOIN)`</span></span>
  * <span data-ttu-id="6560a-119">在 SQL Server 上，不附带 `SELECT` 子句中的 `TOP 100 PERCENT` 的 `ORDER BY` 子句</span><span class="sxs-lookup"><span data-stu-id="6560a-119">On SQL Server, an `ORDER BY` clause that is not accompanied of `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="6560a-120">除 `SELECT` 以外的其他 SQL 语句自动识别为不可编写。</span><span class="sxs-lookup"><span data-stu-id="6560a-120">SQL statements other than `SELECT` are recognized automatically as non-composable.</span></span> <span data-ttu-id="6560a-121">因此，存储过程的完整结果将始终返回到客户端，且在内存中计算 `FromSql` 后应用的任何 LINQ 运算符。</span><span class="sxs-lookup"><span data-stu-id="6560a-121">As a consequence, the full results of stored procedures are always returned to the client and any LINQ operators applied after `FromSql` are evaluated in-memory.</span></span> 

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="6560a-122">基本原生 SQL 查询</span><span class="sxs-lookup"><span data-stu-id="6560a-122">Basic raw SQL queries</span></span>

<span data-ttu-id="6560a-123">可以使用 *FromSql* 扩展方法基于原生 SQL 查询开始 LINQ 查询。</span><span class="sxs-lookup"><span data-stu-id="6560a-123">You can use the *FromSql* extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="6560a-124">原生 SQL 查询可用于执行存储过程。</span><span class="sxs-lookup"><span data-stu-id="6560a-124">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="6560a-125">传递参数</span><span class="sxs-lookup"><span data-stu-id="6560a-125">Passing parameters</span></span>

<span data-ttu-id="6560a-126">正如接受 SQL 的任何 API 一样，务必参数化任何用户输入以抵御 SQL 注入攻击。</span><span class="sxs-lookup"><span data-stu-id="6560a-126">As with any API that accepts SQL, it is important to parameterize any user input to protect against a SQL injection attack.</span></span> <span data-ttu-id="6560a-127">可以将参数占位符包含在 SQL 查询字符串中，然后提供参数值作为其他参数。</span><span class="sxs-lookup"><span data-stu-id="6560a-127">You can include parameter placeholders in the SQL query string and then supply parameter values as additional arguments.</span></span> <span data-ttu-id="6560a-128">提供的任何参数值将自动转换为 `DbParameter`。</span><span class="sxs-lookup"><span data-stu-id="6560a-128">Any parameter values you supply will automatically be converted to a `DbParameter`.</span></span>

<span data-ttu-id="6560a-129">下面的示例将一个参数传递到存储过程。</span><span class="sxs-lookup"><span data-stu-id="6560a-129">The following example passes a single parameter to a stored procedure.</span></span> <span data-ttu-id="6560a-130">尽管这看上去可能像 `String.Format` 语法，但提供的值包装在参数中，且生成的参数名称插入在指定 `{0}` 占位符的位置。</span><span class="sxs-lookup"><span data-stu-id="6560a-130">While this may look like `String.Format` syntax, the supplied value is wrapped in a parameter and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="6560a-131">这是相同的查询，但使用 EF Core 2.0 及更高版本支持的字符串内插语法：</span><span class="sxs-lookup"><span data-stu-id="6560a-131">This is the same query but using string interpolation syntax, which is supported in EF Core 2.0 and above:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="6560a-132">还可以构造 DbParameter 并将其作为参数值提供。</span><span class="sxs-lookup"><span data-stu-id="6560a-132">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="6560a-133">这样可以在 SQL 查询字符串中使用命名参数</span><span class="sxs-lookup"><span data-stu-id="6560a-133">This allows you to use named parameters in the SQL query string</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="6560a-134">使用 LINQ 编写</span><span class="sxs-lookup"><span data-stu-id="6560a-134">Composing with LINQ</span></span>

<span data-ttu-id="6560a-135">如果在数据库中可以在 SQL 查询上进行编写，则可以使用 LINQ 运算符在初始原生 SQL 查询顶部进行编写。</span><span class="sxs-lookup"><span data-stu-id="6560a-135">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="6560a-136">可以使用 `SELECT` 关键字在其上进行编写的 SQL 查询。</span><span class="sxs-lookup"><span data-stu-id="6560a-136">SQL queries that can be composed on being with the `SELECT` keyword.</span></span>

<span data-ttu-id="6560a-137">下面的示例使用可从表值函数 (TVF) 中选择的原生 SQL 查询，然后使用 LINQ 在其上进行编写以执行筛选和排序。</span><span class="sxs-lookup"><span data-stu-id="6560a-137">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a><span data-ttu-id="6560a-138">包括相关数据</span><span class="sxs-lookup"><span data-stu-id="6560a-138">Including related data</span></span>

<span data-ttu-id="6560a-139">使用 LINQ 运算符编写可用于将相关数据包含在查询中。</span><span class="sxs-lookup"><span data-stu-id="6560a-139">Composing with LINQ operators can be used to include related data in the query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> <span data-ttu-id="6560a-140">**始终为原生 SQL 查询使用参数化：** 接受原生 SQL 字符串（如 `FromSql` 和 `ExecuteSqlCommand`）的 API 允许将值作为参数轻松传递。</span><span class="sxs-lookup"><span data-stu-id="6560a-140">**Always use parameterization for raw SQL queries:** APIs that accept a raw SQL string such as `FromSql` and `ExecuteSqlCommand` allow values to be easily passed as parameters.</span></span> <span data-ttu-id="6560a-141">除了验证用户输入以外，始终为原生 SQL 查询/命令中使用的所有值使用参数化。</span><span class="sxs-lookup"><span data-stu-id="6560a-141">In addition to validating user input, always use parameterization for any values used in a raw SQL query/command.</span></span> <span data-ttu-id="6560a-142">如果使用字符串串联来动态生成查询字符串的任何部分，则你将负责验证所有输入以抵御 SQL 注入攻击。</span><span class="sxs-lookup"><span data-stu-id="6560a-142">If you are using string concatenation to dynamically build any part of the query string then you are responsible for validating any input to protect against SQL injection attacks.</span></span>
