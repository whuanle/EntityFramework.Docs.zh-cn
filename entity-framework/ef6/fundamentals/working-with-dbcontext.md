---
title: 使用 DbContext 的 EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: b0e6bddc-8a87-4d51-b1cb-7756df938c23
caps.latest.revision: 3
ms.openlocfilehash: 865c9883ce25f405a173791df4e46b98550cd41f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/08/2018
ms.locfileid: "39119971"
---
# <a name="working-with-dbcontext"></a><span data-ttu-id="1dd05-102">使用 DbContext</span><span class="sxs-lookup"><span data-stu-id="1dd05-102">Working with DbContext</span></span>

<span data-ttu-id="1dd05-103">若要使用实体框架查询、 插入、 更新和删除使用.NET 对象的数据，需首先[创建模型](~/ef6/modeling/index.md)映射的实体和关系数据库中的表在模型中定义的。</span><span class="sxs-lookup"><span data-stu-id="1dd05-103">In order to use Entity Framework to query, insert, update, and delete data using .NET objects, you first need to [Create a Model](~/ef6/modeling/index.md) which maps the entities and relationships that are defined in your model to tables in a database.</span></span>

<span data-ttu-id="1dd05-104">一旦您有一个模型，与你的应用程序进行交互的主类是`System.Data.Entity.DbContext`（通常称为上下文类）。</span><span class="sxs-lookup"><span data-stu-id="1dd05-104">Once you have a model, the primary class your application interacts with is `System.Data.Entity.DbContext` (often referred to as the context class).</span></span> <span data-ttu-id="1dd05-105">可以使用与模型关联的 DbContext:</span><span class="sxs-lookup"><span data-stu-id="1dd05-105">You can use a DbContext associated to a model to:</span></span>
- <span data-ttu-id="1dd05-106">编写和执行查询</span><span class="sxs-lookup"><span data-stu-id="1dd05-106">Write and execute queries</span></span>   
- <span data-ttu-id="1dd05-107">将查询结果具体化为实体对象</span><span class="sxs-lookup"><span data-stu-id="1dd05-107">Materialize query results as entity objects</span></span>
- <span data-ttu-id="1dd05-108">跟踪对这些对象所做的更改</span><span class="sxs-lookup"><span data-stu-id="1dd05-108">Track changes that are made to those objects</span></span>
- <span data-ttu-id="1dd05-109">保存在数据库上返回的对象更改</span><span class="sxs-lookup"><span data-stu-id="1dd05-109">Persist object changes back on the database</span></span>
- <span data-ttu-id="1dd05-110">在内存中对象绑定到 UI 控件</span><span class="sxs-lookup"><span data-stu-id="1dd05-110">Bind objects in memory to UI controls</span></span>

<span data-ttu-id="1dd05-111">此页提供有关如何管理上下文类的一些指导。</span><span class="sxs-lookup"><span data-stu-id="1dd05-111">This page gives some guidance on how to manage the context class.</span></span>  

## <a name="defining-a-dbcontext-derived-class"></a><span data-ttu-id="1dd05-112">定义派生的 DbContext 类</span><span class="sxs-lookup"><span data-stu-id="1dd05-112">Defining a DbContext derived class</span></span>  

<span data-ttu-id="1dd05-113">使用上下文的建议的方法是定义一个派生自 DbContext，并公开 DbSet 属性所表示的上下文中的指定实体的集合类。</span><span class="sxs-lookup"><span data-stu-id="1dd05-113">The recommended way to work with context is to define a class that derives from DbContext and exposes DbSet properties that represent collections of the specified entities in the context.</span></span> <span data-ttu-id="1dd05-114">如果您正在使用 EF 设计器，将为您生成上下文。</span><span class="sxs-lookup"><span data-stu-id="1dd05-114">If you are working with the EF Designer, the context will be generated for you.</span></span> <span data-ttu-id="1dd05-115">如果您正在使用 Code First，你将通常上下文自行编写。</span><span class="sxs-lookup"><span data-stu-id="1dd05-115">If you are working with Code First, you will typically write the context yourself.</span></span>  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

