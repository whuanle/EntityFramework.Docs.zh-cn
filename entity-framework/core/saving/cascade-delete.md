---
title: 级联删除 - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
ms.technology: entity-framework-core
uid: core/saving/cascade-delete
ms.openlocfilehash: 0fc8929c56d4c657b7fb1e3c8e4b1a71659220c9
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812672"
---
# <a name="cascade-delete"></a><span data-ttu-id="50035-102">级联删除</span><span class="sxs-lookup"><span data-stu-id="50035-102">Cascade Delete</span></span>

<span data-ttu-id="50035-103">级联删除通常在数据库术语中使用，用于描述允许删除某行以自动触发删除相关行的特性。</span><span class="sxs-lookup"><span data-stu-id="50035-103">Cascade delete is commonly used in database terminology to describe a characteristic that allows the deletion of a row to automatically trigger the deletion of related rows.</span></span> <span data-ttu-id="50035-104">EF Core 删除行为还介绍了一个密切相关的概念，即子实体与父实体的关系已断开时自动删除该子实体，这通常称为“删除孤立项”。</span><span class="sxs-lookup"><span data-stu-id="50035-104">A closely related concept also covered by EF Core delete behaviors is the automatic deletion of a child entity when it's relationship to a parent has been severed--this is commonly known as "deleting orphans".</span></span>

