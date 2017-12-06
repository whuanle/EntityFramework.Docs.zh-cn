---
title: "正在加载相关数据的 EF 核心"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: cd26bd2e6f85083f73d97b1356d0ba38f53e0b8f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="loading-related-data"></a>加载相关的数据

实体框架核心，可在模型中使用的导航属性，来加载相关的实体。 有三种常见 O/RM 模式，用于加载相关的数据。
* **预先加载**意味着相关的数据从数据库加载作为初始查询的一部分。
* **显式加载**意味着相关的数据在更高版本时显式加载从数据库。
* **延迟加载**意味着访问导航属性时，相关的数据以透明方式加载从数据库。 延迟加载尚不可能与 EF 核心。

> [!TIP]  
> 你可以查看这篇文章[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying)GitHub 上。

## <a name="eager-loading"></a>预先加载

你可以使用`Include`方法，以便指定要包含在查询结果的相关的数据。 在下面的示例中，结果中返回博客将提供了其`Posts`填入相关文章的属性。

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> 与以前已加载到上下文实例的任何其他实体的实体框架核心将自动修复向上导航属性。 因此，即使显式不包含导航属性的数据，可能仍填充属性，如果某些或以前加载所有相关实体。


您可以在单个查询中包含从多个关系的相关的数据。

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>包括多个级别

你可以向下钻取通过关系以包含多个级别的相关的数据使用`ThenInclude`方法。 下面的示例加载所有博客文章、 其相关的文章和每次发布的作者。

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

你可以链接到多个调用`ThenInclude`继续进一步包括的相关数据的级别。

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

你可以将此特性以在同一查询中包含来自多个级别和多个根的相关的数据的所有组合。

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

你可能想要包括一个所包含的实体的多个相关的实体。 例如，当查询`Blog`s，包括`Posts`后想要同时包含这两者`Author`和`Tags`的`Posts`。 若要执行此操作，你需要指定每个包含从根目录开始的路径。 例如， `Blog -> Posts -> Author` 和 `Blog -> Posts -> Tags`。 这并不意味着你将获得冗余联接，在大多数情况下，EF 将合并联接生成 SQL。

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="ignored-includes"></a>忽略包括

如果您更改查询，从而使其不再返回查询开始与实体类型的实例，则会忽略 include 运算符。

在下面的示例中，基于包含运算符`Blog`，但然后`Select`运算符用于更改查询返回一个匿名类型。 在这种情况下，包括运算符产生任何影响。

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

默认情况下，EF 核心将记录一个警告时包括运算符将被忽略。 请参阅[日志记录](../miscellaneous/logging.md)有关查看日志记录输出的详细信息。 包括运算符忽略引发或不执行任何操作时，你可以更改的行为。 这是通常在设置您的上下文中的选项时`DbContext.OnConfiguring`，或在`Startup.cs`如果正在使用 ASP.NET Core。

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a>显式加载

> [!NOTE]  
> EF 核心 1.1 中已引入此功能。

你可以显式加载通过一个导航属性`DbContext.Entry(...)`API。

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

你还可以显式可以通过执行单独的查询返回相关的实体加载导航属性。 如果已启用更改跟踪，则在加载时实体，EF 核心将自动设置新加载的实体，以引用已加载，任何实体的导航属性并设置要引用的已加载实体的导航属性新加载的实体。

### <a name="querying-related-entities"></a>查询相关的实体

你还可以获得表示导航属性的内容的 LINQ 查询。

这允许你执行如运行通过相关的实体的聚合运算符，而无需加载到内存的操作。

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

你还可以筛选哪些相关的实体加载到内存。

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>延迟加载

EF 核心尚不支持延迟加载。 你可以查看[积压工作上的延迟加载项](https://github.com/aspnet/EntityFramework/issues/3797)以跟踪此功能。

## <a name="related-data-and-serialization"></a>相关的数据和序列化

因为 EF 核心将自动修复向上导航属性，你可以得到周期对象关系图中。 例如，正在加载博客和它被相关文章将导致博客对象，它引用的文章的集合。 每个这些文章将具有返回到博客的引用。

某些序列化框架不允许此类周期。 例如，如果循环是遇到 Json.NET 将引发以下异常。

> Newtonsoft.Json.JsonSerializationException： 自助引用与类型 MyApplication.Models.Blog 属性博客检测到循环。

如果你使用的 ASP.NET 核心，你可以配置 Json.NET 若要忽略的对象图中找到的周期。 将执行此操作`ConfigureServices(...)`中的方法`Startup.cs`。

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```
