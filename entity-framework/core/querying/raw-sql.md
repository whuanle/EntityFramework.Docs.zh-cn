---
title: "原始的 SQL 查询的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
ms.technology: entity-framework-core
uid: core/querying/raw-sql
ms.openlocfilehash: 79894c7b9fd9e40cdf14460abf5d872ee2f4b9f0
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/22/2017
---
# <a name="raw-sql-queries"></a>原始的 SQL 查询

实体框架核心，可使用关系数据库时到原始的 SQL 查询下拉列表。 如果你想要执行的查询无法表示使用 LINQ，或如果使用 LINQ 查询导致效率低下发送到数据库的 SQL，这可能很有用。

> [!TIP]  
> 可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying)。

## <a name="limitations"></a>限制

有一些限制需要注意的时使用原始的 SQL 查询：
* 仅可以使用 SQL 查询返回属于您的模型的实体类型。 没有到积压工作上的增强功能[启用从原始的 SQL 查询返回临时类型](https://github.com/aspnet/EntityFramework/issues/1862)。

* SQL 查询必须返回的所有属性的实体类型的数据。

* 在结果集中的列名必须与属性映射到的列名称匹配。 请注意这不同于从 EF6 其中属性/列映射被忽略的原始 SQL 查询和结果集名称必须匹配的属性名称的列。

* SQL 查询不能包含相关的数据。 但是，在许多情况下您可以通过在查询使用之上`Include`运算符可返回相关的数据 (请参阅[包括相关的数据](#including-related-data))。

* `SELECT`语句传递给此方法通常应为可组合： 如果 EF 核心需要评估在服务器上的其他查询运算符 (例如转换后应用的 LINQ 运算符`FromSql`)，提供的 SQL 将被视为子查询。 这意味着 SQL 传递不应包含任何字符或选项不适用于在子查询，如：
  * 尾随的分号
  * SQL Server 上尾随查询级别提示例如`OPTION (HASH JOIN)`
  * SQL Server 上`ORDER BY`子句不附带的`TOP 100 PERCENT`中`SELECT`子句

* 以外的其他 SQL 语句`SELECT`自动识别为不可编写。 因此，存储过程的完整结果将始终返回到客户端和任何 LINQ 运算符后应用`FromSql`是内存计算中。 

## <a name="basic-raw-sql-queries"></a>基本原始的 SQL 查询

你可以使用*FromSql*扩展方法，以开始基于原始的 SQL 查询的 LINQ 查询。

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

可以使用原始的 SQL 查询来执行存储的过程。

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>传递参数

与任何接受 SQL 的 API，务必参数化任何用户输入以防止受到 SQL 注入攻击。 你可以在 SQL 查询字符串中包含参数占位符，然后提供其他参数作为参数值。 你提供任何参数值将自动转换为`DbParameter`。

下面的示例将一个参数传递给存储过程。 尽管这看上去可能如`String.Format`语法，提供的值包装在插入的位置参数和生成的参数名`{0}`指定占位符。

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

这是相同的查询，不过，使用字符串内插语法，在 EF 核心 2.0 及以上版本支持：

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

你还可以构造 dbparameter 时并将其提供作为参数值。 此选项，可以在 SQL 查询字符串中使用命名的参数

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>编写 LINQ

如果在数据库中，会在编写 SQL 查询，然后您可以通过使用 LINQ 运算符初始原始 SQL 查询前面。 可以组合正在使用的 SQL 查询`SELECT`关键字。

下面的示例使用原始的 SQL 查询，以选择从表值函数 (TVF)，然后组合了对其使用 LINQ 进行筛选和排序。

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a>包括相关的数据

编写 LINQ 运算符可以用于在查询中包含相关的数据。

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> **始终的原始 SQL 查询中使用参数化：** Api 接受原始 SQL 字符串如`FromSql`和`ExecuteSqlCommand`允许轻松作为参数传递的值。 除了验证用户输入，始终为原始 SQL 查询/命令中使用的所有值使用参数化。 如果使用字符串串联来动态生成的查询字符串的任何部分，则你将负责验证任何输入，以防止受到 SQL 注入式攻击。
