---
title: "查询类型的 EF 核心"
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: 19a371c65da33e8209cc1ab3423a67c34ddae61e
ms.sourcegitcommit: fc68321c211aca38f7b9dc3a75677c6ca1b2524b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2018
---
# <a name="query-types"></a><span data-ttu-id="d5676-102">查询类型</span><span class="sxs-lookup"><span data-stu-id="d5676-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="d5676-103">此功能是 EF 核心 2.1 中的新增功能</span><span class="sxs-lookup"><span data-stu-id="d5676-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="d5676-104">查询类型都是可以添加到 EF 核心模型的只读查询结果类型。</span><span class="sxs-lookup"><span data-stu-id="d5676-104">Query Types are read-only query result types that can be added to the EF Core model.</span></span> <span data-ttu-id="d5676-105">查询类型启用的即席查询 （如匿名类型），但不更灵活，因为它们可以具有指定的映射配置。</span><span class="sxs-lookup"><span data-stu-id="d5676-105">Query Types enable ad-hoc querying (like anonymous types), but are more flexible because they can have mapping configuration specified.</span></span>

<span data-ttu-id="d5676-106">从概念上讲类似于实体类型中，它们是：</span><span class="sxs-lookup"><span data-stu-id="d5676-106">They are conceptually similar to Entity Types in that:</span></span>

- <span data-ttu-id="d5676-107">它们是 POCO C# 类型添加到模型，在```OnModelCreating```使用```ModelBuilder.Query```方法，或通过 DbContext"集"属性 (对于查询的类型这样的属性被类型化为```DbQuery<T>```而应假定```DbSet<T>```)。</span><span class="sxs-lookup"><span data-stu-id="d5676-107">They are POCO C# types that are added to the model, either in ```OnModelCreating``` using the ```ModelBuilder.Query``` method, or via a DbContext "set" property (for query types such a property is typed as ```DbQuery<T>``` rather that ```DbSet<T>```).</span></span>
- <span data-ttu-id="d5676-108">它们支持很多与常规实体类型相同的映射功能。</span><span class="sxs-lookup"><span data-stu-id="d5676-108">They support much of the same mapping capabilities as regular entity types.</span></span> <span data-ttu-id="d5676-109">示例、 继承映射、 导航 （请参阅下面的限制），然后在关系存储，能够配置目标数据库架构对象通过```ToTable```， ```HasColumn``` fluent api 方法 （或数据注释）。</span><span class="sxs-lookup"><span data-stu-id="d5676-109">For example, inheritance mapping, navigations (see limitiations below) and, on relational stores, the ability to configure the target database schema objects via ```ToTable```, ```HasColumn``` fluent-api methods (or data annotations).</span></span>

<span data-ttu-id="d5676-110">查询类型都不同于实体类型中，它们：</span><span class="sxs-lookup"><span data-stu-id="d5676-110">Query Types are different from entity types in that they:</span></span>

- <span data-ttu-id="d5676-111">不需要一个密钥，以定义。</span><span class="sxs-lookup"><span data-stu-id="d5676-111">Do not require a key to be defined.</span></span>
- <span data-ttu-id="d5676-112">永远不会跟踪由更改跟踪器。</span><span class="sxs-lookup"><span data-stu-id="d5676-112">Are never tracked by the Change Tracker.</span></span>
- <span data-ttu-id="d5676-113">永远不会发现的约定。</span><span class="sxs-lookup"><span data-stu-id="d5676-113">Are never discovered by convention.</span></span>
- <span data-ttu-id="d5676-114">仅支持一部分导航映射功能-具体而言，它们可能永远不会充当一种关系的主体端。</span><span class="sxs-lookup"><span data-stu-id="d5676-114">Only support a subset of navigation mapping capabilities - Specifically, they may never act as the principal end of a relationship.</span></span>
- <span data-ttu-id="d5676-115">可以将映射到_定义查询_-定义的查询是辅助查询可充当数据源的查询类型。</span><span class="sxs-lookup"><span data-stu-id="d5676-115">May be mapped to a _defining query_ - A Defining Query is a secondary query that acts a data source for a Query Type.</span></span>

<span data-ttu-id="d5676-116">下面是一些的查询类型主要使用方案：</span><span class="sxs-lookup"><span data-stu-id="d5676-116">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="d5676-117">映射到数据库视图。</span><span class="sxs-lookup"><span data-stu-id="d5676-117">Mapping to database views.</span></span>
- <span data-ttu-id="d5676-118">将映射到没有定义的主键的表。</span><span class="sxs-lookup"><span data-stu-id="d5676-118">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="d5676-119">即席充当的返回类型```FromSql()```查询。</span><span class="sxs-lookup"><span data-stu-id="d5676-119">Serving as the return type for ad hoc ```FromSql()``` queries.</span></span>
- <span data-ttu-id="d5676-120">将映射到模型中定义的查询。</span><span class="sxs-lookup"><span data-stu-id="d5676-120">Mapping to queries defined in the model.</span></span>

> [!TIP]
> <span data-ttu-id="d5676-121">将查询类型映射到数据库视图使用实现```ToTable```fluent API。</span><span class="sxs-lookup"><span data-stu-id="d5676-121">Mapping a query type to a database view is achieved using the ```ToTable``` fluent API.</span></span>

## <a name="example"></a><span data-ttu-id="d5676-122">示例</span><span class="sxs-lookup"><span data-stu-id="d5676-122">Example</span></span>

<span data-ttu-id="d5676-123">下面的示例演示如何使用查询类型来查询数据库视图。</span><span class="sxs-lookup"><span data-stu-id="d5676-123">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="d5676-124">可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes)。</span><span class="sxs-lookup"><span data-stu-id="d5676-124">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="d5676-125">首先，我们定义一个简单的博客和 Post 模型：</span><span class="sxs-lookup"><span data-stu-id="d5676-125">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="d5676-126">接下来，我们定义一个简单的数据库视图，使我们能够查询数与每个博客文章：</span><span class="sxs-lookup"><span data-stu-id="d5676-126">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

<span data-ttu-id="d5676-127">接下来，我们定义一个类以保留的数据库视图的结果：</span><span class="sxs-lookup"><span data-stu-id="d5676-127">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#QueryType)]

<span data-ttu-id="d5676-128">接下来，我们配置中的查询类型_OnModelCreating_使用```modelBuilder.Query<T>```API。</span><span class="sxs-lookup"><span data-stu-id="d5676-128">Next, we configure the query type in _OnModelCreating_ using the ```modelBuilder.Query<T>``` API.</span></span>
<span data-ttu-id="d5676-129">我们可以使用标准 fluent 配置 Api 来配置查询类型的映射：</span><span class="sxs-lookup"><span data-stu-id="d5676-129">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="d5676-130">最后，我们可以查询数据库视图中的标准方式：</span><span class="sxs-lookup"><span data-stu-id="d5676-130">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="d5676-131">请注意我们还定义了上下文级别查询属性 (DbQuery) 使其作为针对此类型的查询的根。</span><span class="sxs-lookup"><span data-stu-id="d5676-131">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
