---
title: "如何查询工作的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
ms.technology: entity-framework-core
uid: core/querying/overview
ms.openlocfilehash: 7fd2940d559f82016d7a8fc3fdcf3af0d5b8bc8f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="how-queries-work"></a><span data-ttu-id="70b4c-102">查询的工作原理</span><span class="sxs-lookup"><span data-stu-id="70b4c-102">How Queries Work</span></span>

<span data-ttu-id="70b4c-103">实体框架核心使用语言集成查询 (LINQ) 查询数据从数据库。</span><span class="sxs-lookup"><span data-stu-id="70b4c-103">Entity Framework Core uses Language Integrate Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="70b4c-104">LINQ，可使用 C# （或所用.NET 语言的） 来编写基于派生的上下文和实体类的强类型的查询。</span><span class="sxs-lookup"><span data-stu-id="70b4c-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span>

## <a name="the-life-of-a-query"></a><span data-ttu-id="70b4c-105">查询的生命周期</span><span class="sxs-lookup"><span data-stu-id="70b4c-105">The life of a query</span></span>

<span data-ttu-id="70b4c-106">以下是过程的每个查询所经历的高级别概述。</span><span class="sxs-lookup"><span data-stu-id="70b4c-106">The following is a high level overview of the process each query goes through.</span></span>

1. <span data-ttu-id="70b4c-107">LINQ 查询处理由实体框架核心以生成已准备好处理数据库提供程序的表示形式</span><span class="sxs-lookup"><span data-stu-id="70b4c-107">The LINQ query is processed by Entity Framework Core to build a representation that is ready to be processed by the database provider</span></span>
   1. <span data-ttu-id="70b4c-108">以便此处理不需要每次执行查询时进行缓存的结果</span><span class="sxs-lookup"><span data-stu-id="70b4c-108">The result is cached so that this processing does not need to be done every time the query is executed</span></span>
2. <span data-ttu-id="70b4c-109">结果传递给数据库提供程序</span><span class="sxs-lookup"><span data-stu-id="70b4c-109">The result is passed to the database provider</span></span>
   1. <span data-ttu-id="70b4c-110">数据库提供程序标识查询的哪些部分可以计算数据库中</span><span class="sxs-lookup"><span data-stu-id="70b4c-110">The database provider identifies which parts of the query can be evaluated in the database</span></span>
   2. <span data-ttu-id="70b4c-111">这些部分的查询将转换为数据库特定查询语言 (例如关系数据库的 SQL)</span><span class="sxs-lookup"><span data-stu-id="70b4c-111">These parts of the query are translated to database specific query language (e.g. SQL for a relational database)</span></span>
   3. <span data-ttu-id="70b4c-112">一个或多个查询发送到数据库和返回的结果集 （结果是从数据库中不实体实例的值）</span><span class="sxs-lookup"><span data-stu-id="70b4c-112">One or more queries are sent to the database and the result set returned (results are values from the database, not entity instances)</span></span>
3. <span data-ttu-id="70b4c-113">在结果集中的每个项</span><span class="sxs-lookup"><span data-stu-id="70b4c-113">For each item in the result set</span></span>
   1. <span data-ttu-id="70b4c-114">如果这是跟踪查询，EF 检查数据是否表示已在上下文实例的更改跟踪实体</span><span class="sxs-lookup"><span data-stu-id="70b4c-114">If this is a tracking query, EF checks if the data represents an entity already in the change tracker for the context instance</span></span>
      * <span data-ttu-id="70b4c-115">如果是这样，现有实体，并返回</span><span class="sxs-lookup"><span data-stu-id="70b4c-115">If so, the existing entity is returned</span></span>
      * <span data-ttu-id="70b4c-116">否则，创建新实体、 更改跟踪，则安装程序，并返回新的实体</span><span class="sxs-lookup"><span data-stu-id="70b4c-116">If not, a new entity is created, change tracking is setup, and the new entity is returned</span></span>
   2. <span data-ttu-id="70b4c-117">如果这是无跟踪查询，EF 会检查数据是否表示已中的此查询的结果集的实体</span><span class="sxs-lookup"><span data-stu-id="70b4c-117">If this is a no-tracking query, EF checks if the data represents an entity already in the result set for this query</span></span>
      * <span data-ttu-id="70b4c-118">如果现有实体，因此，并返回<sup>(1)</sup></span><span class="sxs-lookup"><span data-stu-id="70b4c-118">If so, the existing entity is returned <sup>(1)</sup></span></span>
      * <span data-ttu-id="70b4c-119">否则，创建并返回一个新实体</span><span class="sxs-lookup"><span data-stu-id="70b4c-119">If not, a new entity is created and returned</span></span>

