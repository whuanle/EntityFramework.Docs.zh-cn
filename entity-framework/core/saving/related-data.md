---
title: "保存相关数据的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
ms.technology: entity-framework-core
uid: core/saving/related-data
ms.openlocfilehash: 078879163002cb66e0f0f439415789963181ec15
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="saving-related-data"></a><span data-ttu-id="5803c-102">保存相关的数据</span><span class="sxs-lookup"><span data-stu-id="5803c-102">Saving Related Data</span></span>

<span data-ttu-id="5803c-103">除了独立的实体，你还可以使用您的模型中定义的关系。</span><span class="sxs-lookup"><span data-stu-id="5803c-103">In addition to isolated entities, you can also make use of the relationships defined in your model.</span></span>

> [!TIP]  
> <span data-ttu-id="5803c-104">你可以查看这篇文章[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/)GitHub 上。</span><span class="sxs-lookup"><span data-stu-id="5803c-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) on GitHub.</span></span>

## <a name="adding-a-graph-of-new-entities"></a><span data-ttu-id="5803c-105">添加新的实体的关系图</span><span class="sxs-lookup"><span data-stu-id="5803c-105">Adding a graph of new entities</span></span>

<span data-ttu-id="5803c-106">如果创建多个新的相关的实体，则将其中一个添加到上下文将使其他人太添加。</span><span class="sxs-lookup"><span data-stu-id="5803c-106">If you create several new related entities, adding one of them to the context will cause the others to be added too.</span></span>

<span data-ttu-id="5803c-107">在下面的示例中，博客和三个相关的文章被所有插入到数据库。</span><span class="sxs-lookup"><span data-stu-id="5803c-107">In the following example, the blog and three related posts are all inserted into the database.</span></span> <span data-ttu-id="5803c-108">文章的发现和添加，因为它们是可以通过访问`Blog.Posts`导航属性。</span><span class="sxs-lookup"><span data-stu-id="5803c-108">The posts are found and added, because they are reachable via the `Blog.Posts` navigation property.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

## <a name="adding-a-related-entity"></a><span data-ttu-id="5803c-109">添加相关的实体</span><span class="sxs-lookup"><span data-stu-id="5803c-109">Adding a related entity</span></span>

<span data-ttu-id="5803c-110">如果从已由上下文跟踪的实体的导航属性引用新实体，将发现实体，并将其插入到数据库中。</span><span class="sxs-lookup"><span data-stu-id="5803c-110">If you reference a new entity from the navigation property of an entity that is already tracked by the context, the entity will be discovered and inserted into the database.</span></span>

<span data-ttu-id="5803c-111">在下面的示例中，`post`因为它将添加到插入实体`Posts`属性`blog`从数据库提取了实体。</span><span class="sxs-lookup"><span data-stu-id="5803c-111">In the following example, the `post` entity is inserted because it is added to the `Posts` property of the `blog` entity which was fetched from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a><span data-ttu-id="5803c-112">更改关系</span><span class="sxs-lookup"><span data-stu-id="5803c-112">Changing relationships</span></span>

<span data-ttu-id="5803c-113">如果你更改实体的导航属性，则将对数据库中的外键列进行相应的更改。</span><span class="sxs-lookup"><span data-stu-id="5803c-113">If you change the navigation property of an entity, the corresponding changes will be made to the foreign key column in the database.</span></span>

<span data-ttu-id="5803c-114">在下面的示例中，`post`更新实体属于新`blog`实体因为其`Blog`导航属性设置为指向`blog`。</span><span class="sxs-lookup"><span data-stu-id="5803c-114">In the following example, the `post` entity is updated to belong to the new `blog` entity because its `Blog` navigation property is set to point to `blog`.</span></span> <span data-ttu-id="5803c-115">请注意，`blog`还将在因为它是由已由上下文跟踪的实体的导航属性引用的新实体插入到数据库 (`post`)。</span><span class="sxs-lookup"><span data-stu-id="5803c-115">Note that `blog` will also be inserted into the database because it is a new entity that is referenced by the navigation property of an entity that is already tracked by the context (`post`).</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a><span data-ttu-id="5803c-116">删除关系</span><span class="sxs-lookup"><span data-stu-id="5803c-116">Removing relationships</span></span>

<span data-ttu-id="5803c-117">你可以通过将引用导航设置为删除关系`null`，或从集合导航删除相关的实体。</span><span class="sxs-lookup"><span data-stu-id="5803c-117">You can remove a relationship by setting a reference navigation to `null`, or removing the related entity from a collection navigation.</span></span>

<span data-ttu-id="5803c-118">删除关系可以依赖的实体上具有副作用，根据级联删除在关系中配置的行为。</span><span class="sxs-lookup"><span data-stu-id="5803c-118">Removing a relationship can have side effects on the dependent entity, according to the cascade delete behavior configured in the relationship.</span></span>

<span data-ttu-id="5803c-119">默认情况下，对于需要关系，级联删除行为配置，并将从数据库中删除子/相关实体。</span><span class="sxs-lookup"><span data-stu-id="5803c-119">By default, for required relationships, a cascade delete behavior is configured and the child/dependent entity will be deleted from the database.</span></span> <span data-ttu-id="5803c-120">对于可选关系，级联删除未配置默认情况下，但外键属性将设置为 null。</span><span class="sxs-lookup"><span data-stu-id="5803c-120">For optional relationships, cascade delete is not configured by default, but the foreign key property will be set to null.</span></span>

<span data-ttu-id="5803c-121">请参阅[必选和可选关系](../modeling/relationships.md#required-and-optional-relationships)若要了解有关如何配置关系的 requiredness。</span><span class="sxs-lookup"><span data-stu-id="5803c-121">See [Required and Optional Relationships](../modeling/relationships.md#required-and-optional-relationships) to learn about how the requiredness of relationships can be configured.</span></span>

<span data-ttu-id="5803c-122">请参阅[级联删除](cascade-delete.md)工作如何级联删除行为的详细信息，它们如何配置显式和约定如何选择它们。</span><span class="sxs-lookup"><span data-stu-id="5803c-122">See [Cascade Delete](cascade-delete.md) for more details on how cascade delete behaviors work, how they can be configured explicitly and  how they are selected by convention.</span></span>

<span data-ttu-id="5803c-123">在以下示例中，级联删除之间的关系在配置`Blog`和`Post`，因此`post`从数据库中删除实体。</span><span class="sxs-lookup"><span data-stu-id="5803c-123">In the following example, a cascade delete is configured on the relationship between `Blog` and `Post`, so the `post` entity is deleted from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
