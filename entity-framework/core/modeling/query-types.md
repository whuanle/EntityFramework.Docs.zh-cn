---
title: 查询类型的 EF Core
author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
uid: core/modeling/query-types
ms.openlocfilehash: bacb121ca00a9b0aa00bfe201de4f95113472d70
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996694"
---
# <a name="query-types"></a><span data-ttu-id="c8241-102">查询类型</span><span class="sxs-lookup"><span data-stu-id="c8241-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="c8241-103">此功能是 EF Core 2.1 中的新增功能</span><span class="sxs-lookup"><span data-stu-id="c8241-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="c8241-104">EF Core 模型可以包含实体类型，除了_查询类型_，这可以用于执行针对未映射到实体类型的数据的数据库查询。</span><span class="sxs-lookup"><span data-stu-id="c8241-104">In addition to entity types, an EF Core model can contain _query types_, which can be used to carry out database queries against data that isn't mapped to entity types.</span></span>

<span data-ttu-id="c8241-105">查询类型与实体类型具有许多相似之处：</span><span class="sxs-lookup"><span data-stu-id="c8241-105">Query types have many similarities with entity types:</span></span>

- <span data-ttu-id="c8241-106">它们还可以添加到模型或者在`OnModelCreating`，或通过"设置"属性上派生_DbContext_。</span><span class="sxs-lookup"><span data-stu-id="c8241-106">They can also be added to the model either in `OnModelCreating`, or via a "set" property on a derived _DbContext_.</span></span>
- <span data-ttu-id="c8241-107">它们支持许多相同的映射功能，如继承映射，导航属性 （请参阅下面的限制），然后在关系存储区，配置目标数据库对象和列通过 fluent API 方法或数据注释的功能。</span><span class="sxs-lookup"><span data-stu-id="c8241-107">They support many of the same mapping capabilities, like inheritance mapping, navigation properties (see limitations below) and, on relational stores, the ability to configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="c8241-108">但是不同于实体中的类型，它们：</span><span class="sxs-lookup"><span data-stu-id="c8241-108">However they are different from entity types in that they:</span></span>

- <span data-ttu-id="c8241-109">不需要定义的键。</span><span class="sxs-lookup"><span data-stu-id="c8241-109">Do not require a key to be defined.</span></span>
- <span data-ttu-id="c8241-110">永远不会跟踪的更改上_DbContext_并因此永远不会插入、 更新或删除数据库。</span><span class="sxs-lookup"><span data-stu-id="c8241-110">Are never tracked for changes on the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="c8241-111">永远不会由约定发现。</span><span class="sxs-lookup"><span data-stu-id="c8241-111">Are never discovered by convention.</span></span>
- <span data-ttu-id="c8241-112">仅支持一部分导航映射功能-具体而言：</span><span class="sxs-lookup"><span data-stu-id="c8241-112">Only support a subset of navigation mapping capabilities - Specifically:</span></span>
  - <span data-ttu-id="c8241-113">它们可能永远不会作为关系的主体端。</span><span class="sxs-lookup"><span data-stu-id="c8241-113">They may never act as the principal end of a relationship.</span></span>
  - <span data-ttu-id="c8241-114">它们仅可包含指向的实体的引用导航属性。</span><span class="sxs-lookup"><span data-stu-id="c8241-114">They can only contain reference navigation properties pointing to entities.</span></span>
  - <span data-ttu-id="c8241-115">实体不能包含查询类型的导航属性。</span><span class="sxs-lookup"><span data-stu-id="c8241-115">Entities cannot contain navigation properties to query types.</span></span>
