---
title: "级联 Delete-EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
ms.technology: entity-framework-core
uid: core/saving/cascade-delete
ms.openlocfilehash: 1ab9d114e27aac0bec972df631a426c8ce87a518
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2018
---
# <a name="cascade-delete"></a><span data-ttu-id="a7b1d-102">级联删除</span><span class="sxs-lookup"><span data-stu-id="a7b1d-102">Cascade Delete</span></span>

<span data-ttu-id="a7b1d-103">级联删除通常在数据库术语用于描述了特性是，它允许自动触发的相关行删除某行的删除。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-103">Cascade delete is commonly used in database terminology to describe a characteristic that allows the deletion of a row to automatically trigger the deletion of related rows.</span></span> <span data-ttu-id="a7b1d-104">通过 EF 核心删除行为还介绍密切相关的概念是子实体与父级的关系时自动删除已断开-通常称为"删除孤立文件"此 i。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-104">A closely related concept also covered by EF Core delete behaviors is the automatic deletion of a child entity when it's relationship to a parent has been severed--this i commonly known as "deleting orphans".</span></span>

<span data-ttu-id="a7b1d-105">EF 核心实现几种不同的删除行为，并允许对单个关系的删除行为的配置。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-105">EF Core implements several different delete behaviors and allows for the configuration of the delete behaviors of individual relationships.</span></span> <span data-ttu-id="a7b1d-106">EF 核心还实现自动配置为基于 [requiredness 关系] 每个关系的有用的默认值删除行为的约定 (../modeling/relationships.md#required-and-optional-relationships)。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-106">EF Core also implements conventions that automatically configure useful default delete behaviors for each relationship based on the [requiredness of the relationship] (../modeling/relationships.md#required-and-optional-relationships).</span></span>

## <a name="delete-behaviors"></a><span data-ttu-id="a7b1d-107">删除行为</span><span class="sxs-lookup"><span data-stu-id="a7b1d-107">Delete behaviors</span></span>
<span data-ttu-id="a7b1d-108">删除中定义行为*DeleteBehavior*枚举器类型和可以传递给*OnDelete* fluent API 来控制是否删除主体/父实体或的断开与依赖于/子实体的关系应该对依赖于/子实体具有副作用。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-108">Delete behaviors are defined in the *DeleteBehavior* enumerator type and can be passed to the *OnDelete* fluent API to control whether the deletion of a principal/parent entity or the severing of the relationship to dependent/child entities should have a side effect on the dependent/child entities.</span></span>

<span data-ttu-id="a7b1d-109">有三个 EF 可执行删除主体/父实体或断开与子之间的关系时的操作：</span><span class="sxs-lookup"><span data-stu-id="a7b1d-109">There are three actions EF can take when a principal/parent entity is deleted or the relationship to the child is severed:</span></span>
* <span data-ttu-id="a7b1d-110">可以删除子/依赖项</span><span class="sxs-lookup"><span data-stu-id="a7b1d-110">The child/dependent can be deleted</span></span>
* <span data-ttu-id="a7b1d-111">可以设置此子级的外键值为 null</span><span class="sxs-lookup"><span data-stu-id="a7b1d-111">The child's foreign key values can be set to null</span></span>
* <span data-ttu-id="a7b1d-112">子保持不变</span><span class="sxs-lookup"><span data-stu-id="a7b1d-112">The child remains unchanged</span></span>

> [!NOTE]  
> <span data-ttu-id="a7b1d-113">使用 EF 核心删除主体实体和依赖实体加载到内存中 （即对于跟踪依赖项） 时，将仅应用在 EF 核心模型中配置删除行为。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-113">The delete behavior configured in the EF Core model is only applied when the principal entity is deleted using EF Core and the dependent entities are loaded in memory (i.e. for tracked dependents).</span></span> <span data-ttu-id="a7b1d-114">要安装程序数据库，以确保未正在由上下文跟踪的数据中具有必要的操作应用相应的级联行为要求。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-114">A corresponding cascade behavior needs to be setup in the database to ensure data that is not being tracked by the context has the necessary action applied.</span></span> <span data-ttu-id="a7b1d-115">如果你使用 EF 核心创建数据库，此 cascade 行为将为你的安装程序。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-115">If you use EF Core to create the database, this cascade behavior will be setup for you.</span></span>

<span data-ttu-id="a7b1d-116">对于上面的第二个操作，将某个外键值设置为 null 无效如果外键不可以为 null。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-116">For the second action above, setting a foreign key value to null is not valid if foreign key is not nullable.</span></span> <span data-ttu-id="a7b1d-117">（不可为 null 的外键相当于必要的关系。）在这些情况下，EF 核心跟踪调用 SaveChanges，哪些时间会引发异常，因为无法将更改保存到数据库之前，已被外键属性标记为 null。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-117">(A non-nullable foreign key is equivalent to a required relationship.) In these cases, EF Core tracks that the foreign key property has been marked as null until SaveChanges is called, at which time an exception is thrown because the change cannot be persisted to the database.</span></span> <span data-ttu-id="a7b1d-118">这是类似于从数据库获取违反了约束。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-118">This is similar to getting a constraint violation from the database.</span></span>

<span data-ttu-id="a7b1d-119">有四个删除行为，如下面的表中列出。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-119">There are four delete behaviors, as listed in the tables below.</span></span> <span data-ttu-id="a7b1d-120">可选关系 （可以为 null 的外键） 它_是_可以保存 null 外键值，这会导致以下影响：</span><span class="sxs-lookup"><span data-stu-id="a7b1d-120">For optional relationships (nullable foreign key) it _is_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="a7b1d-121">行为名称</span><span class="sxs-lookup"><span data-stu-id="a7b1d-121">Behavior Name</span></span>               | <span data-ttu-id="a7b1d-122">对内存中的相关/子的影响</span><span class="sxs-lookup"><span data-stu-id="a7b1d-122">Effect on dependent/child in memory</span></span>    | <span data-ttu-id="a7b1d-123">对依赖于/子数据库中的影响</span><span class="sxs-lookup"><span data-stu-id="a7b1d-123">Effect on dependent/child in database</span></span>  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| <span data-ttu-id="a7b1d-124">**Cascade**</span><span class="sxs-lookup"><span data-stu-id="a7b1d-124">**Cascade**</span></span>                 | <span data-ttu-id="a7b1d-125">在删除实体</span><span class="sxs-lookup"><span data-stu-id="a7b1d-125">Entities are deleted</span></span>                   | <span data-ttu-id="a7b1d-126">在删除实体</span><span class="sxs-lookup"><span data-stu-id="a7b1d-126">Entities are deleted</span></span>                   |
| <span data-ttu-id="a7b1d-127">**ClientSetNull** （默认）</span><span class="sxs-lookup"><span data-stu-id="a7b1d-127">**ClientSetNull** (Default)</span></span> | <span data-ttu-id="a7b1d-128">外键属性设置为 null</span><span class="sxs-lookup"><span data-stu-id="a7b1d-128">Foreign key properties are set to null</span></span> | <span data-ttu-id="a7b1d-129">无</span><span class="sxs-lookup"><span data-stu-id="a7b1d-129">None</span></span>                                   |
| <span data-ttu-id="a7b1d-130">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="a7b1d-130">**SetNull**</span></span>                 | <span data-ttu-id="a7b1d-131">外键属性设置为 null</span><span class="sxs-lookup"><span data-stu-id="a7b1d-131">Foreign key properties are set to null</span></span> | <span data-ttu-id="a7b1d-132">外键属性设置为 null</span><span class="sxs-lookup"><span data-stu-id="a7b1d-132">Foreign key properties are set to null</span></span> |
| <span data-ttu-id="a7b1d-133">**Restrict**</span><span class="sxs-lookup"><span data-stu-id="a7b1d-133">**Restrict**</span></span>                | <span data-ttu-id="a7b1d-134">无</span><span class="sxs-lookup"><span data-stu-id="a7b1d-134">None</span></span>                                   | <span data-ttu-id="a7b1d-135">无</span><span class="sxs-lookup"><span data-stu-id="a7b1d-135">None</span></span>                                   |

<span data-ttu-id="a7b1d-136">对于需要关系 （不可为 null 的外键），它是_不_可以保存 null 外键值，这会导致以下影响：</span><span class="sxs-lookup"><span data-stu-id="a7b1d-136">For required relationships (non-nullable foreign key) it is _not_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="a7b1d-137">行为名称</span><span class="sxs-lookup"><span data-stu-id="a7b1d-137">Behavior Name</span></span>         | <span data-ttu-id="a7b1d-138">对内存中的相关/子的影响</span><span class="sxs-lookup"><span data-stu-id="a7b1d-138">Effect on dependent/child in memory</span></span> | <span data-ttu-id="a7b1d-139">对依赖于/子数据库中的影响</span><span class="sxs-lookup"><span data-stu-id="a7b1d-139">Effect on dependent/child in database</span></span> |
|:----------------------|:------------------------------------|:--------------------------------------|
| <span data-ttu-id="a7b1d-140">**级联**（默认）</span><span class="sxs-lookup"><span data-stu-id="a7b1d-140">**Cascade** (Default)</span></span> | <span data-ttu-id="a7b1d-141">在删除实体</span><span class="sxs-lookup"><span data-stu-id="a7b1d-141">Entities are deleted</span></span>                | <span data-ttu-id="a7b1d-142">在删除实体</span><span class="sxs-lookup"><span data-stu-id="a7b1d-142">Entities are deleted</span></span>                  |
| <span data-ttu-id="a7b1d-143">**ClientSetNull**</span><span class="sxs-lookup"><span data-stu-id="a7b1d-143">**ClientSetNull**</span></span>     | <span data-ttu-id="a7b1d-144">SaveChanges 引发</span><span class="sxs-lookup"><span data-stu-id="a7b1d-144">SaveChanges throws</span></span>                  | <span data-ttu-id="a7b1d-145">无</span><span class="sxs-lookup"><span data-stu-id="a7b1d-145">None</span></span>                                  |
| <span data-ttu-id="a7b1d-146">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="a7b1d-146">**SetNull**</span></span>           | <span data-ttu-id="a7b1d-147">SaveChanges 引发</span><span class="sxs-lookup"><span data-stu-id="a7b1d-147">SaveChanges throws</span></span>                  | <span data-ttu-id="a7b1d-148">SaveChanges 引发</span><span class="sxs-lookup"><span data-stu-id="a7b1d-148">SaveChanges throws</span></span>                    |
| <span data-ttu-id="a7b1d-149">**Restrict**</span><span class="sxs-lookup"><span data-stu-id="a7b1d-149">**Restrict**</span></span>          | <span data-ttu-id="a7b1d-150">无</span><span class="sxs-lookup"><span data-stu-id="a7b1d-150">None</span></span>                                | <span data-ttu-id="a7b1d-151">无</span><span class="sxs-lookup"><span data-stu-id="a7b1d-151">None</span></span>                                  |

<span data-ttu-id="a7b1d-152">在上表中，*无*可能会导致违反了约束。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-152">In the tables above, *None* can result in a constraint violation.</span></span> <span data-ttu-id="a7b1d-153">例如，如果主体/子实体已删除，但不执行任何操作，若要更改依赖于/子的外键，则数据库将可能引发上 SaveChanges 由于外约束冲突。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-153">For example, if a principal/child entity is deleted but no action is taken to change the foreign key of a dependent/child, then the database will likely throw on SaveChanges due to a foreign constraint violation.</span></span>

<span data-ttu-id="a7b1d-154">在高级别中：</span><span class="sxs-lookup"><span data-stu-id="a7b1d-154">At a high level:</span></span>
* <span data-ttu-id="a7b1d-155">如果你有没有父级，不能存在的实体，并且你想让 EF 来处理无足轻重自动删除子级，则使用*Cascade*。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-155">If you have entities that cannot exist without a parent, and you want EF to take care for deleting the children automatically, then use *Cascade*.</span></span>
  * <span data-ttu-id="a7b1d-156">没有父通常会使不能存在的实体的所需的关系，使用为其*Cascade*是默认设置。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-156">Entities that cannot exist without a parent usually make use of required relationships, for which *Cascade* is the default.</span></span>
* <span data-ttu-id="a7b1d-157">如果你具有实体，也可能没有父级，并且你想让 EF 来处理无足轻重的 nulling 出外键，则使用*ClientSetNull*</span><span class="sxs-lookup"><span data-stu-id="a7b1d-157">If you have entities that may or may not have a parent, and you want EF to take care of nulling out the foreign key for you, then use *ClientSetNull*</span></span>
  * <span data-ttu-id="a7b1d-158">没有父通常会使可以存在的实体的可选关系，使用为其*ClientSetNull*是默认设置。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-158">Entities that can exist without a parent usually make use of optional relationships, for which *ClientSetNull* is the default.</span></span>
  * <span data-ttu-id="a7b1d-159">如果你想要还尝试甚至传播到子外键的 null 值的数据库时子实体，将不加载，然后使用*SetNull*。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-159">If you want the database to also try to propagate null values to child foreign keys even when the child entity is not loaded, then use *SetNull*.</span></span> <span data-ttu-id="a7b1d-160">但是，请注意，数据库必须支持此操作，请配置此类数据库可能会导致其他限制，这实际上通常使得此选项不切实际。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-160">However, note that the database must support this, and configuring the database like this can result in other restrictions, which in practice often makes this option impractical.</span></span> <span data-ttu-id="a7b1d-161">正因如此*SetNull*不是默认值。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-161">This is why *SetNull* is not the default.</span></span>
* <span data-ttu-id="a7b1d-162">如果你不希望 EF 核心不断自动删除实体或设置为 null 的外键自动，然后使用*限制*。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-162">If you don't want EF Core to ever delete an entity automatically or null out the foreign key automatically, then use *Restrict*.</span></span> <span data-ttu-id="a7b1d-163">请注意，这需要，你的代码使子实体和其外键值保持同步手动否则约束将会引发异常。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-163">Note that this requires that your code keep child entities and their foreign key values in sync manually otherwise constraint exceptions will be thrown.</span></span>

> [!NOTE]
> <span data-ttu-id="a7b1d-164">在 EF 核，与从 EF6，级联影响的操作未发生立即，而是仅在调用 SaveChanges 时。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-164">In EF Core, unlike EF6, cascading effects do not happen immediately, but instead only when SaveChanges is called.</span></span>

> [!NOTE]  
> <span data-ttu-id="a7b1d-165">**EF 核心 2.0 中的更改：**在以前版本中，*限制*将可选的外键属性导致跟踪依赖的实体设置为 null，并且默认值删除可选关系的行为。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-165">**Changes in EF Core 2.0:** In previous releases, *Restrict* would cause optional foreign key properties in tracked dependent entities to be set to null, and was the default delete behavior for optional relationships.</span></span> <span data-ttu-id="a7b1d-166">在 EF 核心 2.0 中， *ClientSetNull*引入了表示该行为，并成为的默认值为可选的关系。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-166">In EF Core 2.0, the *ClientSetNull* was introduced to represent that behavior and became the default for optional relationships.</span></span> <span data-ttu-id="a7b1d-167">行为*限制*已经过调整永远不会有依赖实体上的任何方面的副作用。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-167">The behavior of *Restrict* was adjusted to never have any side effects on dependent entities.</span></span>

## <a name="entity-deletion-examples"></a><span data-ttu-id="a7b1d-168">实体删除示例</span><span class="sxs-lookup"><span data-stu-id="a7b1d-168">Entity deletion examples</span></span>

<span data-ttu-id="a7b1d-169">下面的代码摘自[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/)，即可下载和运行。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-169">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="a7b1d-170">此示例演示时会发生什么情况为可选和必选的关系的每个删除行为删除父实体。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-170">The sample shows what happens for each delete behavior for both optional and required relationships when a parent entity is deleted.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

<span data-ttu-id="a7b1d-171">让我们演练一下每个变体，以了解发生了什么情况。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-171">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="a7b1d-172">具有必需或可选关系 DeleteBehavior.Cascade</span><span class="sxs-lookup"><span data-stu-id="a7b1d-172">DeleteBehavior.Cascade with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="a7b1d-173">博客标记为已删除</span><span class="sxs-lookup"><span data-stu-id="a7b1d-173">Blog is marked as Deleted</span></span>
* <span data-ttu-id="a7b1d-174">文章最初保持未更改，因为直到 SaveChanges 的级联操作未发生</span><span class="sxs-lookup"><span data-stu-id="a7b1d-174">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="a7b1d-175">SaveChanges 发送删除依赖项/子级 (post) 和然后主体/父 （博客）</span><span class="sxs-lookup"><span data-stu-id="a7b1d-175">SaveChanges sends deletes for both dependents/children (posts) and then the principal/parent (blog)</span></span>
* <span data-ttu-id="a7b1d-176">保存后，会分离所有实体，因为它们现在已从数据库删除</span><span class="sxs-lookup"><span data-stu-id="a7b1d-176">After saving, all entities are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="a7b1d-177">DeleteBehavior.ClientSetNull 或 DeleteBehavior.SetNull 与所需的关系</span><span class="sxs-lookup"><span data-stu-id="a7b1d-177">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="a7b1d-178">博客标记为已删除</span><span class="sxs-lookup"><span data-stu-id="a7b1d-178">Blog is marked as Deleted</span></span>
* <span data-ttu-id="a7b1d-179">文章最初保持未更改，因为直到 SaveChanges 的级联操作未发生</span><span class="sxs-lookup"><span data-stu-id="a7b1d-179">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="a7b1d-180">SaveChanges 尝试将 post FK 设置为 null，但这会失败，因为 FK 不可以为 null</span><span class="sxs-lookup"><span data-stu-id="a7b1d-180">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="a7b1d-181">DeleteBehavior.ClientSetNull 或 DeleteBehavior.SetNull 具有可选的关系</span><span class="sxs-lookup"><span data-stu-id="a7b1d-181">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="a7b1d-182">博客标记为已删除</span><span class="sxs-lookup"><span data-stu-id="a7b1d-182">Blog is marked as Deleted</span></span>
* <span data-ttu-id="a7b1d-183">文章最初保持未更改，因为直到 SaveChanges 的级联操作未发生</span><span class="sxs-lookup"><span data-stu-id="a7b1d-183">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="a7b1d-184">SaveChanges 尝试删除主体/父 （博客） 之前将这两个依赖项/子级 (post) FK 设置为 null</span><span class="sxs-lookup"><span data-stu-id="a7b1d-184">SaveChanges attempts sets the FK of both dependents/children (posts) to null before deleting the principal/parent (blog)</span></span>
* <span data-ttu-id="a7b1d-185">保存后，请主体/父 （博客） 被删除，但仍跟踪依赖项/子级 （文章）</span><span class="sxs-lookup"><span data-stu-id="a7b1d-185">After saving, the principal/parent (blog) is deleted, but the dependents/children (posts) are still tracked</span></span>
* <span data-ttu-id="a7b1d-186">跟踪依赖项/子级 (post) 现在具有空 FK 值和已删除的主体/父 （博客） 对其引用已删除</span><span class="sxs-lookup"><span data-stu-id="a7b1d-186">The tracked dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="a7b1d-187">具有必需或可选关系 DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="a7b1d-187">DeleteBehavior.Restrict with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="a7b1d-188">博客标记为已删除</span><span class="sxs-lookup"><span data-stu-id="a7b1d-188">Blog is marked as Deleted</span></span>
* <span data-ttu-id="a7b1d-189">文章最初保持未更改，因为直到 SaveChanges 的级联操作未发生</span><span class="sxs-lookup"><span data-stu-id="a7b1d-189">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="a7b1d-190">由于*限制*让 EF 不会自动将 FK 设置为 null，它将始终非 null 和 SaveChanges 引发而不保存</span><span class="sxs-lookup"><span data-stu-id="a7b1d-190">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="delete-orphans-examples"></a><span data-ttu-id="a7b1d-191">删除孤立项示例</span><span class="sxs-lookup"><span data-stu-id="a7b1d-191">Delete orphans examples</span></span>

<span data-ttu-id="a7b1d-192">下面的代码摘自[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/)，可以下载将运行。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-192">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded an run.</span></span> <span data-ttu-id="a7b1d-193">此示例演示时会发生什么情况为可选和必选的关系的每个删除行为父/主体及其子级/依赖项之间的关系将被切断。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-193">The sample shows what happens for each delete behavior for both optional and required relationships when the relationship between a parent/principal and its children/dependents is severed.</span></span> <span data-ttu-id="a7b1d-194">在此示例中，关系将被切断通过从主体/父 （博客） 上的集合导航属性中删除依赖项/子级 (post)。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-194">In this example, the relationship is severed by removing the dependents/children (posts) from the collection navigation property on the principal/parent (blog).</span></span> <span data-ttu-id="a7b1d-195">但是，如果此行为是相同主体/父/子依赖于从参考设为改为 null。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-195">However, the behavior is the same if the reference from dependent/child to principal/parent is instead nulled out.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

<span data-ttu-id="a7b1d-196">让我们演练一下每个变体，以了解发生了什么情况。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-196">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="a7b1d-197">具有必需或可选关系 DeleteBehavior.Cascade</span><span class="sxs-lookup"><span data-stu-id="a7b1d-197">DeleteBehavior.Cascade with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="a7b1d-198">文章标记为已修改，因为断开关系导致 FK 被标记为 null</span><span class="sxs-lookup"><span data-stu-id="a7b1d-198">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="a7b1d-199">如果 FK 不可以为 null，然后的实际值将不更改，即使它被标记为 null</span><span class="sxs-lookup"><span data-stu-id="a7b1d-199">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="a7b1d-200">SaveChanges 发送删除依赖项/子级 （文章）</span><span class="sxs-lookup"><span data-stu-id="a7b1d-200">SaveChanges sends deletes for dependents/children (posts)</span></span>
* <span data-ttu-id="a7b1d-201">保存后，将分离的依赖项/子级 （文章），因为它们现在已从数据库删除</span><span class="sxs-lookup"><span data-stu-id="a7b1d-201">After saving, the dependents/children (posts) are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="a7b1d-202">DeleteBehavior.ClientSetNull 或 DeleteBehavior.SetNull 与所需的关系</span><span class="sxs-lookup"><span data-stu-id="a7b1d-202">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="a7b1d-203">文章标记为已修改，因为断开关系导致 FK 被标记为 null</span><span class="sxs-lookup"><span data-stu-id="a7b1d-203">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="a7b1d-204">如果 FK 不可以为 null，然后的实际值将不更改，即使它被标记为 null</span><span class="sxs-lookup"><span data-stu-id="a7b1d-204">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="a7b1d-205">SaveChanges 尝试将 post FK 设置为 null，但这会失败，因为 FK 不可以为 null</span><span class="sxs-lookup"><span data-stu-id="a7b1d-205">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="a7b1d-206">DeleteBehavior.ClientSetNull 或 DeleteBehavior.SetNull 具有可选的关系</span><span class="sxs-lookup"><span data-stu-id="a7b1d-206">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="a7b1d-207">文章标记为已修改，因为断开关系导致 FK 被标记为 null</span><span class="sxs-lookup"><span data-stu-id="a7b1d-207">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="a7b1d-208">如果 FK 不可以为 null，然后的实际值将不更改，即使它被标记为 null</span><span class="sxs-lookup"><span data-stu-id="a7b1d-208">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="a7b1d-209">SaveChanges 将这两个依赖项/子级 (post) FK 设置为 null</span><span class="sxs-lookup"><span data-stu-id="a7b1d-209">SaveChanges sets the FK of both dependents/children (posts) to null</span></span>
* <span data-ttu-id="a7b1d-210">在保存后的依赖项/子级 (post) 现在具有空 FK 值和已删除的主体/父 （博客） 对其引用已删除</span><span class="sxs-lookup"><span data-stu-id="a7b1d-210">After saving, the dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="a7b1d-211">具有必需或可选关系 DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="a7b1d-211">DeleteBehavior.Restrict with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="a7b1d-212">文章标记为已修改，因为断开关系导致 FK 被标记为 null</span><span class="sxs-lookup"><span data-stu-id="a7b1d-212">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="a7b1d-213">如果 FK 不可以为 null，然后的实际值将不更改，即使它被标记为 null</span><span class="sxs-lookup"><span data-stu-id="a7b1d-213">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="a7b1d-214">由于*限制*让 EF 不会自动将 FK 设置为 null，它将始终非 null 和 SaveChanges 引发而不保存</span><span class="sxs-lookup"><span data-stu-id="a7b1d-214">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="cascading-to-untracked-entities"></a><span data-ttu-id="a7b1d-215">到未跟踪的实体级联</span><span class="sxs-lookup"><span data-stu-id="a7b1d-215">Cascading to untracked entities</span></span>

<span data-ttu-id="a7b1d-216">当调用*SaveChanges*，级联删除规则将应用于所有由上下文跟踪的实体。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-216">When you call *SaveChanges*, the cascade delete rules will be applied to any entities that are being tracked by the context.</span></span> <span data-ttu-id="a7b1d-217">这是在所有情况下上面所示的示例，这就是为什么 SQL 生成用于删除主体/父 （博客） 和所有依赖项/子级 (post):</span><span class="sxs-lookup"><span data-stu-id="a7b1d-217">This is the situation in all the examples shown above, which is why SQL was generated to delete both the principal/parent (blog) and all the dependents/children (posts):</span></span>

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="a7b1d-218">如果只有主体加载-例如，当进行查询的博客，而无需`Include(b => b.Posts)`以便也包括文章-然后 SaveChanges 将仅生成 SQL 以删除主体/父：</span><span class="sxs-lookup"><span data-stu-id="a7b1d-218">If only the principal is loaded--for example, when a query is made for a blog without an `Include(b => b.Posts)` to also include posts--then SaveChanges will only generate SQL to delete the principal/parent:</span></span>

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="a7b1d-219">如果数据库具有相应的级联行为配置，将仅删除依赖项/子级 (post)。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-219">The dependents/children (posts) will only be deleted if the database has a corresponding cascade behavior configured.</span></span> <span data-ttu-id="a7b1d-220">如果你使用 EF 创建数据库，此 cascade 行为将为你的安装程序。</span><span class="sxs-lookup"><span data-stu-id="a7b1d-220">If you use EF to create the database, this cascade behavior will be setup for you.</span></span>
