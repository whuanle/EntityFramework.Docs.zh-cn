---
title: "EF Core 1.0 中的新增功能 - EF Core"
author: divega
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 20A25111-AEBE-4BC2-83A5-3F651952DF72
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-1.0
ms.openlocfilehash: e5b9e57a01ff302b1d7bd0fc5419aa5b8213865e
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="features-included-in-ef-core-10"></a><span data-ttu-id="fb849-102">EF Core 1.0 中包含的功能</span><span class="sxs-lookup"><span data-stu-id="fb849-102">Features included in EF Core 1.0</span></span>

## <a name="platforms"></a><span data-ttu-id="fb849-103">平台</span><span class="sxs-lookup"><span data-stu-id="fb849-103">Platforms</span></span>
### <a name="net-framework-451"></a><span data-ttu-id="fb849-104">.NET Framework 4.5.1</span><span class="sxs-lookup"><span data-stu-id="fb849-104">.NET Framework 4.5.1</span></span>
<span data-ttu-id="fb849-105">包括控制台、WPF、WinForms、ASP.NET 4 等。</span><span class="sxs-lookup"><span data-stu-id="fb849-105">Includes Console, WPF, WinForms, ASP.NET 4, etc.</span></span>
### <a name="net-standard-13"></a><span data-ttu-id="fb849-106">.NET Standard 1.3</span><span class="sxs-lookup"><span data-stu-id="fb849-106">.NET Standard 1.3</span></span>
<span data-ttu-id="fb849-107">包括面向 Windows、OSX 和 Linux 上的 .NET Framework 和 .NET Core 的 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="fb849-107">Including ASP.NET Core targeting both .NET Framework and .NET Core on Windows, OSX, and Linux.</span></span>

