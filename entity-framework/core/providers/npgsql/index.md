---
title: "适用于 PostgreSQL 的 Npgsql 数据库提供程序 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 5ecd1b22-c68e-4d87-ba39-b0761f4d5b90
ms.technology: entity-framework-core
uid: core/providers/npgsql/index
ms.openlocfilehash: acf2e18d7a608b0d75b571a5ac0199d84f86066b
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
---
# <a name="npgsql-ef-core-database-provider-for-postgresql"></a><span data-ttu-id="999cd-102">适用于 PostgreSQL 的 Npgsql EF Core 数据库提供程序</span><span class="sxs-lookup"><span data-stu-id="999cd-102">Npgsql EF Core Database Provider for PostgreSQL</span></span>

<span data-ttu-id="999cd-103">此数据库提供程序允许将 Entity Framework Core 与 PostgreSQL 一起使用。</span><span class="sxs-lookup"><span data-stu-id="999cd-103">This database provider allows Entity Framework Core to be used with PostgreSQL.</span></span> <span data-ttu-id="999cd-104">此提供程序作为 [Npgsql 项目](http://www.npgsql.org)的组成部分进行维护。</span><span class="sxs-lookup"><span data-stu-id="999cd-104">The provider is maintained as part of the [Npgsql project](http://www.npgsql.org).</span></span>

> [!NOTE]  
> <span data-ttu-id="999cd-105">此提供程序不作为 Entity Framework Core 项目的组成部分进行维护。</span><span class="sxs-lookup"><span data-stu-id="999cd-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="999cd-106">考虑使用第三方提供程序时，请务必评估质量、授权、支持等因素，确保其满足要求。</span><span class="sxs-lookup"><span data-stu-id="999cd-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="999cd-107">安装</span><span class="sxs-lookup"><span data-stu-id="999cd-107">Install</span></span>

<span data-ttu-id="999cd-108">安装 [Npgsql.EntityFrameworkCore.PostgreSQL NuGet 包](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL)。</span><span class="sxs-lookup"><span data-stu-id="999cd-108">Install the [Npgsql.EntityFrameworkCore.PostgreSQL NuGet package](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).</span></span>

``` powershell
Install-Package Npgsql.EntityFrameworkCore.PostgreSQL
```

## <a name="get-started"></a><span data-ttu-id="999cd-109">开始操作</span><span class="sxs-lookup"><span data-stu-id="999cd-109">Get Started</span></span>

<span data-ttu-id="999cd-110">请参阅 [Npgsql 文档](http://www.npgsql.org/efcore/index.html)，了解如何开始操作。</span><span class="sxs-lookup"><span data-stu-id="999cd-110">See the [Npgsql documentation](http://www.npgsql.org/efcore/index.html) to get started.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="999cd-111">支持的数据库引擎</span><span class="sxs-lookup"><span data-stu-id="999cd-111">Supported Database Engines</span></span>

* <span data-ttu-id="999cd-112">postgresql</span><span class="sxs-lookup"><span data-stu-id="999cd-112">PostgreSQL</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="999cd-113">支持的平台</span><span class="sxs-lookup"><span data-stu-id="999cd-113">Supported Platforms</span></span>

* <span data-ttu-id="999cd-114">.NET Framework（4.5.1 及以上版本）</span><span class="sxs-lookup"><span data-stu-id="999cd-114">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="999cd-115">.NET 核心</span><span class="sxs-lookup"><span data-stu-id="999cd-115">.NET Core</span></span>

* <span data-ttu-id="999cd-116">Mono（4.2.0 及以上版本）</span><span class="sxs-lookup"><span data-stu-id="999cd-116">Mono (4.2.0 onwards)</span></span>
