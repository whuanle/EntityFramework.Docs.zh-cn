---
title: "InMemory 数据库提供程序 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: a8e05f50837f3da554b338475d24215706dfa2ec
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="b4614-102">EF Core In-Memory 数据库提供程序</span><span class="sxs-lookup"><span data-stu-id="b4614-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="b4614-103">此数据库提供程序允许将 Entity Framework Core 和内存数据库一起使用。</span><span class="sxs-lookup"><span data-stu-id="b4614-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="b4614-104">测试使用 Entity Framework Core 的代码时，这非常有用。</span><span class="sxs-lookup"><span data-stu-id="b4614-104">This is useful when testing code that uses Entity Framework Core.</span></span> <span data-ttu-id="b4614-105">此提供程序作为 [EntityFramework GitHub 项目](https://github.com/aspnet/EntityFramework)的组成部分进行维护。</span><span class="sxs-lookup"><span data-stu-id="b4614-105">The provider is maintained as part of the [EntityFramework GitHub project](https://github.com/aspnet/EntityFramework).</span></span>

## <a name="install"></a><span data-ttu-id="b4614-106">安装</span><span class="sxs-lookup"><span data-stu-id="b4614-106">Install</span></span>

<span data-ttu-id="b4614-107">安装 [Microsoft.EntityFrameworkCore.InMemory NuGet 包](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/)。</span><span class="sxs-lookup"><span data-stu-id="b4614-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="b4614-108">开始操作</span><span class="sxs-lookup"><span data-stu-id="b4614-108">Get Started</span></span>

<span data-ttu-id="b4614-109">下列资源可帮助你开始使用此提供程序。</span><span class="sxs-lookup"><span data-stu-id="b4614-109">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="b4614-110">使用 InMemory 进行测试</span><span class="sxs-lookup"><span data-stu-id="b4614-110">Testing with InMemory</span></span>](../../miscellaneous/testing/in-memory.md)

* [<span data-ttu-id="b4614-111">UnicornStore 示例应用程序测试</span><span class="sxs-lookup"><span data-stu-id="b4614-111">UnicornStore Sample Application Tests</span></span>](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a><span data-ttu-id="b4614-112">支持的数据库引擎</span><span class="sxs-lookup"><span data-stu-id="b4614-112">Supported Database Engines</span></span>

* <span data-ttu-id="b4614-113">内置的内存中数据库（仅用于测试目的）</span><span class="sxs-lookup"><span data-stu-id="b4614-113">Built-in in-memory database (designed for testing purposes only)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="b4614-114">支持的平台</span><span class="sxs-lookup"><span data-stu-id="b4614-114">Supported Platforms</span></span>

* <span data-ttu-id="b4614-115">.NET Framework（4.5.1 及以上版本）</span><span class="sxs-lookup"><span data-stu-id="b4614-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="b4614-116">.NET 核心</span><span class="sxs-lookup"><span data-stu-id="b4614-116">.NET Core</span></span>

* <span data-ttu-id="b4614-117">Mono（4.2.0 及以上版本）</span><span class="sxs-lookup"><span data-stu-id="b4614-117">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="b4614-118">通用 Windows 平台</span><span class="sxs-lookup"><span data-stu-id="b4614-118">Universal Windows Platform</span></span>
