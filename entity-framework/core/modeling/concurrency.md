---
title: "并发标记的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: 6574a9098d38c4aa525ffb4896adb01082420b5f
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2017
---
# <a name="concurrency-tokens"></a><span data-ttu-id="9aa71-102">并发标记</span><span class="sxs-lookup"><span data-stu-id="9aa71-102">Concurrency Tokens</span></span>

<span data-ttu-id="9aa71-103">如果属性配置为并发标记 EF 将检查与该记录中保存更改时，没有其他用户已修改数据库中的该值。</span><span class="sxs-lookup"><span data-stu-id="9aa71-103">If a property is configured as a concurrency token then EF will check that no other user has modified that value in the database when saving changes to that record.</span></span> <span data-ttu-id="9aa71-104">EF 使用乐观并发模式，这意味着它将假定未更改值并尝试保存数据，但如果它找到的值已更改引发。</span><span class="sxs-lookup"><span data-stu-id="9aa71-104">EF uses an optimistic concurrency pattern, meaning it will assume the value has not changed and try to save the data, but throw if it finds the value has been changed.</span></span>

<span data-ttu-id="9aa71-105">例如，我们可能需要配置`LastName`上`Person`是并发标记。</span><span class="sxs-lookup"><span data-stu-id="9aa71-105">For example we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="9aa71-106">这意味着，如果一个用户尝试保存对某些更改`Person`，但另一个用户已更改`LastName`则将引发异常。</span><span class="sxs-lookup"><span data-stu-id="9aa71-106">This means that if one user tries to save some changes to a `Person`, but another user has changed the `LastName` then an exception will be thrown.</span></span> <span data-ttu-id="9aa71-107">这可能需要，以便你的应用程序就会提示用户以确保此记录仍在保存其更改之前表示实际是同一个人。</span><span class="sxs-lookup"><span data-stu-id="9aa71-107">This may be desirable so that your application can prompt the user to ensure this record still represents the same actual person before saving their changes.</span></span>

> [!NOTE]
> <span data-ttu-id="9aa71-108">本页介绍如何配置并发标记。</span><span class="sxs-lookup"><span data-stu-id="9aa71-108">This page documents how to configure concurrency tokens.</span></span> <span data-ttu-id="9aa71-109">请参阅[处理并发](../saving/concurrency.md)有关如何在你的应用程序中使用开放式并发的示例。</span><span class="sxs-lookup"><span data-stu-id="9aa71-109">See [Handling Concurrency](../saving/concurrency.md) for examples of how to use optimistic concurrency in your application.</span></span>

## <a name="how-concurrency-tokens-work-in-ef"></a><span data-ttu-id="9aa71-110">并发标记 EF 中的工作原理</span><span class="sxs-lookup"><span data-stu-id="9aa71-110">How concurrency tokens work in EF</span></span>

<span data-ttu-id="9aa71-111">数据存储区可以通过检查任何记录正在更新或删除仍具有相同的值已指派上下文最初从数据库加载数据时的并发标记强制并发标记。</span><span class="sxs-lookup"><span data-stu-id="9aa71-111">Data stores can enforce concurrency tokens by checking that any record being updated or deleted still has the same value for the concurrency token that was assigned when the context originally loaded the data from the database.</span></span>

<span data-ttu-id="9aa71-112">例如，关系数据库来实现此包括中的并发标记`WHERE`子句任何`UPDATE`或`DELETE`命令和检查受影响的行数。</span><span class="sxs-lookup"><span data-stu-id="9aa71-112">For example, relational databases achieve this by including the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` commands and checking the number of rows that were affected.</span></span> <span data-ttu-id="9aa71-113">如果并发标记仍与匹配，则将更新一个行。</span><span class="sxs-lookup"><span data-stu-id="9aa71-113">If the concurrency token still matches then one row will be updated.</span></span> <span data-ttu-id="9aa71-114">如果数据库中的值已更改，将不更新任何行。</span><span class="sxs-lookup"><span data-stu-id="9aa71-114">If the value in the database has changed, then no rows are updated.</span></span>

```sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="conventions"></a><span data-ttu-id="9aa71-115">约定</span><span class="sxs-lookup"><span data-stu-id="9aa71-115">Conventions</span></span>

