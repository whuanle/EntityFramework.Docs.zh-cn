---
title: Entity Framework Core 路线图
author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
uid: core/what-is-new/roadmap
ms.openlocfilehash: 36d5fa8009de0aadba3b636630e1d5bddf41723c
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490826"
---
# <a name="entity-framework-core-roadmap"></a>Entity Framework Core 路线图

> [!IMPORTANT]
> 请注意，将来版本的功能集和计划会发生更改，尽管我们会尽力使此页面保持最新状态，但它可能不会始终反映我们的最新计划。

## <a name="last-release-ef-core-21"></a>上次发布的版本：EF Core 2.1

EF Core 2.1 的稳定版本已于 2018 年 5 月 30 日发布。 可在 [EF Core 2.1 中的新增功能](xref:core/what-is-new/ef-core-2.1)中查找有关此版本的详细信息。

## <a name="future-releases"></a>未来将要发布的版本

### <a name="ef-core-22"></a>EF Core 2.2

此版本将包含多个 Bug 修复和少量新功能。 有关此版本的详细信息，请参阅 [EF Core 2.2 路线图公告](https://github.com/aspnet/Announcements/issues/308)。 

### <a name="ef-core-30"></a>EF Core 3.0

对于 2.2 之后的下一个版本，虽然尚未完成[版本规划过程](#release-planning-process)，但我们目前正计划与 .NET Core 3.0 和 ASP.NET 3.0 一起发布一个主要版本。 

可以在[我们的问题跟踪程序中使用此查询](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc)来查看暂时分配给此未来版本的工作项。

## <a name="schedule"></a>计划

EF Core 的计划与 [.NET Core 计划](https://github.com/dotnet/core/blob/master/roadmap.md)以及 [ASP.NET Core 计划](https://github.com/aspnet/Home/wiki/Roadmap)同步。

## <a name="backlog"></a>积压工作 (backlog)

我们在问题跟踪程序中使用[积压工作 (backlog) 里程碑](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc)，维护问题和功能的详细列表。 客户可评论并投票。

如果我们合理地预期我们将在某个时候处理某个问题，或者社区成员可解决该问题，我们倾向于保持问题的待解决状态，但这并不意味着我们计划在特定时间范围内解决该问题，除非我们在[版本规划过程](#release-planning-process)中为其分配了特定的里程碑。

如果我们没有计划实现某项功能，可能会关闭该问题。 对于已关闭的问题，如果我们稍后获得相关新信息，可重新考虑。

所有这一切表明，我们没有足够的信息可以预测未来，我们不会说我们在 Y 时间/版本中将提供功能 X。正如在所有软件项目中，可以随时更改优先级、发布计划和可用资源等。

## <a name="release-planning-process"></a>版本规划过程

我们常常会被问到如何选择将添加到特定版本的特定功能。 积压工作 (backlog) 肯定不会自动转换成版本计划。 EF6 中功能的状态也并不意味着需要在 EF Core 中实现该功能。

此处很难详细说明版本计划所遵循的整个过程，这一方面是因为主要涉及到特定功能、机会和优先级，另一方面是因为过程会随着每个版本更改。 但是，在确定下一步工作内容时，总结要解决的常见问题相对容易：

1. **我们认为有多少开发人员会使用该功能？该功能会使他们的应用程序/体验有多大的改善？** 我们汇总了众多来源（其中包括对问题的评论和投票）的反馈。

2. **在尚未实现此功能的情况下，用户可用的变通方法是什么？** 例如，许多开发人员可以通过映射联接表格，暂时避开缺少多对多本机支持的问题。 显而易见，并非所有开发人员都能这样做，但这是一个重要方法。

3. **实现此功能是否有助于 EF Core 的体系结构发展，从而帮助我们实现其他功能？** 我们往往喜欢可充当其他功能的构建基块的功能，例如，用于自有类型的表拆分有助于实现 TPT 支持。

4. **功能是可扩展性点吗？** 我们往往喜欢可扩展性点，因为它们能让开发人员更轻松地关联自己的行为，并通过该方式获得一些缺少的功能。 我们计划添加部分功能，作为延迟加载工作的开始。

5. **该功能与其他产品结合使用时的增效作用如何？** 我们往往喜欢允许 EF 与其他产品结合使用或可显著改善使用其他产品体验的功能，如 .NET Core、最新版 Visual Studio、Microsoft Azure 等。

6. **从事功能开发工作的人员的能力如何？如何最充分地利用这些资源？** EF 团队的每个成员以及我们的社区参与者拥有不同领域的不同程度的经验，我们需要进行相应地计划。 即便我们希望“全员就位”开发特定功能，如 GroupBy 转换或多对多功能，但这是不切实际的。

正如前文所述，该过程会随着每个版本变化，将来我们希望为开发人员社区的成员提供更多的机会，为版本计划出谋划策，例如使查看功能和版本计划的建议草稿更加轻松。
