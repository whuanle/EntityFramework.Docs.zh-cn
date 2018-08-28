---
title: 管理数据库架构 - EF Core
author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: c1ebe33b5575cab76a54721ef86ecbcb7ff8b98b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994380"
---
# <a name="managing-database-schemas"></a>管理数据库架构
EF Core 提供两种主要方法来保持 EF Core 模型和数据库架构同步。若要二者择一，请确定真实源是 EF Core 模型还是数据库架构。

如果希望 EF Core 模型为真实源，请使用[迁移][1]。 对 EF Core 模型进行更改时，此方法会以增量方式将相应架构更改应用到数据库，以使数据库保持与 EF Core 模型兼容。

如果希望数据库架构为真实源，请使用[反向工程][2]。 使用此方法，可通过将数据库架构反向工程到 EF Core 模型来建立 DbContext 和实体类型类的基架。

> [!NOTE]
> [创建和删除 API][3] 也可从 EF Core 模型创建数据库架构。 但是，它们主要用于测试、原型制作以及可接受删除数据库的其他方案。


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
