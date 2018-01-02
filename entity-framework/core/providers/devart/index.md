---
title: "Devart 数据库提供程序 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: aad70a75-d04d-4d63-a489-7f9062a3c73d
ms.technology: entity-framework-core
uid: core/providers/devart/index
ms.openlocfilehash: 04de917b3327a6f544292781bca42a1170c199ad
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="devart-ef-core-database-providers"></a><span data-ttu-id="5ea6f-102">Devart EF Core 数据库提供程序</span><span class="sxs-lookup"><span data-stu-id="5ea6f-102">Devart EF Core Database Providers</span></span>

<span data-ttu-id="5ea6f-103">Devart 是一款第三方提供程序编写器，提供适用于多种数据库的数据库提供程序。</span><span class="sxs-lookup"><span data-stu-id="5ea6f-103">Devart is a third party provider writer that offers database providers for a wide range of databases.</span></span> <span data-ttu-id="5ea6f-104">有关详细信息，请访问 [devart.com/dotconnect](https://www.devart.com/dotconnect/)。</span><span class="sxs-lookup"><span data-stu-id="5ea6f-104">Find out more at [devart.com/dotconnect](https://www.devart.com/dotconnect/).</span></span>

> [!NOTE]  
> <span data-ttu-id="5ea6f-105">此提供程序不作为 Entity Framework Core 项目的组成部分进行维护。</span><span class="sxs-lookup"><span data-stu-id="5ea6f-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="5ea6f-106">考虑使用第三方提供程序时，请务必评估质量、授权、支持等因素，确保其满足要求。</span><span class="sxs-lookup"><span data-stu-id="5ea6f-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="paid-versions-only"></a><span data-ttu-id="5ea6f-107">仅限付费版</span><span class="sxs-lookup"><span data-stu-id="5ea6f-107">Paid Versions Only</span></span>

<span data-ttu-id="5ea6f-108">Devart dotConnect 是一款商用第三方提供程序。</span><span class="sxs-lookup"><span data-stu-id="5ea6f-108">Devart dotConnect is a commercial third party provider.</span></span> <span data-ttu-id="5ea6f-109">仅付费版 dotConnect 提供 Entity Framework 支持。</span><span class="sxs-lookup"><span data-stu-id="5ea6f-109">Entity Framework support is only available in paid versions of dotConnect.</span></span>

## <a name="install"></a><span data-ttu-id="5ea6f-110">安装</span><span class="sxs-lookup"><span data-stu-id="5ea6f-110">Install</span></span>

<span data-ttu-id="5ea6f-111">有关安装说明，请参阅 [Devart dotConnect 文档](https://www.devart.com/dotconnect/)。</span><span class="sxs-lookup"><span data-stu-id="5ea6f-111">See the [Devart dotConnect documentation](https://www.devart.com/dotconnect/) for installation instructions.</span></span>

## <a name="get-started"></a><span data-ttu-id="5ea6f-112">开始操作</span><span class="sxs-lookup"><span data-stu-id="5ea6f-112">Get Started</span></span>

<span data-ttu-id="5ea6f-113">请参阅 [Devart dotConnect Entity Framework 文档](https://www.devart.com/dotconnect/entityframework.html)和[关于 Entity Framework Core 1 支持的博客文章](http://blog.devart.com/entity-framework-core-1-entity-framework-7-support.html)，了解如何开始操作。</span><span class="sxs-lookup"><span data-stu-id="5ea6f-113">See the [Devart dotConnect Entity Framework documentation](https://www.devart.com/dotconnect/entityframework.html) and [blog article about Entity Framework Core 1 Support](http://blog.devart.com/entity-framework-core-1-entity-framework-7-support.html) to get started.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="5ea6f-114">支持的数据库引擎</span><span class="sxs-lookup"><span data-stu-id="5ea6f-114">Supported Database Engines</span></span>

* <span data-ttu-id="5ea6f-115">MySQL</span><span class="sxs-lookup"><span data-stu-id="5ea6f-115">MySQL</span></span>

* <span data-ttu-id="5ea6f-116">Oracle</span><span class="sxs-lookup"><span data-stu-id="5ea6f-116">Oracle</span></span>

* <span data-ttu-id="5ea6f-117">postgresql</span><span class="sxs-lookup"><span data-stu-id="5ea6f-117">PostgreSQL</span></span>

* <span data-ttu-id="5ea6f-118">SQLite</span><span class="sxs-lookup"><span data-stu-id="5ea6f-118">SQLite</span></span>

* <span data-ttu-id="5ea6f-119">DB2</span><span class="sxs-lookup"><span data-stu-id="5ea6f-119">DB2</span></span>

* [<span data-ttu-id="5ea6f-120">云应用</span><span class="sxs-lookup"><span data-stu-id="5ea6f-120">Cloud apps</span></span>](https://www.devart.com/dotconnect/#cloud)

## <a name="supported-platforms"></a><span data-ttu-id="5ea6f-121">支持的平台</span><span class="sxs-lookup"><span data-stu-id="5ea6f-121">Supported Platforms</span></span>

* <span data-ttu-id="5ea6f-122">.NET Framework（4.5.1 及以上版本）</span><span class="sxs-lookup"><span data-stu-id="5ea6f-122">.NET Framework (4.5.1 onwards)</span></span>
* <span data-ttu-id="5ea6f-123">.NET Core 1.0 及更高版本（适用于 Oracle、MySQL、PostgreSQL 和 SQLite 的提供程序）</span><span class="sxs-lookup"><span data-stu-id="5ea6f-123">.NET Core 1.0 and higher (providers for Oracle, MySQL, PostgreSQL, SQLite)</span></span>
