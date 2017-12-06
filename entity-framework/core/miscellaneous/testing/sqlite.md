---
title: "使用 SQLite 的 EF 核心测试"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: 8fcb4b1a37da6f52219ebe3c672160627fe28fab
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="testing-with-sqlite"></a><span data-ttu-id="03409-102">使用 SQLite 测试</span><span class="sxs-lookup"><span data-stu-id="03409-102">Testing with SQLite</span></span>

<span data-ttu-id="03409-103">SQLite 具有内存中模式，您可以使用 SQLite 来编写针对关系数据库，而无需实际的数据库操作开销的测试。</span><span class="sxs-lookup"><span data-stu-id="03409-103">SQLite has an in-memory mode that allows you to use SQLite to write tests against a relational database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="03409-104">你可以查看这篇文章[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing)GitHub 上</span><span class="sxs-lookup"><span data-stu-id="03409-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="03409-105">示例测试方案</span><span class="sxs-lookup"><span data-stu-id="03409-105">Example testing scenario</span></span>

<span data-ttu-id="03409-106">请考虑以下允许应用程序代码执行与博客某些操作的服务。</span><span class="sxs-lookup"><span data-stu-id="03409-106">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="03409-107">它在内部使用`DbContext`连接到 SQL Server 数据库。</span><span class="sxs-lookup"><span data-stu-id="03409-107">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="03409-108">它会有用要交换此上下文，以便我们可以编写高效测试此服务，而无需修改代码，或执行大量工作来创建测试连接到内存中 SQLite 数据库上下文的双精度。</span><span class="sxs-lookup"><span data-stu-id="03409-108">It would be useful to swap this context to connect to an in-memory SQLite database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="03409-109">准备你的上下文</span><span class="sxs-lookup"><span data-stu-id="03409-109">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="03409-110">避免配置两个数据库提供程序</span><span class="sxs-lookup"><span data-stu-id="03409-110">Avoid configuring two database providers</span></span>

<span data-ttu-id="03409-111">在测试中要从外部配置要使用 InMemory 提供程序的上下文。</span><span class="sxs-lookup"><span data-stu-id="03409-111">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="03409-112">如果你要配置数据库提供程序通过重写`OnConfiguring`在您的上下文，然后你需要添加一些条件的代码，以确保你仅配置数据库提供程序，如果其中一个不已配置。</span><span class="sxs-lookup"><span data-stu-id="03409-112">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

> [!TIP]  
> <span data-ttu-id="03409-113">如果你使用的 ASP.NET 核心，则应不需要此代码由于数据库提供程序配置 （会在 Startup.cs) 的上下文之外。</span><span class="sxs-lookup"><span data-stu-id="03409-113">If you are using ASP.NET Core, then you should not need this code since your database provider is configured outside of the context (in Startup.cs).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="03409-114">添加用于测试的构造函数</span><span class="sxs-lookup"><span data-stu-id="03409-114">Add a constructor for testing</span></span>

<span data-ttu-id="03409-115">若要启用针对不同的数据库的测试的最简单方法是修改你公开接受一个构造函数的上下文`DbContextOptions<TContext>`。</span><span class="sxs-lookup"><span data-stu-id="03409-115">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="03409-116">`DbContextOptions<TContext>`告知所有其设置，例如要连接到的数据库上下文。</span><span class="sxs-lookup"><span data-stu-id="03409-116">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="03409-117">这是通过在您的上下文中运行 OnConfiguring 方法生成的相同对象。</span><span class="sxs-lookup"><span data-stu-id="03409-117">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="03409-118">编写测试</span><span class="sxs-lookup"><span data-stu-id="03409-118">Writing tests</span></span>

<span data-ttu-id="03409-119">与此提供程序测试的关键是无法识别要使用 SQLite，并控制内存中数据库的作用域的上下文的能力。</span><span class="sxs-lookup"><span data-stu-id="03409-119">The key to testing with this provider is the ability to tell the context to use SQLite, and control the scope of the in-memory database.</span></span> <span data-ttu-id="03409-120">数据库范围的受打开和关闭连接。</span><span class="sxs-lookup"><span data-stu-id="03409-120">The scope of the database is controlled by opening and closing the connection.</span></span> <span data-ttu-id="03409-121">数据库的连接已打开的持续时间限定。</span><span class="sxs-lookup"><span data-stu-id="03409-121">The database is scoped to the duration that the connection is open.</span></span> <span data-ttu-id="03409-122">通常您希望干净的数据库为每个测试方法。</span><span class="sxs-lookup"><span data-stu-id="03409-122">Typically you want a clean database for each test method.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
