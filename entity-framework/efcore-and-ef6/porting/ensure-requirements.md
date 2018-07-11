---
title: 从 EF6 移植到 EF Core-验证要求
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 65bdc8bb9574d37db697aa47c8e8c480cefcb4f7
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949109"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a><span data-ttu-id="7d6fb-102">之前从 EF6 到 EF Core的迁移： 验证应用程序的要求</span><span class="sxs-lookup"><span data-stu-id="7d6fb-102">Before porting from EF6 to EF Core: Validate your Application's Requirements</span></span>

<span data-ttu-id="7d6fb-103">在开始迁移过程之前很重要，以验证 EF Core满足你的应用程序的数据访问要求。</span><span class="sxs-lookup"><span data-stu-id="7d6fb-103">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="7d6fb-104">缺少的功能</span><span class="sxs-lookup"><span data-stu-id="7d6fb-104">Missing features</span></span>

<span data-ttu-id="7d6fb-105">请确保 EF Core具有所需应用程序中使用的所有功能。</span><span class="sxs-lookup"><span data-stu-id="7d6fb-105">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="7d6fb-106">请参阅[功能比较](../features.md)有关如何在 EF Core中设置的功能比较到 ef6 更高版本的详细比较。</span><span class="sxs-lookup"><span data-stu-id="7d6fb-106">See [Feature Comparison](../features.md) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="7d6fb-107">如果缺少任何所需的功能，请确保，你可以补偿缺少这些功能移植到 EF Core之前。</span><span class="sxs-lookup"><span data-stu-id="7d6fb-107">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="7d6fb-108">行为更改</span><span class="sxs-lookup"><span data-stu-id="7d6fb-108">Behavior changes</span></span>

<span data-ttu-id="7d6fb-109">这是从 EF6 和 EF Core之间的行为中的某些更改非详尽列表。</span><span class="sxs-lookup"><span data-stu-id="7d6fb-109">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="7d6fb-110">请务必记住这几点你端口作为你的应用程序因为它们可能更改你的应用程序的行为，但将不显示为编译错误后交换到 EF Core的方式。</span><span class="sxs-lookup"><span data-stu-id="7d6fb-110">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="7d6fb-111">DbSet.Add/Attach 和图形的行为</span><span class="sxs-lookup"><span data-stu-id="7d6fb-111">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="7d6fb-112">在 EF6 中，调用`DbSet.Add()`上实体结果中的所有实体在其导航属性中引用的递归搜索。</span><span class="sxs-lookup"><span data-stu-id="7d6fb-112">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="7d6fb-113">找不到，并且不在上下文中，通过已跟踪的任何实体也将被标记为已添加。</span><span class="sxs-lookup"><span data-stu-id="7d6fb-113">Any entities that are found, and are not already tracked by the context, are also be marked as added.</span></span> <span data-ttu-id="7d6fb-114">`DbSet.Attach()` 行为相同，但所有实体都被都标记为不变。</span><span class="sxs-lookup"><span data-stu-id="7d6fb-114">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="7d6fb-115">**EF Core执行类似的递归搜索，但使用一些略有不同的规则。**</span><span class="sxs-lookup"><span data-stu-id="7d6fb-115">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="7d6fb-116">根实体始终处于请求的状态 (为添加`DbSet.Add`且不会更改为`DbSet.Attach`)。</span><span class="sxs-lookup"><span data-stu-id="7d6fb-116">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="7d6fb-117">**在导航属性的递归搜索期间找到的实体：**</span><span class="sxs-lookup"><span data-stu-id="7d6fb-117">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="7d6fb-118">**如果该实体的主键是生成的存储**</span><span class="sxs-lookup"><span data-stu-id="7d6fb-118">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="7d6fb-119">如果主键不设置为一个值，设置为已添加状态。</span><span class="sxs-lookup"><span data-stu-id="7d6fb-119">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="7d6fb-120">主密钥值被视为"未设置"在赋值的属性类型的 CLR 默认值 (例如，`0`有关`int`，`null`为`string`，等等。)。</span><span class="sxs-lookup"><span data-stu-id="7d6fb-120">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (for example, `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="7d6fb-121">如果为主键设置为一个值，状态将是设置为不变。</span><span class="sxs-lookup"><span data-stu-id="7d6fb-121">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="7d6fb-122">如果主键不是数据库生成，该实体将作为根的相同状态。</span><span class="sxs-lookup"><span data-stu-id="7d6fb-122">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="7d6fb-123">第一个数据库初始化代码</span><span class="sxs-lookup"><span data-stu-id="7d6fb-123">Code First database initialization</span></span>

<span data-ttu-id="7d6fb-124">**EF6 所拥有大量的 magic 将围绕选择数据库连接并初始化该数据库执行。这些规则包括：**</span><span class="sxs-lookup"><span data-stu-id="7d6fb-124">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="7d6fb-125">如果不执行任何配置，则 EF6 将选择在 SQL Express 或 LocalDb 数据库。</span><span class="sxs-lookup"><span data-stu-id="7d6fb-125">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="7d6fb-126">如果连接字符串具有相同名称的上下文是在应用程序中`App/Web.config`文件中，将使用此连接。</span><span class="sxs-lookup"><span data-stu-id="7d6fb-126">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="7d6fb-127">如果数据库不存在，则创建它。</span><span class="sxs-lookup"><span data-stu-id="7d6fb-127">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="7d6fb-128">如果不从模型表的数据库中存在，则将当前模型的架构添加到数据库。</span><span class="sxs-lookup"><span data-stu-id="7d6fb-128">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="7d6fb-129">如果启用了迁移，它们用于创建数据库。</span><span class="sxs-lookup"><span data-stu-id="7d6fb-129">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="7d6fb-130">如果数据库存在且 EF6 先前已创建了架构，然后与当前模型的兼容性检查架构。</span><span class="sxs-lookup"><span data-stu-id="7d6fb-130">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="7d6fb-131">如果自创建架构时，该模型已更改，将引发异常。</span><span class="sxs-lookup"><span data-stu-id="7d6fb-131">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="7d6fb-132">**EF Core不执行任何此幻数。**</span><span class="sxs-lookup"><span data-stu-id="7d6fb-132">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="7d6fb-133">必须在代码中显式配置数据库连接。</span><span class="sxs-lookup"><span data-stu-id="7d6fb-133">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="7d6fb-134">不执行初始化。</span><span class="sxs-lookup"><span data-stu-id="7d6fb-134">No initialization is performed.</span></span> <span data-ttu-id="7d6fb-135">必须使用`DbContext.Database.Migrate()`以应用迁移 (或`DbContext.Database.EnsureCreated()`和`EnsureDeleted()`而无需使用迁移的创建或删除数据库)。</span><span class="sxs-lookup"><span data-stu-id="7d6fb-135">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="7d6fb-136">代码的第一个表命名约定</span><span class="sxs-lookup"><span data-stu-id="7d6fb-136">Code First table naming convention</span></span>

<span data-ttu-id="7d6fb-137">EF6 在复数化服务，以便计算实体映射到的默认表名中运行的实体类名称。</span><span class="sxs-lookup"><span data-stu-id="7d6fb-137">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="7d6fb-138">EF Core使用的名称的`DbSet`派生上下文公开了实体的属性。</span><span class="sxs-lookup"><span data-stu-id="7d6fb-138">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="7d6fb-139">如果实体不具有`DbSet`使用属性，则类名。</span><span class="sxs-lookup"><span data-stu-id="7d6fb-139">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
