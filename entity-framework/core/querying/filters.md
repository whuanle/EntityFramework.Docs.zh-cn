---
title: "全局查询筛选器-EF 核心"
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 4e3c3c99d155f69e00fed99c415f519808ea1a68
ms.sourcegitcommit: 6e379265e4f087fb7cf180c824722c81750554dc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
---
# <a name="global-query-filters"></a><span data-ttu-id="95761-102">全局查询筛选器</span><span class="sxs-lookup"><span data-stu-id="95761-102">Global Query Filters</span></span>

<span data-ttu-id="95761-103">全局查询筛选器是 LINQ 查询谓词 (布尔表达式通常传递到 LINQ*其中*查询运算符) 在元数据模型中应用于实体类型 (通常在*OnModelCreating*)。</span><span class="sxs-lookup"><span data-stu-id="95761-103">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="95761-104">此类筛选器将自动应用于涉及这些实体类型，包括实体类型引用间接，例如通过使用包含或直接导航属性引用任何 LINQ 查询。</span><span class="sxs-lookup"><span data-stu-id="95761-104">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="95761-105">此功能的一些常见的应用程序是：</span><span class="sxs-lookup"><span data-stu-id="95761-105">Some common applications of this feature are:</span></span>

* <span data-ttu-id="95761-106">**软删除**的实体类型定义*IsDeleted*属性。</span><span class="sxs-lookup"><span data-stu-id="95761-106">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="95761-107">**多租户**的实体类型定义*TenantId*属性。</span><span class="sxs-lookup"><span data-stu-id="95761-107">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="95761-108">示例</span><span class="sxs-lookup"><span data-stu-id="95761-108">Example</span></span>

<span data-ttu-id="95761-109">下面的示例演示如何使用全局查询筛选器来实现简单的博客模型中的软删除和多租户功能的查询行为。</span><span class="sxs-lookup"><span data-stu-id="95761-109">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="95761-110">你可以查看这篇文章[示例](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters)GitHub 上。</span><span class="sxs-lookup"><span data-stu-id="95761-110">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="95761-111">首先，定义实体：</span><span class="sxs-lookup"><span data-stu-id="95761-111">First, define the entities:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="95761-112">请注意为 _ 声明_tenantId_字段上_博客_实体。</span><span class="sxs-lookup"><span data-stu-id="95761-112">Note the declaration of a __tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="95761-113">这将用于将每个博客实例与特定租户相关联。</span><span class="sxs-lookup"><span data-stu-id="95761-113">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="95761-114">此外定义是_IsDeleted_属性_Post_实体类型。</span><span class="sxs-lookup"><span data-stu-id="95761-114">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="95761-115">这使用此选项可跟踪是否_Post_实例已被"软删除"。</span><span class="sxs-lookup"><span data-stu-id="95761-115">This is used this to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="95761-116">即实例标记为已删除而不进行以物理方式删除基础数据。</span><span class="sxs-lookup"><span data-stu-id="95761-116">I.e. The instance is marked as deleted withouth physically removing the underlying data.</span></span>

<span data-ttu-id="95761-117">接下来，配置中的查询筛选器_OnModelCreating_使用```HasQueryFilter```API。</span><span class="sxs-lookup"><span data-stu-id="95761-117">Next, configure the query filters in _OnModelCreating_ using the ```HasQueryFilter``` API.</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="95761-118">谓词表达式传递给_HasQueryFilter_调用将立即自动应用于这些类型的任何 LINQ 查询。</span><span class="sxs-lookup"><span data-stu-id="95761-118">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="95761-119">请注意，使用 DbContext 实例级别字段：```_tenantId```用于设置当前的租户。</span><span class="sxs-lookup"><span data-stu-id="95761-119">Note the use of a DbContext instance level field: ```_tenantId``` used to set the current tenant.</span></span> <span data-ttu-id="95761-120">模型级别筛选器将使用从正确的上下文实例的值。</span><span class="sxs-lookup"><span data-stu-id="95761-120">Model-level filters will use the value from the correct context instance.</span></span> <span data-ttu-id="95761-121">即执行查询实例。</span><span class="sxs-lookup"><span data-stu-id="95761-121">I.e. The instance that is executing the query.</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="95761-122">禁用筛选器</span><span class="sxs-lookup"><span data-stu-id="95761-122">Disabling Filters</span></span>

<span data-ttu-id="95761-123">可能使用的单个 LINQ 查询禁用筛选器```IgnoreQueryFilters()```运算符。</span><span class="sxs-lookup"><span data-stu-id="95761-123">Filters may be disabled for individual LINQ queries by using the ```IgnoreQueryFilters()``` operator.</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="95761-124">限制</span><span class="sxs-lookup"><span data-stu-id="95761-124">Limitations</span></span>

<span data-ttu-id="95761-125">全局查询筛选器具有以下限制：</span><span class="sxs-lookup"><span data-stu-id="95761-125">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="95761-126">筛选器不能包含对导航属性的引用。</span><span class="sxs-lookup"><span data-stu-id="95761-126">Filters cannot contain references to navigation properties.</span></span>
* <span data-ttu-id="95761-127">根实体类型的继承层次结构可以仅定义筛选器。</span><span class="sxs-lookup"><span data-stu-id="95761-127">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
