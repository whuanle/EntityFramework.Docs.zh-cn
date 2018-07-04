---
title: 在团队环境的 EF Core中迁移
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 40cbc1c1bb0273bf733fadb884bffadcceeb162b
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
ms.locfileid: "26053797"
---
<a name="migrations-in-team-environments"></a>在团队环境中的迁移
===============================
在团队环境中使用迁移，请额外注意模型快照文件。 此文件会告诉您是否队友的迁移会合并完全与您的是否你需要通过重新创建你迁移共享它之前解决冲突。

<a name="merging"></a>合并
-------
从你的团队成员合并迁移，则可能会模型快照文件中显示冲突。 如果这两个更改是不相关，合并为普通和两个迁移可以共存。 例如，您会收到类似如下所示的客户实体类型配置中的合并冲突：

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

由于这两种属性需要在最终模型中存在，请通过添加这两个属性完成合并。 在许多情况下，版本控制系统可能自动为你合并此类更改。

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

在这些情况下，你的迁移和队友的迁移是相互独立。 由于其中任一个都可能首先得到应用，你不必对你的迁移之前与团队共享进行任何其他更改。

<a name="resolving-conflicts"></a>解决冲突
-------------------
有时你会遇到 true 冲突时合并模型快照模型。 例如，您和队友可能每个已重命名相同的属性。

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

如果遇到这种类型的冲突，请重新创建迁移以解决它。 请执行这些步骤：

1. 中止的合并和回滚到在合并前工作目录
2. 删除你的迁移 （但保留模型更改）
3. 将团队成员那里的更改合并到你的工作目录
4. 重新添加你的迁移

执行此操作后, 两个迁移可以应用正确的顺序。 其迁移已重命名的列的第一次，应用*别名*，此后您迁移将其重命名为*用户名*。

你的迁移可以安全地共享与团队的其余部分。