<span data-ttu-id="70b4c-120"><sup>(1)</sup>没有跟踪的查询使用弱引用可跟踪已返回的实体。</span><span class="sxs-lookup"><span data-stu-id="70b4c-120"><sup>(1)</sup> No tracking queries use weak references to keep track of entities that have already been returned.</span></span> <span data-ttu-id="70b4c-121">如果前一个结果具有相同标识号超出范围，且垃圾回收运行，则可能获得的新实体实例。</span><span class="sxs-lookup"><span data-stu-id="70b4c-121">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span>

## <a name="when-queries-are-executed"></a><span data-ttu-id="70b4c-122">当执行查询</span><span class="sxs-lookup"><span data-stu-id="70b4c-122">When queries are executed</span></span>

<span data-ttu-id="70b4c-123">在调用 LINQ 运算符时，您正在只需生成内存中表示的查询。</span><span class="sxs-lookup"><span data-stu-id="70b4c-123">When you call LINQ operators, you are simply building up an in-memory representation of the query.</span></span> <span data-ttu-id="70b4c-124">使用结果时，查询将只发送到数据库。</span><span class="sxs-lookup"><span data-stu-id="70b4c-124">The query is only sent to the database when the results are consumed.</span></span>

<span data-ttu-id="70b4c-125">导致发送给数据库的查询的最常见操作如下：</span><span class="sxs-lookup"><span data-stu-id="70b4c-125">The most common operations that result in the query being sent to the database are:</span></span>
* <span data-ttu-id="70b4c-126">循环中的结果`for`循环</span><span class="sxs-lookup"><span data-stu-id="70b4c-126">Iterating the results in a `for` loop</span></span>
* <span data-ttu-id="70b4c-127">使用运算符，如`ToList`， `ToArray`， `Single`，`Count`</span><span class="sxs-lookup"><span data-stu-id="70b4c-127">Using an operator such as `ToList`, `ToArray`, `Single`, `Count`</span></span>
* <span data-ttu-id="70b4c-128">数据绑定到 UI 查询的结果</span><span class="sxs-lookup"><span data-stu-id="70b4c-128">Databinding the results of a query to a UI</span></span>

> [!WARNING]  
> <span data-ttu-id="70b4c-129">**始终验证用户输入：**时的 EF 提供保护以防止 SQL 注入攻击，它并不输入任何常规验证。</span><span class="sxs-lookup"><span data-stu-id="70b4c-129">**Always validate user input:** While EF does provide protection from SQL injection attacks, it does not do any general validation of input.</span></span> <span data-ttu-id="70b4c-130">因此如果值传递到 Api，在 LINQ 查询中，分配给等实体属性，使用来自不受信任的源则相应的验证，每个应用程序的要求，应执行。</span><span class="sxs-lookup"><span data-stu-id="70b4c-130">Therefore if values being passed to APIs, used in LINQ queries, assigned to entity properties, etc., come from an untrusted source then appropriate validation, per your application requirements, should be performed.</span></span> <span data-ttu-id="70b4c-131">这包括用于动态构造查询的所有用户输入。</span><span class="sxs-lookup"><span data-stu-id="70b4c-131">This includes any user input used to dynamically construct queries.</span></span> <span data-ttu-id="70b4c-132">即使是在使用 LINQ 中，如果您接受用户输入用于生成的表达式需要确保只有预期表达式比可以用来构造。</span><span class="sxs-lookup"><span data-stu-id="70b4c-132">Even when using LINQ, if you are accepting user input to build expressions you need to make sure than only intended expressions can be constructed.</span></span>
