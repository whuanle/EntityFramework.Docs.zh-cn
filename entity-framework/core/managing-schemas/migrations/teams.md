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
在团队环境中使用迁移，请额外注意模型快照文件。 该文件将告诉你是否需要在提交代码前通过重建迁移来解决你和你队友之间的代码冲突。

<a name="merging"></a>合并
-------
当我们将队友的迁移合并过来时，在模型快照文件中会显示可能遇到的冲突。 如果两边的变更不相关联，合并规模较小，那么两个迁移可以共存。 例如，在 Customer 实体类型的配置中，我们可能遇到如下合并冲突：

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

由于这两个属性都需要被定义在最终的模型中，我们通过添加这两个属性来完成合并。 在许多情况下，版本控制系统可能会自动为你合并此类更改。

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

此时，你的迁移和你队友的迁移是相互独立的。 由于两边中的任何一边都可先应用到数据库，所以在与团队共享你的迁移之前，你无需更多操作。

<a name="resolving-conflicts"></a>解决冲突
-------------------
有时合并模型快照文件时会遇到真正的冲突。 例如，你和你的队友可能重命名了同一个属性。

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

如果遇到这种冲突，请通过重新创建你的迁移来解决。 请执行这些步骤：

1. 中止合并，并回滚工作目录到合并前的状态
2. 删除你自己的迁移 （但保留模型更改）
3. 将你队友的变更合并到工作目录
4. 重新添加你自己的迁移

完成后，可以按正确的顺序应用两个迁移。 先应用你队友的迁移，将该列重命名成*Alias*，然后你的迁移将其重命名为*Username*。

你的迁移可以安全地与团队的其他人共享。
