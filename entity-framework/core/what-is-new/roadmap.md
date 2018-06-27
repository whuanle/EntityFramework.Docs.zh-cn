---
title: Entity Framework Core 路线图
author: divega
ms.author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
ms.technology: entity-framework-core
uid: core/what-is-new/roadmap
ms.openlocfilehash: e23f5d7b1ff95bead310fa8e618a88c161a4e10c
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2018
ms.locfileid: "34754439"
---
# <a name="entity-framework-core-roadmap"></a><span data-ttu-id="ec2a2-102">Entity Framework Core 路线图</span><span class="sxs-lookup"><span data-stu-id="ec2a2-102">Entity Framework Core Roadmap</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ec2a2-103">请注意，将来版本的功能集和计划会发生更改，尽管我们会尽力使此页面保持最新状态，但它可能不会始终反映我们的最新计划。</span><span class="sxs-lookup"><span data-stu-id="ec2a2-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="ec2a2-104">EF Core 2.1 的稳定版本已于 2018 年 5 月 30 日发布。</span><span class="sxs-lookup"><span data-stu-id="ec2a2-104">The stable version of EF Core 2.1 was released on May 30, 2018.</span></span> <span data-ttu-id="ec2a2-105">可在 [EF Core 2.1 中的新增功能](xref:core/what-is-new/ef-core-2.1)中查找有关此版本的详细信息。</span><span class="sxs-lookup"><span data-stu-id="ec2a2-105">You can find more information about this release in [What is new in EF Core 2.1](xref:core/what-is-new/ef-core-2.1).</span></span>

