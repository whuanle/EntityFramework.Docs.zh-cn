---
title: 索引的 EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 605b30ce710d9034deab97f695496ec66a576565
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993212"
---
# <a name="indexes"></a><span data-ttu-id="996e7-102">索引</span><span class="sxs-lookup"><span data-stu-id="996e7-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="996e7-103">一般而言，本部分中的配置适用于关系数据库。</span><span class="sxs-lookup"><span data-stu-id="996e7-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="996e7-104">安装关系数据库提供程序时，此处显示的扩展方法将变为可用（原因在于共享的 Microsoft.EntityFrameworkCore.Relational 包）。</span><span class="sxs-lookup"><span data-stu-id="996e7-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="996e7-105">关系数据库中的索引映射到相同的概念的 Entity Framework core 中的索引。</span><span class="sxs-lookup"><span data-stu-id="996e7-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="996e7-106">约定</span><span class="sxs-lookup"><span data-stu-id="996e7-106">Conventions</span></span>

<span data-ttu-id="996e7-107">按照约定，索引名为`IX_<type name>_<property name>`。</span><span class="sxs-lookup"><span data-stu-id="996e7-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="996e7-108">对于复合索引`<property name>`变得下划线分隔属性名称的列表。</span><span class="sxs-lookup"><span data-stu-id="996e7-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="996e7-109">数据注释</span><span class="sxs-lookup"><span data-stu-id="996e7-109">Data Annotations</span></span>

<span data-ttu-id="996e7-110">索引不使用数据批注配置。</span><span class="sxs-lookup"><span data-stu-id="996e7-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="996e7-111">Fluent API</span><span class="sxs-lookup"><span data-stu-id="996e7-111">Fluent API</span></span>

<span data-ttu-id="996e7-112">可以使用 Fluent API 来配置索引的名称。</span><span class="sxs-lookup"><span data-stu-id="996e7-112">You can use the Fluent API to configure the name of an index.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

<span data-ttu-id="996e7-113">此外可以指定筛选器。</span><span class="sxs-lookup"><span data-stu-id="996e7-113">You can also specify a filter.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

<span data-ttu-id="996e7-114">当使用 EF 的 SQL Server 提供程序将添加 IS NOT NULL' 筛选所有唯一索引的一部分的可以为 null 列。</span><span class="sxs-lookup"><span data-stu-id="996e7-114">When using the SQL Server provider EF adds a 'IS NOT NULL' filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="996e7-115">若要重写可以提供此约定`null`值。</span><span class="sxs-lookup"><span data-stu-id="996e7-115">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
