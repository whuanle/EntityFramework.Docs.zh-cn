---
title: "使用相同的 DbContext 类型的 EF 核心的多个模型之间交替"
author: AndriySvyryd
uid: core/modeling/dynamic-model
ms.openlocfilehash: e6828c62c55ae6f48d46a20528268264c3f22b26
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>使用相同的 DbContext 类型的多个模型之间交替

生成的模型`OnModelCreating`无法使用属性在上下文更改生成模型的方式。 例如它无法用于排除某些属性：

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory
但是如果你尝试执行上述无需进行其他更改则会得到相同的模型每次创建新的上下文时的任何值的`IgnoreIntProperty`。 这引起缓存 EF 使用通过仅调用来提高性能的机制的模型`OnModelCreating`一次，并且缓存模型。

默认情况下 EF 假定，对于任何给定的上下文类型模型处于相同。 若要实现此目的的默认实现`IModelCacheKeyFactory`返回上下文类型中只包含一个键。 若要更改此你需要更换`IModelCacheKeyFactory`服务。 需要对使用其他模型项返回可比较的对象的新实现`Equals`考虑会影响模型的所有变量的方法：

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
