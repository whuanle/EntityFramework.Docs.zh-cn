---
title: 新增功能 - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
caps.latest.revision: 3
ms.openlocfilehash: 0da2ce778a765037ecacd0726cbb7cda08b5683f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/08/2018
ms.locfileid: "37911701"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="5ac73-102">EF6 中的新增功能</span><span class="sxs-lookup"><span data-stu-id="5ac73-102">What's New in EF6</span></span>

<span data-ttu-id="5ac73-103">强烈建议使用最新发布的实体框架版本，以确保获得最新功能和最高稳定性。</span><span class="sxs-lookup"><span data-stu-id="5ac73-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="5ac73-104">但我们也意识到用户可能需要使用以前的版本，或想要尝试最新预发行版中的新改进。</span><span class="sxs-lookup"><span data-stu-id="5ac73-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="5ac73-105">若要安装特定版本的 EF，请参阅[获取实体框架](~/ef6/fundamentals/install.md)。</span><span class="sxs-lookup"><span data-stu-id="5ac73-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

<span data-ttu-id="5ac73-106">本页记录每个新版本中包含的功能。</span><span class="sxs-lookup"><span data-stu-id="5ac73-106">This page documents the features that are included on each new release.</span></span>

## <a name="recent-releases"></a><span data-ttu-id="5ac73-107">最新版本</span><span class="sxs-lookup"><span data-stu-id="5ac73-107">Recent releases</span></span>

### <a name="ef-tools-update-in-visual-studio-2017-157"></a><span data-ttu-id="5ac73-108">Visual Studio 2017 15.7 中的 EF 工具更新</span><span class="sxs-lookup"><span data-stu-id="5ac73-108">EF Tools Update in Visual Studio 2017 15.7</span></span>

<span data-ttu-id="5ac73-109">2018 年 5 月，我们在 Visual Studio 2017 15.7 中发布了更新后的 EF6 工具。</span><span class="sxs-lookup"><span data-stu-id="5ac73-109">In May 2018, we released an updated version of the EF6 Tools in Visual Studio 2017 15.7.</span></span>
<span data-ttu-id="5ac73-110">该版本包括对一些常见难题的改进：</span><span class="sxs-lookup"><span data-stu-id="5ac73-110">It includes improvements in some common pain areas:</span></span>