<span data-ttu-id="50035-105">EF Core 实现多种不同的删除行为，并允许配置各个关系的删除行为。</span><span class="sxs-lookup"><span data-stu-id="50035-105">EF Core implements several different delete behaviors and allows for the configuration of the delete behaviors of individual relationships.</span></span> <span data-ttu-id="50035-106">EF Core 还实现基于[关系的需求](../modeling/relationships.md#required-and-optional-relationships)为每个关系自动配置有用的默认删除行为的约定。</span><span class="sxs-lookup"><span data-stu-id="50035-106">EF Core also implements conventions that automatically configure useful default delete behaviors for each relationship based on the [requiredness of the relationship](../modeling/relationships.md#required-and-optional-relationships).</span></span>

## <a name="delete-behaviors"></a><span data-ttu-id="50035-107">删除行为</span><span class="sxs-lookup"><span data-stu-id="50035-107">Delete behaviors</span></span>
<span data-ttu-id="50035-108">删除行为在 *DeleteBehavior* 枚举器类型中定义，并且可以传递到 *OnDelete* Fluent API 来控制是主体/父实体的删除还是依赖实体/子实体关系的断开会对依赖实体/子实体产生副作用。</span><span class="sxs-lookup"><span data-stu-id="50035-108">Delete behaviors are defined in the *DeleteBehavior* enumerator type and can be passed to the *OnDelete* fluent API to control whether the deletion of a principal/parent entity or the severing of the relationship to dependent/child entities should have a side effect on the dependent/child entities.</span></span>

<span data-ttu-id="50035-109">删除主体/父实体或断开与子实体的关系时有三个 EF 可执行的操作：</span><span class="sxs-lookup"><span data-stu-id="50035-109">There are three actions EF can take when a principal/parent entity is deleted or the relationship to the child is severed:</span></span>
* <span data-ttu-id="50035-110">可以删除子项/依赖项</span><span class="sxs-lookup"><span data-stu-id="50035-110">The child/dependent can be deleted</span></span>
* <span data-ttu-id="50035-111">子项的外键值可以设置为 null</span><span class="sxs-lookup"><span data-stu-id="50035-111">The child's foreign key values can be set to null</span></span>
* <span data-ttu-id="50035-112">子项保持不变</span><span class="sxs-lookup"><span data-stu-id="50035-112">The child remains unchanged</span></span>

> [!NOTE]  
> <span data-ttu-id="50035-113">仅当使用 EF Core 删除主体且将依赖实体加载到内存中（即对于跟踪的依赖项）时才应用 EF Core 模型中配置的删除行为。</span><span class="sxs-lookup"><span data-stu-id="50035-113">The delete behavior configured in the EF Core model is only applied when the principal entity is deleted using EF Core and the dependent entities are loaded in memory (i.e. for tracked dependents).</span></span> <span data-ttu-id="50035-114">需要在数据库中设置相应的级联行为以确保未由上下文跟踪的数据已应用必要的操作。</span><span class="sxs-lookup"><span data-stu-id="50035-114">A corresponding cascade behavior needs to be setup in the database to ensure data that is not being tracked by the context has the necessary action applied.</span></span> <span data-ttu-id="50035-115">如果使用 EF Core 创建数据库，将为你设置此级联行为。</span><span class="sxs-lookup"><span data-stu-id="50035-115">If you use EF Core to create the database, this cascade behavior will be setup for you.</span></span>

<span data-ttu-id="50035-116">对于上面的第二个操作，如果外键不可以为 null，则将某个外键值设置为 null 无效。</span><span class="sxs-lookup"><span data-stu-id="50035-116">For the second action above, setting a foreign key value to null is not valid if foreign key is not nullable.</span></span> <span data-ttu-id="50035-117">（不可为 null 的外键相当于必选关系。）在这些情况下，EF Core 会跟踪到外键属性已标记为 null，直到调用 SaveChanges，由于无法将更改永久保存到数据库，因此会在此时引发异常。</span><span class="sxs-lookup"><span data-stu-id="50035-117">(A non-nullable foreign key is equivalent to a required relationship.) In these cases, EF Core tracks that the foreign key property has been marked as null until SaveChanges is called, at which time an exception is thrown because the change cannot be persisted to the database.</span></span> <span data-ttu-id="50035-118">这类似于从数据库中获取约束冲突。</span><span class="sxs-lookup"><span data-stu-id="50035-118">This is similar to getting a constraint violation from the database.</span></span>

<span data-ttu-id="50035-119">有四个删除行为，如下表中列出。</span><span class="sxs-lookup"><span data-stu-id="50035-119">There are four delete behaviors, as listed in the tables below.</span></span> <span data-ttu-id="50035-120">对于可选关系（可以为 null 的外键），_可以_保存 null 外键值，从而产生以下影响：</span><span class="sxs-lookup"><span data-stu-id="50035-120">For optional relationships (nullable foreign key) it _is_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="50035-121">行为名称</span><span class="sxs-lookup"><span data-stu-id="50035-121">Behavior Name</span></span>               | <span data-ttu-id="50035-122">对内存中的依赖项/子项的影响</span><span class="sxs-lookup"><span data-stu-id="50035-122">Effect on dependent/child in memory</span></span>    | <span data-ttu-id="50035-123">对数据库中的依赖项/子项的影响</span><span class="sxs-lookup"><span data-stu-id="50035-123">Effect on dependent/child in database</span></span>  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| <span data-ttu-id="50035-124">**Cascade**</span><span class="sxs-lookup"><span data-stu-id="50035-124">**Cascade**</span></span>                 | <span data-ttu-id="50035-125">删除实体</span><span class="sxs-lookup"><span data-stu-id="50035-125">Entities are deleted</span></span>                   | <span data-ttu-id="50035-126">删除实体</span><span class="sxs-lookup"><span data-stu-id="50035-126">Entities are deleted</span></span>                   |
| <span data-ttu-id="50035-127">**ClientSetNull**（默认）</span><span class="sxs-lookup"><span data-stu-id="50035-127">**ClientSetNull** (Default)</span></span> | <span data-ttu-id="50035-128">外键属性设置为 null</span><span class="sxs-lookup"><span data-stu-id="50035-128">Foreign key properties are set to null</span></span> | <span data-ttu-id="50035-129">无</span><span class="sxs-lookup"><span data-stu-id="50035-129">None</span></span>                                   |
| <span data-ttu-id="50035-130">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="50035-130">**SetNull**</span></span>                 | <span data-ttu-id="50035-131">外键属性设置为 null</span><span class="sxs-lookup"><span data-stu-id="50035-131">Foreign key properties are set to null</span></span> | <span data-ttu-id="50035-132">外键属性设置为 null</span><span class="sxs-lookup"><span data-stu-id="50035-132">Foreign key properties are set to null</span></span> |
| <span data-ttu-id="50035-133">**Restrict**</span><span class="sxs-lookup"><span data-stu-id="50035-133">**Restrict**</span></span>                | <span data-ttu-id="50035-134">无</span><span class="sxs-lookup"><span data-stu-id="50035-134">None</span></span>                                   | <span data-ttu-id="50035-135">无</span><span class="sxs-lookup"><span data-stu-id="50035-135">None</span></span>                                   |

<span data-ttu-id="50035-136">对于必选关系（不可为 null 的外键），_不可以_保存 null 外键值，从而产生以下影响：</span><span class="sxs-lookup"><span data-stu-id="50035-136">For required relationships (non-nullable foreign key) it is _not_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="50035-137">行为名称</span><span class="sxs-lookup"><span data-stu-id="50035-137">Behavior Name</span></span>         | <span data-ttu-id="50035-138">对内存中的依赖项/子项的影响</span><span class="sxs-lookup"><span data-stu-id="50035-138">Effect on dependent/child in memory</span></span> | <span data-ttu-id="50035-139">对数据库中的依赖项/子项的影响</span><span class="sxs-lookup"><span data-stu-id="50035-139">Effect on dependent/child in database</span></span> |
|:----------------------|:------------------------------------|:--------------------------------------|
| <span data-ttu-id="50035-140">**Cascade**（默认）</span><span class="sxs-lookup"><span data-stu-id="50035-140">**Cascade** (Default)</span></span> | <span data-ttu-id="50035-141">删除实体</span><span class="sxs-lookup"><span data-stu-id="50035-141">Entities are deleted</span></span>                | <span data-ttu-id="50035-142">删除实体</span><span class="sxs-lookup"><span data-stu-id="50035-142">Entities are deleted</span></span>                  |
| <span data-ttu-id="50035-143">**ClientSetNull**</span><span class="sxs-lookup"><span data-stu-id="50035-143">**ClientSetNull**</span></span>     | <span data-ttu-id="50035-144">引发 SaveChanges</span><span class="sxs-lookup"><span data-stu-id="50035-144">SaveChanges throws</span></span>                  | <span data-ttu-id="50035-145">无</span><span class="sxs-lookup"><span data-stu-id="50035-145">None</span></span>                                  |
| <span data-ttu-id="50035-146">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="50035-146">**SetNull**</span></span>           | <span data-ttu-id="50035-147">引发 SaveChanges</span><span class="sxs-lookup"><span data-stu-id="50035-147">SaveChanges throws</span></span>                  | <span data-ttu-id="50035-148">引发 SaveChanges</span><span class="sxs-lookup"><span data-stu-id="50035-148">SaveChanges throws</span></span>                    |
| <span data-ttu-id="50035-149">**Restrict**</span><span class="sxs-lookup"><span data-stu-id="50035-149">**Restrict**</span></span>          | <span data-ttu-id="50035-150">无</span><span class="sxs-lookup"><span data-stu-id="50035-150">None</span></span>                                | <span data-ttu-id="50035-151">无</span><span class="sxs-lookup"><span data-stu-id="50035-151">None</span></span>                                  |

<span data-ttu-id="50035-152">在上表中，“无”可能会造成约束冲突。</span><span class="sxs-lookup"><span data-stu-id="50035-152">In the tables above, *None* can result in a constraint violation.</span></span> <span data-ttu-id="50035-153">例如，如果已删除主体/子实体，但不执行任何操作来更改依赖项/子项的外键，则由于发生外键约束冲突，数据库将可能会引发 SaveChanges。</span><span class="sxs-lookup"><span data-stu-id="50035-153">For example, if a principal/child entity is deleted but no action is taken to change the foreign key of a dependent/child, then the database will likely throw on SaveChanges due to a foreign constraint violation.</span></span>

<span data-ttu-id="50035-154">高级别：</span><span class="sxs-lookup"><span data-stu-id="50035-154">At a high level:</span></span>
* <span data-ttu-id="50035-155">如果实体在没有父项时不能存在，且希望 EF 负责自动删除子项，则使用“Cascade”。</span><span class="sxs-lookup"><span data-stu-id="50035-155">If you have entities that cannot exist without a parent, and you want EF to take care for deleting the children automatically, then use *Cascade*.</span></span>
  * <span data-ttu-id="50035-156">在没有父项时不能存在的实体通常使用必选关系，其中“Cascade”是默认值。</span><span class="sxs-lookup"><span data-stu-id="50035-156">Entities that cannot exist without a parent usually make use of required relationships, for which *Cascade* is the default.</span></span>
* <span data-ttu-id="50035-157">如果实体可能有或可能没有父项，且希望 EF 负责为你将外键变为 null，则使用“ClientSetNull”</span><span class="sxs-lookup"><span data-stu-id="50035-157">If you have entities that may or may not have a parent, and you want EF to take care of nulling out the foreign key for you, then use *ClientSetNull*</span></span>
  * <span data-ttu-id="50035-158">在没有父项时可以存在的实体通常使用可选关系，其中“ClientSetNull”是默认值。</span><span class="sxs-lookup"><span data-stu-id="50035-158">Entities that can exist without a parent usually make use of optional relationships, for which *ClientSetNull* is the default.</span></span>
  * <span data-ttu-id="50035-159">如果希望数据库即使在未加载子实体时也尝试将 null 值传播到子外键，则使用“SetNull”。</span><span class="sxs-lookup"><span data-stu-id="50035-159">If you want the database to also try to propagate null values to child foreign keys even when the child entity is not loaded, then use *SetNull*.</span></span> <span data-ttu-id="50035-160">但是，请注意，数据库必须支持该值，配置此类数据库可能会导致其他限制，这实际上通常使得此选项不切实际。</span><span class="sxs-lookup"><span data-stu-id="50035-160">However, note that the database must support this, and configuring the database like this can result in other restrictions, which in practice often makes this option impractical.</span></span> <span data-ttu-id="50035-161">这是“SetNull”不是默认值的原因。</span><span class="sxs-lookup"><span data-stu-id="50035-161">This is why *SetNull* is not the default.</span></span>
* <span data-ttu-id="50035-162">如果不希望 EF Core 始终自动删除实体或自动将外键变为 null，则使用“Restrict”。</span><span class="sxs-lookup"><span data-stu-id="50035-162">If you don't want EF Core to ever delete an entity automatically or null out the foreign key automatically, then use *Restrict*.</span></span> <span data-ttu-id="50035-163">请注意，这要求使用代码手动同步子实体及其外键值，否则将引发约束异常。</span><span class="sxs-lookup"><span data-stu-id="50035-163">Note that this requires that your code keep child entities and their foreign key values in sync manually otherwise constraint exceptions will be thrown.</span></span>

> [!NOTE]
> <span data-ttu-id="50035-164">在 EF Core（与 EF6 不同）中，不会立即产生级联影响，而是仅在调用 SaveChanges 时产生。</span><span class="sxs-lookup"><span data-stu-id="50035-164">In EF Core, unlike EF6, cascading effects do not happen immediately, but instead only when SaveChanges is called.</span></span>

> [!NOTE]  
> <span data-ttu-id="50035-165">**EF Core 2.0 中的更改：** 在以前版本中，“Restrict”将导致跟踪的依赖实体中的可选外键属性设置为 null，并且是可选关系的默认删除行为。</span><span class="sxs-lookup"><span data-stu-id="50035-165">**Changes in EF Core 2.0:** In previous releases, *Restrict* would cause optional foreign key properties in tracked dependent entities to be set to null, and was the default delete behavior for optional relationships.</span></span> <span data-ttu-id="50035-166">在 EF Core 2.0 中，引入了“ClientSetNull”以表示该行为，并且它会成为可选关系的默认值。</span><span class="sxs-lookup"><span data-stu-id="50035-166">In EF Core 2.0, the *ClientSetNull* was introduced to represent that behavior and became the default for optional relationships.</span></span> <span data-ttu-id="50035-167">“Restrict”的行为已调整为永远不会对依赖实体产生副作用。</span><span class="sxs-lookup"><span data-stu-id="50035-167">The behavior of *Restrict* was adjusted to never have any side effects on dependent entities.</span></span>

## <a name="entity-deletion-examples"></a><span data-ttu-id="50035-168">实体删除示例</span><span class="sxs-lookup"><span data-stu-id="50035-168">Entity deletion examples</span></span>

<span data-ttu-id="50035-169">以下代码是可下载并运行的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/)的一部分。</span><span class="sxs-lookup"><span data-stu-id="50035-169">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="50035-170">此示例显示了，当删除父实体时，可选关系和必选关系的每个删除行为会发生的情况。</span><span class="sxs-lookup"><span data-stu-id="50035-170">The sample shows what happens for each delete behavior for both optional and required relationships when a parent entity is deleted.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

