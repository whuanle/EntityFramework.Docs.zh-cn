---
title: "IBM 数据服务器 (DB2) 数据库提供程序 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 02/15/2017
ms.assetid: 825e5332-5aa3-4600-9efb-ab71aaff59ec
ms.technology: entity-framework-core
uid: core/providers/ibm/index
ms.openlocfilehash: a9caa8df63850d4f6b5f2164dad7ac5af7504076
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2018
---
# <a name="ibm-data-server-db2-ef-core-database-providers"></a><span data-ttu-id="4ae20-102">IBM 数据服务器 (DB2) EF Core 数据库提供程序</span><span class="sxs-lookup"><span data-stu-id="4ae20-102">IBM Data Server (DB2) EF Core Database Providers</span></span>

<span data-ttu-id="4ae20-103">此数据库提供程序允许将 Entity Framework Core 与 IBM 数据服务器一起使用。</span><span class="sxs-lookup"><span data-stu-id="4ae20-103">This database provider allows Entity Framework Core to be used with IBM Data Server.</span></span> <span data-ttu-id="4ae20-104">此提供程序由 IBM 进行维护。</span><span class="sxs-lookup"><span data-stu-id="4ae20-104">The provider is maintained by IBM.</span></span>

> [!NOTE]  
> <span data-ttu-id="4ae20-105">此提供程序不作为 Entity Framework Core 项目的组成部分进行维护。</span><span class="sxs-lookup"><span data-stu-id="4ae20-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="4ae20-106">考虑使用第三方提供程序时，请务必评估质量、授权、支持等因素，确保其满足要求。</span><span class="sxs-lookup"><span data-stu-id="4ae20-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="4ae20-107">安装</span><span class="sxs-lookup"><span data-stu-id="4ae20-107">Install</span></span>

<span data-ttu-id="4ae20-108">要在 Windows 上使用 IBM 数据服务器，请安装 [IBM.EntityFrameworkCore NuGet 包](https://www.nuget.org/packages/IBM.EntityFrameworkCore)。</span><span class="sxs-lookup"><span data-stu-id="4ae20-108">To work with IBM Data Server on Windows, install the [IBM.EntityFrameworkCore NuGet package](https://www.nuget.org/packages/IBM.EntityFrameworkCore).</span></span>

``` powershell
Install-Package IBM.EntityFrameworkCore
```

<span data-ttu-id="4ae20-109">要在 Linux 上使用 IBM 数据服务器，请安装 [IBM.EntityFrameworkCore-lnx NuGet 包](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx)。</span><span class="sxs-lookup"><span data-stu-id="4ae20-109">To work with IBM Data Server on Linux, install the [IBM.EntityFrameworkCore-lnx NuGet package](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx).</span></span>

``` powershell
Install-Package IBM.EntityFrameworkCore-lnx
```

## <a name="get-started"></a><span data-ttu-id="4ae20-110">开始操作</span><span class="sxs-lookup"><span data-stu-id="4ae20-110">Get Started</span></span>

[<span data-ttu-id="4ae20-111">开始使用适用于 .NET Core 的 IBM .NET 提供程序</span><span class="sxs-lookup"><span data-stu-id="4ae20-111">Getting started with IBM .NET Provider for .NET Core</span></span>](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/DB2DotnetCore?lang=en)

## <a name="supported-database-engines"></a><span data-ttu-id="4ae20-112">支持的数据库引擎</span><span class="sxs-lookup"><span data-stu-id="4ae20-112">Supported Database Engines</span></span>

* <span data-ttu-id="4ae20-113">zOS</span><span class="sxs-lookup"><span data-stu-id="4ae20-113">zOS</span></span>
* <span data-ttu-id="4ae20-114">LUW（包括 IBM dashDB）</span><span class="sxs-lookup"><span data-stu-id="4ae20-114">LUW including IBM dashDB</span></span>
* <span data-ttu-id="4ae20-115">IBM I</span><span class="sxs-lookup"><span data-stu-id="4ae20-115">IBM I</span></span>
* <span data-ttu-id="4ae20-116">Informix</span><span class="sxs-lookup"><span data-stu-id="4ae20-116">Informix</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="4ae20-117">支持的平台</span><span class="sxs-lookup"><span data-stu-id="4ae20-117">Supported Platforms</span></span>

* <span data-ttu-id="4ae20-118">.NET Framework（4.6 及以上版本）</span><span class="sxs-lookup"><span data-stu-id="4ae20-118">.NET Framework (4.6 onwards)</span></span>
* <span data-ttu-id="4ae20-119">.NET 核心</span><span class="sxs-lookup"><span data-stu-id="4ae20-119">.NET Core</span></span>
