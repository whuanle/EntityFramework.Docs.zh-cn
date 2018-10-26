---
title: Load 方法的 EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
ms.openlocfilehash: bcea8ab2477f44281cd5de824457a72a84ccc766
ms.sourcegitcommit: 4a795285004612ac03ab26532ac09ca333cb4c8f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2018
ms.locfileid: "50123812"
---
# <a name="the-load-method"></a><span data-ttu-id="81373-102">加载方法</span><span class="sxs-lookup"><span data-stu-id="81373-102">The Load Method</span></span>
<span data-ttu-id="81373-103">有几种方案，你可能想要实体从数据库加载到上下文中而无需立即执行与这些实体的任何内容。</span><span class="sxs-lookup"><span data-stu-id="81373-103">There are several scenarios where you may want to load entities from the database into the context without immediately doing anything with those entities.</span></span> <span data-ttu-id="81373-104">很好的例子加载实体，可用于数据绑定，如中所述[本地数据](~/ef6/querying/local-data.md)。</span><span class="sxs-lookup"><span data-stu-id="81373-104">A good example of this is loading entities for data binding as described in [Local Data](~/ef6/querying/local-data.md).</span></span> <span data-ttu-id="81373-105">一种常用的方式来执行此操作是编写 LINQ 查询，然后对它，只是为了立即放弃创建的列表调用 ToList。</span><span class="sxs-lookup"><span data-stu-id="81373-105">One common way to do this is to write a LINQ query and then call ToList on it, only to immediately discard the created list.</span></span> <span data-ttu-id="81373-106">Load 扩展方法工作方式相似 ToList，不同之处在于它完全避免了列表的创建。</span><span class="sxs-lookup"><span data-stu-id="81373-106">The Load extension method works just like ToList except that it avoids the creation of the list altogether.</span></span>  

<span data-ttu-id="81373-107">本主题所介绍的方法同样适用于查询使用 Code First 和 EF 设计器创建的模型。</span><span class="sxs-lookup"><span data-stu-id="81373-107">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="81373-108">下面是使用负载的两个示例。</span><span class="sxs-lookup"><span data-stu-id="81373-108">Here are two examples of using Load.</span></span> <span data-ttu-id="81373-109">第一个执行从 Windows 窗体数据绑定应用程序负载可查询的实体绑定到的本地集合之前，如中所述[本地数据](~/ef6/querying/local-data.md):</span><span class="sxs-lookup"><span data-stu-id="81373-109">The first is taken from a Windows Forms data binding application where Load is used to query for entities before binding to the local collection, as described in [Local Data](~/ef6/querying/local-data.md):</span></span>  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

<span data-ttu-id="81373-110">使用负载加载经过筛选的相关实体集合中所述的第二个示例所示[加载相关实体](~/ef6/querying/related-data.md):</span><span class="sxs-lookup"><span data-stu-id="81373-110">The second example shows using Load to load a filtered collection of related entities, as described in [Loading Related Entities](~/ef6/querying/related-data.md):</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework"))
        .Load();
}
```  