<span data-ttu-id="9aa71-116">按照约定，属性被永远不会配置为并发标记。</span><span class="sxs-lookup"><span data-stu-id="9aa71-116">By convention, properties are never configured as concurrency tokens.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="9aa71-117">数据注释</span><span class="sxs-lookup"><span data-stu-id="9aa71-117">Data Annotations</span></span>

<span data-ttu-id="9aa71-118">你可以使用数据注释将属性配置为并发标记。</span><span class="sxs-lookup"><span data-stu-id="9aa71-118">You can use the Data Annotations to configure a property as a concurrency token.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a><span data-ttu-id="9aa71-119">Fluent API</span><span class="sxs-lookup"><span data-stu-id="9aa71-119">Fluent API</span></span>

<span data-ttu-id="9aa71-120">Fluent API 可用于将属性配置为并发标记。</span><span class="sxs-lookup"><span data-stu-id="9aa71-120">You can use the Fluent API to configure a property as a concurrency token.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a><span data-ttu-id="9aa71-121">时间戳/行版本</span><span class="sxs-lookup"><span data-stu-id="9aa71-121">Timestamp/row version</span></span>

<span data-ttu-id="9aa71-122">时间戳是每次插入或更新行时，由数据库生成一个新值是其中一个属性。</span><span class="sxs-lookup"><span data-stu-id="9aa71-122">A timestamp is a property where a new value is generated by the database every time a row is inserted or updated.</span></span> <span data-ttu-id="9aa71-123">属性也将被视为并发标记。</span><span class="sxs-lookup"><span data-stu-id="9aa71-123">The property is also treated as a concurrency token.</span></span> <span data-ttu-id="9aa71-124">这可确保如果其他人进行了修改你尝试更新由于数据查询的行，则将收到异常。</span><span class="sxs-lookup"><span data-stu-id="9aa71-124">This ensures you will get an exception if anyone else has modified a row that you are trying to update since you queried for the data.</span></span>

<span data-ttu-id="9aa71-125">这如何实现由正在使用的数据库提供程序。</span><span class="sxs-lookup"><span data-stu-id="9aa71-125">How this is achieved is up to the database provider being used.</span></span> <span data-ttu-id="9aa71-126">对于 SQL Server，时间戳通常使用上*byte []*属性，用于将设置为*ROWVERSION*数据库中的列。</span><span class="sxs-lookup"><span data-stu-id="9aa71-126">For SQL Server, timestamp is usually used on a *byte[]* property, which will be setup as a *ROWVERSION* column in the database.</span></span>

### <a name="conventions"></a><span data-ttu-id="9aa71-127">约定</span><span class="sxs-lookup"><span data-stu-id="9aa71-127">Conventions</span></span>

<span data-ttu-id="9aa71-128">按照约定，属性被永远不会配置为时间戳。</span><span class="sxs-lookup"><span data-stu-id="9aa71-128">By convention, properties are never configured as timestamps.</span></span>

### <a name="data-annotations"></a><span data-ttu-id="9aa71-129">数据注释</span><span class="sxs-lookup"><span data-stu-id="9aa71-129">Data Annotations</span></span>

<span data-ttu-id="9aa71-130">数据注释可用于将属性配置为时间戳。</span><span class="sxs-lookup"><span data-stu-id="9aa71-130">You can use Data Annotations to configure a property as a timestamp.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a><span data-ttu-id="9aa71-131">Fluent API</span><span class="sxs-lookup"><span data-stu-id="9aa71-131">Fluent API</span></span>

<span data-ttu-id="9aa71-132">Fluent API 可用于将属性配置为时间戳。</span><span class="sxs-lookup"><span data-stu-id="9aa71-132">You can use the Fluent API to configure a property as a timestamp.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
