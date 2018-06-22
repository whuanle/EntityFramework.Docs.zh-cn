---
title: 继承的 EF 核心
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
ms.technology: entity-framework-core
uid: core/modeling/inheritance
ms.openlocfilehash: f0394bc55dfbfea8277b1ddf898361165dd1fe51
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052777"
---
# <a name="inheritance"></a><span data-ttu-id="17c4a-102">继承</span><span class="sxs-lookup"><span data-stu-id="17c4a-102">Inheritance</span></span>

<span data-ttu-id="17c4a-103">EF 模型中的继承用于控制如何在数据库中表示的实体类中的继承。</span><span class="sxs-lookup"><span data-stu-id="17c4a-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="17c4a-104">约定</span><span class="sxs-lookup"><span data-stu-id="17c4a-104">Conventions</span></span>

<span data-ttu-id="17c4a-105">按照约定，由数据库提供程序，以确定继承在数据库中的表示方式。</span><span class="sxs-lookup"><span data-stu-id="17c4a-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="17c4a-106">请参阅[继承 （关系数据库）](relational/inheritance.md)这是如何处理与关系数据库提供程序。</span><span class="sxs-lookup"><span data-stu-id="17c4a-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="17c4a-107">如果模型中显式包含两个或多个继承的类型，EF 将仅设置继承。</span><span class="sxs-lookup"><span data-stu-id="17c4a-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="17c4a-108">EF 将不会扫描基表或派生类型，否则不包括在模型中。</span><span class="sxs-lookup"><span data-stu-id="17c4a-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="17c4a-109">你可以在模型中包含类型，通过公开*DbSet<TEntity>* 继承层次结构中的每个类型。</span><span class="sxs-lookup"><span data-stu-id="17c4a-109">You can include types in the model by exposing a *DbSet<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="17c4a-110">如果你不想要公开*DbSet<TEntity>* 对于层次结构中的一个或多个实体，你可以使用 Fluent API 以确保它们包括在模型中。</span><span class="sxs-lookup"><span data-stu-id="17c4a-110">If you don't want to expose a *DbSet<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="17c4a-111">如果您不依赖于约定可以指定使用显式的基类型和`HasBaseType`。</span><span class="sxs-lookup"><span data-stu-id="17c4a-111">And if you don't rely on conventions you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="17c4a-112">你可以使用`.HasBaseType((Type)null)`以层次结构中删除的实体类型。</span><span class="sxs-lookup"><span data-stu-id="17c4a-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="17c4a-113">数据注释</span><span class="sxs-lookup"><span data-stu-id="17c4a-113">Data Annotations</span></span>

<span data-ttu-id="17c4a-114">数据注释不能用于配置继承。</span><span class="sxs-lookup"><span data-stu-id="17c4a-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="17c4a-115">Fluent API</span><span class="sxs-lookup"><span data-stu-id="17c4a-115">Fluent API</span></span>

<span data-ttu-id="17c4a-116">用于继承的 Fluent API 取决于你使用的数据库提供程序。</span><span class="sxs-lookup"><span data-stu-id="17c4a-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="17c4a-117">请参阅[继承 （关系数据库）](relational/inheritance.md)您可以对关系数据库提供程序执行的配置。</span><span class="sxs-lookup"><span data-stu-id="17c4a-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
