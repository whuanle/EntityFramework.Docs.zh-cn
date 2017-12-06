---
title: "原始的 SQL 查询的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
ms.technology: entity-framework-core
uid: core/querying/raw-sql
ms.openlocfilehash: ddf3a841800684688d50dcf9323f4d83c851222f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="raw-sql-queries"></a><span data-ttu-id="22482-102">原始的 SQL 查询</span><span class="sxs-lookup"><span data-stu-id="22482-102">Raw SQL Queries</span></span>

<span data-ttu-id="22482-103">实体框架核心，可使用关系数据库时到原始的 SQL 查询下拉列表。</span><span class="sxs-lookup"><span data-stu-id="22482-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="22482-104">如果你想要执行的查询无法表示使用 LINQ，或如果使用 LINQ 查询导致效率低下发送到数据库的 SQL，这可能很有用。</span><span class="sxs-lookup"><span data-stu-id="22482-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in inefficient SQL being sent to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="22482-105">你可以查看这篇文章[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying)GitHub 上。</span><span class="sxs-lookup"><span data-stu-id="22482-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="limitations"></a><span data-ttu-id="22482-106">限制</span><span class="sxs-lookup"><span data-stu-id="22482-106">Limitations</span></span>

<span data-ttu-id="22482-107">有一些限制需要注意的时使用原始的 SQL 查询：</span><span class="sxs-lookup"><span data-stu-id="22482-107">There are a couple of limitations to be aware of when using raw SQL queries:</span></span>
* <span data-ttu-id="22482-108">仅可以使用 SQL 查询返回属于您的模型的实体类型。</span><span class="sxs-lookup"><span data-stu-id="22482-108">SQL queries can only be used to return entity types that are part of your model.</span></span> <span data-ttu-id="22482-109">没有到积压工作上的增强功能[启用从原始的 SQL 查询返回临时类型](https://github.com/aspnet/EntityFramework/issues/1862)。</span><span class="sxs-lookup"><span data-stu-id="22482-109">There is an enhancement on our backlog to [enable returning ad-hoc types from raw SQL queries](https://github.com/aspnet/EntityFramework/issues/1862).</span></span>

* <span data-ttu-id="22482-110">SQL 查询必须返回的所有属性的实体类型的数据。</span><span class="sxs-lookup"><span data-stu-id="22482-110">The SQL query must return data for all properties of the entity type.</span></span>

* <span data-ttu-id="22482-111">在结果集中的列名必须与属性映射到的列名称匹配。</span><span class="sxs-lookup"><span data-stu-id="22482-111">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="22482-112">请注意这不同于从 EF6 其中属性/列映射被忽略的原始 SQL 查询和结果集名称必须匹配的属性名称的列。</span><span class="sxs-lookup"><span data-stu-id="22482-112">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="22482-113">SQL 查询不能包含相关的数据。</span><span class="sxs-lookup"><span data-stu-id="22482-113">The SQL query cannot contain related data.</span></span> <span data-ttu-id="22482-114">但是，在许多情况下您可以通过在查询使用之上`Include`运算符可返回相关的数据 (请参阅[包括相关的数据](#including-related-data))。</span><span class="sxs-lookup"><span data-stu-id="22482-114">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="22482-115">基本原始的 SQL 查询</span><span class="sxs-lookup"><span data-stu-id="22482-115">Basic raw SQL queries</span></span>

<span data-ttu-id="22482-116">你可以使用*FromSql*扩展方法，以开始基于原始的 SQL 查询的 LINQ 查询。</span><span class="sxs-lookup"><span data-stu-id="22482-116">You can use the *FromSql* extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="22482-117">可以使用原始的 SQL 查询来执行存储的过程。</span><span class="sxs-lookup"><span data-stu-id="22482-117">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="22482-118">传递参数</span><span class="sxs-lookup"><span data-stu-id="22482-118">Passing parameters</span></span>

<span data-ttu-id="22482-119">与任何接受 SQL 的 API，务必参数化任何用户输入以防止受到 SQL 注入攻击。</span><span class="sxs-lookup"><span data-stu-id="22482-119">As with any API that accepts SQL, it is important to parameterize any user input to protect against a SQL injection attack.</span></span> <span data-ttu-id="22482-120">你可以在 SQL 查询字符串中包含参数占位符，然后提供其他参数作为参数值。</span><span class="sxs-lookup"><span data-stu-id="22482-120">You can include parameter placeholders in the SQL query string and then supply parameter values as additional arguments.</span></span> <span data-ttu-id="22482-121">你提供任何参数值将自动转换为`DbParameter`。</span><span class="sxs-lookup"><span data-stu-id="22482-121">Any parameter values you supply will automatically be converted to a `DbParameter`.</span></span>

<span data-ttu-id="22482-122">下面的示例将一个参数传递给存储过程。</span><span class="sxs-lookup"><span data-stu-id="22482-122">The following example passes a single parameter to a stored procedure.</span></span> <span data-ttu-id="22482-123">尽管这看上去可能如`String.Format`语法，提供的值包装在插入的位置参数和生成的参数名`{0}`指定占位符。</span><span class="sxs-lookup"><span data-stu-id="22482-123">While this may look like `String.Format` syntax, the supplied value is wrapped in a parameter and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="22482-124">这是相同的查询，不过，使用字符串内插语法，在 EF 核心 2.0 及以上版本支持：</span><span class="sxs-lookup"><span data-stu-id="22482-124">This is the same query but using string interpolation syntax, which is supported in EF Core 2.0 and above:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="22482-125">你还可以构造 dbparameter 时并将其提供作为参数值。</span><span class="sxs-lookup"><span data-stu-id="22482-125">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="22482-126">此选项，可以在 SQL 查询字符串中使用命名的参数</span><span class="sxs-lookup"><span data-stu-id="22482-126">This allows you to use named parameters in the SQL query string</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="22482-127">编写 LINQ</span><span class="sxs-lookup"><span data-stu-id="22482-127">Composing with LINQ</span></span>

<span data-ttu-id="22482-128">如果在数据库中，会在编写 SQL 查询，然后您可以通过使用 LINQ 运算符初始原始 SQL 查询前面。</span><span class="sxs-lookup"><span data-stu-id="22482-128">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="22482-129">可以组合正在使用的 SQL 查询`SELECT`关键字。</span><span class="sxs-lookup"><span data-stu-id="22482-129">SQL queries that can be composed on being with the `SELECT` keyword.</span></span>

<span data-ttu-id="22482-130">下面的示例使用原始的 SQL 查询，以选择从表值函数 (TVF)，然后组合了对其使用 LINQ 进行筛选和排序。</span><span class="sxs-lookup"><span data-stu-id="22482-130">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a><span data-ttu-id="22482-131">包括相关的数据</span><span class="sxs-lookup"><span data-stu-id="22482-131">Including related data</span></span>

<span data-ttu-id="22482-132">编写 LINQ 运算符可以用于在查询中包含相关的数据。</span><span class="sxs-lookup"><span data-stu-id="22482-132">Composing with LINQ operators can be used to include related data in the query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> <span data-ttu-id="22482-133">**始终的原始 SQL 查询中使用参数化：** Api 接受原始 SQL 字符串如`FromSql`和`ExecuteSqlCommand`允许轻松作为参数传递的值。</span><span class="sxs-lookup"><span data-stu-id="22482-133">**Always use parameterization for raw SQL queries:** APIs that accept a raw SQL string such as `FromSql` and `ExecuteSqlCommand` allow values to be easily passed as parameters.</span></span> <span data-ttu-id="22482-134">除了验证用户输入，始终为原始 SQL 查询/命令中使用的所有值使用参数化。</span><span class="sxs-lookup"><span data-stu-id="22482-134">In addition to validating user input, always use parameterization for any values used in a raw SQL query/command.</span></span> <span data-ttu-id="22482-135">如果使用字符串串联来动态生成的查询字符串的任何部分，则你将负责验证任何输入，以防止受到 SQL 注入式攻击。</span><span class="sxs-lookup"><span data-stu-id="22482-135">If you are using string concatenation to dynamically build any part of the query string then you are responsible for validating any input to protect against SQL injection attacks.</span></span>