<span data-ttu-id="ec2a2-106">我们尚未完成 2.1 后的下一个版本的[发布规划进程](#release-planning-process)。</span><span class="sxs-lookup"><span data-stu-id="ec2a2-106">We have not completed the [release planning process](#release-planning-process) for the next release after 2.1.</span></span>

## <a name="schedule"></a><span data-ttu-id="ec2a2-107">计划</span><span class="sxs-lookup"><span data-stu-id="ec2a2-107">Schedule</span></span>

<span data-ttu-id="ec2a2-108">EF Core 的计划与 [.NET Core 计划](https://github.com/dotnet/core/blob/master/roadmap.md)以及 [ASP.NET Core 计划](https://github.com/aspnet/Home/wiki/Roadmap)同步。</span><span class="sxs-lookup"><span data-stu-id="ec2a2-108">The schedule for EF Core is in-sync with the [.NET Core schedule](https://github.com/dotnet/core/blob/master/roadmap.md) and [ASP.NET Core schedule](https://github.com/aspnet/Home/wiki/Roadmap).</span></span>

## <a name="backlog"></a><span data-ttu-id="ec2a2-109">积压工作 (backlog)</span><span class="sxs-lookup"><span data-stu-id="ec2a2-109">Backlog</span></span>

<span data-ttu-id="ec2a2-110">我们在问题跟踪程序中使用[积压工作 (backlog) 里程碑](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc)，维护问题和功能的详细列表。</span><span class="sxs-lookup"><span data-stu-id="ec2a2-110">We use the [Backlog Milestone](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) in our issue tracker to maintain a detailed list of issues and features.</span></span> <span data-ttu-id="ec2a2-111">客户可评论并投票。</span><span class="sxs-lookup"><span data-stu-id="ec2a2-111">Customers can comment and up-vote these.</span></span>

<span data-ttu-id="ec2a2-112">如果我们合理地预期我们将在某个时候处理某个问题，或者社区成员可解决该问题，我们倾向于保持问题的待解决状态，但这并不意味着我们计划在特定时间范围内解决该问题，除非我们在[版本规划过程](#release-planning-process)中为其分配了特定的里程碑。</span><span class="sxs-lookup"><span data-stu-id="ec2a2-112">We tend to leave issues open that we reasonably expect we will work on at some point, or that someone from the community could tackle, but that does not imply the intent to resolve them in a specific timeframe until we assign them to a specific milestone as part of our [release planning process](#release-planning-process).</span></span>

<span data-ttu-id="ec2a2-113">如果我们没有计划实现某项功能，可能会关闭该问题。</span><span class="sxs-lookup"><span data-stu-id="ec2a2-113">If we do not plan to ever implement a feature, we will likely close the issue.</span></span> <span data-ttu-id="ec2a2-114">对于已关闭的问题，如果我们稍后获得相关新信息，可重新考虑。</span><span class="sxs-lookup"><span data-stu-id="ec2a2-114">An issue that we closed can be reconsidered at a later point if we obtain new information about it.</span></span>

<span data-ttu-id="ec2a2-115">所有这一切表明，我们没有足够的信息可以预测未来，我们不会说我们在 Y 时间/版本中将提供功能 X。正如在所有软件项目中，可以随时更改优先级、发布计划和可用资源等。</span><span class="sxs-lookup"><span data-stu-id="ec2a2-115">All that said, we don’t have enough information about the future to be able to say that feature X will be resolved by time/release Y. As in all software projects, priorities, release schedules, and available resources can change at any point.</span></span>

## <a name="release-planning-process"></a><span data-ttu-id="ec2a2-116">版本规划过程</span><span class="sxs-lookup"><span data-stu-id="ec2a2-116">Release planning process</span></span>

<span data-ttu-id="ec2a2-117">我们常常会被问到如何选择将添加到特定版本的特定功能。</span><span class="sxs-lookup"><span data-stu-id="ec2a2-117">We often get questions about how we choose specific features to go into a particular release.</span></span> <span data-ttu-id="ec2a2-118">积压工作 (backlog) 肯定不会自动转换成版本计划。</span><span class="sxs-lookup"><span data-stu-id="ec2a2-118">Our backlog certainly does not automatically translate into release plans.</span></span> <span data-ttu-id="ec2a2-119">EF6 中功能的状态也并不意味着需要在 EF Core 中实现该功能。</span><span class="sxs-lookup"><span data-stu-id="ec2a2-119">The presence of a feature in EF6 also does not automatically mean that the feature needs to be implemented in EF Core.</span></span>

<span data-ttu-id="ec2a2-120">此处很难详细说明版本计划所遵循的整个过程，这一方面是因为主要涉及到特定功能、机会和优先级，另一方面是因为过程会随着每个版本更改。</span><span class="sxs-lookup"><span data-stu-id="ec2a2-120">It is difficult to detail here the whole process we follow to plan a release, partly because a lot of it is discussing the specific features, opportunities and priorities, and partly because the process itself usually evolves with every release.</span></span> <span data-ttu-id="ec2a2-121">但是，在确定下一步工作内容时，总结要解决的常见问题相对容易：</span><span class="sxs-lookup"><span data-stu-id="ec2a2-121">However, it is relatively easy to summarize the common questions we try to answer when deciding what to work on next:</span></span>

1. <span data-ttu-id="ec2a2-122">**我们认为有多少开发人员会使用该功能？该功能会使他们的应用程序/体验有多大的改善？**</span><span class="sxs-lookup"><span data-stu-id="ec2a2-122">**How many developers we think will use the feature and how much better will it make their applications/experience?**</span></span> <span data-ttu-id="ec2a2-123">我们汇总了众多来源（其中包括对问题的评论和投票）的反馈。</span><span class="sxs-lookup"><span data-stu-id="ec2a2-123">We aggregate feedback from many sources into this — Comments and votes on issues is one of those sources.</span></span>

2. <span data-ttu-id="ec2a2-124">**在尚未实现此功能的情况下，用户可用的变通方法是什么？**</span><span class="sxs-lookup"><span data-stu-id="ec2a2-124">**What are the workarounds people can use if we don’t implement this feature yet?**</span></span> <span data-ttu-id="ec2a2-125">例如，许多开发人员可以通过映射联接表格，暂时避开缺少多对多本机支持的问题。</span><span class="sxs-lookup"><span data-stu-id="ec2a2-125">For example, many developers are able to map a join table in order to work around lack of native many-to-many support.</span></span> <span data-ttu-id="ec2a2-126">显而易见，并非所有开发人员都能这样做，但这是一个重要方法。</span><span class="sxs-lookup"><span data-stu-id="ec2a2-126">Obviously, not all developers can do this, but many can, and this is a factor which counts.</span></span>

3. <span data-ttu-id="ec2a2-127">**实现此功能是否有助于 EF Core 的体系结构发展，从而帮助我们实现其他功能？**</span><span class="sxs-lookup"><span data-stu-id="ec2a2-127">**Does implementing this feature evolve the architecture of EF Core such that it moves us closer to implementing other features?**</span></span> <span data-ttu-id="ec2a2-128">我们往往喜欢可充当其他功能的构建基块的功能，例如，用于自有类型的表拆分有助于实现 TPT 支持。</span><span class="sxs-lookup"><span data-stu-id="ec2a2-128">We tend to favor features that act as building blocks for other features—for example, the table splitting that was done for owned types helps us move towards TPT support.</span></span>

4. <span data-ttu-id="ec2a2-129">**功能是可扩展性点吗？**</span><span class="sxs-lookup"><span data-stu-id="ec2a2-129">**Is the feature an extensibility point?**</span></span> <span data-ttu-id="ec2a2-130">我们往往喜欢可扩展性点，因为它们能让开发人员更轻松地关联自己的行为，并通过该方式获得一些缺少的功能。</span><span class="sxs-lookup"><span data-stu-id="ec2a2-130">We tend to favor extensibility points because they enable developers to more easily hook in their own behaviors and get some of the missing functionality that way.</span></span> <span data-ttu-id="ec2a2-131">我们计划添加部分功能，作为延迟加载工作的开始。</span><span class="sxs-lookup"><span data-stu-id="ec2a2-131">We’re planning to do some of this as a start to the lazy loading work.</span></span>

5. <span data-ttu-id="ec2a2-132">**该功能与其他产品结合使用时的增效作用如何？**</span><span class="sxs-lookup"><span data-stu-id="ec2a2-132">**What is the synergy of the feature when used in combination with other products?**</span></span> <span data-ttu-id="ec2a2-133">我们往往喜欢允许 EF 与其他产品结合使用或可显著改善使用其他产品体验的功能，如 .NET Core、最新版 Visual Studio、Microsoft Azure 等。</span><span class="sxs-lookup"><span data-stu-id="ec2a2-133">We tend to favor features that allow EF Core to be used with other products or to significantly improve the experience of using other products, such as .NET Core, the latest version of Visual Studio, Microsoft Azure, etc.</span></span>

6. <span data-ttu-id="ec2a2-134">**从事功能开发工作的人员的能力如何？如何最充分地利用这些资源？**</span><span class="sxs-lookup"><span data-stu-id="ec2a2-134">**What are the capabilities of the people available to work on a feature, and how to best leverage these resources?**</span></span> <span data-ttu-id="ec2a2-135">EF 团队的每个成员以及我们的社区参与者拥有不同领域的不同程度的经验，我们需要进行相应地计划。</span><span class="sxs-lookup"><span data-stu-id="ec2a2-135">Each member of the EF team and even our community contributors have different levels of experience in different areas and we have to plan accordingly.</span></span> <span data-ttu-id="ec2a2-136">即便我们希望“全员就位”开发特定功能，如 GroupBy 转换或多对多功能，但这是不切实际的。</span><span class="sxs-lookup"><span data-stu-id="ec2a2-136">Even if we wanted to have “all hands on deck” to work on a specific feature like GroupBy translations, or many-to-many, that wouldn’t be practical.</span></span>

<span data-ttu-id="ec2a2-137">正如前文所述，这个过程会随着每个版本变化，将来我们希望为开发人员社区的成员提供更多的机会，为版本计划出谋划策，例如，使查看功能和版本计划的建议草稿更加轻松。</span><span class="sxs-lookup"><span data-stu-id="ec2a2-137">As mentioned before, this process evolves on every release, and in the future we would like to add more opportunities for members of the developer community to provide inputs into release plans, e.g. by making it easier to review proposed drafts of the features and of the release plan itself.</span></span>
