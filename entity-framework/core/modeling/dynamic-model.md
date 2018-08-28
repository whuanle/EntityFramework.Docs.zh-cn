---
title: 使用相同的 DbContext 类型的 EF Core 的多个模型之间交替
author: AndriySvyryd
ms.date: 12/10/2017
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 1d87efb668c12a2862583fba16a6c444b3cda9de
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994981"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="edd7c-102">具有相同 DbContext 类型的多个模型之间切换</span><span class="sxs-lookup"><span data-stu-id="edd7c-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="edd7c-103">生成的模型`OnModelCreating`无法使用属性的上下文更改如何构建的模型。</span><span class="sxs-lookup"><span data-stu-id="edd7c-103">The model built in `OnModelCreating` could use a property on the context to change how the model is built.</span></span> <span data-ttu-id="edd7c-104">例如它可用来排除某些属性：</span><span class="sxs-lookup"><span data-stu-id="edd7c-104">For example it could be used to exclude a certain property:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="edd7c-105">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="edd7c-105">IModelCacheKeyFactory</span></span>
<span data-ttu-id="edd7c-106">但是如果您尝试执行上述无需进行其他更改则会得到相同的模型每次新的上下文创建的任何值的`IgnoreIntProperty`。</span><span class="sxs-lookup"><span data-stu-id="edd7c-106">However if you tried doing the above without additional changes you would get the same model every time a new context is created for any value of `IgnoreIntProperty`.</span></span> <span data-ttu-id="edd7c-107">这由于缓存的机制 EF 使用通过仅调用来提高性能的模型`OnModelCreating`一次，并且缓存模型。</span><span class="sxs-lookup"><span data-stu-id="edd7c-107">This is caused by the model caching mechanism EF uses to improve the performance by only invoking `OnModelCreating` once and caching the model.</span></span>

<span data-ttu-id="edd7c-108">默认情况下 EF 假定任何给定的上下文类型的模型都将是相同。</span><span class="sxs-lookup"><span data-stu-id="edd7c-108">By default EF assumes that for any given context type the model will be the same.</span></span> <span data-ttu-id="edd7c-109">若要实现此目的的默认实现`IModelCacheKeyFactory`返回只包含上下文类型的密钥。</span><span class="sxs-lookup"><span data-stu-id="edd7c-109">To accomplish this the default implementation of `IModelCacheKeyFactory` returns a key that just contains the context type.</span></span> <span data-ttu-id="edd7c-110">若要更改此你需要更换`IModelCacheKeyFactory`服务。</span><span class="sxs-lookup"><span data-stu-id="edd7c-110">To change this you need to replace the `IModelCacheKeyFactory` service.</span></span> <span data-ttu-id="edd7c-111">新的实现需要对使用其他模型项返回一个对象进行比较，`Equals`会考虑会影响模型的所有变量的方法：</span><span class="sxs-lookup"><span data-stu-id="edd7c-111">The new implementation needs to return an object that can be compared to other model keys using the `Equals` method that takes into account all the variables that affect the model:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