<span data-ttu-id="50035-171">我们来看一看每个变化以了解所发生的情况。</span><span class="sxs-lookup"><span data-stu-id="50035-171">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="50035-172">具有必选或可选关系的 DeleteBehavior.Cascade</span><span class="sxs-lookup"><span data-stu-id="50035-172">DeleteBehavior.Cascade with required or optional relationship</span></span>

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

* <span data-ttu-id="50035-173">博客标记为已删除</span><span class="sxs-lookup"><span data-stu-id="50035-173">Blog is marked as Deleted</span></span>
* <span data-ttu-id="50035-174">文章最初保持不变，因为在调用 SaveChanges 之前不会发生级联</span><span class="sxs-lookup"><span data-stu-id="50035-174">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="50035-175">SaveChanges 发送对依赖项/子项（文章）和主体/父项（博客）的删除</span><span class="sxs-lookup"><span data-stu-id="50035-175">SaveChanges sends deletes for both dependents/children (posts) and then the principal/parent (blog)</span></span>
* <span data-ttu-id="50035-176">保存后，所有实体都会分离，因为它们现在已从数据库中删除</span><span class="sxs-lookup"><span data-stu-id="50035-176">After saving, all entities are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="50035-177">具有必选关系的 DeleteBehavior.ClientSetNull 或 DeleteBehavior.SetNull</span><span class="sxs-lookup"><span data-stu-id="50035-177">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

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