- <span data-ttu-id="c8241-116">得到解决_ModelBuilder_使用`Query`方法而不是`Entity`方法。</span><span class="sxs-lookup"><span data-stu-id="c8241-116">Are addressed on the _ModelBuilder_ using the `Query` method rather than the `Entity` method.</span></span>
- <span data-ttu-id="c8241-117">在映射_DbContext_类型的属性通过`DbQuery<T>`而非 `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="c8241-117">Are mapped on the _DbContext_ through properties of type `DbQuery<T>` rather than `DbSet<T>`</span></span>
- <span data-ttu-id="c8241-118">映射到使用的数据库对象`ToView`方法，而非`ToTable`。</span><span class="sxs-lookup"><span data-stu-id="c8241-118">Are mapped to database objects using the `ToView` method, rather than `ToTable`.</span></span>
- <span data-ttu-id="c8241-119">可以映射到_定义查询_-定义查询是可查询类型的数据源在模型中声明的第二次查询。</span><span class="sxs-lookup"><span data-stu-id="c8241-119">May be mapped to a _defining query_ - A defining query is a secondary query declared in the model that acts a data source for a query type.</span></span>

<span data-ttu-id="c8241-120">下面是一些查询类型主要使用方案：</span><span class="sxs-lookup"><span data-stu-id="c8241-120">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="c8241-121">作为返回类型的即席`FromSql()`查询。</span><span class="sxs-lookup"><span data-stu-id="c8241-121">Serving as the return type for ad hoc `FromSql()` queries.</span></span>
- <span data-ttu-id="c8241-122">映射到数据库视图。</span><span class="sxs-lookup"><span data-stu-id="c8241-122">Mapping to database views.</span></span>
- <span data-ttu-id="c8241-123">映射到不具有定义的主键的表。</span><span class="sxs-lookup"><span data-stu-id="c8241-123">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="c8241-124">映射到模型中定义的查询。</span><span class="sxs-lookup"><span data-stu-id="c8241-124">Mapping to queries defined in the model.</span></span>

> [!TIP]
> <span data-ttu-id="c8241-125">查询类型映射到的数据库对象通过实现`ToView`fluent API。</span><span class="sxs-lookup"><span data-stu-id="c8241-125">Mapping a query type to a database object is achieved using the `ToView` fluent API.</span></span> <span data-ttu-id="c8241-126">此方法中指定的数据库对象是从 EF Core 的角度来看，_视图_，这意味着它将被视为只读查询源和不能作为目标的更新、 插入或删除操作。</span><span class="sxs-lookup"><span data-stu-id="c8241-126">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="c8241-127">但是，这并不意味着该数据库对象是实际需要的数据库视图-它或者可以是以只读方式将被视为一个数据库表。</span><span class="sxs-lookup"><span data-stu-id="c8241-127">However, this does not mean that the database object is actually required to be a database view - It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="c8241-128">相反，实体类型，EF Core 假定数据库对象中指定`ToTable`方法可以视为_表_、 意味着它可以用作查询源，但是还针对由更新、 删除和插入操作。</span><span class="sxs-lookup"><span data-stu-id="c8241-128">Conversely, for entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="c8241-129">事实上，可以指定数据库视图中的名称`ToTable`和所有内容应该能够正常工作，只要视图被配置为可在数据库上更新。</span><span class="sxs-lookup"><span data-stu-id="c8241-129">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

## <a name="example"></a><span data-ttu-id="c8241-130">示例</span><span class="sxs-lookup"><span data-stu-id="c8241-130">Example</span></span>

<span data-ttu-id="c8241-131">下面的示例演示如何使用查询类型来查询数据库视图。</span><span class="sxs-lookup"><span data-stu-id="c8241-131">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="c8241-132">可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/QueryTypes)。</span><span class="sxs-lookup"><span data-stu-id="c8241-132">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="c8241-133">首先，我们定义一个简单的博客和文章模型：</span><span class="sxs-lookup"><span data-stu-id="c8241-133">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="c8241-134">接下来，我们定义一个简单的数据库视图，这样就可以查询与每个博客帖子数：</span><span class="sxs-lookup"><span data-stu-id="c8241-134">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#View)]

<span data-ttu-id="c8241-135">接下来，我们定义一个类来保存数据库视图的结果：</span><span class="sxs-lookup"><span data-stu-id="c8241-135">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#QueryType)]

<span data-ttu-id="c8241-136">接下来，我们配置中的查询类型_OnModelCreating_使用`modelBuilder.Query<T>`API。</span><span class="sxs-lookup"><span data-stu-id="c8241-136">Next, we configure the query type in _OnModelCreating_ using the `modelBuilder.Query<T>` API.</span></span>
<span data-ttu-id="c8241-137">我们使用标准的 fluent 配置 Api 来配置查询类型的映射：</span><span class="sxs-lookup"><span data-stu-id="c8241-137">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="c8241-138">最后，我们可以采用标准方式来查询数据库视图：</span><span class="sxs-lookup"><span data-stu-id="c8241-138">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="c8241-139">请注意我们还定义了上下文级别查询属性 (DbQuery) 作为针对此类型的查询的根。</span><span class="sxs-lookup"><span data-stu-id="c8241-139">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
