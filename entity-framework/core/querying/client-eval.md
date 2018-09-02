---
title: 客户端与服务器评估 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
uid: core/querying/client-eval
ms.openlocfilehash: 78f8d9576748a725634665f915def80b5a13820c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997872"
---
# <a name="client-vs-server-evaluation"></a><span data-ttu-id="b7aaf-102">客户端与服务器评估</span><span class="sxs-lookup"><span data-stu-id="b7aaf-102">Client vs. Server Evaluation</span></span>

<span data-ttu-id="b7aaf-103">Entity Framework Core 支持在客户端上评估查询的各个部分，并将查询的各个部分推送到数据库。</span><span class="sxs-lookup"><span data-stu-id="b7aaf-103">Entity Framework Core supports parts of the query being evaluated on the client and parts of it being pushed to the database.</span></span> <span data-ttu-id="b7aaf-104">由数据库提供程序确定将在数据库中评估查询的哪些部分。</span><span class="sxs-lookup"><span data-stu-id="b7aaf-104">It is up to the database provider to determine which parts of the query will be evaluated in the database.</span></span>

> [!TIP]  
> <span data-ttu-id="b7aaf-105">可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying)。</span><span class="sxs-lookup"><span data-stu-id="b7aaf-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="client-evaluation"></a><span data-ttu-id="b7aaf-106">客户端评估</span><span class="sxs-lookup"><span data-stu-id="b7aaf-106">Client evaluation</span></span>

<span data-ttu-id="b7aaf-107">在下面的示例中，Helper 方法用于标准化从 SQL Server 数据库中返回的博客的 URL。</span><span class="sxs-lookup"><span data-stu-id="b7aaf-107">In the following example a helper method is used to standardize URLs for blogs that are returned from a SQL Server database.</span></span> <span data-ttu-id="b7aaf-108">由于 SQL Server 提供程序对此方法的实现方式没有任何见解，因此不可以将其转换为 SQL。</span><span class="sxs-lookup"><span data-stu-id="b7aaf-108">Because the SQL Server provider has no insight into how this method is implemented, it is not possible to translate it into SQL.</span></span> <span data-ttu-id="b7aaf-109">在数据库中评估查询的所有其他方面，但会在客户端上通过此方法传递返回的 `URL`。</span><span class="sxs-lookup"><span data-stu-id="b7aaf-109">All other aspects of the query are evaluated in the database, but passing the returned `URL` through this method is performed on the client.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs?highlight=6)] -->
``` csharp
var blogs = context.Blogs
    .OrderByDescending(blog => blog.Rating)
    .Select(blog => new
    {
        Id = blog.BlogId,
        Url = StandardizeUrl(blog.Url)
    })
    .ToList();
```

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
public static string StandardizeUrl(string url)
{
    url = url.ToLower();

    if (!url.StartsWith("http://"))
    {
        url = string.Concat("http://", url);
    }

    return url;
}
```

## <a name="disabling-client-evaluation"></a><span data-ttu-id="b7aaf-110">禁用客户端评估</span><span class="sxs-lookup"><span data-stu-id="b7aaf-110">Disabling client evaluation</span></span>

<span data-ttu-id="b7aaf-111">虽然客户端评估非常有用，但在某些情况下可能会导致性能不佳。</span><span class="sxs-lookup"><span data-stu-id="b7aaf-111">While client evaluation can be very useful, in some instances it can result in poor performance.</span></span> <span data-ttu-id="b7aaf-112">请考虑以下查询，其中 Helper 方法现已在筛选器中使用。</span><span class="sxs-lookup"><span data-stu-id="b7aaf-112">Consider the following query, where the helper method is now used in a filter.</span></span> <span data-ttu-id="b7aaf-113">由于无法在数据库中执行此操作，因此所有数据将被拉入内存中，然后会在客户端上应用筛选器。</span><span class="sxs-lookup"><span data-stu-id="b7aaf-113">Because this can't be performed in the database, all the data is pulled into memory and then the filter is applied on the client.</span></span> <span data-ttu-id="b7aaf-114">根据数据量以及筛选出的数据量，这可能会导致性能不佳。</span><span class="sxs-lookup"><span data-stu-id="b7aaf-114">Depending on the amount of data, and how much of that data is filtered out, this could result in poor performance.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

<span data-ttu-id="b7aaf-115">默认情况下，当执行客户端评估时，EF Core 将记录警告。</span><span class="sxs-lookup"><span data-stu-id="b7aaf-115">By default, EF Core will log a warning when client evaluation is performed.</span></span> <span data-ttu-id="b7aaf-116">有关查看日志记录输出的详细信息，请参阅[日志记录](../miscellaneous/logging.md)。</span><span class="sxs-lookup"><span data-stu-id="b7aaf-116">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="b7aaf-117">当客户端评估引发异常或不执行任何操作时，可以更改行为。</span><span class="sxs-lookup"><span data-stu-id="b7aaf-117">You can change the behavior when client evaluation occurs to either throw or do nothing.</span></span> <span data-ttu-id="b7aaf-118">这是在为上下文（如果使用的是 ASP.NET Core，则通常在 `DbContext.OnConfiguring` 或 `Startup.cs` 中）设置选项时完成的。</span><span class="sxs-lookup"><span data-stu-id="b7aaf-118">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
