---
title: "跟踪 vs。否跟踪查询的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
ms.technology: entity-framework-core
uid: core/querying/tracking
ms.openlocfilehash: 9a22c893f3b1e9991560e25e0252287a2844b39e
ms.sourcegitcommit: 3b6159db8a6c0653f13c7b528367b4e69ac3d51e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2017
---
# <a name="tracking-vs-no-tracking-queries"></a>跟踪 vs。否跟踪查询

跟踪行为控件，无论实体 Framework 内核将其更改跟踪器中保留有关实体实例信息。 如果跟踪的实体，实体中检测到任何更改将永久保存到数据库期间`SaveChanges()`。 获取跟踪查询的实体与以前已加载到 DbContext 实例的实体之间，实体框架核心将还修复向上导航属性。

> [!TIP]  
> 你可以查看这篇文章[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying)GitHub 上。

## <a name="tracking-queries"></a>跟踪查询

默认情况下，跟踪返回实体类型的查询。 这意味着你可以对这些实体实例进行更改，并且这些更改保存的`SaveChanges()`。

在下面的示例中，检测到并持久化到数据库中，在过程更改为博客分级`SaveChanges()`。

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a>否跟踪查询

在只读方案中使用结果时，没有跟踪查询很有用。 它们可以更加快速地执行，因为无需安装更改跟踪信息。

你可以交换是否-跟踪的单个查询：

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

你还可以更改默认跟踪上下文实例级别的行为：

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> 没有跟踪的查询仍然执行标识解析在执行查询。 如果结果集包含相同的实体多次，将返回结果集中的每个匹配项的实体类的同一个实例。 但是，弱引用用于跟踪的已返回的实体。 如果前一个结果具有相同标识号超出范围，且垃圾回收运行，则可能获得的新实体实例。 有关详细信息，请参阅[查询的工作原理](overview.md)。

## <a name="tracking-and-projections"></a>跟踪和投影

即使查询的结果类型不是实体类型，如果结果包含实体类型它们将仍在默认情况下跟踪。 在下面的查询，它返回匿名类型的实例`Blog`集将跟踪结果中。

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=7)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Blog = b,
                Posts = b.Posts.Count()
            });
}
```

如果结果集不包含任何实体类型，会不执行任何跟踪。 在下面的查询，这将返回一个匿名类型使用某些实体 （但不实际实体类型的实例） 中的值时，没有任何跟踪执行。

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Id = b.BlogId,
                Url = b.Url
            });
}
```
