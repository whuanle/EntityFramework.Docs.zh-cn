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
<a name="migrations-in-team-environments"></a><span data-ttu-id="69e81-102">在团队环境中的迁移</span><span class="sxs-lookup"><span data-stu-id="69e81-102">Migrations in Team Environments</span></span>
===============================
<span data-ttu-id="69e81-103">在团队环境中使用迁移，应额外注意模型快照文件。</span><span class="sxs-lookup"><span data-stu-id="69e81-103">When working with Migrations in team environments, pay extra attention to the model snapshot file.</span></span> <span data-ttu-id="69e81-104">如果你的队友迁移将与你完全合并或需要通过重新创建共享它之前迁移来解决冲突，此文件可以告诉您。</span><span class="sxs-lookup"><span data-stu-id="69e81-104">This file can tell you if your teammate's migration merges cleanly with yours or if you need to resolve a conflict by re-creating your migration before sharing it.</span></span>

<a name="merging"></a><span data-ttu-id="69e81-105">合并</span><span class="sxs-lookup"><span data-stu-id="69e81-105">Merging</span></span>
-------
<span data-ttu-id="69e81-106">从你的团队成员进行合并的迁移时，可能会在你的模型快照文件中显示冲突。</span><span class="sxs-lookup"><span data-stu-id="69e81-106">When you merge migrations from your teammates, you may get conflicts in your model snapshot file.</span></span> <span data-ttu-id="69e81-107">如果这两个更改是不相关，合并无关紧要，但两个迁移可以共存。</span><span class="sxs-lookup"><span data-stu-id="69e81-107">If both changes are unrelated, the merge is trivial and the two migrations can coexist.</span></span> <span data-ttu-id="69e81-108">例如，你可能会如下所示的客户实体类型配置中的合并冲突：</span><span class="sxs-lookup"><span data-stu-id="69e81-108">For example, you may get a merge conflict in the customer entity type configuration that looks like this:</span></span>

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

<span data-ttu-id="69e81-109">由于两个属性需要在最终模型中存在，请通过添加这两个属性完成合并。</span><span class="sxs-lookup"><span data-stu-id="69e81-109">Since both of these properties need to exist in the final model, complete the merge by adding both properties.</span></span> <span data-ttu-id="69e81-110">在许多情况下，版本控制系统可能会自动为你合并此类更改。</span><span class="sxs-lookup"><span data-stu-id="69e81-110">In many cases, your version control system may automatically merge such changes for you.</span></span>

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

<span data-ttu-id="69e81-111">在这些情况下，迁移和你的队友迁移是相互独立。</span><span class="sxs-lookup"><span data-stu-id="69e81-111">In these cases, your migration and your teammate's migration are independent of each other.</span></span> <span data-ttu-id="69e81-112">由于无法首先应用其中任一个，不需要对你迁移，然后才能与团队共享进行任何其他更改。</span><span class="sxs-lookup"><span data-stu-id="69e81-112">Since either of them could be applied first, you don't need to make any additional changes to your migration before sharing it with your team.</span></span>

<a name="resolving-conflicts"></a><span data-ttu-id="69e81-113">解决冲突</span><span class="sxs-lookup"><span data-stu-id="69e81-113">Resolving conflicts</span></span>
-------------------
<span data-ttu-id="69e81-114">有时合并模型快照模型时遇到冲突，则返回 true。</span><span class="sxs-lookup"><span data-stu-id="69e81-114">Sometimes you encounter a true conflict when merging the model snapshot model.</span></span> <span data-ttu-id="69e81-115">例如，你和你的队友可能每个已重命名相同的属性。</span><span class="sxs-lookup"><span data-stu-id="69e81-115">For example, you and your teammate may each have renamed the same property.</span></span>

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

<span data-ttu-id="69e81-116">如果遇到这种冲突，解决通过重新创建迁移。</span><span class="sxs-lookup"><span data-stu-id="69e81-116">If you encounter this kind of conflict, resolve it by re-creating your migration.</span></span> <span data-ttu-id="69e81-117">请执行这些步骤：</span><span class="sxs-lookup"><span data-stu-id="69e81-117">Follow these steps:</span></span>

1. <span data-ttu-id="69e81-118">中止合并转换和回滚到在合并前在工作目录</span><span class="sxs-lookup"><span data-stu-id="69e81-118">Abort the merge and rollback to your working directory before the merge</span></span>
2. <span data-ttu-id="69e81-119">删除迁移 （但保留模型更改）</span><span class="sxs-lookup"><span data-stu-id="69e81-119">Remove your migration (but keep your model changes)</span></span>
3. <span data-ttu-id="69e81-120">将你的队友更改合并到工作目录</span><span class="sxs-lookup"><span data-stu-id="69e81-120">Merge your teammate's changes into your working directory</span></span>
4. <span data-ttu-id="69e81-121">重新添加迁移</span><span class="sxs-lookup"><span data-stu-id="69e81-121">Re-add your migration</span></span>

<span data-ttu-id="69e81-122">完成后，可以按正确的顺序应用两个迁移。</span><span class="sxs-lookup"><span data-stu-id="69e81-122">After doing this, the two migrations can be applied in the correct order.</span></span> <span data-ttu-id="69e81-123">其迁移，首先应用重命名的列*别名*，此后您迁移将其重命名为*用户名*。</span><span class="sxs-lookup"><span data-stu-id="69e81-123">Their migration is applied first, renaming the column to *Alias*, thereafter your migration renames it to *Username*.</span></span>

<span data-ttu-id="69e81-124">迁移可以安全地共享与团队的其余部分。</span><span class="sxs-lookup"><span data-stu-id="69e81-124">Your migration can safely be shared with the rest of the team.</span></span>
