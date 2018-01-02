---
title: "Microsoft SQL Server 数据库提供程序 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/index
ms.openlocfilehash: b2faf932e0484da4df0c1774afa7ba7ae2d077a5
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="edd03-102">Microsoft SQL Server EF Core 数据库提供程序</span><span class="sxs-lookup"><span data-stu-id="edd03-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="edd03-103">此数据库提供程序允许将 Entity Framework Core 与 Microsoft SQL Server（包括 SQL Azure）一起使用。</span><span class="sxs-lookup"><span data-stu-id="edd03-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="edd03-104">此提供程序作为 [EntityFramework GitHub 项目](https://github.com/aspnet/EntityFramework)的组成部分进行维护。</span><span class="sxs-lookup"><span data-stu-id="edd03-104">The provider is maintained as part of the [EntityFramework GitHub project](https://github.com/aspnet/EntityFramework).</span></span>

## <a name="install"></a><span data-ttu-id="edd03-105">安装</span><span class="sxs-lookup"><span data-stu-id="edd03-105">Install</span></span>

<span data-ttu-id="edd03-106">安装 [Microsoft.EntityFrameworkCore.SqlServer NuGet 包](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)。</span><span class="sxs-lookup"><span data-stu-id="edd03-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a><span data-ttu-id="edd03-107">开始操作</span><span class="sxs-lookup"><span data-stu-id="edd03-107">Get Started</span></span>

<span data-ttu-id="edd03-108">下列资源可帮助你开始使用此提供程序。</span><span class="sxs-lookup"><span data-stu-id="edd03-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="edd03-109">在 .NET Framework（控制台、WinForms、WPF 等）上开始使用</span><span class="sxs-lookup"><span data-stu-id="edd03-109">Getting Started on .NET Framework (Console, WinForms, WPF, etc.)</span></span>](../../get-started/full-dotnet/index.md)

* [<span data-ttu-id="edd03-110">在 ASP.NET Core 上开始使用</span><span class="sxs-lookup"><span data-stu-id="edd03-110">Getting Started on ASP.NET Core</span></span>](../../get-started/aspnetcore/index.md)

* [<span data-ttu-id="edd03-111">UnicornStore 示例应用程序</span><span class="sxs-lookup"><span data-stu-id="edd03-111">UnicornStore Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)

## <a name="supported-database-engines"></a><span data-ttu-id="edd03-112">支持的数据库引擎</span><span class="sxs-lookup"><span data-stu-id="edd03-112">Supported Database Engines</span></span>

* <span data-ttu-id="edd03-113">Microsoft SQL Server（2008 及以上版本）</span><span class="sxs-lookup"><span data-stu-id="edd03-113">Microsoft SQL Server (2008 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="edd03-114">支持的平台</span><span class="sxs-lookup"><span data-stu-id="edd03-114">Supported Platforms</span></span>

* <span data-ttu-id="edd03-115">.NET Framework（4.5.1 及以上版本）</span><span class="sxs-lookup"><span data-stu-id="edd03-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="edd03-116">.NET 核心</span><span class="sxs-lookup"><span data-stu-id="edd03-116">.NET Core</span></span>

* <span data-ttu-id="edd03-117">Mono（4.2.0 及以上版本）</span><span class="sxs-lookup"><span data-stu-id="edd03-117">Mono (4.2.0 onwards)</span></span>

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
