---
title: 在团队环境的 EF Core中迁移
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: e8ff7f468d5ab6dbd6285f1abf9199e413288d10
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997690"
---
<a name="migrations-in-team-environments"></a>在团队环境中的迁移
===============================
在团队环境中使用迁移，应额外注意模型快照文件。 如果你的队友迁移将与你完全合并或需要通过重新创建共享它之前迁移来解决冲突，此文件可以告诉您。

<a name="merging"></a>合并
-------
从你的团队成员进行合并的迁移时，可能会在你的模型快照文件中显示冲突。 如果这两个更改是不相关，合并无关紧要，但两个迁移可以共存。 例如，你可能会如下所示的客户实体类型配置中的合并冲突：

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

由于两个属性需要在最终模型中存在，请通过添加这两个属性完成合并。 在许多情况下，版本控制系统可能会自动为你合并此类更改。

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

在这些情况下，迁移和你的队友迁移是相互独立。 由于无法首先应用其中任一个，不需要对你迁移，然后才能与团队共享进行任何其他更改。

<a name="resolving-conflicts"></a>解决冲突
-------------------
有时合并模型快照模型时遇到冲突，则返回 true。 例如，你和你的队友可能每个已重命名相同的属性。

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

如果遇到这种冲突，解决通过重新创建迁移。 请执行这些步骤：

1. 中止合并转换和回滚到在合并前在工作目录
2. 删除迁移 （但保留模型更改）
3. 将你的队友更改合并到工作目录
4. 重新添加迁移

完成后，可以按正确的顺序应用两个迁移。 其迁移，首先应用重命名的列*别名*，此后您迁移将其重命名为*用户名*。

迁移可以安全地共享与团队的其余部分。