* <span data-ttu-id="50035-178">博客标记为已删除</span><span class="sxs-lookup"><span data-stu-id="50035-178">Blog is marked as Deleted</span></span>
* <span data-ttu-id="50035-179">文章最初保持不变，因为在调用 SaveChanges 之前不会发生级联</span><span class="sxs-lookup"><span data-stu-id="50035-179">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="50035-180">SaveChanges 尝试将文章外键设置为 null，但会失败，因为外键不可以为 null</span><span class="sxs-lookup"><span data-stu-id="50035-180">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="50035-181">具有可选关系的 DeleteBehavior.ClientSetNull 或 DeleteBehavior.SetNull</span><span class="sxs-lookup"><span data-stu-id="50035-181">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

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

* <span data-ttu-id="50035-182">博客标记为已删除</span><span class="sxs-lookup"><span data-stu-id="50035-182">Blog is marked as Deleted</span></span>
* <span data-ttu-id="50035-183">文章最初保持不变，因为在调用 SaveChanges 之前不会发生级联</span><span class="sxs-lookup"><span data-stu-id="50035-183">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="50035-184">SaveChanges 尝试在删除主体/父项（博客）之前将依赖项/子项（文章）的外键设置为 null</span><span class="sxs-lookup"><span data-stu-id="50035-184">SaveChanges attempts sets the FK of both dependents/children (posts) to null before deleting the principal/parent (blog)</span></span>
* <span data-ttu-id="50035-185">保存后，将删除主体/父项（博客），但仍会跟踪依赖项/子项（文章）</span><span class="sxs-lookup"><span data-stu-id="50035-185">After saving, the principal/parent (blog) is deleted, but the dependents/children (posts) are still tracked</span></span>
* <span data-ttu-id="50035-186">跟踪的依赖项/子项（文章）现在具有 null 外键值，并且对删除的主体/父项（博客）的引用已删除</span><span class="sxs-lookup"><span data-stu-id="50035-186">The tracked dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="50035-187">具有必选或可选关系的 DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="50035-187">DeleteBehavior.Restrict with required or optional relationship</span></span>

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