## <a name="modelling"></a><span data-ttu-id="fb849-108">建模</span><span class="sxs-lookup"><span data-stu-id="fb849-108">Modelling</span></span>
### <a name="basic-modelling"></a><span data-ttu-id="fb849-109">基本建模</span><span class="sxs-lookup"><span data-stu-id="fb849-109">Basic modelling</span></span>
<span data-ttu-id="fb849-110">基于具有常用标量类型（`int`、`string` 等）的 get/set 属性的 POCO 实体。</span><span class="sxs-lookup"><span data-stu-id="fb849-110">Based on POCO entities with get/set properties of common scalar types (`int`, `string`, etc.).</span></span>
### <a name="relationships-and-navigation-properties"></a><span data-ttu-id="fb849-111">关系和导航属性</span><span class="sxs-lookup"><span data-stu-id="fb849-111">Relationships and navigation properties</span></span>
<span data-ttu-id="fb849-112">可以在模型中根据外键指定一对多和一对零或一对一关系。</span><span class="sxs-lookup"><span data-stu-id="fb849-112">One-to-many and One-to-zero-or-one relationships can be specified in the model based on a foreign key.</span></span> <span data-ttu-id="fb849-113">简单集合或引用类型的导航属性可以与这些关系相关联。</span><span class="sxs-lookup"><span data-stu-id="fb849-113">Navigation properties of simple collection or reference types can be associated with these relationships.</span></span>
### <a name="built-in-conventions"></a><span data-ttu-id="fb849-114">内置约定</span><span class="sxs-lookup"><span data-stu-id="fb849-114">Built-in conventions</span></span>
<span data-ttu-id="fb849-115">这些约定基于实体类的形状构建初始模型。</span><span class="sxs-lookup"><span data-stu-id="fb849-115">These build an initial model based on the shape of the entity classes.</span></span>
### <a name="fluent-api"></a><span data-ttu-id="fb849-116">Fluent API</span><span class="sxs-lookup"><span data-stu-id="fb849-116">Fluent API</span></span>
<span data-ttu-id="fb849-117">允许覆盖上下文中的 `OnModelCreating` 方法，以进一步配置按约定发现的模型。</span><span class="sxs-lookup"><span data-stu-id="fb849-117">Allows you to override the `OnModelCreating` method on your context to further configure the model that was discovered by convention.</span></span>
### <a name="data-annotations"></a><span data-ttu-id="fb849-118">数据注释</span><span class="sxs-lookup"><span data-stu-id="fb849-118">Data annotations</span></span>
<span data-ttu-id="fb849-119">可以添加到实体类/属性并可影响 EF 模型的属性（即添加 [必需] 将让 EF 知道属性是必需的）。</span><span class="sxs-lookup"><span data-stu-id="fb849-119">Are attributes that can be added to your entity classes/properties and will influence the EF model (i.e. adding [Required] will let EF know that a property is required).</span></span>
### <a name="relational-table-mapping"></a><span data-ttu-id="fb849-120">关系表映射</span><span class="sxs-lookup"><span data-stu-id="fb849-120">Relational Table mapping</span></span>
<span data-ttu-id="fb849-121">允许实体映射到表/列。</span><span class="sxs-lookup"><span data-stu-id="fb849-121">Allows entities to be mapped to tables/columns.</span></span>
### <a name="key-value-generation"></a><span data-ttu-id="fb849-122">键值生成</span><span class="sxs-lookup"><span data-stu-id="fb849-122">Key value generation</span></span>
<span data-ttu-id="fb849-123">包括客户端生成和数据库生成。</span><span class="sxs-lookup"><span data-stu-id="fb849-123">Including client-side generation and database generation.</span></span>
### <a name="database-generated-values"></a><span data-ttu-id="fb849-124">数据库生成的值</span><span class="sxs-lookup"><span data-stu-id="fb849-124">Database generated values</span></span>
<span data-ttu-id="fb849-125">允许在插入（默认值）或更新（计算列）时由数据库生成值。</span><span class="sxs-lookup"><span data-stu-id="fb849-125">Allows for values to be generated by the database on insert (default values) or update (computed columns).</span></span>
### <a name="sequences-in-sql-server"></a><span data-ttu-id="fb849-126">SQL Server 中的序列</span><span class="sxs-lookup"><span data-stu-id="fb849-126">Sequences in SQL Server</span></span>
<span data-ttu-id="fb849-127">允许在模型中定义序列对象。</span><span class="sxs-lookup"><span data-stu-id="fb849-127">Allows for sequence objects to be defined in the model.</span></span>
### <a name="unique-constraints"></a><span data-ttu-id="fb849-128">UNIQUE 约束</span><span class="sxs-lookup"><span data-stu-id="fb849-128">Unique constraints</span></span>
<span data-ttu-id="fb849-129">允许定义备用键以及面向该键的关系的功能。</span><span class="sxs-lookup"><span data-stu-id="fb849-129">Allows for the definition of alternate keys and the ability to define relationships that target that key.</span></span>
### <a name="indexes"></a><span data-ttu-id="fb849-130">索引</span><span class="sxs-lookup"><span data-stu-id="fb849-130">Indexes</span></span>
<span data-ttu-id="fb849-131">在模型中定义索引会自动在数据库中引入索引。</span><span class="sxs-lookup"><span data-stu-id="fb849-131">Defining indexes in the model automatically introduces indexes in the database.</span></span> <span data-ttu-id="fb849-132">此外，还支持唯一索引。</span><span class="sxs-lookup"><span data-stu-id="fb849-132">Unique indexes are also supported.</span></span>
### <a name="shadow-state-properties"></a><span data-ttu-id="fb849-133">阴影状态属性</span><span class="sxs-lookup"><span data-stu-id="fb849-133">Shadow state properties</span></span>
<span data-ttu-id="fb849-134">允许在未在 .NET 类中声明和存储的模型中定义属性，但可以由 EF Core 进行跟踪和更新。</span><span class="sxs-lookup"><span data-stu-id="fb849-134">Allows for properties to be defined in the model that are not declared and are not stored in the .NET class but can be tracked and updated by EF Core.</span></span> <span data-ttu-id="fb849-135">不需要在对象中公开这些属性时，通常用于外键属性。</span><span class="sxs-lookup"><span data-stu-id="fb849-135">Commonly used for foreign key properties when exposing these in the object is not desired.</span></span>
### <a name="table-per-hierarchy-inheritance-pattern"></a><span data-ttu-id="fb849-136">“每个层次结构一张表”继承模式</span><span class="sxs-lookup"><span data-stu-id="fb849-136">Table-Per-Hierarchy inheritance pattern</span></span>
<span data-ttu-id="fb849-137">允许将继承层次结构中的实体使用鉴别器列保存到单个表中，以在数据库中针对给定记录标识实体类型。</span><span class="sxs-lookup"><span data-stu-id="fb849-137">Allows entities in an inheritance hierarchy to be saved to a single table using a discriminator column to identify they entity type for a given record in the database.</span></span>
### <a name="model-validation"></a><span data-ttu-id="fb849-138">模型验证</span><span class="sxs-lookup"><span data-stu-id="fb849-138">Model validation</span></span>
<span data-ttu-id="fb849-139">检测模型中的无效模式并提供有用的错误消息。</span><span class="sxs-lookup"><span data-stu-id="fb849-139">Detects invalid patterns in the model and provides helpful error messages.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="fb849-140">Change tracking</span><span class="sxs-lookup"><span data-stu-id="fb849-140">Change tracking</span></span>
### <a name="snapshot-change-tracking"></a><span data-ttu-id="fb849-141">快照更改跟踪</span><span class="sxs-lookup"><span data-stu-id="fb849-141">Snapshot change tracking</span></span>
<span data-ttu-id="fb849-142">通过将当前状态与原始状态的副本（快照）进行比较，可以自动检测实体中的更改。</span><span class="sxs-lookup"><span data-stu-id="fb849-142">Allows changes in entities to be detected automatically by comparing current state against a copy (snapshot) of the original state.</span></span>
### <a name="notification-change-tracking"></a><span data-ttu-id="fb849-143">通知更改追踪</span><span class="sxs-lookup"><span data-stu-id="fb849-143">Notification change tracking</span></span>
<span data-ttu-id="fb849-144">修改属性值后，允许实体通知更改跟踪器。</span><span class="sxs-lookup"><span data-stu-id="fb849-144">Allows your entities to notify the change tracker when property values are modified.</span></span>
### <a name="accessing-tracked-state"></a><span data-ttu-id="fb849-145">访问跟踪的状态</span><span class="sxs-lookup"><span data-stu-id="fb849-145">Accessing tracked state</span></span>
<span data-ttu-id="fb849-146">通过 `DbContext.Entry` 和 `DbContext.ChangeTracker`。</span><span class="sxs-lookup"><span data-stu-id="fb849-146">Via `DbContext.Entry` and `DbContext.ChangeTracker`.</span></span>
### <a name="attaching-detached-entitiesgraphs"></a><span data-ttu-id="fb849-147">附加分离的实体/图</span><span class="sxs-lookup"><span data-stu-id="fb849-147">Attaching detached entities/graphs</span></span>
<span data-ttu-id="fb849-148">新的 `DbContext.AttachGraph` API 有助于将实体重新附加到上下文，以保存新的/修改的实体。</span><span class="sxs-lookup"><span data-stu-id="fb849-148">The new `DbContext.AttachGraph` API helps re-attach entities to a context in order to save new/modified entities.</span></span>

