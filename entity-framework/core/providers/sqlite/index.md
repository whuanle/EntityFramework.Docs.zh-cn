---
title: "SQLite 数据库提供程序 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
ms.technology: entity-framework-core
uid: core/providers/sqlite/index
ms.openlocfilehash: 2e392f382f0e6f4d092a362c44f2149eb336db17
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2018
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="a19ab-102">SQLite EF Core 数据库提供程序</span><span class="sxs-lookup"><span data-stu-id="a19ab-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="a19ab-103">此数据库提供程序允许将 Entity Framework Core 与 SQLite 一起使用。</span><span class="sxs-lookup"><span data-stu-id="a19ab-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="a19ab-104">该提供程序作为 [Entity Framework Core 项目](https://github.com/aspnet/EntityFrameworkCore)的组成部分进行维护。</span><span class="sxs-lookup"><span data-stu-id="a19ab-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="a19ab-105">安装</span><span class="sxs-lookup"><span data-stu-id="a19ab-105">Install</span></span>

<span data-ttu-id="a19ab-106">安装 [Microsoft.EntityFrameworkCore.Sqlite NuGet 包](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/)。</span><span class="sxs-lookup"><span data-stu-id="a19ab-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a><span data-ttu-id="a19ab-107">开始操作</span><span class="sxs-lookup"><span data-stu-id="a19ab-107">Get Started</span></span>

<span data-ttu-id="a19ab-108">下列资源可帮助你开始使用此提供程序。</span><span class="sxs-lookup"><span data-stu-id="a19ab-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="a19ab-109">UWP 上的本地 SQLite </span><span class="sxs-lookup"><span data-stu-id="a19ab-109">Local SQLite on UWP</span></span>](../../get-started/uwp/getting-started.md)

* [<span data-ttu-id="a19ab-110">.NET Core 应用程序到新 SQLite 数据库</span><span class="sxs-lookup"><span data-stu-id="a19ab-110">.NET Core Application to New SQLite Database</span></span>](../../get-started/netcore/new-db-sqlite.md)

* [<span data-ttu-id="a19ab-111">Unicorn Clicker 示例应用程序</span><span class="sxs-lookup"><span data-stu-id="a19ab-111">Unicorn Clicker Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [<span data-ttu-id="a19ab-112">Unicorn Packer 示例应用程序</span><span class="sxs-lookup"><span data-stu-id="a19ab-112">Unicorn Packer Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a><span data-ttu-id="a19ab-113">支持的数据库引擎</span><span class="sxs-lookup"><span data-stu-id="a19ab-113">Supported Database Engines</span></span>

* <span data-ttu-id="a19ab-114">SQLite（3.7 及以上版本）</span><span class="sxs-lookup"><span data-stu-id="a19ab-114">SQLite (3.7 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="a19ab-115">支持的平台</span><span class="sxs-lookup"><span data-stu-id="a19ab-115">Supported Platforms</span></span>

* <span data-ttu-id="a19ab-116">.NET Framework（4.5.1 及以上版本）</span><span class="sxs-lookup"><span data-stu-id="a19ab-116">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="a19ab-117">.NET 核心</span><span class="sxs-lookup"><span data-stu-id="a19ab-117">.NET Core</span></span>

* <span data-ttu-id="a19ab-118">Mono（4.2.0 及以上版本）</span><span class="sxs-lookup"><span data-stu-id="a19ab-118">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="a19ab-119">通用 Windows 平台</span><span class="sxs-lookup"><span data-stu-id="a19ab-119">Universal Windows Platform</span></span>

## <a name="limitations"></a><span data-ttu-id="a19ab-120">限制</span><span class="sxs-lookup"><span data-stu-id="a19ab-120">Limitations</span></span>

<span data-ttu-id="a19ab-121">有关 SQLite 提供程序的一些重要限制，请参阅 [SQLite 限制](limitations.md)。</span><span class="sxs-lookup"><span data-stu-id="a19ab-121">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