* <span data-ttu-id="50035-188">博客标记为已删除</span><span class="sxs-lookup"><span data-stu-id="50035-188">Blog is marked as Deleted</span></span>
* <span data-ttu-id="50035-189">文章最初保持不变，因为在调用 SaveChanges 之前不会发生级联</span><span class="sxs-lookup"><span data-stu-id="50035-189">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="50035-190">由于“Restrict”指示 EF 不要自动将外键设置为 null，因此它保留非 null，SaveChanges 引发异常且不保存</span><span class="sxs-lookup"><span data-stu-id="50035-190">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="delete-orphans-examples"></a><span data-ttu-id="50035-191">删除孤立项示例</span><span class="sxs-lookup"><span data-stu-id="50035-191">Delete orphans examples</span></span>

<span data-ttu-id="50035-192">以下代码是可下载并运行的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/)的一部分。</span><span class="sxs-lookup"><span data-stu-id="50035-192">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded an run.</span></span> <span data-ttu-id="50035-193">此示例显示了，当断开父项/主体及其子项/依赖项之间的关系时，可选关系和必选关系的每个删除行为会发生的情况。</span><span class="sxs-lookup"><span data-stu-id="50035-193">The sample shows what happens for each delete behavior for both optional and required relationships when the relationship between a parent/principal and its children/dependents is severed.</span></span> <span data-ttu-id="50035-194">在此示例中，通过从主体/父项（博客）上的集合导航属性中删除依赖项/子项（文章）来断开关系。</span><span class="sxs-lookup"><span data-stu-id="50035-194">In this example, the relationship is severed by removing the dependents/children (posts) from the collection navigation property on the principal/parent (blog).</span></span> <span data-ttu-id="50035-195">但是，如果将从依赖项/子项到主体/父项的引用变为 null，则行为相同。</span><span class="sxs-lookup"><span data-stu-id="50035-195">However, the behavior is the same if the reference from dependent/child to principal/parent is instead nulled out.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

