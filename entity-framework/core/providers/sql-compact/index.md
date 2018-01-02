---
title: "Microsoft SQL Server Compact Edition 数据库提供程序 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 073f0004-3eb5-4618-ab93-0674910e1819
ms.technology: entity-framework-core
uid: core/providers/sql-compact/index
ms.openlocfilehash: d8b73621bdd363efec5bb7728886e0a0f6bdcf76
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
---
# <a name="microsoft-sql-server-compact-edition-ef-core-database-provider"></a><span data-ttu-id="a0a58-102">Microsoft SQL Server Compact Edition EF Core 数据库提供程序</span><span class="sxs-lookup"><span data-stu-id="a0a58-102">Microsoft SQL Server Compact Edition EF Core Database Provider</span></span>

<span data-ttu-id="a0a58-103">此数据库提供程序允许将 Entity Framework Core 与 SQL Server Compact Edition 一起使用。</span><span class="sxs-lookup"><span data-stu-id="a0a58-103">This database provider allows Entity Framework Core to be used with SQL Server Compact Edition.</span></span> <span data-ttu-id="a0a58-104">此提供程序作为 [ErikEJ/EntityFramework.SqlServerCompact GitHub 项目](https://github.com/ErikEJ/EntityFramework.SqlServerCompact)的组成部分进行维护。</span><span class="sxs-lookup"><span data-stu-id="a0a58-104">The provider is maintained as part of the [ErikEJ/EntityFramework.SqlServerCompact GitHub Project](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span></span>

> [!NOTE]  
> <span data-ttu-id="a0a58-105">此提供程序不作为 Entity Framework Core 项目的组成部分进行维护。</span><span class="sxs-lookup"><span data-stu-id="a0a58-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="a0a58-106">考虑使用第三方提供程序时，请务必评估质量、授权、支持等因素，确保其满足要求。</span><span class="sxs-lookup"><span data-stu-id="a0a58-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="a0a58-107">安装</span><span class="sxs-lookup"><span data-stu-id="a0a58-107">Install</span></span>

<span data-ttu-id="a0a58-108">要使用 SQL Server Compact Edition 4.0，请安装 [EntityFrameworkCore.SqlServerCompact40 NuGet 包](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40)。</span><span class="sxs-lookup"><span data-stu-id="a0a58-108">To work with SQL Server Compact Edition 4.0, install the [EntityFrameworkCore.SqlServerCompact40 NuGet package](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40).</span></span>

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact40
```

<span data-ttu-id="a0a58-109">要使用 SQL Server Compact Edition 3.5，请安装 [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35)。</span><span class="sxs-lookup"><span data-stu-id="a0a58-109">To work with SQL Server Compact Edition 3.5, install the [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35).</span></span>

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact35
```

## <a name="get-started"></a><span data-ttu-id="a0a58-110">开始操作</span><span class="sxs-lookup"><span data-stu-id="a0a58-110">Get Started</span></span>

<span data-ttu-id="a0a58-111">请参阅[项目网站上的入门文档](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)</span><span class="sxs-lookup"><span data-stu-id="a0a58-111">See the [getting started documentation on the project site](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="a0a58-112">支持的数据库引擎</span><span class="sxs-lookup"><span data-stu-id="a0a58-112">Supported Database Engines</span></span>

* <span data-ttu-id="a0a58-113">SQL Server Compact Edition 3.5</span><span class="sxs-lookup"><span data-stu-id="a0a58-113">SQL Server Compact Edition 3.5</span></span>

* <span data-ttu-id="a0a58-114">SQL Server Compact Edition 4.0</span><span class="sxs-lookup"><span data-stu-id="a0a58-114">SQL Server Compact Edition 4.0</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="a0a58-115">支持的平台</span><span class="sxs-lookup"><span data-stu-id="a0a58-115">Supported Platforms</span></span>

* <span data-ttu-id="a0a58-116">.NET Framework（4.5.1 及以上版本）</span><span class="sxs-lookup"><span data-stu-id="a0a58-116">.NET Framework (4.5.1 onwards)</span></span>
