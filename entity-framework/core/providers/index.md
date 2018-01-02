---
title: "数据库提供程序 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
ms.technology: entity-framework-core
uid: core/providers/index
ms.openlocfilehash: 19c275b7e89c62e79c8bded977e39b2cfb2b439a
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="database-providers"></a><span data-ttu-id="669d8-102">数据库提供程序</span><span class="sxs-lookup"><span data-stu-id="669d8-102">Database Providers</span></span>

<span data-ttu-id="669d8-103">Entity Framework Core 使用某个提供程序模型，从而允许使用 EF 访问多个不同的数据库。</span><span class="sxs-lookup"><span data-stu-id="669d8-103">Entity Framework Core uses a provider model to allow EF to be used to access many different databases.</span></span> <span data-ttu-id="669d8-104">一些概念为大多数据库共有，它们包含于 EF Core 主要组件中。</span><span class="sxs-lookup"><span data-stu-id="669d8-104">Some concepts are common to most databases, and are included in the primary EF Core components.</span></span> <span data-ttu-id="669d8-105">此类概念包括 LINQ 中的表达查询、事务以及对象从数据库加载后的的跟踪更改。</span><span class="sxs-lookup"><span data-stu-id="669d8-105">Such concepts include expressing queries in LINQ, transactions, and tacking changes to objects once they are loaded from the database.</span></span> <span data-ttu-id="669d8-106">另一些概念特定于具体的提供程序。</span><span class="sxs-lookup"><span data-stu-id="669d8-106">Some concepts are specific to a particular provider.</span></span> <span data-ttu-id="669d8-107">例如，通过 SQL Server 提供程序可配置内存优化表（一种特定于 SQL Server 的功能）。</span><span class="sxs-lookup"><span data-stu-id="669d8-107">For example, the SQL Server provider allows you to configure memory-optimized tables (a feature specific to SQL Server).</span></span> <span data-ttu-id="669d8-108">其他一些概念特定于某一类提供程序。</span><span class="sxs-lookup"><span data-stu-id="669d8-108">Other concepts are specific to a class of providers.</span></span> <span data-ttu-id="669d8-109">例如，用于关系数据库的 EF Core 提供程序构建于公共 `Microsoft.EntityFrameworkCore.Relational` 库上，该库提供的 API 可用于配置表和列映射、外键约束等。</span><span class="sxs-lookup"><span data-stu-id="669d8-109">For example, EF Core providers for relational databases build on the common `Microsoft.EntityFrameworkCore.Relational` library, which provides APIs for configuring table and column mappings, foreign key constraints, etc.</span></span>

<span data-ttu-id="669d8-110">EF Core 提供程序由多种源生成。</span><span class="sxs-lookup"><span data-stu-id="669d8-110">EF Core providers are built by a variety of sources.</span></span> <span data-ttu-id="669d8-111">并非所有提供程序均作为 Entity Framework Core 项目的组成部分进行维护。</span><span class="sxs-lookup"><span data-stu-id="669d8-111">Not all providers are maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="669d8-112">考虑使用第三方提供程序时，请务必评估质量、授权、支持等因素，确保其满足要求。</span><span class="sxs-lookup"><span data-stu-id="669d8-112">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>
