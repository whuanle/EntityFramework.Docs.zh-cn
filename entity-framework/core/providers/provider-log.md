---
title: 提供程序影响的更改-EF Core 的日志
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: 5da1043310e2858638c81a0654a9cab23e39c220
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250811"
---
# <a name="provider-impacting-changes"></a><span data-ttu-id="f97c0-102">提供程序影响的更改</span><span class="sxs-lookup"><span data-stu-id="f97c0-102">Provider-impacting changes</span></span>

<span data-ttu-id="f97c0-103">此页包含用于拉取请求可能需要的其他数据库提供程序以响应作者在 EF Core 存储库进行链接。</span><span class="sxs-lookup"><span data-stu-id="f97c0-103">This page contains links to pull requests made on the EF Core repo that may require authors of other database providers to react.</span></span> <span data-ttu-id="f97c0-104">目的是要更新到新版本的提供商时，适用于现有第三方数据库提供程序的作者提供一个起始点。</span><span class="sxs-lookup"><span data-stu-id="f97c0-104">The intention is to provide a starting point for authors of existing third-party database providers when updating their provider to a new version.</span></span>

<span data-ttu-id="f97c0-105">我们此日志着手从 2.1 到 2.2 的更改。</span><span class="sxs-lookup"><span data-stu-id="f97c0-105">We are starting this log with changes from 2.1 to 2.2.</span></span> <span data-ttu-id="f97c0-106">在 2.1 之前，我们使用[ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware)并[ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi)上我们的问题和拉取请求标签。</span><span class="sxs-lookup"><span data-stu-id="f97c0-106">Prior to 2.1 we used the [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) and [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) labels on our issues and pull requests.</span></span>

## <a name="21-----22"></a><span data-ttu-id="f97c0-107">2.1---> 2.2</span><span class="sxs-lookup"><span data-stu-id="f97c0-107">2.1 ---> 2.2</span></span>

### <a name="test-only-changes"></a><span data-ttu-id="f97c0-108">仅限测试的更改</span><span class="sxs-lookup"><span data-stu-id="f97c0-108">Test-only changes</span></span>

* <span data-ttu-id="f97c0-109">https://github.com/aspnet/EntityFrameworkCore/pull/12057 -在测试中允许可自定义 SQL 分隔符</span><span class="sxs-lookup"><span data-stu-id="f97c0-109">https://github.com/aspnet/EntityFrameworkCore/pull/12057 - Allow customizable SQL delimeters in tests</span></span>
  * <span data-ttu-id="f97c0-110">测试 BuiltInDataTypesTestBase 中允许非严格的浮动点比较的更改</span><span class="sxs-lookup"><span data-stu-id="f97c0-110">Test changes that allow non-strict floating point comparisons in BuiltInDataTypesTestBase</span></span>
  * <span data-ttu-id="f97c0-111">测试更改，可查询的测试以重新用于其他 SQL 分隔符</span><span class="sxs-lookup"><span data-stu-id="f97c0-111">Test changes that allow query tests to be re-used with different SQL delimeters</span></span>
* <span data-ttu-id="f97c0-112">https://github.com/aspnet/EntityFrameworkCore/pull/12072 -将 DbFunction 测试添加到关系规范测试</span><span class="sxs-lookup"><span data-stu-id="f97c0-112">https://github.com/aspnet/EntityFrameworkCore/pull/12072 - Add DbFunction tests to the relational specification tests</span></span>
  * <span data-ttu-id="f97c0-113">如此一来，可以针对所有数据库提供程序运行这些测试</span><span class="sxs-lookup"><span data-stu-id="f97c0-113">Such that these tests can be run against all database providers</span></span>
* <span data-ttu-id="f97c0-114">https://github.com/aspnet/EntityFrameworkCore/pull/12362 -异步测试清理</span><span class="sxs-lookup"><span data-stu-id="f97c0-114">https://github.com/aspnet/EntityFrameworkCore/pull/12362 - Async test cleanup</span></span>
  * <span data-ttu-id="f97c0-115">删除`Wait`调用，不需要的异步，和重命名某些测试方法</span><span class="sxs-lookup"><span data-stu-id="f97c0-115">Remove `Wait` calls, unneeded async, and renamed some test methods</span></span>
* <span data-ttu-id="f97c0-116">https://github.com/aspnet/EntityFrameworkCore/pull/12666 -统一日志记录测试基础结构</span><span class="sxs-lookup"><span data-stu-id="f97c0-116">https://github.com/aspnet/EntityFrameworkCore/pull/12666 - Unify logging test infrastructure</span></span>
  * <span data-ttu-id="f97c0-117">添加`CreateListLoggerFactory`和删除一些以前日志记录基础结构，这将需要使用这些测试能够做出反应的提供程序</span><span class="sxs-lookup"><span data-stu-id="f97c0-117">Added `CreateListLoggerFactory` and removed some previous logging infrastructure, which will require providers using these tests to react</span></span>
* <span data-ttu-id="f97c0-118">https://github.com/aspnet/EntityFrameworkCore/pull/12500 -以异步方式和同步方式运行更多查询测试</span><span class="sxs-lookup"><span data-stu-id="f97c0-118">https://github.com/aspnet/EntityFrameworkCore/pull/12500 - Run more query tests both synchronously and asynchronously</span></span>
  * <span data-ttu-id="f97c0-119">测试名称和组成要素已更改，这将需要使用这些测试能够做出反应的提供程序</span><span class="sxs-lookup"><span data-stu-id="f97c0-119">Test names and factoring has changed, which will require providers using these tests to react</span></span>
