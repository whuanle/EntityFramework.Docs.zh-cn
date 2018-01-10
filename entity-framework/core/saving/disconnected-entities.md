---
title: "断开连接的实体的 EF 核心"
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
ms.technology: entity-framework-core
uid: core/saving/disconnected-entities
ms.openlocfilehash: 0ea02876b9594d54c971a7b70fcf7ce591e56ba0
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/22/2017
---
# <a name="disconnected-entities"></a><span data-ttu-id="3d3a1-102">断开连接的实体</span><span class="sxs-lookup"><span data-stu-id="3d3a1-102">Disconnected entities</span></span>

<span data-ttu-id="3d3a1-103">DbContext 实例自动将跟踪从数据库中返回的实体。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-103">A DbContext instance will automatically track entities returned from the database.</span></span> <span data-ttu-id="3d3a1-104">当调用 SaveChanges，并且根据需要将更新数据库，然后将检测到这些实体所做的更改。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-104">Changes made to these entities will then be detected when SaveChanges is called and the database will be updated as needed.</span></span> <span data-ttu-id="3d3a1-105">请参阅[基本保存](basic.md)和[相关数据](related-data.md)有关详细信息。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-105">See [Basic Save](basic.md) and [Related Data](related-data.md) for details.</span></span>

<span data-ttu-id="3d3a1-106">但是，有时实体都被查询使用一个上下文实例，然后再保存使用不同的实例。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-106">However, sometimes entities are queried using one context instance and then saved using a different instance.</span></span> <span data-ttu-id="3d3a1-107">这通常发生在"已断开连接"的情况下，如 web 应用程序其中实体是查询，发送到客户端、 修改、 发送回在请求中，服务器，然后保存。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-107">This often happens in "disconnected" scenarios such as a web application where the entities are queried, sent to the client, modified, sent back to the server in a request, and then saved.</span></span> <span data-ttu-id="3d3a1-108">在这种情况下，第二个上下文实例要知道实体的新 （应插入） 的需求或现有 （应会更新）。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-108">In this case, the second context instance needs to know whether the entities are new (should be inserted) or existing (should be updated).</span></span>

> [!TIP]  
> <span data-ttu-id="3d3a1-109">可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/)。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) on GitHub.</span></span>

## <a name="identifying-new-entities"></a><span data-ttu-id="3d3a1-110">标识新实体</span><span class="sxs-lookup"><span data-stu-id="3d3a1-110">Identifying new entities</span></span>

### <a name="client-identifies-new-entities"></a><span data-ttu-id="3d3a1-111">客户端标识新的实体</span><span class="sxs-lookup"><span data-stu-id="3d3a1-111">Client identifies new entities</span></span>

<span data-ttu-id="3d3a1-112">客户端通知服务器实体是新的或现有时最简单的情况处理。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-112">The simplest case to deal with is when the client informs the server whether the entity is new or existing.</span></span> <span data-ttu-id="3d3a1-113">例如，通常要插入新实体的请求是不同的更新现有实体的请求。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-113">For example, often the request to insert a new entity is different from the request to update an existing entity.</span></span>

<span data-ttu-id="3d3a1-114">本部分的其余部分介绍的情况下，需要以某种其他方式的信息确定是否要插入或更新。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-114">The remainder of this section covers the cases where it necessary to determine in some other way whether to insert or update.</span></span>

### <a name="with-auto-generated-keys"></a><span data-ttu-id="3d3a1-115">使用自动生成密钥</span><span class="sxs-lookup"><span data-stu-id="3d3a1-115">With auto-generated keys</span></span>

<span data-ttu-id="3d3a1-116">自动生成的键的值通常用于确定需要要插入或更新实体。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-116">The value of an automatically generated key can often be used to determine whether an entity needs to be inserted or updated.</span></span> <span data-ttu-id="3d3a1-117">如果密钥不具有已设置 （即它仍具有 null、 零等的 CLR 默认值），则必须为新实体，并需要插入。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-117">If the key has not been set (i.e. it still has the CLR default value of null, zero, etc.), then the entity must be new and needs inserting.</span></span> <span data-ttu-id="3d3a1-118">另一方面，如果已设置的密钥值，然后必须已以前保存，并且现在需要更新。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-118">On the other hand, if the key value has been set, then it must have already been previously saved and now needs updating.</span></span> <span data-ttu-id="3d3a1-119">换而言之，如果项具有一个值，然后实体查询，发送到客户端，并具有现在返回更新。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-119">In other words, if the key has a value, then entity was queried, sent to the client, and has now come back to be updated.</span></span>