<span data-ttu-id="50035-196">我们来看一看每个变化以了解所发生的情况。</span><span class="sxs-lookup"><span data-stu-id="50035-196">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="50035-197">具有必选或可选关系的 DeleteBehavior.Cascade</span><span class="sxs-lookup"><span data-stu-id="50035-197">DeleteBehavior.Cascade with required or optional relationship</span></span>

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

* <span data-ttu-id="50035-198">文章标记为已修改，因为断开关系导致外键标记为 null</span><span class="sxs-lookup"><span data-stu-id="50035-198">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="50035-199">如果外键不可以为 null，则即使实际值标记为 null 也不会更改</span><span class="sxs-lookup"><span data-stu-id="50035-199">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="50035-200">SaveChanges 发送对依赖项/子项（文章）的删除</span><span class="sxs-lookup"><span data-stu-id="50035-200">SaveChanges sends deletes for dependents/children (posts)</span></span>
* <span data-ttu-id="50035-201">保存后，依赖项/子项（文章）会分离，因为它们现在已从数据库中删除</span><span class="sxs-lookup"><span data-stu-id="50035-201">After saving, the dependents/children (posts) are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="50035-202">具有必选关系的 DeleteBehavior.ClientSetNull 或 DeleteBehavior.SetNull</span><span class="sxs-lookup"><span data-stu-id="50035-202">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

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