- <span data-ttu-id="5ac73-111">全面检查了用户界面的辅助功能</span><span class="sxs-lookup"><span data-stu-id="5ac73-111">An overhaul to the accessibility of the user interface</span></span>
- <span data-ttu-id="5ac73-112">解决了 SQL Server 在反向工程上性能退化的问题 [#4](https://github.com/aspnet/entityframework6/issues/4)</span><span class="sxs-lookup"><span data-stu-id="5ac73-112">Workaround for SQL Server performance regression on reverse engineering [#4](https://github.com/aspnet/entityframework6/issues/4)</span></span>
- <span data-ttu-id="5ac73-113">根据数据库支持更新模型，从而在 SQL Server 上获得较大模型 [#185](https://github.com/aspnet/EntityFramework6/issues/185)</span><span class="sxs-lookup"><span data-stu-id="5ac73-113">Update model from database support for larger models on SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)</span></span>

<span data-ttu-id="5ac73-114">在新项目中创建模型时，此新版本的 EF 工具会安装 EF 6.2 运行时。</span><span class="sxs-lookup"><span data-stu-id="5ac73-114">This new version of EF Tools installs the EF 6.2 runtime when creating a model in a new project.</span></span> <span data-ttu-id="5ac73-115">借助较旧版本的 Visual Studio，可通过安装相应版本的 NuGet 包来使用 EF 6.2 运行时（以及以前任何版本的 EF）。</span><span class="sxs-lookup"><span data-stu-id="5ac73-115">With older versions of Visual Studio, it is possible to use the EF 6.2 runtime (as well as any past version of EF) by installing the corresponding version of the NuGet package.</span></span>

### <a name="ef-62-runtime"></a><span data-ttu-id="5ac73-116">EF 6.2 运行时</span><span class="sxs-lookup"><span data-stu-id="5ac73-116">EF 6.2 Runtime</span></span>

<span data-ttu-id="5ac73-117">EF 6.2 运行时已于 2017 年 10 月发布到 NuGet。</span><span class="sxs-lookup"><span data-stu-id="5ac73-117">The EF 6.2 runtime was released to NuGet in October of 2017.</span></span>
<span data-ttu-id="5ac73-118">在我们开源社区参与者的努力下，EF 6.2 包括大量的 [bug 修复](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug)和[产品增强功能](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20)。</span><span class="sxs-lookup"><span data-stu-id="5ac73-118">Thanks in great part to the efforts our community of open source contributors, EF 6.2 includes numerous [bugs fixes](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) and [product enhancements](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).</span></span>

<span data-ttu-id="5ac73-119">下表简要列出了影响 EF 6.2 运行时的最重要的更改：</span><span class="sxs-lookup"><span data-stu-id="5ac73-119">Here is a brief list of the most important changes affecting the EF 6.2 runtime:</span></span>

- <span data-ttu-id="5ac73-120">通过加载持久性缓存中已完成的 Code First 模型来加快启动 [#275](https://github.com/aspnet/EntityFramework6/issues/275)</span><span class="sxs-lookup"><span data-stu-id="5ac73-120">Reduce start up time by loading finished code first models from a persistent cache [#275](https://github.com/aspnet/EntityFramework6/issues/275)</span></span>
- <span data-ttu-id="5ac73-121">采用 Fluent API 定义索引 [#274](https://github.com/aspnet/EntityFramework6/issues/274)</span><span class="sxs-lookup"><span data-stu-id="5ac73-121">Fluent API to define indexes [#274](https://github.com/aspnet/EntityFramework6/issues/274)</span></span>
- <span data-ttu-id="5ac73-122">通过 DbFunctions.Like() 编写在 SQL 中转换为 LIKE 的 LINQ 查询 [#241](https://github.com/aspnet/EntityFramework6/issues/241)</span><span class="sxs-lookup"><span data-stu-id="5ac73-122">DbFunctions.Like() to enable writing LINQ queries that translate to LIKE in SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241)</span></span>
- <span data-ttu-id="5ac73-123">Migrate.exe 现支持脚本选项 [#240](https://github.com/aspnet/EntityFramework6/issues/240)</span><span class="sxs-lookup"><span data-stu-id="5ac73-123">Migrate.exe now supports -script option [#240](https://github.com/aspnet/EntityFramework6/issues/240)</span></span>
- <span data-ttu-id="5ac73-124">EF6 现在可使用 SQL Server 中的序列生成的键值 [#165](https://github.com/aspnet/EntityFramework6/issues/165)</span><span class="sxs-lookup"><span data-stu-id="5ac73-124">EF6 can now work with key values generated by a sequence in SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165)</span></span>
- <span data-ttu-id="5ac73-125">更新 SQL Azure 执行策略的暂时性错误列表 [#83](https://github.com/aspnet/EntityFramework6/issues/83)</span><span class="sxs-lookup"><span data-stu-id="5ac73-125">Update list of transient errors for SQL Azure Execution Strategy [#83](https://github.com/aspnet/EntityFramework6/issues/83)</span></span>
- <span data-ttu-id="5ac73-126">Bug：重试查询或 SQL 命令失败，“另一 SqlParameterCollection 中已包含 SqlParameter”[#81](https://github.com/aspnet/EntityFramework6/issues/81)</span><span class="sxs-lookup"><span data-stu-id="5ac73-126">Bug: Retrying queries or SQL commands fails with "The SqlParameter is already contained by another SqlParameterCollection" [#81](https://github.com/aspnet/EntityFramework6/issues/81)</span></span>
- <span data-ttu-id="5ac73-127">Bug：DbQuery.ToString() 评估在调试程序中经常超时 [#73](https://github.com/aspnet/EntityFramework6/issues/73)</span><span class="sxs-lookup"><span data-stu-id="5ac73-127">Bug: Evaluation of DbQuery.ToString() frequently times out in the debugger [#73](https://github.com/aspnet/EntityFramework6/issues/73)</span></span>

## <a name="future-releases"></a><span data-ttu-id="5ac73-128">未来将要发布的版本</span><span class="sxs-lookup"><span data-stu-id="5ac73-128">Future Releases</span></span>

<span data-ttu-id="5ac73-129">有关未来将要发布的 EF6 版本的信息，请参阅[路线图](roadmap.md)。</span><span class="sxs-lookup"><span data-stu-id="5ac73-129">For information on future version of EF6, please look at our [Roadmap](roadmap.md).</span></span>

## <a name="past-releases"></a><span data-ttu-id="5ac73-130">过去的版本</span><span class="sxs-lookup"><span data-stu-id="5ac73-130">Past Releases</span></span>

<span data-ttu-id="5ac73-131">[过去的版本](past-releases.md)页包含过去所有版本的 EF 的存档以及各版本中引入的主要功能。</span><span class="sxs-lookup"><span data-stu-id="5ac73-131">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span> 
