---
title: "级联 Delete-EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
ms.technology: entity-framework-core
uid: core/saving/cascade-delete
ms.openlocfilehash: a9481fe851cc264ab3eaecad052c2e683ae57a44
ms.sourcegitcommit: 5367516f063cb42804ec92c31cdf76322554f2b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/08/2017
---
# <a name="cascade-delete"></a>级联删除

级联删除通常在数据库术语用于描述了特性是，它允许自动触发的相关行删除某行的删除。 通过 EF 核心删除行为还介绍密切相关的概念是子实体与父级的关系时自动删除已断开-通常称为"删除孤立文件"此 i。

EF 核心实现几种不同的删除行为，并允许对单个关系的删除行为的配置。 EF 核心还实现自动配置为基于 [requiredness 关系](../modeling/relationships.md#required-and-optional-relationships) 每个关系的有用的默认值删除行为的约定 。

## <a name="delete-behaviors"></a>删除行为
删除中定义行为*DeleteBehavior*枚举器类型和可以传递给*OnDelete* fluent API 来控制是否删除主体/父实体或的断开与依赖于/子实体的关系应该对依赖于/子实体具有副作用。

有三个 EF 可执行删除主体/父实体或断开与子之间的关系时的操作：
* 可以删除子/依赖项
* 可以设置此子级的外键值为 null
* 子保持不变

> [!NOTE]  
> 使用 EF 核心删除主体实体和依赖实体加载到内存中 （即对于跟踪依赖项） 时，将仅应用在 EF 核心模型中配置删除行为。 要安装程序数据库，以确保未正在由上下文跟踪的数据中具有必要的操作应用相应的级联行为要求。 如果你使用 EF 核心创建数据库，此 cascade 行为将为你的安装程序。

对于上面的第二个操作，将某个外键值设置为 null 无效如果外键不可以为 null。 （不可为 null 的外键相当于必要的关系。）在这些情况下，EF 核心跟踪调用 SaveChanges，哪些时间会引发异常，因为无法将更改保存到数据库之前，已被外键属性标记为 null。 这是类似于从数据库获取违反了约束。

有四个删除行为，如下面的表中列出。 可选关系 （可以为 null 的外键） 它_是_可以保存 null 外键值，这会导致以下影响：

| 行为名称 | 对内存中的相关/子的影响 | 对依赖于/子数据库中的影响
|-|-|-
| **级联** | 在删除实体 | 在删除实体
| **ClientSetNull** （默认） | 外键属性设置为 null | 无
| **SetNull** | 外键属性设置为 null | 外键属性设置为 null
| **限制** | 无 | 无

对于需要关系 （不可为 null 的外键），它是_不_可以保存 null 外键值，这会导致以下影响：

| 行为名称 | 对内存中的相关/子的影响 | 对依赖于/子数据库中的影响
|-|-|-
| **级联**（默认） | 在删除实体 | 在删除实体
| **ClientSetNull** | SaveChanges 引发 | 无
| **SetNull** | SaveChanges 引发 | SaveChanges 引发
| **限制** | 无 | 无

在上表中，*无*可能会导致违反了约束。 例如，如果主体/子实体已删除，但不执行任何操作，若要更改依赖于/子的外键，则数据库将可能引发上 SaveChanges 由于外约束冲突。

在高级别中：
* 如果你有没有父级，不能存在的实体，并且你想让 EF 来处理无足轻重自动删除子级，则使用*Cascade*。
  * 没有父通常会使不能存在的实体的所需的关系，使用为其*Cascade*是默认设置。
* 如果你具有实体，也可能没有父级，并且你想让 EF 来处理无足轻重的 nulling 出外键，则使用*ClientSetNull*
  * 没有父通常会使可以存在的实体的可选关系，使用为其*ClientSetNull*是默认设置。
  * 如果你想要还尝试甚至传播到子外键的 null 值的数据库时子实体，将不加载，然后使用*SetNull*。 但是，请注意，数据库必须支持此操作，请配置此类数据库可能会导致其他限制，这实际上通常使得此选项不切实际。 正因如此*SetNull*不是默认值。
* 如果你不希望 EF 核心不断自动删除实体或设置为 null 的外键自动，然后使用*限制*。 请注意，这需要，你的代码使子实体和其外键值保持同步手动否则约束将会引发异常。

> [!NOTE]
> 在 EF 核，与从 EF6，级联影响的操作未发生立即，而是仅在调用 SaveChanges 时。

> [!NOTE]  
> **EF 核心 2.0 中的更改：**在以前版本中，*限制*将可选的外键属性导致跟踪依赖的实体设置为 null，并且默认值删除可选关系的行为。 在 EF 核心 2.0 中， *ClientSetNull*引入了表示该行为，并成为的默认值为可选的关系。 行为*限制*已经过调整永远不会有依赖实体上的任何方面的副作用。

## <a name="entity-deletion-examples"></a>实体删除示例

下面的代码摘自[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/)，可以下载将运行。 此示例演示时会发生什么情况为可选和必选的关系的每个删除行为删除父实体。

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

让我们演练一下每个变体，以了解发生了什么情况。

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>具有必需或可选关系 DeleteBehavior.Cascade

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

* 博客标记为已删除
* 文章最初保持未更改，因为直到 SaveChanges 的级联操作未发生
* SaveChanges 发送删除依赖项/子级 (post) 和然后主体/父 （博客）
* 保存后，会分离所有实体，因为它们现在已从数据库删除

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull 或 DeleteBehavior.SetNull 与所需的关系

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

* 博客标记为已删除
* 文章最初保持未更改，因为直到 SaveChanges 的级联操作未发生
* SaveChanges 尝试将 post FK 设置为 null，但这会失败，因为 FK 不可以为 null

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull 或 DeleteBehavior.SetNull 具有可选的关系

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

* 博客标记为已删除
* 文章最初保持未更改，因为直到 SaveChanges 的级联操作未发生
* SaveChanges 尝试删除主体/父 （博客） 之前将这两个依赖项/子级 (post) FK 设置为 null
* 保存后，请主体/父 （博客） 被删除，但仍跟踪依赖项/子级 （文章）
* 跟踪依赖项/子级 (post) 现在具有空 FK 值和已删除的主体/父 （博客） 对其引用已删除

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>具有必需或可选关系 DeleteBehavior.Restrict

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

* 博客标记为已删除
* 文章最初保持未更改，因为直到 SaveChanges 的级联操作未发生
* 由于*限制*让 EF 不会自动将 FK 设置为 null，它将始终非 null 和 SaveChanges 引发而不保存

## <a name="delete-orphans-examples"></a>删除孤立项示例

下面的代码摘自[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/)，可以下载将运行。 此示例演示时会发生什么情况为可选和必选的关系的每个删除行为父/主体及其子级/依赖项之间的关系将被切断。 在此示例中，关系将被切断通过从主体/父 （博客） 上的集合导航属性中删除依赖项/子级 (post)。 但是，如果此行为是相同主体/父/子依赖于从参考设为改为 null。

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

让我们演练一下每个变体，以了解发生了什么情况。

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>具有必需或可选关系 DeleteBehavior.Cascade

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

* 文章标记为已修改，因为断开关系导致 FK 被标记为 null
  * 如果 FK 不可以为 null，然后的实际值将不更改，即使它被标记为 null
* SaveChanges 发送删除依赖项/子级 （文章）
* 保存后，将分离的依赖项/子级 （文章），因为它们现在已从数据库删除

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull 或 DeleteBehavior.SetNull 与所需的关系

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

* 文章标记为已修改，因为断开关系导致 FK 被标记为 null
  * 如果 FK 不可以为 null，然后的实际值将不更改，即使它被标记为 null
* SaveChanges 尝试将 post FK 设置为 null，但这会失败，因为 FK 不可以为 null

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull 或 DeleteBehavior.SetNull 具有可选的关系

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

* 文章标记为已修改，因为断开关系导致 FK 被标记为 null
  * 如果 FK 不可以为 null，然后的实际值将不更改，即使它被标记为 null
* SaveChanges 将这两个依赖项/子级 (post) FK 设置为 null
* 在保存后的依赖项/子级 (post) 现在具有空 FK 值和已删除的主体/父 （博客） 对其引用已删除

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>具有必需或可选关系 DeleteBehavior.Restrict

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

* 文章标记为已修改，因为断开关系导致 FK 被标记为 null
  * 如果 FK 不可以为 null，然后的实际值将不更改，即使它被标记为 null
* 由于*限制*让 EF 不会自动将 FK 设置为 null，它将始终非 null 和 SaveChanges 引发而不保存

## <a name="cascading-to-untracked-entities"></a>到未跟踪的实体级联

当调用*SaveChanges*，级联删除规则将应用于所有由上下文跟踪的实体。 这是在所有情况下上面所示的示例，这就是为什么 SQL 生成用于删除主体/父 （博客） 和所有依赖项/子级 (post):

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

如果只有主体加载-例如，当进行查询的博客，而无需`Include(b => b.Posts)`以便也包括文章-然后 SaveChanges 将仅生成 SQL 以删除主体/父：

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

如果数据库具有相应的级联行为配置，将仅删除依赖项/子级 (post)。 如果你使用 EF 创建数据库，此 cascade 行为将为你的安装程序。
