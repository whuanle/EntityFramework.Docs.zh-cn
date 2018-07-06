---
title: 测试以及 InMemory 的 EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 33690e3424d0777930d3cb8167575fb0f4ddd8f7
ms.sourcegitcommit: d096484dcf9eff73d9943fa60db7a418b10ca0b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2018
ms.locfileid: "27995582"
---
# <a name="testing-with-inmemory"></a><span data-ttu-id="68cef-102">测试以及 InMemory</span><span class="sxs-lookup"><span data-stu-id="68cef-102">Testing with InMemory</span></span>

<span data-ttu-id="68cef-103">你想要测试使用的，它模拟连接到真实的数据库，而无需实际的数据库操作开销的组件时，InMemory 提供程序非常有用。</span><span class="sxs-lookup"><span data-stu-id="68cef-103">The InMemory provider is useful when you want to test components using something that approximates connecting to the real database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="68cef-104">可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing)。</span><span class="sxs-lookup"><span data-stu-id="68cef-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub.</span></span>

## <a name="inmemory-is-not-a-relational-database"></a><span data-ttu-id="68cef-105">InMemory 不是一个关系数据库</span><span class="sxs-lookup"><span data-stu-id="68cef-105">InMemory is not a relational database</span></span>

<span data-ttu-id="68cef-106">EF Core 数据库提供程序不需要是关系数据库。</span><span class="sxs-lookup"><span data-stu-id="68cef-106">EF Core database providers do not have to be relational databases.</span></span> <span data-ttu-id="68cef-107">InMemory 旨在作为一般用途数据库进行测试，而不是要模拟关系数据库。</span><span class="sxs-lookup"><span data-stu-id="68cef-107">InMemory is designed to be a general purpose database for testing, and is not designed to mimic a relational database.</span></span>

<span data-ttu-id="68cef-108">包括一些示例：</span><span class="sxs-lookup"><span data-stu-id="68cef-108">Some examples of this include:</span></span>
* <span data-ttu-id="68cef-109">InMemory 将允许你保存将违反引用完整性约束关系数据库中的数据。</span><span class="sxs-lookup"><span data-stu-id="68cef-109">InMemory will allow you to save data that would violate referential integrity constraints in a relational database.</span></span>

* <span data-ttu-id="68cef-110">如果在模型中的属性使用 DefaultValueSql(string)，这是一个关系数据库 API，针对 InMemory 运行时将产生任何影响。</span><span class="sxs-lookup"><span data-stu-id="68cef-110">If you use DefaultValueSql(string) for a property in your model, this is a relational database API and will have no effect when running against InMemory.</span></span>

> [!TIP]  
> <span data-ttu-id="68cef-111">对于许多测试目的这些差异将不起作用。</span><span class="sxs-lookup"><span data-stu-id="68cef-111">For many test purposes these differences will not matter.</span></span> <span data-ttu-id="68cef-112">但是，如果你想要用作测试依据的表现得更像 true 的关系数据库内容，则可以考虑使用[SQLite 内存中模式](sqlite.md)。</span><span class="sxs-lookup"><span data-stu-id="68cef-112">However, if you want to test against something that behaves more like a true relational database, then consider using [SQLite in-memory mode](sqlite.md).</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="68cef-113">示例测试方案</span><span class="sxs-lookup"><span data-stu-id="68cef-113">Example testing scenario</span></span>

<span data-ttu-id="68cef-114">请考虑以下允许应用程序代码执行与博客某些操作的服务。</span><span class="sxs-lookup"><span data-stu-id="68cef-114">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="68cef-115">它在内部使用`DbContext`连接到 SQL Server 数据库。</span><span class="sxs-lookup"><span data-stu-id="68cef-115">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="68cef-116">将发挥作用交换此上下文，以便我们可以编写高效测试此服务，而无需修改代码，或执行大量工作来创建测试连接到 InMemory 数据库上下文的双精度。</span><span class="sxs-lookup"><span data-stu-id="68cef-116">It would be useful to swap this context to connect to an InMemory database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="68cef-117">准备你的上下文</span><span class="sxs-lookup"><span data-stu-id="68cef-117">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="68cef-118">避免配置两个数据库提供程序</span><span class="sxs-lookup"><span data-stu-id="68cef-118">Avoid configuring two database providers</span></span>

<span data-ttu-id="68cef-119">在测试中要从外部配置要使用 InMemory 提供程序的上下文。</span><span class="sxs-lookup"><span data-stu-id="68cef-119">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="68cef-120">如果你要配置数据库提供程序通过重写`OnConfiguring`在您的上下文，然后你需要添加一些条件的代码，以确保你仅配置数据库提供程序，如果其中一个不已配置。</span><span class="sxs-lookup"><span data-stu-id="68cef-120">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> <span data-ttu-id="68cef-121">如果你使用的 ASP.NET Core，则应不需要此代码由于外部 （会在 Startup.cs) 的上下文已配置数据库提供程序。</span><span class="sxs-lookup"><span data-stu-id="68cef-121">If you are using ASP.NET Core, then you should not need this code since your database provider is already configured outside of the context (in Startup.cs).</span></span>

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="68cef-122">添加用于测试的构造函数</span><span class="sxs-lookup"><span data-stu-id="68cef-122">Add a constructor for testing</span></span>

<span data-ttu-id="68cef-123">若要启用针对不同的数据库的测试的最简单方法是修改你公开接受一个构造函数的上下文`DbContextOptions<TContext>`。</span><span class="sxs-lookup"><span data-stu-id="68cef-123">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="68cef-124">`DbContextOptions<TContext>`告知所有其设置，例如要连接到的数据库上下文。</span><span class="sxs-lookup"><span data-stu-id="68cef-124">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="68cef-125">这是通过在您的上下文中运行 OnConfiguring 方法生成的相同对象。</span><span class="sxs-lookup"><span data-stu-id="68cef-125">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="68cef-126">编写测试</span><span class="sxs-lookup"><span data-stu-id="68cef-126">Writing tests</span></span>

<span data-ttu-id="68cef-127">与此提供程序测试的关键是无法识别要使用的 InMemory 提供程序，并控制内存中数据库的作用域的上下文的能力。</span><span class="sxs-lookup"><span data-stu-id="68cef-127">The key to testing with this provider is the ability to tell the context to use the InMemory provider, and control the scope of the in-memory database.</span></span> <span data-ttu-id="68cef-128">通常您希望干净的数据库为每个测试方法。</span><span class="sxs-lookup"><span data-stu-id="68cef-128">Typically you want a clean database for each test method.</span></span>

<span data-ttu-id="68cef-129">下面是使用 InMemory 数据库测试类的一个示例。</span><span class="sxs-lookup"><span data-stu-id="68cef-129">Here is an example of a test class that uses the InMemory database.</span></span> <span data-ttu-id="68cef-130">每个测试方法指定一个唯一的数据库名称，这意味着每个方法都有自己 InMemory 数据库。</span><span class="sxs-lookup"><span data-stu-id="68cef-130">Each test method specifies a unique database name, meaning each method has its own InMemory database.</span></span>

>[!TIP]
> <span data-ttu-id="68cef-131">若要使用`.UseInMemoryDatabase()`扩展方法，引用的 NuGet 包`Microsoft.EntityFrameworkCore.InMemory`。</span><span class="sxs-lookup"><span data-stu-id="68cef-131">To use the `.UseInMemoryDatabase()` extension method, reference the NuGet package `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