<span data-ttu-id="1dd05-116">上下文后，你将查询，添加 (使用`Add`或`Attach`方法) 或删除 (使用`Remove`) 通过这些属性的上下文中的实体。</span><span class="sxs-lookup"><span data-stu-id="1dd05-116">Once you have a context, you would query for, add (using `Add` or `Attach` methods ) or remove (using `Remove`) entities in the context through these properties.</span></span> <span data-ttu-id="1dd05-117">访问`DbSet`上下文对象上的属性表示返回指定类型的所有实体的起始查询。</span><span class="sxs-lookup"><span data-stu-id="1dd05-117">Accessing a `DbSet` property on a context object represent a starting query that returns all entities of the specified type.</span></span> <span data-ttu-id="1dd05-118">请注意，只需访问属性时，将执行查询。</span><span class="sxs-lookup"><span data-stu-id="1dd05-118">Note that just accessing a property will not execute the query.</span></span> <span data-ttu-id="1dd05-119">执行查询时：</span><span class="sxs-lookup"><span data-stu-id="1dd05-119">A query is executed when:</span></span>  

- <span data-ttu-id="1dd05-120">`foreach` (C#) 或 `For Each` (Visual Basic) 语句枚举对象。</span><span class="sxs-lookup"><span data-stu-id="1dd05-120">It is enumerated by a `foreach` (C#) or `For Each` (Visual Basic) statement.</span></span>  
- <span data-ttu-id="1dd05-121">它枚举一组操作如`ToArray`， `ToDictionary`，或`ToList`。</span><span class="sxs-lookup"><span data-stu-id="1dd05-121">It is enumerated by a collection operation such as `ToArray`, `ToDictionary`, or `ToList`.</span></span>  
- <span data-ttu-id="1dd05-122">LINQ 运算符，如`First`或`Any`查询的最外面部分中指定。</span><span class="sxs-lookup"><span data-stu-id="1dd05-122">LINQ operators such as `First` or `Any` are specified in the outermost part of the query.</span></span>  
- <span data-ttu-id="1dd05-123">调用以下方法之一：`Load`扩展方法`DbEntityEntry.Reload`， `Database.ExecuteSqlCommand`，和`DbSet<T>.Find`，如果具有指定键的实体找不到已加载的上下文中。</span><span class="sxs-lookup"><span data-stu-id="1dd05-123">One of the following methods are called: the `Load` extension method, `DbEntityEntry.Reload`,  `Database.ExecuteSqlCommand`, and `DbSet<T>.Find`, if an entity with the specified key is not found already loaded in the context.</span></span>  

## <a name="lifetime"></a><span data-ttu-id="1dd05-124">生存期</span><span class="sxs-lookup"><span data-stu-id="1dd05-124">Lifetime</span></span>  

<span data-ttu-id="1dd05-125">上下文的生存期开始时创建实例，并结束时释放或垃圾回收该实例。</span><span class="sxs-lookup"><span data-stu-id="1dd05-125">The lifetime of the context begins when the instance is created and ends when the instance is either disposed or garbage-collected.</span></span> <span data-ttu-id="1dd05-126">使用**使用**如果你想在块的末尾释放上下文控制的所有资源。</span><span class="sxs-lookup"><span data-stu-id="1dd05-126">Use **using** if you want all the resources that the context controls to be disposed at the end of the block.</span></span> <span data-ttu-id="1dd05-127">当你使用**使用**，编译器将自动创建 try/finally 块中调用释放**最后**块。</span><span class="sxs-lookup"><span data-stu-id="1dd05-127">When you use **using**, the compiler automatically creates a try/finally block and calls dispose in the **finally** block.</span></span>  

``` csharp
public void UseProducts()
{
    using (var context = new ProductContext())
    {     
        // Perform data access using the context
    }
}
```  

<span data-ttu-id="1dd05-128">在决定对上下文的生存期时，以下是一些通用准则：</span><span class="sxs-lookup"><span data-stu-id="1dd05-128">Here are some general guidelines when deciding on the lifetime of the context:</span></span>  

- <span data-ttu-id="1dd05-129">使用 Web 应用程序，使用每个请求的上下文实例。</span><span class="sxs-lookup"><span data-stu-id="1dd05-129">When working with Web applications, use a context instance per request.</span></span>  
- <span data-ttu-id="1dd05-130">使用 Windows Presentation Foundation (WPF) 或 Windows 窗体，使用每个窗体的上下文实例。</span><span class="sxs-lookup"><span data-stu-id="1dd05-130">When working with Windows Presentation Foundation (WPF) or Windows Forms, use a context instance per form.</span></span> <span data-ttu-id="1dd05-131">这使您可以使用更改跟踪功能提供了该上下文。</span><span class="sxs-lookup"><span data-stu-id="1dd05-131">This lets you use change-tracking functionality that context provides.</span></span>  
- <span data-ttu-id="1dd05-132">如果上下文实例创建了依赖关系注入容器，它通常是容器的要释放上下文的责任。</span><span class="sxs-lookup"><span data-stu-id="1dd05-132">If the context instance is created by a dependency injection container, it is usually the responsibility of the container to dispose the context.</span></span>
- <span data-ttu-id="1dd05-133">如果应用程序代码中创建了上下文，请记住在不再需要时释放上下文。</span><span class="sxs-lookup"><span data-stu-id="1dd05-133">If the context is created in application code, remember to dispose of the context when it is no longer required.</span></span>  
- <span data-ttu-id="1dd05-134">使用长时间运行的上下文时考虑以下方面：</span><span class="sxs-lookup"><span data-stu-id="1dd05-134">When working with long-running context consider the following:</span></span>  
    - <span data-ttu-id="1dd05-135">加载到内存更多对象和它们的引用，如上下文的内存消耗可能会快速增加。</span><span class="sxs-lookup"><span data-stu-id="1dd05-135">As you load more objects and their references into memory, the memory consumption of the context may increase rapidly.</span></span> <span data-ttu-id="1dd05-136">这可能会导致性能问题。</span><span class="sxs-lookup"><span data-stu-id="1dd05-136">This may cause performance issues.</span></span>  
    - <span data-ttu-id="1dd05-137">上下文不是线程安全的因此它不应共享跨多个线程同时执行它的工作。</span><span class="sxs-lookup"><span data-stu-id="1dd05-137">The context is not thread-safe, therefore it should not be shared across multiple threads doing work on it concurrently.</span></span>
    - <span data-ttu-id="1dd05-138">如果异常会导致上下文处于不可恢复的状态，可能会终止整个应用程序。</span><span class="sxs-lookup"><span data-stu-id="1dd05-138">If an exception causes the context to be in an unrecoverable state, the whole application may terminate.</span></span>  
    - <span data-ttu-id="1dd05-139">随着查询数据的时间和更新数据的时间的差距增大，出现与并发性相关的问题的可能性将会增加。</span><span class="sxs-lookup"><span data-stu-id="1dd05-139">The chances of running into concurrency-related issues increase as the gap between the time when the data is queried and updated grows.</span></span>  

## <a name="connections"></a><span data-ttu-id="1dd05-140">连接</span><span class="sxs-lookup"><span data-stu-id="1dd05-140">Connections</span></span>  

<span data-ttu-id="1dd05-141">默认情况下，上下文管理与数据库的连接。</span><span class="sxs-lookup"><span data-stu-id="1dd05-141">By default, the context manages connections to the database.</span></span> <span data-ttu-id="1dd05-142">上下文将打开和关闭连接，根据需要。</span><span class="sxs-lookup"><span data-stu-id="1dd05-142">The context opens and closes connections as needed.</span></span> <span data-ttu-id="1dd05-143">例如，上下文打开连接来执行查询，，并在处理所有结果集时，然后关闭连接。</span><span class="sxs-lookup"><span data-stu-id="1dd05-143">For example, the context opens a connection to execute a query, and then closes the connection when all the result sets have been processed.</span></span>  

<span data-ttu-id="1dd05-144">在某些情况下，您需要加强控制应在哪些情况下打开和关闭连接。</span><span class="sxs-lookup"><span data-stu-id="1dd05-144">There are cases when you want to have more control over when the connection opens and closes.</span></span> <span data-ttu-id="1dd05-145">例如，在使用 SQL Server Compact，通常建议来维护与数据库的应用程序以提高性能的生存期内的单独打开连接。</span><span class="sxs-lookup"><span data-stu-id="1dd05-145">For example, when working with SQL Server Compact, it is often recommended to maintain a separate open connection to the database for the lifetime of the application to improve performance.</span></span> <span data-ttu-id="1dd05-146">您可以使用 `Connection` 属性手动管理此过程。</span><span class="sxs-lookup"><span data-stu-id="1dd05-146">You can manage this process manually by using the `Connection` property.</span></span>  