<span data-ttu-id="3d3a1-120">很容易地知道实体类型时检查取消设置的密钥：</span><span class="sxs-lookup"><span data-stu-id="3d3a1-120">It is easy to check for an unset key when the entity type is known:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

<span data-ttu-id="3d3a1-121">但是，EF 还具有一种要这样做的任何实体类型和密钥类型的内置方法：</span><span class="sxs-lookup"><span data-stu-id="3d3a1-121">However, EF also has a built-in way to do this for any entity type and key type:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> <span data-ttu-id="3d3a1-122">已设置项就会立即实体进行跟踪上下文中，即使实体处于 Added 状态。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-122">Keys are set as soon as entities are tracked by the context, even if the entity is in the Added state.</span></span> <span data-ttu-id="3d3a1-123">这有助于遍历关系图的实体和确定应使用 TrackGraph API 时，如何处理每个，如一样。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-123">This helps when traversing a graph of entities and deciding what to do with each, such as when using the TrackGraph API.</span></span> <span data-ttu-id="3d3a1-124">密钥值仅应在此处显示的方式_之前_进行任何调用，以便跟踪实体。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-124">The key value should only be used in the way shown here _before_ any call is made to track the entity.</span></span>

### <a name="with-other-keys"></a><span data-ttu-id="3d3a1-125">与其他键</span><span class="sxs-lookup"><span data-stu-id="3d3a1-125">With other keys</span></span>

<span data-ttu-id="3d3a1-126">需要某种其他机制来确定新实体时不会自动生成密钥值。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-126">Some other mechanism is needed to identify new entities when key values are not generated automatically.</span></span> <span data-ttu-id="3d3a1-127">有这两种常规方法：</span><span class="sxs-lookup"><span data-stu-id="3d3a1-127">There are two general approaches to this:</span></span>
 * <span data-ttu-id="3d3a1-128">实体的查询</span><span class="sxs-lookup"><span data-stu-id="3d3a1-128">Query for the entity</span></span>
 * <span data-ttu-id="3d3a1-129">一个标志传递从客户端</span><span class="sxs-lookup"><span data-stu-id="3d3a1-129">Pass a flag from the client</span></span>

<span data-ttu-id="3d3a1-130">若要查询的实体，只需使用查找方法：</span><span class="sxs-lookup"><span data-stu-id="3d3a1-130">To query for the entity, just use the Find method:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

<span data-ttu-id="3d3a1-131">它不在本文档，以显示有关从客户端传递一个标志的完整代码的范围。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-131">It is beyond the scope of this document to show the full code for passing a flag from a client.</span></span> <span data-ttu-id="3d3a1-132">在 web 应用中，则通常意味着发出执行不同操作的不同请求或传递在请求中的某些状态，然后在控制器中对其进行提取。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-132">In a web app, it usually means making different requests for different actions, or passing some state in the request then extracting it in the controller.</span></span>

## <a name="saving-single-entities"></a><span data-ttu-id="3d3a1-133">保存单个实体</span><span class="sxs-lookup"><span data-stu-id="3d3a1-133">Saving single entities</span></span>

<span data-ttu-id="3d3a1-134">如果已经知道是否需要插入或更新，则可以相应地使用添加或更新：</span><span class="sxs-lookup"><span data-stu-id="3d3a1-134">If it is known whether or not an insert or update is needed, then either Add or Update can be used appropriately:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

<span data-ttu-id="3d3a1-135">但是，如果该实体使用自动生成的密钥值，则可以使用更新方法，这两种情况：</span><span class="sxs-lookup"><span data-stu-id="3d3a1-135">However, if the entity uses auto-generated key values, then the Update method can be used for both cases:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

<span data-ttu-id="3d3a1-136">更新方法通常将标记更新，未插入的实体。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-136">The Update method normally marks the entity for update, not insert.</span></span> <span data-ttu-id="3d3a1-137">但是，如果实体具有自动生成的密钥，并且已设置了任何密钥值，则实体会自动将其标记为插入。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-137">However, if the entity has a auto-generated key, and no key value has been set, then the entity is instead automatically marked for insert.</span></span>

