---
title: "索引的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: f577fccfefc6908edf2ac47ae630323d7a9f5f2b
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2018
---
# <a name="indexes"></a><span data-ttu-id="96dd5-102">索引</span><span class="sxs-lookup"><span data-stu-id="96dd5-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="96dd5-103">一般而言，本部分中的配置适用于关系数据库。</span><span class="sxs-lookup"><span data-stu-id="96dd5-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="96dd5-104">安装关系数据库提供程序时，此处显示的扩展方法将变为可用（原因在于共享的 Microsoft.EntityFrameworkCore.Relational 包）。</span><span class="sxs-lookup"><span data-stu-id="96dd5-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="96dd5-105">关系数据库中的索引映射到索引中的实体框架的核心概念相同。</span><span class="sxs-lookup"><span data-stu-id="96dd5-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="96dd5-106">约定</span><span class="sxs-lookup"><span data-stu-id="96dd5-106">Conventions</span></span>

<span data-ttu-id="96dd5-107">按照约定，名为索引`IX_<type name>_<property name>`。</span><span class="sxs-lookup"><span data-stu-id="96dd5-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="96dd5-108">对于组合索引`<property name>`将成为下划线分隔列表的属性名称。</span><span class="sxs-lookup"><span data-stu-id="96dd5-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="96dd5-109">数据注释</span><span class="sxs-lookup"><span data-stu-id="96dd5-109">Data Annotations</span></span>

<span data-ttu-id="96dd5-110">不能使用数据注释配置索引。</span><span class="sxs-lookup"><span data-stu-id="96dd5-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="96dd5-111">Fluent API</span><span class="sxs-lookup"><span data-stu-id="96dd5-111">Fluent API</span></span>

<span data-ttu-id="96dd5-112">Fluent API 可用于配置索引的名称。</span><span class="sxs-lookup"><span data-stu-id="96dd5-112">You can use the Fluent API to configure the name of an index.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

<span data-ttu-id="96dd5-113">你还可以指定筛选器。</span><span class="sxs-lookup"><span data-stu-id="96dd5-113">You can also specify a filter.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

<span data-ttu-id="96dd5-114">当使用 SQL Server 提供程序 EF 将添加 IS NOT NULL 筛选所有可以为 null 的列都是唯一索引的一部分。</span><span class="sxs-lookup"><span data-stu-id="96dd5-114">When using the SQL Server provider EF adds a 'IS NOT NULL' filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="96dd5-115">若要重写你可以提供此约定`null`值。</span><span class="sxs-lookup"><span data-stu-id="96dd5-115">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
