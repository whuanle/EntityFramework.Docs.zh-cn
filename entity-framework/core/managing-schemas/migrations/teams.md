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
<a name="migrations-in-team-environments"></a><span data-ttu-id="0cac5-102">在团队环境中的迁移</span><span class="sxs-lookup"><span data-stu-id="0cac5-102">Migrations in Team Environments</span></span>
===============================
<span data-ttu-id="0cac5-103">在团队环境中使用迁移时，需额外注意模型快照文件。</span><span class="sxs-lookup"><span data-stu-id="0cac5-103">When working with Migrations in team environments, pay extra attention to the model snapshot file.</span></span> <span data-ttu-id="0cac5-104">该文件会告诉你团队成员的迁移是否能够与你的迁移顺利合并，以及你是否需要通过重新创建迁移来解决代码冲突，然后再进行共享。</span><span class="sxs-lookup"><span data-stu-id="0cac5-104">This file can tell you if your teammate's migration merges cleanly with yours or if you need to resolve a conflict by re-creating your migration before sharing it.</span></span>

<a name="merging"></a><span data-ttu-id="0cac5-105">合并</span><span class="sxs-lookup"><span data-stu-id="0cac5-105">Merging</span></span>
-------
<span data-ttu-id="0cac5-106">合并团队成员的迁移时，可能会在模型快照文件中出现冲突。</span><span class="sxs-lookup"><span data-stu-id="0cac5-106">When you merge migrations from your teammates, you may get conflicts in your model snapshot file.</span></span> <span data-ttu-id="0cac5-107">如果两边的变更不相关联且合并规模较小，则两个迁移可以共存。</span><span class="sxs-lookup"><span data-stu-id="0cac5-107">If both changes are unrelated, the merge is trivial and the two migrations can coexist.</span></span> <span data-ttu-id="0cac5-108">例如，可能会在客户实体类型配置中出现如下所示的合并冲突：</span><span class="sxs-lookup"><span data-stu-id="0cac5-108">For example, you may get a merge conflict in the customer entity type configuration that looks like this:</span></span>

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

<span data-ttu-id="0cac5-109">由于这两个属性需要在最终模型中共存，因此请同时添加这两个属性来完成合并。</span><span class="sxs-lookup"><span data-stu-id="0cac5-109">Since both of these properties need to exist in the final model, complete the merge by adding both properties.</span></span> <span data-ttu-id="0cac5-110">在许多情况下，版本控制系统可能会自动为你合并此类更改。</span><span class="sxs-lookup"><span data-stu-id="0cac5-110">In many cases, your version control system may automatically merge such changes for you.</span></span>

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

<span data-ttu-id="0cac5-111">在此类情况下，你的迁移和团队成员的迁移是相互独立的。</span><span class="sxs-lookup"><span data-stu-id="0cac5-111">In these cases, your migration and your teammate's migration are independent of each other.</span></span> <span data-ttu-id="0cac5-112">由于可以首先应用任一迁移，因此在与团队共享你的迁移之前，无需对其进行更多的更改。</span><span class="sxs-lookup"><span data-stu-id="0cac5-112">Since either of them could be applied first, you don't need to make any additional changes to your migration before sharing it with your team.</span></span>

<a name="resolving-conflicts"></a><span data-ttu-id="0cac5-113">解决冲突</span><span class="sxs-lookup"><span data-stu-id="0cac5-113">Resolving conflicts</span></span>
-------------------
<span data-ttu-id="0cac5-114">有时会在合并模型快照文件的过程中遇到真正的冲突。</span><span class="sxs-lookup"><span data-stu-id="0cac5-114">Sometimes you encounter a true conflict when merging the model snapshot model.</span></span> <span data-ttu-id="0cac5-115">例如，你和团队成员可能重命名了同一属性。</span><span class="sxs-lookup"><span data-stu-id="0cac5-115">For example, you and your teammate may each have renamed the same property.</span></span>

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

<span data-ttu-id="0cac5-116">如果遇到这种冲突，请通过重新创建你的迁移来解决。</span><span class="sxs-lookup"><span data-stu-id="0cac5-116">If you encounter this kind of conflict, resolve it by re-creating your migration.</span></span> <span data-ttu-id="0cac5-117">请执行以下步骤：</span><span class="sxs-lookup"><span data-stu-id="0cac5-117">Follow these steps:</span></span>

1. <span data-ttu-id="0cac5-118">中止合并，回退到合并前的工作目录</span><span class="sxs-lookup"><span data-stu-id="0cac5-118">Abort the merge and rollback to your working directory before the merge</span></span>
2. <span data-ttu-id="0cac5-119">删除迁移 （但保留模型更改）</span><span class="sxs-lookup"><span data-stu-id="0cac5-119">Remove your migration (but keep your model changes)</span></span>
3. <span data-ttu-id="0cac5-120">将团队成员的更改合并到你的工作目录</span><span class="sxs-lookup"><span data-stu-id="0cac5-120">Merge your teammate's changes into your working directory</span></span>
4. <span data-ttu-id="0cac5-121">重新添加迁移</span><span class="sxs-lookup"><span data-stu-id="0cac5-121">Re-add your migration</span></span>

<span data-ttu-id="0cac5-122">完成后，可以按正确的顺序应用两个迁移。</span><span class="sxs-lookup"><span data-stu-id="0cac5-122">After doing this, the two migrations can be applied in the correct order.</span></span> <span data-ttu-id="0cac5-123">先应用团队成员的迁移，将该列重命名为 *Alias*，然后你的迁移将其重命名为 *Username*。</span><span class="sxs-lookup"><span data-stu-id="0cac5-123">Their migration is applied first, renaming the column to *Alias*, thereafter your migration renames it to *Username*.</span></span>

<span data-ttu-id="0cac5-124">你的迁移可以安全地与团队的其他人共享。</span><span class="sxs-lookup"><span data-stu-id="0cac5-124">Your migration can safely be shared with the rest of the team.</span></span>
