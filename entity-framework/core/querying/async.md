---
title: "异步查询的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
ms.technology: entity-framework-core
uid: core/querying/async
ms.openlocfilehash: 6554f04d0edfe0ca2ee72ebed8b878a1997a9500
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="asynchronous-queries"></a><span data-ttu-id="2e5f1-102">异步查询</span><span class="sxs-lookup"><span data-stu-id="2e5f1-102">Asynchronous Queries</span></span>

<span data-ttu-id="2e5f1-103">异步查询避免阻止线程，而在数据库中执行查询。</span><span class="sxs-lookup"><span data-stu-id="2e5f1-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="2e5f1-104">这可用于避免冻结粗客户端应用程序的 UI。</span><span class="sxs-lookup"><span data-stu-id="2e5f1-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="2e5f1-105">异步操作还可以增加 web 应用程序，其中释放线程以在数据库操作完成时处理其他请求中的吞吐量。</span><span class="sxs-lookup"><span data-stu-id="2e5f1-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="2e5f1-106">有关详细信息，请参阅[C# 中的异步编程](https://docs.microsoft.com/dotnet/csharp/async)。</span><span class="sxs-lookup"><span data-stu-id="2e5f1-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="2e5f1-107">EF 核心不支持在同一上下文实例上运行的多个并行操作。</span><span class="sxs-lookup"><span data-stu-id="2e5f1-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="2e5f1-108">你应始终等待操作完成，然后再开始下一步操作。</span><span class="sxs-lookup"><span data-stu-id="2e5f1-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="2e5f1-109">这通常是通过使用`await`上每个异步操作的关键字。</span><span class="sxs-lookup"><span data-stu-id="2e5f1-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="2e5f1-110">实体框架内核提供了一组可用作导致要执行查询的 LINQ 方法并返回结果的替代方法的异步扩展方法。</span><span class="sxs-lookup"><span data-stu-id="2e5f1-110">Entity Framework Core provides a set of asynchronous extension methods that can be used as an alternative to the LINQ methods that cause a query to be executed and results returned.</span></span> <span data-ttu-id="2e5f1-111">示例包括`ToListAsync()`， `ToArrayAsync()`， `SingleAsync()`，等等。不存在的 LINQ 运算符的异步版本如`Where(...)`， `OrderBy(...)`，因为这些方法仅生成 LINQ 表达式树，并且不会导致数据库中执行的查询等等。</span><span class="sxs-lookup"><span data-stu-id="2e5f1-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etc. There are not async versions of LINQ operators such as `Where(...)`, `OrderBy(...)`, etc. because these methods only build up the LINQ expression tree and do not cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="2e5f1-112">EF 核心异步扩展方法定义中`Microsoft.EntityFrameworkCore`命名空间。</span><span class="sxs-lookup"><span data-stu-id="2e5f1-112">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="2e5f1-113">可用的方法，必须导入此命名空间。</span><span class="sxs-lookup"><span data-stu-id="2e5f1-113">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
