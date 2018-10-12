---
title: 查询的工作原理 - EF Core
author: rowanmiller
ms.date: 09/26/2018
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/overview
ms.openlocfilehash: 23d26f9c0ac17fc0df744f5339946947ea366911
ms.sourcegitcommit: 15022dd06d919c29b1189c82611ea32f9fdc6617
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2018
ms.locfileid: "47415726"
---
# <a name="how-queries-work"></a><span data-ttu-id="1c99d-102">查询的工作原理</span><span class="sxs-lookup"><span data-stu-id="1c99d-102">How Queries Work</span></span>

<span data-ttu-id="1c99d-103">Entity Framework Core 使用语言集成查询 (LINQ) 来查询数据库中的数据。</span><span class="sxs-lookup"><span data-stu-id="1c99d-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="1c99d-104">通过 LINQ 可使用 C#（或你选择的 .NET 语言）基于派生上下文和实体类编写强类型查询。</span><span class="sxs-lookup"><span data-stu-id="1c99d-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span>

## <a name="the-life-of-a-query"></a><span data-ttu-id="1c99d-105">查询的寿命</span><span class="sxs-lookup"><span data-stu-id="1c99d-105">The life of a query</span></span>

<span data-ttu-id="1c99d-106">下面是每个查询所经历的过程的高级概述。</span><span class="sxs-lookup"><span data-stu-id="1c99d-106">The following is a high level overview of the process each query goes through.</span></span>

1. <span data-ttu-id="1c99d-107">LINQ 查询由 Entity Framework Core 处理，用于生成已准备好由数据库提供程序处理的表示形式</span><span class="sxs-lookup"><span data-stu-id="1c99d-107">The LINQ query is processed by Entity Framework Core to build a representation that is ready to be processed by the database provider</span></span>
   1. <span data-ttu-id="1c99d-108">结果会被缓存，以便每次执行查询时无需进行此处理</span><span class="sxs-lookup"><span data-stu-id="1c99d-108">The result is cached so that this processing does not need to be done every time the query is executed</span></span>
2. <span data-ttu-id="1c99d-109">结果会传递到数据库提供程序</span><span class="sxs-lookup"><span data-stu-id="1c99d-109">The result is passed to the database provider</span></span>
   1. <span data-ttu-id="1c99d-110">数据库提供程序确定可以在数据库中评估查询的哪些部分</span><span class="sxs-lookup"><span data-stu-id="1c99d-110">The database provider identifies which parts of the query can be evaluated in the database</span></span>
   2. <span data-ttu-id="1c99d-111">查询的这些部分会转换为数据库特定的查询语言（例如，关系数据库的 SQL）</span><span class="sxs-lookup"><span data-stu-id="1c99d-111">These parts of the query are translated to database specific query language (for example, SQL for a relational database)</span></span>
   3. <span data-ttu-id="1c99d-112">一个或多个查询会发送到数据库和返回的结果集（结果是数据库中的值，而不是实体实例）</span><span class="sxs-lookup"><span data-stu-id="1c99d-112">One or more queries are sent to the database and the result set returned (results are values from the database, not entity instances)</span></span>
3. <span data-ttu-id="1c99d-113">对于结果集中的每一项</span><span class="sxs-lookup"><span data-stu-id="1c99d-113">For each item in the result set</span></span>
   1. <span data-ttu-id="1c99d-114">如果这是跟踪查询，EF 会检查数据是否表示已在上下文实例的更改跟踪器中的实体</span><span class="sxs-lookup"><span data-stu-id="1c99d-114">If this is a tracking query, EF checks if the data represents an entity already in the change tracker for the context instance</span></span>
      * <span data-ttu-id="1c99d-115">如果是，则会返回现有实体</span><span class="sxs-lookup"><span data-stu-id="1c99d-115">If so, the existing entity is returned</span></span>
      * <span data-ttu-id="1c99d-116">如果不是，则会创建新实体、设置更改跟踪并返回该新实体</span><span class="sxs-lookup"><span data-stu-id="1c99d-116">If not, a new entity is created, change tracking is setup, and the new entity is returned</span></span>
   2. <span data-ttu-id="1c99d-117">如果这是非跟踪查询，EF 会检查数据是否表示已在此查询的结果集中的实体</span><span class="sxs-lookup"><span data-stu-id="1c99d-117">If this is a no-tracking query, EF checks if the data represents an entity already in the result set for this query</span></span>
      * <span data-ttu-id="1c99d-118">如果是，则会返回现有实体 <sup>(1)</sup></span><span class="sxs-lookup"><span data-stu-id="1c99d-118">If so, the existing entity is returned <sup>(1)</sup></span></span>
      * <span data-ttu-id="1c99d-119">如果不是，则会创建新实体并返回该新实体</span><span class="sxs-lookup"><span data-stu-id="1c99d-119">If not, a new entity is created and returned</span></span>

