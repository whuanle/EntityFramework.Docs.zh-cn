---
title: "FirebirdSQL 数据库提供程序 - EF Core"
author: ralmsdeveloper
ms.author: ralmsdeveloper
ms.date: 11/22/2017
ms.assetid: d0168c04-d30d-4219-98f8-a54690cea3c6
ms.technology: entity-framework-core
uid: core/providers/firebird-community/index
ms.openlocfilehash: 682988a91ef04dbd552588a537f53124b931f17d
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2018
---
# <a name="firebird-ef-core-database-provider"></a><span data-ttu-id="196da-102">Firebird EF Core 数据库提供程序</span><span class="sxs-lookup"><span data-stu-id="196da-102">Firebird EF Core Database Provider</span></span>

<span data-ttu-id="196da-103">此数据库提供程序允许将 Entity Framework Core 与 FirebirdSQL 一起使用。</span><span class="sxs-lookup"><span data-stu-id="196da-103">This database provider allows Entity Framework Core to be used with FirebirdSQL.</span></span> <span data-ttu-id="196da-104">此提供程序作为 [ralmsdeveloper/EntityFrameworkCore.FirebirdSQL GitHub 项目](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL)的组成部分进行维护。</span><span class="sxs-lookup"><span data-stu-id="196da-104">The provider is maintained as part of the [ralmsdeveloper/EntityFrameworkCore.FirebirdSQL GitHub Project](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL).</span></span>

> [!NOTE]  
>
> <span data-ttu-id="196da-105">此提供程序不作为 Entity Framework Core 项目的组成部分进行维护。</span><span class="sxs-lookup"><span data-stu-id="196da-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="196da-106">考虑使用第三方提供程序时，请务必评估质量、授权、支持等因素，确保其满足要求。</span><span class="sxs-lookup"><span data-stu-id="196da-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="196da-107">安装</span><span class="sxs-lookup"><span data-stu-id="196da-107">Install</span></span>

<span data-ttu-id="196da-108">安装 [EntityFrameworkCore.FirebirdSQL NuGet 包](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSQL)。</span><span class="sxs-lookup"><span data-stu-id="196da-108">Install the [EntityFrameworkCore.FirebirdSQL NuGet package](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSQL).</span></span>

``` powershell
Install-Package EntityFrameworkCore.FirebirdSQL
```

## <a name="get-started"></a><span data-ttu-id="196da-109">开始操作</span><span class="sxs-lookup"><span data-stu-id="196da-109">Get Started</span></span>

<span data-ttu-id="196da-110">请参阅[项目网站上的入门文档](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)</span><span class="sxs-lookup"><span data-stu-id="196da-110">See the [getting started documentation on the project site](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="196da-111">支持的数据库引擎</span><span class="sxs-lookup"><span data-stu-id="196da-111">Supported Database Engines</span></span>

* <span data-ttu-id="196da-112">FirebirdSql 2.5</span><span class="sxs-lookup"><span data-stu-id="196da-112">FirebirdSql 2.5</span></span>
* <span data-ttu-id="196da-113">FirebirdSql 3.X</span><span class="sxs-lookup"><span data-stu-id="196da-113">FirebirdSql 3.X</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="196da-114">支持的平台</span><span class="sxs-lookup"><span data-stu-id="196da-114">Supported Platforms</span></span>

* <span data-ttu-id="196da-115">.NET Framework（4.5.1 及以上版本）</span><span class="sxs-lookup"><span data-stu-id="196da-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="196da-116">.NET 核心</span><span class="sxs-lookup"><span data-stu-id="196da-116">.NET Core</span></span>

* <span data-ttu-id="196da-117">Mono（4.2.0 及以上版本）</span><span class="sxs-lookup"><span data-stu-id="196da-117">Mono (4.2.0 onwards)</span></span>