> [!TIP]  
> <span data-ttu-id="3d3a1-138">EF 核心 2.0 中引入了此行为。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-138">This behavior was introduced in EF Core 2.0.</span></span> <span data-ttu-id="3d3a1-139">对于早期版本是始终需要明确选择添加或更新。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-139">For earlier releases it is always necessary to explicitly choose either Add or Update.</span></span>

<span data-ttu-id="3d3a1-140">如果实体不使用自动生成的键，则应用程序必须确定是否应插入或更新实体： 例如：</span><span class="sxs-lookup"><span data-stu-id="3d3a1-140">If the entity is not using auto-generated keys, then the application must decide whether the entity should be inserted or updated: For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

<span data-ttu-id="3d3a1-141">此处的步骤如下：</span><span class="sxs-lookup"><span data-stu-id="3d3a1-141">The steps here are:</span></span>
* <span data-ttu-id="3d3a1-142">如果查找返回 null，则数据库尚未包含博客具有此 ID，因此我们调用添加将其标记插入操作。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-142">If Find returns null, then the database doesn't already contain the blog with this ID, so we call Add mark it for insertion.</span></span>
* <span data-ttu-id="3d3a1-143">如果查找返回实体，则在数据库中存在以及上下文现在正在跟踪的现有实体</span><span class="sxs-lookup"><span data-stu-id="3d3a1-143">If Find returns an entity, then it exists in the database and the context is now tracking the existing entity</span></span>
  * <span data-ttu-id="3d3a1-144">我们然后使用 SetValues 此实体与来自客户端上设置的所有属性值。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-144">We then use SetValues to set the values for all properties on this entity to those that came from the client.</span></span>
  * <span data-ttu-id="3d3a1-145">SetValues 调用会将根据需要更新的实体标记。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-145">The SetValues call will mark the entity to be updated as needed.</span></span>

> [!TIP]  
> <span data-ttu-id="3d3a1-146">SetValues 仅将标记为已修改具有不同的值中的被跟踪实体的属性。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-146">SetValues will only mark as modified the properties that have different values to those in the tracked entity.</span></span> <span data-ttu-id="3d3a1-147">这意味着当发送更新，将更新已实际更改的那些列。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-147">This means that when the update is sent, only those columns that have actually changed will be updated.</span></span> <span data-ttu-id="3d3a1-148">（并且未进行任何更改，如果没有更新将发送根本。）</span><span class="sxs-lookup"><span data-stu-id="3d3a1-148">(And if nothing has changed, then no update will be sent at all.)</span></span>

## <a name="working-with-graphs"></a><span data-ttu-id="3d3a1-149">使用关系图</span><span class="sxs-lookup"><span data-stu-id="3d3a1-149">Working with graphs</span></span>

### <a name="all-newall-existing-entities"></a><span data-ttu-id="3d3a1-150">所有新的或所有现有实体</span><span class="sxs-lookup"><span data-stu-id="3d3a1-150">All new/all existing entities</span></span>

<span data-ttu-id="3d3a1-151">使用关系图的一个示例是插入或更新其集合关联的文章和博客。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-151">An example of working with graphs is inserting or updating a blog together with its collection of associated posts.</span></span> <span data-ttu-id="3d3a1-152">如果应插入在图中的所有实体，或应更新所有，则过程是上文所述的单个实体的相同。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-152">If all the entities in the graph should be inserted, or all should be updated, then the process is the same as described above for single entities.</span></span> <span data-ttu-id="3d3a1-153">例如，博客文章创建的关系图所示：</span><span class="sxs-lookup"><span data-stu-id="3d3a1-153">For example, a graph of blogs and posts created like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

<span data-ttu-id="3d3a1-154">可将插入如下：</span><span class="sxs-lookup"><span data-stu-id="3d3a1-154">can be inserted like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

<span data-ttu-id="3d3a1-155">博客和所有要插入的文章，则会将标记添加到调用。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-155">The call to Add will mark the blog and all the posts to be inserted.</span></span>

<span data-ttu-id="3d3a1-156">同样，如果关系图中的所有实体都需要更新，然后更新可以使用：</span><span class="sxs-lookup"><span data-stu-id="3d3a1-156">Likewise, if all the entities in a graph need to be updated, then Update can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

