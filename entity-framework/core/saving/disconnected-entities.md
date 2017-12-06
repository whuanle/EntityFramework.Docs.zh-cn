---
title: "断开连接的实体的 EF 核心"
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
ms.technology: entity-framework-core
uid: core/saving/disconnected-entities
ms.openlocfilehash: b9d9662ce277e4f7b3d6f997a5117a0592f59fa3
ms.sourcegitcommit: c72d85805db0aa95f980514a18381fdc5e17c786
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2017
---
# <a name="disconnected-entities"></a>断开连接的实体

DbContext 实例自动将跟踪从数据库中返回的实体。 当调用 SaveChanges，并且根据需要将更新数据库，然后将检测到这些实体所做的更改。 请参阅[基本保存](basic.md)和[相关数据](related-data.md)有关详细信息。

但是，有时实体都被查询使用一个上下文实例，然后再保存使用不同的实例。 这通常发生在"已断开连接"的情况下，如 web 应用程序其中实体是查询，发送到客户端、 修改、 发送回在请求中，服务器，然后保存。 在这种情况下，第二个上下文实例要知道实体的新 （应插入） 的需求或现有 （应会更新）。

> [!TIP]  
> 你可以查看这篇文章[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/)GitHub 上。

## <a name="identifying-new-entities"></a>标识新实体

### <a name="client-identifies-new-entities"></a>客户端标识新的实体

客户端通知服务器实体是新的或现有时最简单的情况处理。 例如，通常要插入新实体的请求是不同的更新现有实体的请求。

本部分的其余部分介绍的情况下，需要以某种其他方式的信息确定是否要插入或更新。

### <a name="with-auto-generated-keys"></a>使用自动生成密钥

自动生成的键的值通常用于确定需要要插入或更新实体。 如果密钥不具有已设置 （即它仍具有 null、 零等的 CLR 默认值），则必须为新实体，并需要插入。 另一方面，如果已设置的密钥值，然后必须已以前保存，并且现在需要更新。 换而言之，如果项具有一个值，然后实体查询，发送到客户端，并具有现在返回更新。

很容易地知道实体类型时检查取消设置的密钥：

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

但是，EF 还具有一种要这样做的任何实体类型和密钥类型的内置方法：

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> 已设置项就会立即实体进行跟踪上下文中，即使实体处于 Added 状态。 这有助于遍历关系图的实体和确定应使用 TrackGraph API 时，如何处理每个，如一样。 密钥值仅应在此处显示的方式_之前_进行任何调用，以便跟踪实体。

### <a name="with-other-keys"></a>与其他键

标识新实体时不会自动生成密钥的值需要某种其他机制。 有这两种常规方法：
 * 实体的查询
 * 一个标志传递从客户端

若要查询的实体，只需使用查找方法：

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

它不在本文档，以显示有关从客户端传递一个标志的完整代码的范围。 在 web 应用中，则通常意味着发出执行不同操作的不同请求或传递在请求中的某些状态，然后在控制器中对其进行提取。

## <a name="saving-single-entities"></a>保存单个实体

如果已经知道是否需要插入或更新，则可以相应地使用添加或更新：

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

但是，如果该实体使用自动生成的密钥值，则可以使用更新方法，这两种情况：

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

更新方法通常将标记更新，未插入的实体。 但是，如果实体具有自动生成的密钥，并且已设置了任何密钥值，则实体会自动将其标记为插入。

> [!TIP]  
> EF 核心 2.0 中引入了此行为。 对于早期版本是始终需要明确选择添加或更新。

如果实体不使用自动生成的键，则应用程序必须确定是否应插入或更新实体： 例如：

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

此处的步骤如下：
* 如果查找返回 null，则数据库尚未包含博客具有此 ID，因此我们调用添加将其标记插入操作。
* 如果查找返回实体，则在数据库中存在以及上下文现在正在跟踪的现有实体
  * 我们然后使用 SetValues 此实体与来自客户端上设置的所有属性值。
  * SetValues 调用会将根据需要更新的实体标记。

> [!TIP]  
> SetValues 仅将标记为已修改具有不同的值中的被跟踪实体的属性。 这意味着当发送更新，将更新已实际更改的那些列。 （并且未进行任何更改，如果没有更新将发送根本。）

## <a name="working-with-graphs"></a>使用关系图

### <a name="all-newall-existing-entities"></a>所有新的或所有现有实体

使用关系图的一个示例是插入或更新其集合关联的文章和博客。 如果应插入在图中的所有实体，或应更新所有，则过程是上文所述的单个实体的相同。 例如，博客文章创建的关系图所示：

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

可将插入如下：

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

博客和所有要插入的文章，则会将标记添加到调用。

同样，如果关系图中的所有实体都需要更新，然后更新可以使用：

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

博客和所有其文章将标记为要更新。

### <a name="mix-of-new-and-existing-entities"></a>新的和现有的实体的组合

使用自动生成密钥，更新可以再次进行插入和更新，即使使用关系图包含需要插入的实体和那些需要进行更新的组合：

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

更新会将标记为以图表、 博客或 post，插入操作，如果它不具有密钥值集，而所有其他实体标记为更新任何实体。

因为之前，不使用自动生成密钥时查询和一些处理可使用：

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a>处理删除

删除可能会很棘手，来处理自实体没有通常意味着应删除。 与此处理的一种方法是使用"软删除"，以便该实体标记为已删除，而不是实际删除。 删除，然后将更新相同。 在使用可实现软删除[查询筛选器](xref:core/querying/filters)。

对于 true 删除一种常见模式是使用扩展的查询模式来执行什么是实质上是关系图差异例如：

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a>TrackGraph

在内部，添加、 附加和更新使用与决定对有关是否要将其标记为 Added （若要插入），修改时间 （以更新），每个实体进行未更改的关系图遍历 （不执行任何操作），或已删除 （若要删除）。 此机制是通过 TrackGraph API 公开的。 例如，假设，当客户端发回的实体的图它会将某些标志设置对，该值指示应如何处理每个实体。 TrackGraph 然后可以用于处理此标志：

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

标志仅显示为简单起见，该示例的实体的一部分。 通常标志应该是 DTO 或包含在请求中的某个其他状态的一部分。
