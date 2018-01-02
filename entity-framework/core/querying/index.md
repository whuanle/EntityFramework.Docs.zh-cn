---
title: "查询数据 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
ms.technology: entity-framework-core
uid: core/querying/index
ms.openlocfilehash: a2dd830b25c64b007a881c105a87b5c631b00266
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="querying-data"></a><span data-ttu-id="05804-102">查询数据</span><span class="sxs-lookup"><span data-stu-id="05804-102">Querying Data</span></span>

<span data-ttu-id="05804-103">Entity Framework Core 使用语言集成查询 (LINQ) 来查询数据库中的数据。</span><span class="sxs-lookup"><span data-stu-id="05804-103">Entity Framework Core uses Language Integrate Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="05804-104">通过 LINQ 可使用 C#（或你选择的 .NET 语言）基于派生上下文和实体类编写强类型查询。</span><span class="sxs-lookup"><span data-stu-id="05804-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span> <span data-ttu-id="05804-105">LINQ 查询的一种表示形式会传递给数据库提供程序，进而转换为特定于数据库的查询语言（例如，适用于关系数据库的 SQL）。</span><span class="sxs-lookup"><span data-stu-id="05804-105">A representation of the LINQ query is passed to the database provider, to be translated in database-specific query language (e.g. SQL for a relational database).</span></span> <span data-ttu-id="05804-106">有关如何处理查询的更多详细信息，请参阅[查询的工作原理](overview.md)。</span><span class="sxs-lookup"><span data-stu-id="05804-106">For more detailed information on how a query is processed, see [How Query Works](overview.md).</span></span>
