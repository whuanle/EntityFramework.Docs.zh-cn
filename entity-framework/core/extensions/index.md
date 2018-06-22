---
title: 工具和扩展 - EF Core
author: ErikEJ
ms.author: divega
ms.date: 7/3/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
ms.technology: entity-framework-core
uid: core/extensions/index
ms.openlocfilehash: 6c8cb3e0d8552f274118e4020b7e2e8009af7e11
ms.sourcegitcommit: fc68321c211aca38f7b9dc3a75677c6ca1b2524b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
ms.locfileid: "29769434"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="2a7e6-102">EF Core 工具和扩展</span><span class="sxs-lookup"><span data-stu-id="2a7e6-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="2a7e6-103">工具和扩展为 Entity Framework Core 提供了额外功能。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-103">Tools and extensions provide additional functionality for Entity Framework Core.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="2a7e6-104">扩展由多种源生成，并不会作为 Entity Framework Core 项目的组成部分进行维护。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-104">Extensions are built by a variety of sources and not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="2a7e6-105">考虑使用第三方扩展时，请务必评估质量、授权、兼容性、支持等因素，确保其满足要求。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-105">When considering a third party extension, be sure to evaluate quality, licensing, compatibility, support, etc. to ensure they meet your requirements.</span></span>

## <a name="tools"></a><span data-ttu-id="2a7e6-106">工具</span><span class="sxs-lookup"><span data-stu-id="2a7e6-106">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="2a7e6-107">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="2a7e6-107">LLBLGen Pro</span></span>

<span data-ttu-id="2a7e6-108">LLBLGen Pro 是一种实体建模解决方案，包含对 Entity Framework 和 Entity Framework Core 的支持。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-108">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="2a7e6-109">借助它可轻松通过 Database First 或 Model First 定义实体模型并将其映射到数据库中，使你可以立即开始编写查询。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-109">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span>

