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
<a name="migrations-in-team-environments"></a><span data-ttu-id="fdefc-102">在团队环境中的迁移</span><span class="sxs-lookup"><span data-stu-id="fdefc-102">Migrations in Team Environments</span></span>
===============================
<span data-ttu-id="fdefc-103">在团队环境中使用迁移，请额外注意模型快照文件。</span><span class="sxs-lookup"><span data-stu-id="fdefc-103">When working with Migrations in team environments, pay extra attention to the model snapshot file.</span></span> <span data-ttu-id="fdefc-104">此文件会告诉您是否队友的迁移会合并完全与您的是否你需要通过重新创建你迁移共享它之前解决冲突。</span><span class="sxs-lookup"><span data-stu-id="fdefc-104">This file can tell you if your teammate's migration merges cleanly with yours of if you need to resolve a conflict by re-creating your migration before sharing it.</span></span>

<a name="merging"></a><span data-ttu-id="fdefc-105">合并</span><span class="sxs-lookup"><span data-stu-id="fdefc-105">Merging</span></span>
-------
<span data-ttu-id="fdefc-106">从你的团队成员合并迁移，则可能会模型快照文件中显示冲突。</span><span class="sxs-lookup"><span data-stu-id="fdefc-106">When you merge migrations from your teammates, you may get conflicts in your model snapshot file.</span></span> <span data-ttu-id="fdefc-107">如果这两个更改是不相关，合并为普通和两个迁移可以共存。</span><span class="sxs-lookup"><span data-stu-id="fdefc-107">If both changes are unrelated, the merge is trivial and the two migrations can coexist.</span></span> <span data-ttu-id="fdefc-108">例如，您会收到类似如下所示的客户实体类型配置中的合并冲突：</span><span class="sxs-lookup"><span data-stu-id="fdefc-108">For example, you may get a merge conflict in the customer entity type configuration that looks like this:</span></span>

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

<span data-ttu-id="fdefc-109">由于这两种属性需要在最终模型中存在，请通过添加这两个属性完成合并。</span><span class="sxs-lookup"><span data-stu-id="fdefc-109">Since both of these properties need to exist in the final model, complete the merge by adding both properties.</span></span> <span data-ttu-id="fdefc-110">在许多情况下，版本控制系统可能自动为你合并此类更改。</span><span class="sxs-lookup"><span data-stu-id="fdefc-110">In many cases, your version control system may automatically merge such changes for you.</span></span>

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

<span data-ttu-id="fdefc-111">在这些情况下，你的迁移和队友的迁移是相互独立。</span><span class="sxs-lookup"><span data-stu-id="fdefc-111">In these cases, your migration and your teammate's migration are independent of each other.</span></span> <span data-ttu-id="fdefc-112">由于其中任一个都可能首先得到应用，你不必对你的迁移之前与团队共享进行任何其他更改。</span><span class="sxs-lookup"><span data-stu-id="fdefc-112">Since either of them could be applied first, you don't need to make any additional changes to your migration before sharing it with your team.</span></span>

<a name="resolving-conflicts"></a><span data-ttu-id="fdefc-113">解决冲突</span><span class="sxs-lookup"><span data-stu-id="fdefc-113">Resolving conflicts</span></span>
-------------------
<span data-ttu-id="fdefc-114">有时你会遇到 true 冲突时合并模型快照模型。</span><span class="sxs-lookup"><span data-stu-id="fdefc-114">Sometimes you encounter a true conflict when merging the model snapshot model.</span></span> <span data-ttu-id="fdefc-115">例如，您和队友可能每个已重命名相同的属性。</span><span class="sxs-lookup"><span data-stu-id="fdefc-115">For example, you and your teammate may each have renamed the same property.</span></span>

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

<span data-ttu-id="fdefc-116">如果遇到这种类型的冲突，请重新创建迁移以解决它。</span><span class="sxs-lookup"><span data-stu-id="fdefc-116">If you encounter this kind of conflict, resolve it by re-creating your migration.</span></span> <span data-ttu-id="fdefc-117">请执行这些步骤：</span><span class="sxs-lookup"><span data-stu-id="fdefc-117">Follow these steps:</span></span>

1. <span data-ttu-id="fdefc-118">中止的合并和回滚到在合并前工作目录</span><span class="sxs-lookup"><span data-stu-id="fdefc-118">Abort the merge and rollback to your working directory before the merge</span></span>
2. <span data-ttu-id="fdefc-119">删除你的迁移 （但保留模型更改）</span><span class="sxs-lookup"><span data-stu-id="fdefc-119">Remove your migration (but keep your model changes)</span></span>
3. <span data-ttu-id="fdefc-120">将团队成员那里的更改合并到你的工作目录</span><span class="sxs-lookup"><span data-stu-id="fdefc-120">Merge your teammate's changes into your working directory</span></span>
4. <span data-ttu-id="fdefc-121">重新添加你的迁移</span><span class="sxs-lookup"><span data-stu-id="fdefc-121">Re-add your migration</span></span>

<span data-ttu-id="fdefc-122">执行此操作后, 两个迁移可以应用正确的顺序。</span><span class="sxs-lookup"><span data-stu-id="fdefc-122">After doing this, the two migrations can be applied in the correct order.</span></span> <span data-ttu-id="fdefc-123">其迁移已重命名的列的第一次，应用*别名*，此后您迁移将其重命名为*用户名*。</span><span class="sxs-lookup"><span data-stu-id="fdefc-123">Their migration is applied first, renaming the column to *Alias*, thereafter your migration renames it to *Username*.</span></span>

<span data-ttu-id="fdefc-124">你的迁移可以安全地共享与团队的其余部分。</span><span class="sxs-lookup"><span data-stu-id="fdefc-124">Your migration can safely be shared with the rest of the team.</span></span>