## <a name="saving-data"></a><span data-ttu-id="fb849-149">保存数据</span><span class="sxs-lookup"><span data-stu-id="fb849-149">Saving data</span></span>
### <a name="basic-save-functionality"></a><span data-ttu-id="fb849-150">基本保存功能</span><span class="sxs-lookup"><span data-stu-id="fb849-150">Basic save functionality</span></span>
<span data-ttu-id="fb849-151">允许将对实体实例的更改持久保存到数据库。</span><span class="sxs-lookup"><span data-stu-id="fb849-151">Allows changes to entity instances to be persisted to the database.</span></span>
### <a name="optimistic-concurrency"></a><span data-ttu-id="fb849-152">开放式并发</span><span class="sxs-lookup"><span data-stu-id="fb849-152">Optimistic Concurrency</span></span>
<span data-ttu-id="fb849-153">防止由于从数据库中提取数据而覆盖其他用户所做的更改。</span><span class="sxs-lookup"><span data-stu-id="fb849-153">Protects against overwriting changes made by another user since data was fetched from the database.</span></span>
### <a name="async-savechanges"></a><span data-ttu-id="fb849-154">异步 SaveChanges</span><span class="sxs-lookup"><span data-stu-id="fb849-154">Async SaveChanges</span></span>
<span data-ttu-id="fb849-155">在数据库处理从 `SaveChanges` 发出的命令时，可以释放当前线程以处理其他请求。</span><span class="sxs-lookup"><span data-stu-id="fb849-155">Can free up the current thread to process other requests while the database processes the commands issued from `SaveChanges`.</span></span>
### <a name="database-transactions"></a><span data-ttu-id="fb849-156">数据库事务</span><span class="sxs-lookup"><span data-stu-id="fb849-156">Database Transactions</span></span>
<span data-ttu-id="fb849-157">表示 `SaveChanges` 始终是原子（意味着它或者完全成功，或者不对数据库进行更改）。</span><span class="sxs-lookup"><span data-stu-id="fb849-157">Means that `SaveChanges` is always atomic (meaning it either completely succeeds, or no changes are made to the database).</span></span> <span data-ttu-id="fb849-158">还存在事务相关的 API，允许在上下文实例之间共享事务等。</span><span class="sxs-lookup"><span data-stu-id="fb849-158">There are also transaction related APIs to allow sharing transactions between context instances etc.</span></span>
### <a name="relational-batching-of-statements"></a><span data-ttu-id="fb849-159">关系：批处理语句</span><span class="sxs-lookup"><span data-stu-id="fb849-159">Relational: Batching of statements</span></span>
<span data-ttu-id="fb849-160">通过将多个 INSERT/UPDATE/DELETE 命令批量放到数据库的单个往返路线中来提供更好的性能。</span><span class="sxs-lookup"><span data-stu-id="fb849-160">Provides better performance by batching up multiple INSERT/UPDATE/DELETE commands into a single roundtrip to the database.</span></span>

