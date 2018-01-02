---
title: "比较 EF Core 和 EF6"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 4442e6931327f6a07d98aee47211ddbeed51a467
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="compare-ef-core--ef6"></a>比较 EF Core 和 EF6

Entity Framework 有两个版本，即 Entity Framework Core 和 Entity Framework 6。

## <a name="entity-framework-6"></a>Entity Framework 6

Entity Framework 6 (EF6) 是经过反复测试的数据访问技术，其功能和稳定性已沿用多年。 它作为 .NET Framework 3.5 SP1 和 Visual Studio 2008 SP1 的一部分，于 2008 年初次发布。 从 EF4.1 版本开始，它已经作为 [EntityFramework NuGet 包](https://www.nuget.org/packages/EntityFramework/)发布 - 目前是 NuGet.org 上最受欢迎的包。

EF6 产品仍受支持，并将在未来一段时间内继续提供 bug 修复和细微改进。

## <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework Core (EF Core) 是轻量化、可扩展和跨平台版的 Entity Framework。 与 EF6 相比，EF Core 引入了众多改进和新功能。 不过，EF Core 是一个新代码基，不如 EF6 成熟。

EF Core 保留了 EF6 的开发人员体验，并且大多数顶级 API 也保持不变，因此已使用过 EF6 的开发人员会感觉 EF Core 非常熟悉。 不过，EF Core 是构建在一套全新的核心组件之上的。 这意味着 EF Core 不会自动继承 EF6 的所有功能。 其中一些功能（例如延迟加载和连接弹性）会在将来版本中提供，其他一些不太常用的功能不会在 EF Core 中提供。

凭借可扩展、轻量化的新核心，我们还可以将一些 EF6 中不提供的功能添加到 EF Core（例如 LINQ 查询中的备用键和混合客户端/数据库评估）。
