---
title: InMemory 数据库提供程序 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: ca802f55d973b9f79073c2507c1e0c7a853a1fce
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993315"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="6c7c5-102">EF Core In-Memory 数据库提供程序</span><span class="sxs-lookup"><span data-stu-id="6c7c5-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="6c7c5-103">此数据库提供程序允许将 Entity Framework Core 和内存数据库一起使用。</span><span class="sxs-lookup"><span data-stu-id="6c7c5-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="6c7c5-104">这对测试非常有用，尽管内存中模式下的 SQLite 提供程序可能是针对关系数据库的更合适的测试替代。</span><span class="sxs-lookup"><span data-stu-id="6c7c5-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="6c7c5-105">该提供程序作为 [Entity Framework Core 项目](https://github.com/aspnet/EntityFrameworkCore)的组成部分进行维护。</span><span class="sxs-lookup"><span data-stu-id="6c7c5-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="6c7c5-106">安装</span><span class="sxs-lookup"><span data-stu-id="6c7c5-106">Install</span></span>

<span data-ttu-id="6c7c5-107">安装 [Microsoft.EntityFrameworkCore.InMemory NuGet 包](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/)。</span><span class="sxs-lookup"><span data-stu-id="6c7c5-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="6c7c5-108">开始操作</span><span class="sxs-lookup"><span data-stu-id="6c7c5-108">Get Started</span></span>

<span data-ttu-id="6c7c5-109">下列资源可帮助你开始使用此提供程序。</span><span class="sxs-lookup"><span data-stu-id="6c7c5-109">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="6c7c5-110">使用 InMemory 进行测试</span><span class="sxs-lookup"><span data-stu-id="6c7c5-110">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)

* [<span data-ttu-id="6c7c5-111">UnicornStore 示例应用程序测试</span><span class="sxs-lookup"><span data-stu-id="6c7c5-111">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="6c7c5-112">支持的数据库引擎</span><span class="sxs-lookup"><span data-stu-id="6c7c5-112">Supported Database Engines</span></span>

* <span data-ttu-id="6c7c5-113">内置的内存中数据库（仅用于测试目的）</span><span class="sxs-lookup"><span data-stu-id="6c7c5-113">Built-in in-memory database (designed for testing purposes only)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="6c7c5-114">支持的平台</span><span class="sxs-lookup"><span data-stu-id="6c7c5-114">Supported Platforms</span></span>

* <span data-ttu-id="6c7c5-115">.NET Framework（4.5.1 及以上版本）</span><span class="sxs-lookup"><span data-stu-id="6c7c5-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="6c7c5-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="6c7c5-116">.NET Core</span></span>

* <span data-ttu-id="6c7c5-117">Mono（4.2.0 及以上版本）</span><span class="sxs-lookup"><span data-stu-id="6c7c5-117">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="6c7c5-118">通用 Windows 平台</span><span class="sxs-lookup"><span data-stu-id="6c7c5-118">Universal Windows Platform</span></span>
