---
title: "MySQL 数据库提供程序 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4900b882-79c5-40d2-a44a-ccb0292f6ed9
ms.technology: entity-framework-core
uid: core/providers/mysql/index
ms.openlocfilehash: 1500d017cb463c3f394131a79b9063ff90cce5e2
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2018
---
# <a name="mysql-ef-core-database-provider"></a><span data-ttu-id="2d815-102">MySQL EF Core 数据库提供程序</span><span class="sxs-lookup"><span data-stu-id="2d815-102">MySQL EF Core Database Provider</span></span>

<span data-ttu-id="2d815-103">此数据库提供程序允许将 Entity Framework Core 与 MySQL 一起使用。</span><span class="sxs-lookup"><span data-stu-id="2d815-103">This database provider allows Entity Framework Core to be used with MySQL.</span></span> <span data-ttu-id="2d815-104">此提供程序作为 [MySQL 项目](http://dev.mysql.com)的组成部分进行维护。</span><span class="sxs-lookup"><span data-stu-id="2d815-104">The provider is maintained as part of the [MySQL project](http://dev.mysql.com).</span></span>

> [!WARNING]  
> <span data-ttu-id="2d815-105">此提供程序为预发布版本。</span><span class="sxs-lookup"><span data-stu-id="2d815-105">This provider is pre-release.</span></span>

> [!NOTE]  
> <span data-ttu-id="2d815-106">此提供程序不作为 Entity Framework Core 项目的组成部分进行维护。</span><span class="sxs-lookup"><span data-stu-id="2d815-106">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="2d815-107">考虑使用第三方提供程序时，请务必评估质量、授权、支持等因素，确保其满足要求。</span><span class="sxs-lookup"><span data-stu-id="2d815-107">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="2d815-108">安装</span><span class="sxs-lookup"><span data-stu-id="2d815-108">Install</span></span>

<span data-ttu-id="2d815-109">安装 [MySql.Data.EntityFrameworkCore NuGet 包](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore)。</span><span class="sxs-lookup"><span data-stu-id="2d815-109">Install the [MySql.Data.EntityFrameworkCore NuGet package](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).</span></span>

``` powershell
Install-Package MySql.Data.EntityFrameworkCore -Pre
```

## <a name="get-started"></a><span data-ttu-id="2d815-110">开始操作</span><span class="sxs-lookup"><span data-stu-id="2d815-110">Get Started</span></span>

<span data-ttu-id="2d815-111">请参阅[以 MySQL EF Core 提供程序和 Connector/Net 7.0.4 开始](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/)。</span><span class="sxs-lookup"><span data-stu-id="2d815-111">See [Starting with MySQL EF Core provider and Connector/Net 7.0.4](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/).</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="2d815-112">支持的数据库引擎</span><span class="sxs-lookup"><span data-stu-id="2d815-112">Supported Database Engines</span></span>

* <span data-ttu-id="2d815-113">MySQL</span><span class="sxs-lookup"><span data-stu-id="2d815-113">MySQL</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="2d815-114">支持的平台</span><span class="sxs-lookup"><span data-stu-id="2d815-114">Supported Platforms</span></span>

* <span data-ttu-id="2d815-115">.NET Framework（4.5.1 及以上版本）</span><span class="sxs-lookup"><span data-stu-id="2d815-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="2d815-116">.NET 核心</span><span class="sxs-lookup"><span data-stu-id="2d815-116">.NET Core</span></span>

<span data-ttu-id="2d815-117">请务必查阅[此处](https://dev.mysql.com/doc/connector-net/en/connector-net-versions.html)和[此处](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)的 MySQL 文档，获取有关版本兼容性的信息</span><span class="sxs-lookup"><span data-stu-id="2d815-117">Be sure to review the MySQL documentation for version compatibility information [here](https://dev.mysql.com/doc/connector-net/en/connector-net-versions.html) and [here](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)</span></span>
