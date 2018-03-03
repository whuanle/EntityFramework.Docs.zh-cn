---
title: "Microsoft SQL Server Compact Edition 数据库提供程序 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 073f0004-3eb5-4618-ab93-0674910e1819
ms.technology: entity-framework-core
uid: core/providers/sql-compact/index
ms.openlocfilehash: d8b73621bdd363efec5bb7728886e0a0f6bdcf76
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2018
---
# <a name="microsoft-sql-server-compact-edition-ef-core-database-provider"></a><span data-ttu-id="64fff-102">Microsoft SQL Server Compact Edition EF Core 数据库提供程序</span><span class="sxs-lookup"><span data-stu-id="64fff-102">Microsoft SQL Server Compact Edition EF Core Database Provider</span></span>

<span data-ttu-id="64fff-103">此数据库提供程序允许将 Entity Framework Core 与 SQL Server Compact Edition 一起使用。</span><span class="sxs-lookup"><span data-stu-id="64fff-103">This database provider allows Entity Framework Core to be used with SQL Server Compact Edition.</span></span> <span data-ttu-id="64fff-104">此提供程序作为 [ErikEJ/EntityFramework.SqlServerCompact GitHub 项目](https://github.com/ErikEJ/EntityFramework.SqlServerCompact)的组成部分进行维护。</span><span class="sxs-lookup"><span data-stu-id="64fff-104">The provider is maintained as part of the [ErikEJ/EntityFramework.SqlServerCompact GitHub Project](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span></span>

> [!NOTE]  
> <span data-ttu-id="64fff-105">此提供程序不作为 Entity Framework Core 项目的组成部分进行维护。</span><span class="sxs-lookup"><span data-stu-id="64fff-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="64fff-106">考虑使用第三方提供程序时，请务必评估质量、授权、支持等因素，确保其满足要求。</span><span class="sxs-lookup"><span data-stu-id="64fff-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="64fff-107">安装</span><span class="sxs-lookup"><span data-stu-id="64fff-107">Install</span></span>

<span data-ttu-id="64fff-108">要使用 SQL Server Compact Edition 4.0，请安装 [EntityFrameworkCore.SqlServerCompact40 NuGet 包](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40)。</span><span class="sxs-lookup"><span data-stu-id="64fff-108">To work with SQL Server Compact Edition 4.0, install the [EntityFrameworkCore.SqlServerCompact40 NuGet package](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40).</span></span>

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact40
```

<span data-ttu-id="64fff-109">要使用 SQL Server Compact Edition 3.5，请安装 [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35)。</span><span class="sxs-lookup"><span data-stu-id="64fff-109">To work with SQL Server Compact Edition 3.5, install the [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35).</span></span>

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact35
```

## <a name="get-started"></a><span data-ttu-id="64fff-110">开始操作</span><span class="sxs-lookup"><span data-stu-id="64fff-110">Get Started</span></span>

<span data-ttu-id="64fff-111">请参阅[项目网站上的入门文档](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)</span><span class="sxs-lookup"><span data-stu-id="64fff-111">See the [getting started documentation on the project site](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="64fff-112">支持的数据库引擎</span><span class="sxs-lookup"><span data-stu-id="64fff-112">Supported Database Engines</span></span>

* <span data-ttu-id="64fff-113">SQL Server Compact Edition 3.5</span><span class="sxs-lookup"><span data-stu-id="64fff-113">SQL Server Compact Edition 3.5</span></span>

* <span data-ttu-id="64fff-114">SQL Server Compact Edition 4.0</span><span class="sxs-lookup"><span data-stu-id="64fff-114">SQL Server Compact Edition 4.0</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="64fff-115">支持的平台</span><span class="sxs-lookup"><span data-stu-id="64fff-115">Supported Platforms</span></span>

* <span data-ttu-id="64fff-116">.NET Framework（4.5.1 及以上版本）</span><span class="sxs-lookup"><span data-stu-id="64fff-116">.NET Framework (4.5.1 onwards)</span></span>
