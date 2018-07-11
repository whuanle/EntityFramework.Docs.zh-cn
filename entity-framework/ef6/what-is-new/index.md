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
# <a name="whats-new-in-ef6"></a>EF6 中的新增功能

强烈建议使用最新发布的实体框架版本，以确保获得最新功能和最高稳定性。
但我们也意识到用户可能需要使用以前的版本，或想要尝试最新预发行版中的新改进。
若要安装特定版本的 EF，请参阅[获取实体框架](~/ef6/fundamentals/install.md)。

本页记录每个新版本中包含的功能。

## <a name="recent-releases"></a>最新版本

### <a name="ef-tools-update-in-visual-studio-2017-157"></a>Visual Studio 2017 15.7 中的 EF 工具更新

2018 年 5 月，我们在 Visual Studio 2017 15.7 中发布了更新后的 EF6 工具。
该版本包括对一些常见难题的改进：

- 全面检查了用户界面的辅助功能
- 解决了 SQL Server 在反向工程上性能退化的问题 [#4](https://github.com/aspnet/entityframework6/issues/4)
- 根据数据库支持更新模型，从而在 SQL Server 上获得较大模型 [#185](https://github.com/aspnet/EntityFramework6/issues/185)

在新项目中创建模型时，此新版本的 EF 工具会安装 EF 6.2 运行时。 借助较旧版本的 Visual Studio，可通过安装相应版本的 NuGet 包来使用 EF 6.2 运行时（以及以前任何版本的 EF）。

### <a name="ef-62-runtime"></a>EF 6.2 运行时

EF 6.2 运行时已于 2017 年 10 月发布到 NuGet。
在我们开源社区参与者的努力下，EF 6.2 包括大量的 [bug 修复](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug)和[产品增强功能](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20)。

下表简要列出了影响 EF 6.2 运行时的最重要的更改：

- 通过加载持久性缓存中已完成的 Code First 模型来加快启动 [#275](https://github.com/aspnet/EntityFramework6/issues/275)
- 采用 Fluent API 定义索引 [#274](https://github.com/aspnet/EntityFramework6/issues/274)
- 通过 DbFunctions.Like() 编写在 SQL 中转换为 LIKE 的 LINQ 查询 [#241](https://github.com/aspnet/EntityFramework6/issues/241)
- Migrate.exe 现支持脚本选项 [#240](https://github.com/aspnet/EntityFramework6/issues/240)
- EF6 现在可使用 SQL Server 中的序列生成的键值 [#165](https://github.com/aspnet/EntityFramework6/issues/165)
- 更新 SQL Azure 执行策略的暂时性错误列表 [#83](https://github.com/aspnet/EntityFramework6/issues/83)
- Bug：重试查询或 SQL 命令失败，“另一 SqlParameterCollection 中已包含 SqlParameter”[#81](https://github.com/aspnet/EntityFramework6/issues/81)
- Bug：DbQuery.ToString() 评估在调试程序中经常超时 [#73](https://github.com/aspnet/EntityFramework6/issues/73)

## <a name="future-releases"></a>未来将要发布的版本

有关未来将要发布的 EF6 版本的信息，请参阅[路线图](roadmap.md)。

## <a name="past-releases"></a>过去的版本

[过去的版本](past-releases.md)页包含过去所有版本的 EF 的存档以及各版本中引入的主要功能。 
