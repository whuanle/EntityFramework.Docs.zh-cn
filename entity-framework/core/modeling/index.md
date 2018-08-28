---
title: 创建模型 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 9f702d5833b88e6eb77c0afefdae0ed3bc162ec8
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993928"
---
# <a name="creating-a-model"></a><span data-ttu-id="706c2-102">创建模型</span><span class="sxs-lookup"><span data-stu-id="706c2-102">Creating a Model</span></span>

<span data-ttu-id="706c2-103">Entity Framework 使用一组约定基于实体类的形状构建模型。</span><span class="sxs-lookup"><span data-stu-id="706c2-103">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="706c2-104">可指定其他配置补充和/或替代由约定发现的内容。</span><span class="sxs-lookup"><span data-stu-id="706c2-104">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="706c2-105">本文介绍可应用于面向任何数据存储的模型的配置，以及面向任意关系数据库时可应用的配置。</span><span class="sxs-lookup"><span data-stu-id="706c2-105">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="706c2-106">提供程序还可支持特定于具体数据存储的配置。</span><span class="sxs-lookup"><span data-stu-id="706c2-106">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="706c2-107">有关提供程序特定配置的文档，请参阅[数据库提供程序](../providers/index.md)部分。</span><span class="sxs-lookup"><span data-stu-id="706c2-107">For documentation on provider specific configuration see the [Database Providers](../providers/index.md) section.</span></span>

> [!TIP]  
> <span data-ttu-id="706c2-108">可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples)。</span><span class="sxs-lookup"><span data-stu-id="706c2-108">You can view this article’s [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="methods-of-configuration"></a><span data-ttu-id="706c2-109">配置方法</span><span class="sxs-lookup"><span data-stu-id="706c2-109">Methods of configuration</span></span>

### <a name="fluent-api"></a><span data-ttu-id="706c2-110">Fluent API</span><span class="sxs-lookup"><span data-stu-id="706c2-110">Fluent API</span></span>

<span data-ttu-id="706c2-111">可在派生上下文中替代 `OnModelCreating` 方法，并使用 `ModelBuilder API` 来配置模型。</span><span class="sxs-lookup"><span data-stu-id="706c2-111">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="706c2-112">此配置方法最为有效，并可在不修改实体类的情况下指定配置。</span><span class="sxs-lookup"><span data-stu-id="706c2-112">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="706c2-113">Fluent API 配置具有最高优先级，并将替代约定和数据注释。</span><span class="sxs-lookup"><span data-stu-id="706c2-113">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Required.cs?range=5-15&highlight=5-10)] -->

``` csharp
    class MyContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Blog>()
                .Property(b => b.Url)
                .IsRequired();
        }
    }
```

### <a name="data-annotations"></a><span data-ttu-id="706c2-114">数据注释</span><span class="sxs-lookup"><span data-stu-id="706c2-114">Data Annotations</span></span>

<span data-ttu-id="706c2-115">也可将特性（称为数据注释）应用于类和属性。</span><span class="sxs-lookup"><span data-stu-id="706c2-115">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="706c2-116">数据注释会替代约定，但会被 Fluent API 配置覆盖。</span><span class="sxs-lookup"><span data-stu-id="706c2-116">Data annotations will override conventions, but will be overwritten by Fluent API configuration.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?range=11-16&highlight=4)] -->
``` csharp
    public class Blog
    {
        public int BlogId { get; set; }
        [Required]
        public string Url { get; set; }
    }
```
