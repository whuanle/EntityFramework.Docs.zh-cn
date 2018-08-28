---
title: 非跟踪查询的 EF6
author: divega
ms.date: 2016-10-23
ms.assetid: f80ac260-c2dc-484d-94a3-3424fd862f8b
ms.openlocfilehash: dba4127ade9481b40d4fd3c4323532ddfedf6980
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994235"
---
# <a name="no-tracking-queries"></a>非跟踪查询
有时你可能想要从查询中返回获取实体，但不是具有由上下文跟踪这些实体。 查询大量的只读方案中的实体时，可能会导致更好的性能。 本主题所介绍的方法同样适用于查询使用 Code First 和 EF 设计器创建的模型。  

新的扩展方法 AsNoTracking 允许在这种方式中运行任何查询。 例如：  

``` csharp
using (var context = new BloggingContext())
{
    // Query for all blogs without tracking them
    var blogs1 = context.Blogs.AsNoTracking();

    // Query for some blogs without tracking them
    var blogs2 = context.Blogs
                        .Where(b => b.Name.Contains(".NET"))
                        .AsNoTracking()
                        .ToList();
}
```  
