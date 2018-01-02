---
title: "MySQL 数据库提供程序 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4900b882-79c5-40d2-a44a-ccb0292f6ed9
ms.technology: entity-framework-core
uid: core/providers/mysql/index
ms.openlocfilehash: c151845c8b08ef6a668b352f15545752156b0a9d
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
---
# <a name="mysql-ef-core-database-provider"></a><span data-ttu-id="4e96f-102">MySQL EF Core 数据库提供程序</span><span class="sxs-lookup"><span data-stu-id="4e96f-102">MySQL EF Core Database Provider</span></span>

<span data-ttu-id="4e96f-103">此数据库提供程序允许将 Entity Framework Core 与 MySQL 一起使用。</span><span class="sxs-lookup"><span data-stu-id="4e96f-103">This database provider allows Entity Framework Core to be used with MySQL.</span></span> <span data-ttu-id="4e96f-104">此提供程序作为 [MySQL 项目](http://dev.mysql.com)的组成部分进行维护。</span><span class="sxs-lookup"><span data-stu-id="4e96f-104">The provider is maintained as part of the [MySQL project](http://dev.mysql.com).</span></span>

> [!WARNING]  
> <span data-ttu-id="4e96f-105">此提供程序为预发布版本。</span><span class="sxs-lookup"><span data-stu-id="4e96f-105">This provider is pre-release.</span></span>

> [!NOTE]  
> <span data-ttu-id="4e96f-106">此提供程序不作为 Entity Framework Core 项目的组成部分进行维护。</span><span class="sxs-lookup"><span data-stu-id="4e96f-106">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="4e96f-107">考虑使用第三方提供程序时，请务必评估质量、授权、支持等因素，确保其满足要求。</span><span class="sxs-lookup"><span data-stu-id="4e96f-107">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="4e96f-108">安装</span><span class="sxs-lookup"><span data-stu-id="4e96f-108">Install</span></span>

<span data-ttu-id="4e96f-109">安装 [MySql.Data.EntityFrameworkCore NuGet 包](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore)。</span><span class="sxs-lookup"><span data-stu-id="4e96f-109">Install the [MySql.Data.EntityFrameworkCore NuGet package](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).</span></span>

``` powershell
Install-Package MySql.Data.EntityFrameworkCore -Pre
```

## <a name="get-started"></a><span data-ttu-id="4e96f-110">开始操作</span><span class="sxs-lookup"><span data-stu-id="4e96f-110">Get Started</span></span>

<span data-ttu-id="4e96f-111">请参阅[以 MySQL EF Core 提供程序和 Connector/Net 7.0.4 开始](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/)。</span><span class="sxs-lookup"><span data-stu-id="4e96f-111">See [Starting with MySQL EF Core provider and Connector/Net 7.0.4](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/).</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="4e96f-112">支持的数据库引擎</span><span class="sxs-lookup"><span data-stu-id="4e96f-112">Supported Database Engines</span></span>

* <span data-ttu-id="4e96f-113">MySQL</span><span class="sxs-lookup"><span data-stu-id="4e96f-113">MySQL</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="4e96f-114">支持的平台</span><span class="sxs-lookup"><span data-stu-id="4e96f-114">Supported Platforms</span></span>

* <span data-ttu-id="4e96f-115">.NET Framework（4.5.1 及以上版本）</span><span class="sxs-lookup"><span data-stu-id="4e96f-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="4e96f-116">.NET 核心</span><span class="sxs-lookup"><span data-stu-id="4e96f-116">.NET Core</span></span>
