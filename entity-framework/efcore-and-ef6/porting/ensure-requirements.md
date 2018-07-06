---
title: 从 EF6 移植到 EF Core-验证要求
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 2f45039e63546d266ec6ce0bfa66ef7e9fb3d7e7
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052857"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a><span data-ttu-id="45627-102">之前从 EF6 到 EF Core 的迁移： 验证应用程序的要求</span><span class="sxs-lookup"><span data-stu-id="45627-102">Before porting from EF6 to EF Core: Validate your Application's Requirements</span></span>

<span data-ttu-id="45627-103">在开始迁移过程之前很重要，以验证 EF Core 满足你的应用程序的数据访问要求。</span><span class="sxs-lookup"><span data-stu-id="45627-103">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="45627-104">缺少的功能</span><span class="sxs-lookup"><span data-stu-id="45627-104">Missing features</span></span>

<span data-ttu-id="45627-105">请确保 EF Core 具有所需应用程序中使用的所有功能。</span><span class="sxs-lookup"><span data-stu-id="45627-105">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="45627-106">请参阅[功能比较](../features.md)有关如何在 EF Core 中设置的功能比较到 ef6 更高版本的详细比较。</span><span class="sxs-lookup"><span data-stu-id="45627-106">See [Feature Comparison](../features.md) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="45627-107">如果缺少任何所需的功能，请确保，你可以补偿缺少这些功能移植到 EF Core 之前。</span><span class="sxs-lookup"><span data-stu-id="45627-107">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="45627-108">行为更改</span><span class="sxs-lookup"><span data-stu-id="45627-108">Behavior changes</span></span>

<span data-ttu-id="45627-109">这是从 EF6 和 EF Core 之间的行为中的某些更改非详尽列表。</span><span class="sxs-lookup"><span data-stu-id="45627-109">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="45627-110">请务必记住这几点你端口作为你的应用程序因为它们可能更改你的应用程序的行为，但将不显示为编译错误后交换到 EF Core 的方式。</span><span class="sxs-lookup"><span data-stu-id="45627-110">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="45627-111">DbSet.Add/Attach 和 graph 行为</span><span class="sxs-lookup"><span data-stu-id="45627-111">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="45627-112">在从 EF6，调用`DbSet.Add()`对实体的所有实体在其导航属性中引用的递归搜索。</span><span class="sxs-lookup"><span data-stu-id="45627-112">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="45627-113">发现了，并且不由上下文中，已跟踪的任何实体也是标记为添加。</span><span class="sxs-lookup"><span data-stu-id="45627-113">Any entities that are found, and are not already tracked by the context, are also be marked as added.</span></span> <span data-ttu-id="45627-114">`DbSet.Attach()`行为相同，但所有实体都标记为不变。</span><span class="sxs-lookup"><span data-stu-id="45627-114">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="45627-115">**EF Core 执行类似的递归搜索，但使用一些略有不同的规则。**</span><span class="sxs-lookup"><span data-stu-id="45627-115">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="45627-116">根实体始终处于请求的状态 (为添加`DbSet.Add`和不变`DbSet.Attach`)。</span><span class="sxs-lookup"><span data-stu-id="45627-116">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="45627-117">**适用于导航属性在递归搜索过程中找到的实体：**</span><span class="sxs-lookup"><span data-stu-id="45627-117">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="45627-118">**如果实体的主键是生成的存储**</span><span class="sxs-lookup"><span data-stu-id="45627-118">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="45627-119">如果主键不设置为一个值，则状态将设置为添加。</span><span class="sxs-lookup"><span data-stu-id="45627-119">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="45627-120">主键值被视为"未设置"被分配的属性类型的 CLR 默认值 (即`0`为`int`，`null`为`string`等。)。</span><span class="sxs-lookup"><span data-stu-id="45627-120">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (i.e. `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="45627-121">如果为主键设置为一个值，状态将是设置为不变。</span><span class="sxs-lookup"><span data-stu-id="45627-121">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="45627-122">如果主键不是数据库生成，则会将实体放在与根相同的状态。</span><span class="sxs-lookup"><span data-stu-id="45627-122">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="45627-123">第一个数据库初始化的代码</span><span class="sxs-lookup"><span data-stu-id="45627-123">Code First database initialization</span></span>

<span data-ttu-id="45627-124">**从 EF6 具有大量的幻它围绕选择数据库连接和初始化数据库执行。这些规则包括：**</span><span class="sxs-lookup"><span data-stu-id="45627-124">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="45627-125">如果不执行任何配置，则从 EF6 将选择 SQL Express 或 LocalDb 上的数据库。</span><span class="sxs-lookup"><span data-stu-id="45627-125">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="45627-126">如果连接字符串具有相同名称作为上下文是在应用程序`App/Web.config`文件，将使用此连接。</span><span class="sxs-lookup"><span data-stu-id="45627-126">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="45627-127">如果数据库不存在，则创建它。</span><span class="sxs-lookup"><span data-stu-id="45627-127">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="45627-128">如果没有任何从模型表的数据库中存在，则会将当前模型的架构添加到数据库。</span><span class="sxs-lookup"><span data-stu-id="45627-128">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="45627-129">如果启用迁移后，它们用于创建数据库。</span><span class="sxs-lookup"><span data-stu-id="45627-129">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="45627-130">如果数据库存在并从 EF6 以前已创建了架构，然后针对与当前模型的兼容性检查架构。</span><span class="sxs-lookup"><span data-stu-id="45627-130">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="45627-131">如果自创建架构时，该模型已更改，将引发异常。</span><span class="sxs-lookup"><span data-stu-id="45627-131">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="45627-132">**EF Core 不执行任何此幻数。**</span><span class="sxs-lookup"><span data-stu-id="45627-132">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="45627-133">必须在代码中显式配置数据库连接。</span><span class="sxs-lookup"><span data-stu-id="45627-133">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="45627-134">不执行初始化。</span><span class="sxs-lookup"><span data-stu-id="45627-134">No initialization is performed.</span></span> <span data-ttu-id="45627-135">必须使用`DbContext.Database.Migrate()`将迁移 (或`DbContext.Database.EnsureCreated()`和`EnsureDeleted()`而无需使用迁移的创建或删除数据库)。</span><span class="sxs-lookup"><span data-stu-id="45627-135">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="45627-136">代码的第一个表命名约定</span><span class="sxs-lookup"><span data-stu-id="45627-136">Code First table naming convention</span></span>

<span data-ttu-id="45627-137">从 EF6 通过计算该实体映射到的默认表名称的复数形式服务运行的实体类名。</span><span class="sxs-lookup"><span data-stu-id="45627-137">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="45627-138">EF Core使用的名称的`DbSet`派生上下文公开了实体的属性。</span><span class="sxs-lookup"><span data-stu-id="45627-138">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="45627-139">如果该实体没有`DbSet`使用属性，则类名称。</span><span class="sxs-lookup"><span data-stu-id="45627-139">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
