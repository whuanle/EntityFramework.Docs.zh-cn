---
title: 定义 Dbset 的 EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 4528a509-ace7-4dfb-8065-1b833f5e03a0
caps.latest.revision: 3
ms.openlocfilehash: 8a495c6ce74d9a346a84b0e10fb28395f4dce07b
ms.sourcegitcommit: 00cb52625b57c1ea339ded1454179fe89b6bcfea
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/16/2018
ms.locfileid: "39120706"
---
# <a name="defining-dbsets"></a><span data-ttu-id="12513-102">定义 Dbset</span><span class="sxs-lookup"><span data-stu-id="12513-102">Defining DbSets</span></span>
<span data-ttu-id="12513-103">使用代码优先工作流进行开发时定义一个派生的 DbContext 来表示与数据库的会话，并公开您的模型中每个类型的 DbSet。</span><span class="sxs-lookup"><span data-stu-id="12513-103">When developing with the Code First workflow you define a derived DbContext that represents your session with the database and exposes a DbSet for each type in your model.</span></span> <span data-ttu-id="12513-104">本主题介绍您可以定义 DbSet 属性的各种方法。</span><span class="sxs-lookup"><span data-stu-id="12513-104">This topic covers the various ways you can define the DbSet properties.</span></span>  

## <a name="dbcontext-with-dbset-properties"></a><span data-ttu-id="12513-105">使用 DbSet 属性的 DbContext</span><span class="sxs-lookup"><span data-stu-id="12513-105">DbContext with DbSet properties</span></span>  

<span data-ttu-id="12513-106">第一个代码示例所示的常见情况是模型的拥有与您的实体类型的公共自动 DbSet 属性 DbContext。</span><span class="sxs-lookup"><span data-stu-id="12513-106">The common case shown in Code First examples is to have a DbContext with public automatic DbSet properties for the entity types of your model.</span></span> <span data-ttu-id="12513-107">例如：</span><span class="sxs-lookup"><span data-stu-id="12513-107">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="12513-108">Code First 模式中使用时，这会配置为实体类型，以及配置可从这些其他类型的博客和文章。</span><span class="sxs-lookup"><span data-stu-id="12513-108">When used in Code First mode, this will configure Blogs and Posts as entity types, as well as configuring other types reachable from these.</span></span> <span data-ttu-id="12513-109">此外 DbContext 自动将这些属性来设置相应的 DbSet 的实例的每个调用 setter。</span><span class="sxs-lookup"><span data-stu-id="12513-109">In addition DbContext will automatically call the setter for each of these properties to set an instance of the appropriate DbSet.</span></span>  

## <a name="dbcontext-with-idbset-properties"></a><span data-ttu-id="12513-110">DbContext IDbSet 属性</span><span class="sxs-lookup"><span data-stu-id="12513-110">DbContext with IDbSet properties</span></span>  

<span data-ttu-id="12513-111">有些情况下，例如当创建模拟或 fakes，其中很多有用声明您使用的接口的组属性。</span><span class="sxs-lookup"><span data-stu-id="12513-111">There are situations, such as when creating mocks or fakes, where it is more useful to declare your set properties using an interface.</span></span> <span data-ttu-id="12513-112">在这种情况下 IDbSet 接口可以代替 DbSet。</span><span class="sxs-lookup"><span data-stu-id="12513-112">In such cases the IDbSet interface can be used in place of DbSet.</span></span> <span data-ttu-id="12513-113">例如：</span><span class="sxs-lookup"><span data-stu-id="12513-113">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="12513-114">此上下文的工作方式完全相同的方式为使用 DbSet 类对于其设置的属性的上下文中。</span><span class="sxs-lookup"><span data-stu-id="12513-114">This context works in exactly the same way as the context that uses the DbSet class for its set properties.</span></span>  

## <a name="dbcontext-with-read-only-set-properties"></a><span data-ttu-id="12513-115">使用只读的 DbContext 设置属性</span><span class="sxs-lookup"><span data-stu-id="12513-115">DbContext with read-only set properties</span></span>  

<span data-ttu-id="12513-116">如果不希望公开 DbSet 或 IDbSet 属性而是可以创建只读属性，然后自行创建集实例的公共 setter。</span><span class="sxs-lookup"><span data-stu-id="12513-116">If you do not wish to expose public setters for your DbSet or IDbSet properties you can instead create read-only properties and create the set instances yourself.</span></span> <span data-ttu-id="12513-117">例如：</span><span class="sxs-lookup"><span data-stu-id="12513-117">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs
    {
        get { return Set<Blog>(); }
    }

    public DbSet<Post> Posts
    {
        get { return Set<Post>(); }
    }
}
```  

<span data-ttu-id="12513-118">请注意，DbContext 缓存，以便每个属性将返回相同的实例，每次调用时它从 Set 方法返回的 DbSet 的实例。</span><span class="sxs-lookup"><span data-stu-id="12513-118">Note that DbContext caches the instance of DbSet returned from the Set method so that each of these properties will return the same instance every time it is called.</span></span>  

<span data-ttu-id="12513-119">相同的方式中的 Code First 工作原理的实体类型的发现像它那样具有公共 getter 和 setter 的属性。</span><span class="sxs-lookup"><span data-stu-id="12513-119">Discovery of entity types for Code First works in the same way here as it does for properties with public getters and setters.</span></span>  
