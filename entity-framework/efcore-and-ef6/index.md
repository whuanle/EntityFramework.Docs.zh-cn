---
title: 比较 EF Core 和 EF6
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 09ffd8408ea8575ea367eaf2bdab4002db5c619e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997119"
---
# <a name="compare-ef-core--ef6"></a><span data-ttu-id="5a472-102">比较 EF Core 和 EF6</span><span class="sxs-lookup"><span data-stu-id="5a472-102">Compare EF Core & EF6</span></span>

<span data-ttu-id="5a472-103">Entity Framework 有两个不同版本，即 Entity Framework Core 和 Entity Framework 6。</span><span class="sxs-lookup"><span data-stu-id="5a472-103">There are two different versions of Entity Framework, Entity Framework Core and Entity Framework 6.</span></span>

## <a name="entity-framework-6"></a><span data-ttu-id="5a472-104">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="5a472-104">Entity Framework 6</span></span>

<span data-ttu-id="5a472-105">Entity Framework 6 (EF6) 是经过反复测试的数据访问技术，其功能和稳定性已沿用多年。</span><span class="sxs-lookup"><span data-stu-id="5a472-105">Entity Framework 6 (EF6) is a tried and tested data access technology with many years of features and stabilization.</span></span> <span data-ttu-id="5a472-106">它作为 .NET Framework 3.5 SP1 和 Visual Studio 2008 SP1 的一部分，于 2008 年初次发布。</span><span class="sxs-lookup"><span data-stu-id="5a472-106">It first released in 2008, as part of .NET Framework 3.5 SP1 and Visual Studio 2008 SP1.</span></span> <span data-ttu-id="5a472-107">从 EF4.1 版本开始，它已经作为 [EntityFramework NuGet 包](https://www.nuget.org/packages/EntityFramework/)发布 - 目前是 NuGet.org 上最受欢迎的包之一。</span><span class="sxs-lookup"><span data-stu-id="5a472-107">Starting with the EF4.1 release it has shipped as the [EntityFramework NuGet package](https://www.nuget.org/packages/EntityFramework/) - currently one of the most popular packages on NuGet.org.</span></span>

<span data-ttu-id="5a472-108">EF6 产品仍受支持，并将在未来一段时间内继续提供 bug 修复和细微改进。</span><span class="sxs-lookup"><span data-stu-id="5a472-108">EF6 continues to be a supported product, and will continue to see bug fixes and minor improvements for some time to come.</span></span>

## <a name="entity-framework-core"></a><span data-ttu-id="5a472-109">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="5a472-109">Entity Framework Core</span></span>

<span data-ttu-id="5a472-110">Entity Framework Core (EF Core) 是轻量化、可扩展和跨平台版的 Entity Framework。</span><span class="sxs-lookup"><span data-stu-id="5a472-110">Entity Framework Core (EF Core) is a lightweight, extensible, and cross-platform version of Entity Framework.</span></span> <span data-ttu-id="5a472-111">与 EF6 相比，EF Core 引入了众多改进和新功能。</span><span class="sxs-lookup"><span data-stu-id="5a472-111">EF Core introduces many improvements and new features when compared with EF6.</span></span> <span data-ttu-id="5a472-112">不过，EF Core 是一个新代码基，不如 EF6 成熟。</span><span class="sxs-lookup"><span data-stu-id="5a472-112">At the same time, EF Core is a new code base and not as mature as EF6.</span></span>

<span data-ttu-id="5a472-113">EF Core 保留了 EF6 的开发人员体验，并且大多数顶级 API 也保持不变，因此已使用过 EF6 的开发人员会感觉 EF Core 非常熟悉。</span><span class="sxs-lookup"><span data-stu-id="5a472-113">EF Core keeps the developer experience from EF6, and most of the top-level APIs remain the same too, so EF Core will feel very familiar to folks who have used EF6.</span></span> <span data-ttu-id="5a472-114">不过，EF Core 是构建在一套全新的核心组件之上的。</span><span class="sxs-lookup"><span data-stu-id="5a472-114">At the same time, EF Core is built over a completely new set of core components.</span></span> <span data-ttu-id="5a472-115">这意味着 EF Core 不会自动继承 EF6 的所有功能。</span><span class="sxs-lookup"><span data-stu-id="5a472-115">This means EF Core doesn't automatically inherit all the features from EF6.</span></span> <span data-ttu-id="5a472-116">虽然其中一些功能会在将来版本中提供，但其他一些不太常用的功能不会在 EF Core 中提供。</span><span class="sxs-lookup"><span data-stu-id="5a472-116">While some of these features will show up in future releases, other less commonly used features will not be implemented in EF Core.</span></span>

<span data-ttu-id="5a472-117">凭借可扩展、轻量化的新核心，我们还可以将一些 EF6 中不提供的功能添加到 EF Core（例如 LINQ 查询中的备用键、批量更新和混合客户端/数据库评估）。</span><span class="sxs-lookup"><span data-stu-id="5a472-117">The new, extensible, and lightweight core has also allowed us to add some features to EF Core that will not be implemented in EF6 (such as alternate keys, batch updates, and mixed client/database evaluation in LINQ queries).</span></span>
