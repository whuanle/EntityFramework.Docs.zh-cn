---
title: "Pomelo MySQL 数据库提供程序 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d0198c04-d30d-4419-98f8-a54690cea3c8
ms.technology: entity-framework-core
uid: core/providers/pomelo/index
ms.openlocfilehash: 9ddcda1ae7b058c01937867817e2666b97e7f37a
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
---
# <a name="pomelo-ef-core-database-provider-for-mysql"></a><span data-ttu-id="8e72b-102">适用于 MySQL 的 Pomelo EF Core 数据库提供程序</span><span class="sxs-lookup"><span data-stu-id="8e72b-102">Pomelo EF Core Database Provider for MySQL</span></span>

<span data-ttu-id="8e72b-103">此数据库提供程序允许将 Entity Framework Core 与 MySQL 一起使用。</span><span class="sxs-lookup"><span data-stu-id="8e72b-103">This database provider allows Entity Framework Core to be used with MySQL.</span></span> <span data-ttu-id="8e72b-104">此提供程序作为 [Pomelo Foundation 项目](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql)的组成部分进行维护。</span><span class="sxs-lookup"><span data-stu-id="8e72b-104">The provider is maintained as part of the [Pomelo Foundation Project](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql).</span></span>

> [!NOTE]  
>
> <span data-ttu-id="8e72b-105">此提供程序不作为 Entity Framework Core 项目的组成部分进行维护。</span><span class="sxs-lookup"><span data-stu-id="8e72b-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="8e72b-106">考虑使用第三方提供程序时，请务必评估质量、授权、支持等因素，确保其满足要求。</span><span class="sxs-lookup"><span data-stu-id="8e72b-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="8e72b-107">安装</span><span class="sxs-lookup"><span data-stu-id="8e72b-107">Install</span></span>

<span data-ttu-id="8e72b-108">安装 [Pomelo.EntityFrameworkCore.MySql NuGet 包](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql)。</span><span class="sxs-lookup"><span data-stu-id="8e72b-108">Install the [Pomelo.EntityFrameworkCore.MySql NuGet package](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql).</span></span>

``` powershell
Install-Package Pomelo.EntityFrameworkCore.MySql
```

## <a name="get-started"></a><span data-ttu-id="8e72b-109">开始操作</span><span class="sxs-lookup"><span data-stu-id="8e72b-109">Get Started</span></span>

<span data-ttu-id="8e72b-110">下列资源可帮助你开始使用此提供程序。</span><span class="sxs-lookup"><span data-stu-id="8e72b-110">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="8e72b-111">入门文档</span><span class="sxs-lookup"><span data-stu-id="8e72b-111">Getting started documentation</span></span>](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md#getting-started)

* [<span data-ttu-id="8e72b-112">Yuuko 博客示例应用程序</span><span class="sxs-lookup"><span data-stu-id="8e72b-112">Yuuko Blog sample application</span></span>](https://github.com/PomeloFoundation/YuukoBlog)

## <a name="supported-database-engines"></a><span data-ttu-id="8e72b-113">支持的数据库引擎</span><span class="sxs-lookup"><span data-stu-id="8e72b-113">Supported Database Engines</span></span>

* <span data-ttu-id="8e72b-114">MySQL</span><span class="sxs-lookup"><span data-stu-id="8e72b-114">MySQL</span></span>
* <span data-ttu-id="8e72b-115">MariaDB</span><span class="sxs-lookup"><span data-stu-id="8e72b-115">MariaDB</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="8e72b-116">支持的平台</span><span class="sxs-lookup"><span data-stu-id="8e72b-116">Supported Platforms</span></span>

* <span data-ttu-id="8e72b-117">.NET Framework（4.5.1 及以上版本）</span><span class="sxs-lookup"><span data-stu-id="8e72b-117">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="8e72b-118">.NET 核心</span><span class="sxs-lookup"><span data-stu-id="8e72b-118">.NET Core</span></span>

* <span data-ttu-id="8e72b-119">Mono（4.2.0 及以上版本）</span><span class="sxs-lookup"><span data-stu-id="8e72b-119">Mono (4.2.0 onwards)</span></span>