* <span data-ttu-id="f97c0-120">https://github.com/aspnet/EntityFrameworkCore/pull/12766 -重命名 ComplexNavigations 模型中的导航</span><span class="sxs-lookup"><span data-stu-id="f97c0-120">https://github.com/aspnet/EntityFrameworkCore/pull/12766 - Renaming navigations in the ComplexNavigations model</span></span>
  * <span data-ttu-id="f97c0-121">提供程序使用这些测试可能需要做出响应</span><span class="sxs-lookup"><span data-stu-id="f97c0-121">Providers using these tests may need to react</span></span>
* <span data-ttu-id="f97c0-122">https://github.com/aspnet/EntityFrameworkCore/pull/12141 -返回到池而不是功能测试中释放上下文</span><span class="sxs-lookup"><span data-stu-id="f97c0-122">https://github.com/aspnet/EntityFrameworkCore/pull/12141 - Return the context to the pool instead of disposing in functional tests</span></span>
  * <span data-ttu-id="f97c0-123">此更改包括这可能需要提供程序以响应某些测试重构</span><span class="sxs-lookup"><span data-stu-id="f97c0-123">This change includes some test refactoring which may require providers to react</span></span>


### <a name="test-and-product-code-changes"></a><span data-ttu-id="f97c0-124">测试和产品代码更改</span><span class="sxs-lookup"><span data-stu-id="f97c0-124">Test and product code changes</span></span>

* <span data-ttu-id="f97c0-125">https://github.com/aspnet/EntityFrameworkCore/pull/12109 -合并 RelationalTypeMapping.Clone 方法</span><span class="sxs-lookup"><span data-stu-id="f97c0-125">https://github.com/aspnet/EntityFrameworkCore/pull/12109 - Consolidate RelationalTypeMapping.Clone methods</span></span>
  * <span data-ttu-id="f97c0-126">允许在派生类中简化 RelationalTypeMapping 到 2.1 中的更改。</span><span class="sxs-lookup"><span data-stu-id="f97c0-126">Changes in 2.1 to the RelationalTypeMapping allowed for a simplification in derived classes.</span></span> <span data-ttu-id="f97c0-127">我们不相信这中断到提供程序，但提供程序可以充分利用此更改及其派生类型中将类映射。</span><span class="sxs-lookup"><span data-stu-id="f97c0-127">We don't believe this was breaking to providers, but providers can take advantage of this change in their derived type mapping classes.</span></span>
* <span data-ttu-id="f97c0-128">https://github.com/aspnet/EntityFrameworkCore/pull/12069 -标记或命名查询</span><span class="sxs-lookup"><span data-stu-id="f97c0-128">https://github.com/aspnet/EntityFrameworkCore/pull/12069 - Tagged or named queries</span></span>
  * <span data-ttu-id="f97c0-129">添加用于标记 LINQ 查询并显示为 SQL 中的注释这些标记的基础结构。</span><span class="sxs-lookup"><span data-stu-id="f97c0-129">Adds infrastructure for tagging LINQ queries and having those tags show up as comments in the SQL.</span></span> <span data-ttu-id="f97c0-130">这可能需要提供程序中的 SQL 生成的做出响应。</span><span class="sxs-lookup"><span data-stu-id="f97c0-130">This may require providers to react in SQL generation.</span></span>
* <span data-ttu-id="f97c0-131">https://github.com/aspnet/EntityFrameworkCore/pull/13115 -支持通过 NTS 空间数据</span><span class="sxs-lookup"><span data-stu-id="f97c0-131">https://github.com/aspnet/EntityFrameworkCore/pull/13115 - Support spatial data via NTS</span></span>
  * <span data-ttu-id="f97c0-132">允许类型映射和成员翻译人员外部提供程序注册</span><span class="sxs-lookup"><span data-stu-id="f97c0-132">Allows type mappings and member translators to be registered outside of the provider</span></span>
    * <span data-ttu-id="f97c0-133">提供程序必须调用基。在其工作其 ITypeMappingSource 实现中 FindMapping()</span><span class="sxs-lookup"><span data-stu-id="f97c0-133">Providers must call base.FindMapping() in their ITypeMappingSource implementation for it to work</span></span>
  * <span data-ttu-id="f97c0-134">遵循此模式将空间支持添加到您在提供程序都是一致的提供商。</span><span class="sxs-lookup"><span data-stu-id="f97c0-134">Follow this pattern to add spatial support to your provider that is consistent across providers.</span></span>
* <span data-ttu-id="f97c0-135">https://github.com/aspnet/EntityFrameworkCore/pull/13199 -添加用于创建服务提供程序增强调试</span><span class="sxs-lookup"><span data-stu-id="f97c0-135">https://github.com/aspnet/EntityFrameworkCore/pull/13199 - Add enhanced debugging for service provider creation</span></span>
  * <span data-ttu-id="f97c0-136">允许 DbContextOptionsExtensions 实现新的接口，可帮助用户了解为什么将内部服务提供商正在重新生成</span><span class="sxs-lookup"><span data-stu-id="f97c0-136">Allows DbContextOptionsExtensions to implement a new interface that can help people understand why the internal service provider is being re-built</span></span>
