---
title: 全局查询筛选器 - EF Core
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 4e3c3c99d155f69e00fed99c415f519808ea1a68
ms.sourcegitcommit: 6e379265e4f087fb7cf180c824722c81750554dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
ms.locfileid: "26053897"
---
# <a name="global-query-filters"></a><span data-ttu-id="35188-102">全局查询筛选器</span><span class="sxs-lookup"><span data-stu-id="35188-102">Global Query Filters</span></span>

<span data-ttu-id="35188-103">全局查询筛选器是应用于元数据模型（通常为 *OnModelCreating*）中的实体类型的 LINQ 查询谓词（通常传递给 LINQ *Where* 查询运算符的布尔表达式）。</span><span class="sxs-lookup"><span data-stu-id="35188-103">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="35188-104">此类筛选器自动应用于涉及这些实体类型（包括通过使用 Include 或直接导航属性引用等方式间接引用的实体类型）的所有 LINQ 查询。</span><span class="sxs-lookup"><span data-stu-id="35188-104">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="35188-105">此功能的一些常见应用如下：</span><span class="sxs-lookup"><span data-stu-id="35188-105">Some common applications of this feature are:</span></span>

* <span data-ttu-id="35188-106">**软删除** - 实体类型定义“IsDeleted”属性。</span><span class="sxs-lookup"><span data-stu-id="35188-106">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="35188-107">**多租户** - 实体类型定义“TenantId”属性。</span><span class="sxs-lookup"><span data-stu-id="35188-107">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="35188-108">示例</span><span class="sxs-lookup"><span data-stu-id="35188-108">Example</span></span>

<span data-ttu-id="35188-109">下面的示例显示了如何使用全局查询筛选器在简单的博客模型中实现软删除和多租户查询行为。</span><span class="sxs-lookup"><span data-stu-id="35188-109">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="35188-110">可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters)。</span><span class="sxs-lookup"><span data-stu-id="35188-110">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="35188-111">首先，定义实体：</span><span class="sxs-lookup"><span data-stu-id="35188-111">First, define the entities:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="35188-112">请记住_博客_实体上 __tenantId_ 字段的声明。</span><span class="sxs-lookup"><span data-stu-id="35188-112">Note the declaration of a __tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="35188-113">这会用于将每个博客实例与特定租户相关联。</span><span class="sxs-lookup"><span data-stu-id="35188-113">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="35188-114">还会定义_文章_实体类型上的 _IsDeleted_ 属性。</span><span class="sxs-lookup"><span data-stu-id="35188-114">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="35188-115">这会用于跟踪_文章_实例是否已“软删除”。</span><span class="sxs-lookup"><span data-stu-id="35188-115">This is used this to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="35188-116">即实例标记为已删除，而实际上不会删除基础数据。</span><span class="sxs-lookup"><span data-stu-id="35188-116">I.e. The instance is marked as deleted withouth physically removing the underlying data.</span></span>

<span data-ttu-id="35188-117">接下来，使用 ```HasQueryFilter``` API 在 _OnModelCreating_ 中配置查询筛选器。</span><span class="sxs-lookup"><span data-stu-id="35188-117">Next, configure the query filters in _OnModelCreating_ using the ```HasQueryFilter``` API.</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="35188-118">传递给 _HasQueryFilter_ 调用的谓词表达式将立即自动应用于这些类型的任何 LINQ 查询。</span><span class="sxs-lookup"><span data-stu-id="35188-118">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="35188-119">请注意 DbContext 实例级别字段的使用：```_tenantId``` 用于设置当前租户。</span><span class="sxs-lookup"><span data-stu-id="35188-119">Note the use of a DbContext instance level field: ```_tenantId``` used to set the current tenant.</span></span> <span data-ttu-id="35188-120">模型级别筛选器将使用来自正确上下文实例的值。</span><span class="sxs-lookup"><span data-stu-id="35188-120">Model-level filters will use the value from the correct context instance.</span></span> <span data-ttu-id="35188-121">即执行查询的实例。</span><span class="sxs-lookup"><span data-stu-id="35188-121">I.e. The instance that is executing the query.</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="35188-122">禁用筛选器</span><span class="sxs-lookup"><span data-stu-id="35188-122">Disabling Filters</span></span>

<span data-ttu-id="35188-123">可使用 ```IgnoreQueryFilters()``` 运算符对各个 LINQ 查询禁用筛选器。</span><span class="sxs-lookup"><span data-stu-id="35188-123">Filters may be disabled for individual LINQ queries by using the ```IgnoreQueryFilters()``` operator.</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="35188-124">限制</span><span class="sxs-lookup"><span data-stu-id="35188-124">Limitations</span></span>

<span data-ttu-id="35188-125">全局查询筛选器具有以下限制：</span><span class="sxs-lookup"><span data-stu-id="35188-125">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="35188-126">筛选器不能包含对导航属性的引用。</span><span class="sxs-lookup"><span data-stu-id="35188-126">Filters cannot contain references to navigation properties.</span></span>
* <span data-ttu-id="35188-127">仅可为继承层次结构的根实体类型定义筛选器。</span><span class="sxs-lookup"><span data-stu-id="35188-127">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
