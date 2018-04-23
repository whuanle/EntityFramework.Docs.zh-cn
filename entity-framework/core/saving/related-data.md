---
title: 保存相关数据的 EF 核心
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
ms.technology: entity-framework-core
uid: core/saving/related-data
ms.openlocfilehash: b0ed25267c85e82db18d8a89693b6040db7e4b34
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/16/2018
---
# <a name="saving-related-data"></a>保存相关的数据

除了独立的实体，你还可以使用您的模型中定义的关系。

> [!TIP]  
> 可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/)。

## <a name="adding-a-graph-of-new-entities"></a>添加新的实体的关系图

如果创建多个新的相关的实体，则将其中一个添加到上下文将使其他人太添加。

在下面的示例中，博客和三个相关的文章被所有插入到数据库。 文章的发现和添加，因为它们是可以通过访问`Blog.Posts`导航属性。

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> 使用 EntityEntry.State 属性设置的单个实体的状态。 例如 `context.Entry(blog).State = EntityState.Modified`。

## <a name="adding-a-related-entity"></a>添加相关的实体

如果从已由上下文跟踪的实体的导航属性引用新实体，将发现实体，并将其插入到数据库中。

在下面的示例中，`post`因为它将添加到插入实体`Posts`属性`blog`从数据库提取了实体。

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a>更改关系

如果你更改实体的导航属性，则将对数据库中的外键列进行相应的更改。

在下面的示例中，`post`更新实体属于新`blog`实体因为其`Blog`导航属性设置为指向`blog`。 请注意，`blog`还将在因为它是由已由上下文跟踪的实体的导航属性引用的新实体插入到数据库 (`post`)。

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a>删除关系

你可以通过将引用导航设置为删除关系`null`，或从集合导航删除相关的实体。

删除关系可以依赖的实体上具有副作用，根据级联删除在关系中配置的行为。

默认情况下，对于需要关系，级联删除行为配置，并将从数据库中删除子/相关实体。 对于可选关系，级联删除未配置默认情况下，但外键属性将设置为 null。

请参阅[必选和可选关系](../modeling/relationships.md#required-and-optional-relationships)若要了解有关如何配置关系的 requiredness。

请参阅[级联删除](cascade-delete.md)工作如何级联删除行为的详细信息，它们如何配置显式和约定如何选择它们。

在以下示例中，级联删除之间的关系在配置`Blog`和`Post`，因此`post`从数据库中删除实体。

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