<span data-ttu-id="3d3a1-157">博客和所有其文章将标记为要更新。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-157">The blog and all its posts will be marked to be updated.</span></span>

### <a name="mix-of-new-and-existing-entities"></a><span data-ttu-id="3d3a1-158">新的和现有的实体的组合</span><span class="sxs-lookup"><span data-stu-id="3d3a1-158">Mix of new and existing entities</span></span>

<span data-ttu-id="3d3a1-159">使用自动生成密钥，更新可以再次进行插入和更新，即使使用关系图包含需要插入的实体和那些需要进行更新的组合：</span><span class="sxs-lookup"><span data-stu-id="3d3a1-159">With auto-generated keys, Update can again be used for both inserts and updates, even if the graph contains a mix of entities that require inserting and those that require updating:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

<span data-ttu-id="3d3a1-160">更新会将标记为以图表、 博客或 post，插入操作，如果它不具有密钥值集，而所有其他实体标记为更新任何实体。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-160">Update will mark any entity in the graph, blog or post, for insertion if it does not have a key value set, while all other entities are marked for update.</span></span>

<span data-ttu-id="3d3a1-161">因为之前，不使用自动生成密钥时查询和一些处理可使用：</span><span class="sxs-lookup"><span data-stu-id="3d3a1-161">As before, when not using auto-generated keys, a query and some processing can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a><span data-ttu-id="3d3a1-162">处理删除</span><span class="sxs-lookup"><span data-stu-id="3d3a1-162">Handling deletes</span></span>

<span data-ttu-id="3d3a1-163">删除可能会很棘手，来处理自实体没有通常意味着应删除。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-163">Delete can be tricky to handle since often the absence of an entity means that it should be deleted.</span></span> <span data-ttu-id="3d3a1-164">与此处理的一种方法是使用"软删除"，以便该实体标记为已删除，而不是实际删除。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-164">One way to deal with this is to use "soft deletes" such that the entity is marked as deleted rather than actually being deleted.</span></span> <span data-ttu-id="3d3a1-165">删除，然后将更新相同。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-165">Deletes then becomes the same as updates.</span></span> <span data-ttu-id="3d3a1-166">在使用可实现软删除[查询筛选器](xref:core/querying/filters)。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-166">Soft deletes can be implemented in using [query filters](xref:core/querying/filters).</span></span>

<span data-ttu-id="3d3a1-167">对于 true 删除一种常见模式是使用扩展的查询模式来执行什么是实质上是关系图差异例如:</span><span class="sxs-lookup"><span data-stu-id="3d3a1-167">For true deletes, a common pattern is to use an extension of the query pattern to perform what is essentially a graph diff. For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a><span data-ttu-id="3d3a1-168">TrackGraph</span><span class="sxs-lookup"><span data-stu-id="3d3a1-168">TrackGraph</span></span>

<span data-ttu-id="3d3a1-169">在内部，添加、 附加和更新使用与决定对有关是否要将其标记为 Added （若要插入），修改时间 （以更新），每个实体进行未更改的关系图遍历 （不执行任何操作），或已删除 （若要删除）。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-169">Internally, Add, Attach, and Update use graph-traversal with a determination made for each entity as to whether it should be marked as Added (to insert), Modified (to update), Unchanged (do nothing), or Deleted (to delete).</span></span> <span data-ttu-id="3d3a1-170">此机制是通过 TrackGraph API 公开的。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-170">This mechanism is exposed via the TrackGraph API.</span></span> <span data-ttu-id="3d3a1-171">例如，假设，当客户端发回的实体的图它会将某些标志设置对，该值指示应如何处理每个实体。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-171">For example, let's assume that when the client sends back a graph of entities it sets some flag on each entity indicating how it should be handled.</span></span> <span data-ttu-id="3d3a1-172">TrackGraph 然后可以用于处理此标志：</span><span class="sxs-lookup"><span data-stu-id="3d3a1-172">TrackGraph can then be used to process this flag:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

<span data-ttu-id="3d3a1-173">标志仅显示为简单起见，该示例的实体的一部分。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-173">The flags are only shown as part of the entity for simplicity of the example.</span></span> <span data-ttu-id="3d3a1-174">通常标志应该是 DTO 或包含在请求中的某个其他状态的一部分。</span><span class="sxs-lookup"><span data-stu-id="3d3a1-174">Typically the flags would be part of a DTO or some other state included in the request.</span></span>