## <a name="query"></a><span data-ttu-id="fb849-161">查询</span><span class="sxs-lookup"><span data-stu-id="fb849-161">Query</span></span>
### <a name="basic-linq-support"></a><span data-ttu-id="fb849-162">基本 LINQ 支持</span><span class="sxs-lookup"><span data-stu-id="fb849-162">Basic LINQ support</span></span>
<span data-ttu-id="fb849-163">提供使用 LINQ 从数据库检索数据的功能。</span><span class="sxs-lookup"><span data-stu-id="fb849-163">Provides the ability to use LINQ to retrieve data from the database.</span></span>
### <a name="mixed-clientserver-evaluation"></a><span data-ttu-id="fb849-164">混合客户端/服务器评估</span><span class="sxs-lookup"><span data-stu-id="fb849-164">Mixed client/server evaluation</span></span>
<span data-ttu-id="fb849-165">使查询能够包含无法在数据库中评估的逻辑，因此，必须在将数据检索到内存后进行评估。</span><span class="sxs-lookup"><span data-stu-id="fb849-165">Enables queries to contain logic that cannot be evaluated in the database, and must therefore be evaluated after the data is retrieved into memory.</span></span>
### <a name="notracking"></a><span data-ttu-id="fb849-166">NoTracking</span><span class="sxs-lookup"><span data-stu-id="fb849-166">NoTracking</span></span>
<span data-ttu-id="fb849-167">当上下文不需要监视实体实例的变化（即结果是只读的）时，可加快查询执行速度。</span><span class="sxs-lookup"><span data-stu-id="fb849-167">Queries enables quicker query execution when the context does not need to monitor for changes to the entity instances (i.e. the results are read-only).</span></span>
### <a name="eager-loading"></a><span data-ttu-id="fb849-168">预先加载</span><span class="sxs-lookup"><span data-stu-id="fb849-168">Eager loading</span></span>
<span data-ttu-id="fb849-169">提供 `Include` 和 `ThenInclude` 方法来标识在查询时也应提取的相关数据。</span><span class="sxs-lookup"><span data-stu-id="fb849-169">Provides the `Include` and `ThenInclude` methods to identify related data that should also be fetched when querying.</span></span>
### <a name="async-query"></a><span data-ttu-id="fb849-170">异步查询</span><span class="sxs-lookup"><span data-stu-id="fb849-170">Async query</span></span>
<span data-ttu-id="fb849-171">当数据库处理查询时，可以释放当前线程（及其相关资源）以处理其他请求。</span><span class="sxs-lookup"><span data-stu-id="fb849-171">Can free up the current thread (and it's associated resources) to process other requests while the database processes the query.</span></span>
### <a name="raw-sql-queries"></a><span data-ttu-id="fb849-172">原始 SQL 查询</span><span class="sxs-lookup"><span data-stu-id="fb849-172">Raw SQL queries</span></span>
<span data-ttu-id="fb849-173">提供 `DbSet.FromSql` 方法以使用原始 SQL 查询提取数据。</span><span class="sxs-lookup"><span data-stu-id="fb849-173">Provides the `DbSet.FromSql` method to use raw SQL queries to fetch data.</span></span> <span data-ttu-id="fb849-174">也可以使用 LINQ 编写这些查询。</span><span class="sxs-lookup"><span data-stu-id="fb849-174">These queries can also be composed on using LINQ.</span></span>

## <a name="database-schema-management"></a><span data-ttu-id="fb849-175">数据库架构管理</span><span class="sxs-lookup"><span data-stu-id="fb849-175">Database schema management</span></span>       
### <a name="database-creationdeletion-apis"></a><span data-ttu-id="fb849-176">数据库创建/删除 API</span><span class="sxs-lookup"><span data-stu-id="fb849-176">Database creation/deletion APIs</span></span>
<span data-ttu-id="fb849-177">多数旨在测试你希望在不使用迁移的情况下快速创建/删除数据库的位置。</span><span class="sxs-lookup"><span data-stu-id="fb849-177">Are mostly designed for testing where you want to quickly create/delete the database without using migrations.</span></span>
### <a name="relational-database-migrations"></a><span data-ttu-id="fb849-178">关系数据库迁移</span><span class="sxs-lookup"><span data-stu-id="fb849-178">Relational database migrations</span></span>
<span data-ttu-id="fb849-179">允许关系数据库架构随模型更改而演进。</span><span class="sxs-lookup"><span data-stu-id="fb849-179">Allow a relational database schema to evolve overtime as your model changes.</span></span>
### <a name="reverse-engineer-from-database"></a><span data-ttu-id="fb849-180">从数据库反向工程</span><span class="sxs-lookup"><span data-stu-id="fb849-180">Reverse engineer from database</span></span>
<span data-ttu-id="fb849-181">基于现有关系数据库架构搭建 EF 模型基架。</span><span class="sxs-lookup"><span data-stu-id="fb849-181">Scaffolds an EF model based on an existing relational database schema.</span></span>

## <a name="database-providers"></a><span data-ttu-id="fb849-182">数据库提供程序</span><span class="sxs-lookup"><span data-stu-id="fb849-182">Database providers</span></span>
### <a name="sql-server"></a><span data-ttu-id="fb849-183">SQL Server</span><span class="sxs-lookup"><span data-stu-id="fb849-183">SQL Server</span></span>
<span data-ttu-id="fb849-184">连接到 Microsoft SQL Server 2008 及以上版本。</span><span class="sxs-lookup"><span data-stu-id="fb849-184">Connects to Microsoft SQL Server 2008 onwards.</span></span>
### <a name="sqlite"></a><span data-ttu-id="fb849-185">SQLite</span><span class="sxs-lookup"><span data-stu-id="fb849-185">SQLite</span></span>
<span data-ttu-id="fb849-186">连接到 SQLite 3 数据库。</span><span class="sxs-lookup"><span data-stu-id="fb849-186">Connects to a SQLite 3 database.</span></span>
### <a name="in-memory"></a><span data-ttu-id="fb849-187">内存中</span><span class="sxs-lookup"><span data-stu-id="fb849-187">In-Memory</span></span>
<span data-ttu-id="fb849-188">旨在实现无需连接到真实的数据库即可轻松启用测试。</span><span class="sxs-lookup"><span data-stu-id="fb849-188">Is designed to easily enable testing without connecting to a real database.</span></span>
### <a name="3rd-party-providers"></a><span data-ttu-id="fb849-189">第三方提供程序</span><span class="sxs-lookup"><span data-stu-id="fb849-189">3rd party providers</span></span>
<span data-ttu-id="fb849-190">多个提供程序可用于其他数据库引擎。</span><span class="sxs-lookup"><span data-stu-id="fb849-190">Several providers are available for other database engines.</span></span> <span data-ttu-id="fb849-191">有关完整的列表，请参阅[数据库提供程序](../providers/index.md)。</span><span class="sxs-lookup"><span data-stu-id="fb849-191">See [Database Providers](../providers/index.md) for a complete list.</span></span>
