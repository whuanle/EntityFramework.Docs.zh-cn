---
title: "SQLite 数据库提供程序 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
ms.technology: entity-framework-core
uid: core/providers/sqlite/index
ms.openlocfilehash: 0f3905a491fb5f7a657ce9037d166771e1f326d8
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="7ba7f-102">SQLite EF Core 数据库提供程序</span><span class="sxs-lookup"><span data-stu-id="7ba7f-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="7ba7f-103">此数据库提供程序允许将 Entity Framework Core 与 SQLite 一起使用。</span><span class="sxs-lookup"><span data-stu-id="7ba7f-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="7ba7f-104">此提供程序作为 [EntityFramework GitHub 项目](https://github.com/aspnet/EntityFramework)的组成部分进行维护。</span><span class="sxs-lookup"><span data-stu-id="7ba7f-104">The provider is maintained as part of the [EntityFramework GitHub project](https://github.com/aspnet/EntityFramework).</span></span>

## <a name="install"></a><span data-ttu-id="7ba7f-105">安装</span><span class="sxs-lookup"><span data-stu-id="7ba7f-105">Install</span></span>

<span data-ttu-id="7ba7f-106">安装 [Microsoft.EntityFrameworkCore.Sqlite NuGet 包](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/)。</span><span class="sxs-lookup"><span data-stu-id="7ba7f-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a><span data-ttu-id="7ba7f-107">开始操作</span><span class="sxs-lookup"><span data-stu-id="7ba7f-107">Get Started</span></span>

<span data-ttu-id="7ba7f-108">下列资源可帮助你开始使用此提供程序。</span><span class="sxs-lookup"><span data-stu-id="7ba7f-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="7ba7f-109">UWP 上的本地 SQLite </span><span class="sxs-lookup"><span data-stu-id="7ba7f-109">Local SQLite on UWP</span></span>](../../get-started/uwp/getting-started.md)

* [<span data-ttu-id="7ba7f-110">.NET Core 应用程序到新 SQLite 数据库</span><span class="sxs-lookup"><span data-stu-id="7ba7f-110">.NET Core Application to New SQLite Database</span></span>](../../get-started/netcore/new-db-sqlite.md)

* [<span data-ttu-id="7ba7f-111">Unicorn Clicker 示例应用程序</span><span class="sxs-lookup"><span data-stu-id="7ba7f-111">Unicorn Clicker Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [<span data-ttu-id="7ba7f-112">Unicorn Packer 示例应用程序</span><span class="sxs-lookup"><span data-stu-id="7ba7f-112">Unicorn Packer Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a><span data-ttu-id="7ba7f-113">支持的数据库引擎</span><span class="sxs-lookup"><span data-stu-id="7ba7f-113">Supported Database Engines</span></span>

* <span data-ttu-id="7ba7f-114">SQLite（3.7 及以上版本）</span><span class="sxs-lookup"><span data-stu-id="7ba7f-114">SQLite (3.7 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="7ba7f-115">支持的平台</span><span class="sxs-lookup"><span data-stu-id="7ba7f-115">Supported Platforms</span></span>

* <span data-ttu-id="7ba7f-116">.NET Framework（4.5.1 及以上版本）</span><span class="sxs-lookup"><span data-stu-id="7ba7f-116">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="7ba7f-117">.NET 核心</span><span class="sxs-lookup"><span data-stu-id="7ba7f-117">.NET Core</span></span>

* <span data-ttu-id="7ba7f-118">Mono（4.2.0 及以上版本）</span><span class="sxs-lookup"><span data-stu-id="7ba7f-118">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="7ba7f-119">通用 Windows 平台</span><span class="sxs-lookup"><span data-stu-id="7ba7f-119">Universal Windows Platform</span></span>

## <a name="limitations"></a><span data-ttu-id="7ba7f-120">限制</span><span class="sxs-lookup"><span data-stu-id="7ba7f-120">Limitations</span></span>

<span data-ttu-id="7ba7f-121">有关 SQLite 提供程序的一些重要限制，请参阅 [SQLite 限制](limitations.md)。</span><span class="sxs-lookup"><span data-stu-id="7ba7f-121">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