<span data-ttu-id="1c99d-120"><sup>(1)</sup> 非跟踪查询使用弱引用跟踪已返回的实体。</span><span class="sxs-lookup"><span data-stu-id="1c99d-120"><sup>(1)</sup> No tracking queries use weak references to keep track of entities that have already been returned.</span></span> <span data-ttu-id="1c99d-121">如果具有相同标识的上一个结果超出范围，并运行垃圾回收，则可能会获得新的实体实例。</span><span class="sxs-lookup"><span data-stu-id="1c99d-121">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span>

## <a name="when-queries-are-executed"></a><span data-ttu-id="1c99d-122">执行查询时</span><span class="sxs-lookup"><span data-stu-id="1c99d-122">When queries are executed</span></span>

<span data-ttu-id="1c99d-123">调用 LINQ 运算符时，只会生成查询的内存中表示形式。</span><span class="sxs-lookup"><span data-stu-id="1c99d-123">When you call LINQ operators, you are simply building up an in-memory representation of the query.</span></span> <span data-ttu-id="1c99d-124">使用结果时，查询只会发送到数据库。</span><span class="sxs-lookup"><span data-stu-id="1c99d-124">The query is only sent to the database when the results are consumed.</span></span>

<span data-ttu-id="1c99d-125">导致查询发送到数据库的最常见操作如下：</span><span class="sxs-lookup"><span data-stu-id="1c99d-125">The most common operations that result in the query being sent to the database are:</span></span>
* <span data-ttu-id="1c99d-126">在 `for` 循环中循环访问结果</span><span class="sxs-lookup"><span data-stu-id="1c99d-126">Iterating the results in a `for` loop</span></span>
* <span data-ttu-id="1c99d-127">使用 `ToList`、`ToArray`、`Single`、`Count` 等运算符</span><span class="sxs-lookup"><span data-stu-id="1c99d-127">Using an operator such as `ToList`, `ToArray`, `Single`, `Count`</span></span>
* <span data-ttu-id="1c99d-128">将查询结果数据绑定到 UI</span><span class="sxs-lookup"><span data-stu-id="1c99d-128">Databinding the results of a query to a UI</span></span>

> [!WARNING]  
> <span data-ttu-id="1c99d-129">始终验证用户输入：虽然 EF Core通过在查询中使用参数和转义文字来防止 SQL 注入攻击，但它不会验证输入。</span><span class="sxs-lookup"><span data-stu-id="1c99d-129">**Always validate user input:** While EF Core protects against SQL injection attacks by using parameters and escaping literals in queries, it does not validate inputs.</span></span> <span data-ttu-id="1c99d-130">根据应用程序的要求，在将 LINQ 查询中使用的来自不受信任的源的值分配给实体属性或传递给其他 EF Core API 之前，应执行相应的验证。</span><span class="sxs-lookup"><span data-stu-id="1c99d-130">Appropriate validation, per the application's requirements, should be performed before values from untrusted sources are used in LINQ queries, assigned to entity properties, or passed to other EF Core APIs.</span></span> <span data-ttu-id="1c99d-131">这包括用于动态构造查询的所有用户输入。</span><span class="sxs-lookup"><span data-stu-id="1c99d-131">This includes any user input used to dynamically construct queries.</span></span> <span data-ttu-id="1c99d-132">即使在使用 LINQ 时，如果接受用于生成表达式的用户输入，也会需要确保只能构造预期表达式。</span><span class="sxs-lookup"><span data-stu-id="1c99d-132">Even when using LINQ, if you are accepting user input to build expressions, you need to make sure that only intended expressions can be constructed.</span></span>