[<span data-ttu-id="2a7e6-110">网站</span><span class="sxs-lookup"><span data-stu-id="2a7e6-110">website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="2a7e6-111">Devart Entity Developer</span><span class="sxs-lookup"><span data-stu-id="2a7e6-111">Devart Entity Developer</span></span>

<span data-ttu-id="2a7e6-112">Devart Entity Developer 是一种用于 ADO.NET 实体框架、NHibernate、LinqConnect、Telerik 数据访问和 LINQ to SQL 的强大 ORM 设计器。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-112">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="2a7e6-113">可使用 Model-First 和 Database-First 方法来设计 ORM 模型并为其生成 C# 或 Visual Basic .NET 代码。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-113">You can use  Model-First and Database-First approaches to design your ORM model and generate C# or Visual Basic .NET code for it.</span></span> <span data-ttu-id="2a7e6-114">它引入了新方法来设计 ORM 模型、提高工作效率并促进数据库应用程序的开发。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-114">It introduces new approaches for designing ORM models, boosts productivity, and facilitates the development of database applications.</span></span>

[<span data-ttu-id="2a7e6-115">网站</span><span class="sxs-lookup"><span data-stu-id="2a7e6-115">website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a><span data-ttu-id="2a7e6-116">EF Core Power Tools</span><span class="sxs-lookup"><span data-stu-id="2a7e6-116">EF Core Power Tools</span></span>

<span data-ttu-id="2a7e6-117">Visual Studio 2017 及以上版本扩展。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-117">Visual Studio 2017+ extension.</span></span> <span data-ttu-id="2a7e6-118">可从现有数据库或 SQL Server 数据库项目反向 DbContext 和 POCO 类的工程，并采用各种方式可视化和检查 DbContext。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-118">You can reverse engineer of DbContext and POCO classes from an existing database or SQL Server Database project, and visualize and inspect your DbContext in various ways.</span></span>

[<span data-ttu-id="2a7e6-119">GitHub wiki</span><span class="sxs-lookup"><span data-stu-id="2a7e6-119">GitHub wiki</span></span>](https://github.com/ErikEJ/SqlCeToolbox/wiki/EF-Core-Power-Tools)

## <a name="extensions"></a><span data-ttu-id="2a7e6-120">扩展</span><span class="sxs-lookup"><span data-stu-id="2a7e6-120">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="2a7e6-121">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="2a7e6-121">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="2a7e6-122">Microsoft.EntityFrameworkCore 的一个插件，支持自动记录数据更改历史记录。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-122">A plugin for Microsoft.EntityFrameworkCore to support automatically recording data changes history.</span></span>

[<span data-ttu-id="2a7e6-123">GitHub 存储库</span><span class="sxs-lookup"><span data-stu-id="2a7e6-123">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a><span data-ttu-id="2a7e6-124">Microsoft.EntityFrameworkCore.DynamicLinq</span><span class="sxs-lookup"><span data-stu-id="2a7e6-124">Microsoft.EntityFrameworkCore.DynamicLinq</span></span>

<span data-ttu-id="2a7e6-125">Microsoft.EntityFrameworkCore 的 Dynamic Linq 扩展，添加了异步支持</span><span class="sxs-lookup"><span data-stu-id="2a7e6-125">Dynamic Linq extensions for Microsoft.EntityFrameworkCore which adds Async support</span></span>

 [<span data-ttu-id="2a7e6-126">GitHub 存储库</span><span class="sxs-lookup"><span data-stu-id="2a7e6-126">GitHub repository</span></span>](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efcorepractices"></a><span data-ttu-id="2a7e6-127">EFCore.Practices</span><span class="sxs-lookup"><span data-stu-id="2a7e6-127">EFCore.Practices</span></span>

<span data-ttu-id="2a7e6-128">尝试在支持测试的 API 中捕获一些良好做法或最佳做法 - 包括用于扫描 N+1 查询的小框架。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-128">Attempt to capture some good or best practices in an API that supports testing – including a small framework to scan for N+1 queries.</span></span>

[<span data-ttu-id="2a7e6-129">GitHub 存储库</span><span class="sxs-lookup"><span data-stu-id="2a7e6-129">GitHub repository</span></span>](https://github.com/riezebosch/efcore-practices/tree/master/src/EFCore.Practices/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="2a7e6-130">EFSecondLevelCache.Core</span><span class="sxs-lookup"><span data-stu-id="2a7e6-130">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="2a7e6-131">二级缓存库。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-131">Second Level Caching Library.</span></span> <span data-ttu-id="2a7e6-132">二级缓存是一个查询缓存。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-132">Second level caching is a query cache.</span></span> <span data-ttu-id="2a7e6-133">EF 命令的结果将存储在该缓存中，这样相同的 EF 命令将从该缓存检索其数据，而不是再次针对数据库进行执行。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-133">The results of EF commands will be stored in the cache, so that the same EF commands will retrieve their data from the cache rather than executing them against the database again.</span></span>

[<span data-ttu-id="2a7e6-134">GitHub 存储库</span><span class="sxs-lookup"><span data-stu-id="2a7e6-134">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="detachedentityframework"></a><span data-ttu-id="2a7e6-135">Detached.EntityFramework</span><span class="sxs-lookup"><span data-stu-id="2a7e6-135">Detached.EntityFramework</span></span>

<span data-ttu-id="2a7e6-136">加载和保存整个分离式实体图（具有其子实体和列表的实体）。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-136">Loads and saves entire detached entity graphs (the entity with their child entities and lists).</span></span> <span data-ttu-id="2a7e6-137">灵感来自 [GraphDiff](https://github.com/refactorthis/GraphDiff/)。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-137">Inspired by [GraphDiff](https://github.com/refactorthis/GraphDiff/).</span></span> <span data-ttu-id="2a7e6-138">此想法也添加了一些插件来简化一些重复任务，例如审计和分页。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-138">The idea is also add some plugins to simplificate some repetitive tasks, like auditing and pagination.</span></span>

[<span data-ttu-id="2a7e6-139">GitHub 存储库</span><span class="sxs-lookup"><span data-stu-id="2a7e6-139">GitHub repository</span></span>](https://github.com/leonardoporro/Detached/)

### <a name="entityframeworkcoreprimarykey"></a><span data-ttu-id="2a7e6-140">EntityFrameworkCore.PrimaryKey</span><span class="sxs-lookup"><span data-stu-id="2a7e6-140">EntityFrameworkCore.PrimaryKey</span></span>

<span data-ttu-id="2a7e6-141">从任何作为字典的实体中检索主键（包括复合键）。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-141">Retrieve the primary key (including composite keys) from any entity as a dictionary.</span></span>

[<span data-ttu-id="2a7e6-142">GitHub 存储库</span><span class="sxs-lookup"><span data-stu-id="2a7e6-142">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcorerx"></a><span data-ttu-id="2a7e6-143">EntityFrameworkCore.Rx</span><span class="sxs-lookup"><span data-stu-id="2a7e6-143">EntityFrameworkCore.Rx</span></span>

<span data-ttu-id="2a7e6-144">为 Entity Framework 实体的常用可观察对象重新激活扩展包装器。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-144">Reactive extension wrappers for hot observables of Entity Framework entities.</span></span>

[<span data-ttu-id="2a7e6-145">GitHub 存储库</span><span class="sxs-lookup"><span data-stu-id="2a7e6-145">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.Rx/)

### <a name="entityframeworkcoretriggers"></a><span data-ttu-id="2a7e6-146">EntityFrameworkCore.Triggers</span><span class="sxs-lookup"><span data-stu-id="2a7e6-146">EntityFrameworkCore.Triggers</span></span>

<span data-ttu-id="2a7e6-147">使用插入、更新和删除事件将触发器添加到实体中。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-147">Add triggers to your entities with insert, update, and delete events.</span></span> <span data-ttu-id="2a7e6-148">每个事件具有三种情况：之前、之后和失败时。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-148">There are three events for each: before, after, and upon failure.</span></span>

[<span data-ttu-id="2a7e6-149">GitHub 存储库</span><span class="sxs-lookup"><span data-stu-id="2a7e6-149">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.Triggers/)

### <a name="entityframeworkcoretypedoriginalvalues"></a><span data-ttu-id="2a7e6-150">EntityFrameworkCore.TypedOriginalValues</span><span class="sxs-lookup"><span data-stu-id="2a7e6-150">EntityFrameworkCore.TypedOriginalValues</span></span>

<span data-ttu-id="2a7e6-151">获取对实体属性的 OriginalValue 的类型化访问权限。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-151">Get typed access to the OriginalValue of your entity properties.</span></span> <span data-ttu-id="2a7e6-152">支持简单的和复杂的属性，不支持导航/集合。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-152">Simple and complex properties are supported, navigation/collections are not.</span></span>

[<span data-ttu-id="2a7e6-153">GitHub 存储库</span><span class="sxs-lookup"><span data-stu-id="2a7e6-153">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a><span data-ttu-id="2a7e6-154">Geco</span><span class="sxs-lookup"><span data-stu-id="2a7e6-154">Geco</span></span>

<span data-ttu-id="2a7e6-155">Geco 提供一个支持复数形式/单数形式的反向模型生成器，和基于 C# 6.0 内插字符串且在 .Net Core 上运行的可编辑模板。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-155">Geco provides a Reverse Model generator with support for Pluralization/Singularization and editable templates based on C# 6.0 interpolated strings and running on .Net Core.</span></span> <span data-ttu-id="2a7e6-156">它还提供一个具有 SQL Merge 脚本的源脚本生成器和一个脚本运行程序。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-156">It also provides an Seed script generator with SQL Merge scripts and an script runner.</span></span>

[<span data-ttu-id="2a7e6-157">Github 存储库</span><span class="sxs-lookup"><span data-stu-id="2a7e6-157">Github repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a><span data-ttu-id="2a7e6-158">LinqKit.Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="2a7e6-158">LinqKit.Microsoft.EntityFrameworkCore</span></span>

<span data-ttu-id="2a7e6-159">LinqKit.Microsoft.EntityFrameworkCore 是一组适用于 LINQ to SQL 和 EntityFrameworkCore 高级用户的免费扩展。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-159">LinqKit.Microsoft.EntityFrameworkCore is a free set of extensions for LINQ to SQL and EntityFrameworkCore power users.</span></span> <span data-ttu-id="2a7e6-160">支持 Include(...) 和 IDbAsync。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-160">With Include(...) and IDbAsync support.</span></span>

[<span data-ttu-id="2a7e6-161">GitHub 存储库</span><span class="sxs-lookup"><span data-stu-id="2a7e6-161">GitHub repository</span></span>](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="2a7e6-162">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="2a7e6-162">NeinLinq.EntityFrameworkCore</span></span>

<span data-ttu-id="2a7e6-163">NeinLinq.EntityFrameworkCore 提供对以下操作有所帮助的扩展：使用 LINQ 提供程序（例如仅支持一小部分 .NET 函数的 Entity Framework ）、重用函数、重写查询、甚至让它们处于 null safe 状态以及使用可转换谓词和选择器生成动态查询。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-163">NeinLinq.EntityFrameworkCore provides helpful extensions for using LINQ providers such as Entity Framework that support only a minor subset of .NET functions, reusing functions, rewriting queries, even making them null-safe, and building dynamic queries using translatable predicates and selectors.</span></span>

[<span data-ttu-id="2a7e6-164">GitHub 存储库</span><span class="sxs-lookup"><span data-stu-id="2a7e6-164">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="2a7e6-165">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="2a7e6-165">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="2a7e6-166">Microsoft.EntityFrameworkCore 的一个插件，支持存储库、工作模式单元和多个具有支持的分布式事务的数据库。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-166">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple database with distributed transaction supported.</span></span>

[<span data-ttu-id="2a7e6-167">GitHub 存储库</span><span class="sxs-lookup"><span data-stu-id="2a7e6-167">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="entityframeworklazyloading"></a><span data-ttu-id="2a7e6-168">EntityFramework.LazyLoading</span><span class="sxs-lookup"><span data-stu-id="2a7e6-168">EntityFramework.LazyLoading</span></span>

<span data-ttu-id="2a7e6-169">EF Core 1.1 延迟加载</span><span class="sxs-lookup"><span data-stu-id="2a7e6-169">Lazy Loading for EF Core 1.1</span></span>

[<span data-ttu-id="2a7e6-170">GitHub 存储库</span><span class="sxs-lookup"><span data-stu-id="2a7e6-170">GitHub repository</span></span>](https://github.com/darxis/EntityFramework.LazyLoading)

### <a name="efcorebulkextensions"></a><span data-ttu-id="2a7e6-171">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="2a7e6-171">EFCore.BulkExtensions</span></span>

<span data-ttu-id="2a7e6-172">用于批量操作（插入、更新、删除）的 EntityFrameworkCore 扩展。</span><span class="sxs-lookup"><span data-stu-id="2a7e6-172">EntityFrameworkCore extensions for Bulk operations (Insert, Update, Delete).</span></span>

[<span data-ttu-id="2a7e6-173">GitHub 存储库</span><span class="sxs-lookup"><span data-stu-id="2a7e6-173">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)