* <span data-ttu-id="50035-203">文章标记为已修改，因为断开关系导致外键标记为 null</span><span class="sxs-lookup"><span data-stu-id="50035-203">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="50035-204">如果外键不可以为 null，则即使实际值标记为 null 也不会更改</span><span class="sxs-lookup"><span data-stu-id="50035-204">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="50035-205">SaveChanges 尝试将文章外键设置为 null，但会失败，因为外键不可以为 null</span><span class="sxs-lookup"><span data-stu-id="50035-205">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="50035-206">具有可选关系的 DeleteBehavior.ClientSetNull 或 DeleteBehavior.SetNull</span><span class="sxs-lookup"><span data-stu-id="50035-206">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

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

* <span data-ttu-id="50035-207">文章标记为已修改，因为断开关系导致外键标记为 null</span><span class="sxs-lookup"><span data-stu-id="50035-207">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="50035-208">如果外键不可以为 null，则即使实际值标记为 null 也不会更改</span><span class="sxs-lookup"><span data-stu-id="50035-208">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="50035-209">SaveChanges 将依赖项/子项（文章）的外键设置为 null</span><span class="sxs-lookup"><span data-stu-id="50035-209">SaveChanges sets the FK of both dependents/children (posts) to null</span></span>
* <span data-ttu-id="50035-210">保存后，依赖项/子项（文章）现在具有 null 外键值，并且对删除的主体/父项（博客）的引用已删除</span><span class="sxs-lookup"><span data-stu-id="50035-210">After saving, the dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="50035-211">具有必选或可选关系的 DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="50035-211">DeleteBehavior.Restrict with required or optional relationship</span></span>

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

* <span data-ttu-id="50035-212">文章标记为已修改，因为断开关系导致外键标记为 null</span><span class="sxs-lookup"><span data-stu-id="50035-212">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="50035-213">如果外键不可以为 null，则即使实际值标记为 null 也不会更改</span><span class="sxs-lookup"><span data-stu-id="50035-213">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="50035-214">由于“Restrict”指示 EF 不要自动将外键设置为 null，因此它保留非 null，SaveChanges 引发异常且不保存</span><span class="sxs-lookup"><span data-stu-id="50035-214">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="cascading-to-untracked-entities"></a><span data-ttu-id="50035-215">级联到未跟踪的实体</span><span class="sxs-lookup"><span data-stu-id="50035-215">Cascading to untracked entities</span></span>

<span data-ttu-id="50035-216">调用“SaveChanges”时，级联删除规则将应用于由上下文跟踪的所有实体。</span><span class="sxs-lookup"><span data-stu-id="50035-216">When you call *SaveChanges*, the cascade delete rules will be applied to any entities that are being tracked by the context.</span></span> <span data-ttu-id="50035-217">这是上述所有示例的情况，即生成用于删除主体/父项（博客）和所有依赖项/子项（文章）的 SQL 的原因：</span><span class="sxs-lookup"><span data-stu-id="50035-217">This is the situation in all the examples shown above, which is why SQL was generated to delete both the principal/parent (blog) and all the dependents/children (posts):</span></span>

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="50035-218">如果仅加载主体（例如，当为不含 `Include(b => b.Posts)` 的博客创建查询以包含文章时），则 SaveChanges 只会生成用于删除主体/父项的 SQL：</span><span class="sxs-lookup"><span data-stu-id="50035-218">If only the principal is loaded--for example, when a query is made for a blog without an `Include(b => b.Posts)` to also include posts--then SaveChanges will only generate SQL to delete the principal/parent:</span></span>

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="50035-219">如果数据库已配置相应的级联行为，则只会删除依赖项/子项（文章）。</span><span class="sxs-lookup"><span data-stu-id="50035-219">The dependents/children (posts) will only be deleted if the database has a corresponding cascade behavior configured.</span></span> <span data-ttu-id="50035-220">如果使用 EF 创建数据库，将为你设置此级联行为。</span><span class="sxs-lookup"><span data-stu-id="50035-220">If you use EF to create the database, this cascade behavior will be setup for you.</span></span>
