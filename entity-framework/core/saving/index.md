---
title: 保存数据 - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
ms.technology: entity-framework-core
uid: core/saving/index
ms.openlocfilehash: 97d7f1248a8d0adeed9714619c1364fa8f9822db
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949390"
---
# <a name="saving-data"></a>保存数据

每个上下文实例都有一个 `ChangeTracker`，它负责跟踪需要写入数据库的更改。 更改实体类的实例时，这些更改会记录在 `ChangeTracker` 中，然后在调用 `SaveChanges` 时被写入数据库。 此数据库提供程序负责将更改转换为特定于数据库的操作（例如，关系数据库的 `INSERT`、`UPDATE` 和 `DELETE` 命令）。
