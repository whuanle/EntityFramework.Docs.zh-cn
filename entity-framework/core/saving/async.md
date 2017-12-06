---
title: "异步保存的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
ms.technology: entity-framework-core
uid: core/saving/async
ms.openlocfilehash: 640e5f131b0e9ef4e5028d1dcaf80f3e5bbd9d9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="asynchronous-saving"></a><span data-ttu-id="11f1e-102">异步保存</span><span class="sxs-lookup"><span data-stu-id="11f1e-102">Asynchronous Saving</span></span>

<span data-ttu-id="11f1e-103">异步保存可避免阻止线程，而所做的更改写入到数据库。</span><span class="sxs-lookup"><span data-stu-id="11f1e-103">Asynchronous saving avoids blocking a thread while the changes are written to the database.</span></span> <span data-ttu-id="11f1e-104">这可用于避免冻结粗客户端应用程序的 UI。</span><span class="sxs-lookup"><span data-stu-id="11f1e-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="11f1e-105">异步操作还可以增加 web 应用程序，其中释放线程以在数据库操作完成时处理其他请求中的吞吐量。</span><span class="sxs-lookup"><span data-stu-id="11f1e-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="11f1e-106">有关详细信息，请参阅[C# 中的异步编程](https://docs.microsoft.com/dotnet/csharp/async)。</span><span class="sxs-lookup"><span data-stu-id="11f1e-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="11f1e-107">EF 核心不支持在同一上下文实例上运行的多个并行操作。</span><span class="sxs-lookup"><span data-stu-id="11f1e-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="11f1e-108">你应始终等待操作完成，然后再开始下一步操作。</span><span class="sxs-lookup"><span data-stu-id="11f1e-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="11f1e-109">这通常是通过使用`await`上每个异步操作的关键字。</span><span class="sxs-lookup"><span data-stu-id="11f1e-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="11f1e-110">实体框架内核提供了`DbContext.SaveChangesAsync()`异步除了`DbContext.SaveChanges()`。</span><span class="sxs-lookup"><span data-stu-id="11f1e-110">Entity Framework Core provides `DbContext.SaveChangesAsync()` as an asynchronous alternative to `DbContext.SaveChanges()`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
