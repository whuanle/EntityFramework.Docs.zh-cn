---
title: 使用代理的 EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 869ee4dc-06f1-471d-8e0e-0a1a2bc59c30
caps.latest.revision: 3
ms.openlocfilehash: 4632e246d28a3cd53dabe5ac76e44f4538739abc
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/08/2018
ms.locfileid: "39119976"
---
# <a name="working-with-proxies"></a><span data-ttu-id="509be-102">使用代理</span><span class="sxs-lookup"><span data-stu-id="509be-102">Working with proxies</span></span>
<span data-ttu-id="509be-103">创建 POCO 实体类型的实例时，实体框架通常会创建动态生成的派生类型的代理的实体的实例。</span><span class="sxs-lookup"><span data-stu-id="509be-103">When creating instances of POCO entity types, Entity Framework often creates instances of a dynamically generated derived type that acts as a proxy for the entity.</span></span> <span data-ttu-id="509be-104">此代理将覆盖要插入挂钩，以便访问属性时自动执行的操作的实体的某些虚拟属性。</span><span class="sxs-lookup"><span data-stu-id="509be-104">This proxy overrides some virtual properties of the entity to insert hooks for performing actions automatically when the property is accessed.</span></span> <span data-ttu-id="509be-105">例如，这种机制用于支持延迟加载的关系。</span><span class="sxs-lookup"><span data-stu-id="509be-105">For example, this mechanism is used to support lazy loading of relationships.</span></span> <span data-ttu-id="509be-106">本主题所介绍的方法同样适用于查询使用 Code First 和 EF 设计器创建的模型。</span><span class="sxs-lookup"><span data-stu-id="509be-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="disabling-proxy-creation"></a><span data-ttu-id="509be-107">禁用代理创建</span><span class="sxs-lookup"><span data-stu-id="509be-107">Disabling proxy creation</span></span>  

<span data-ttu-id="509be-108">有时，它可用于防止 Entity Framework 创建代理实例了。</span><span class="sxs-lookup"><span data-stu-id="509be-108">Sometimes it is useful to prevent Entity Framework from creating proxy instances.</span></span> <span data-ttu-id="509be-109">例如，序列化非代理实例比更容易得多序列化代理实例。</span><span class="sxs-lookup"><span data-stu-id="509be-109">For example, serializing non-proxy instances is considerably easier than serializing proxy instances.</span></span> <span data-ttu-id="509be-110">可以通过清除 ProxyCreationEnabled 标志关闭代理创建。</span><span class="sxs-lookup"><span data-stu-id="509be-110">Proxy creation can be turned off by clearing the ProxyCreationEnabled flag.</span></span> <span data-ttu-id="509be-111">无法执行此操作的一个位置是上下文的您的构造函数中。</span><span class="sxs-lookup"><span data-stu-id="509be-111">One place you could do this is in the constructor of your context.</span></span> <span data-ttu-id="509be-112">例如：</span><span class="sxs-lookup"><span data-stu-id="509be-112">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.ProxyCreationEnabled = false;
    }  

    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="509be-113">请注意，EF 将不会创建代理类型没有任何关于要执行的代理的信息。</span><span class="sxs-lookup"><span data-stu-id="509be-113">Note that the EF will not create proxies for types where there is nothing for the proxy to do.</span></span> <span data-ttu-id="509be-114">这意味着您可以通过采用类型都密封的和/或不包含虚拟属性中还避免代理。</span><span class="sxs-lookup"><span data-stu-id="509be-114">This means that you can also avoid proxies by having types that are sealed and/or have no virtual properties.</span></span>  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a><span data-ttu-id="509be-115">显式创建代理的实例</span><span class="sxs-lookup"><span data-stu-id="509be-115">Explicitly creating an instance of a proxy</span></span>  

<span data-ttu-id="509be-116">如果创建的实体使用 new 运算符实例，将无法创建代理实例。</span><span class="sxs-lookup"><span data-stu-id="509be-116">A proxy instance will not be created if you create an instance of an entity using the new operator.</span></span> <span data-ttu-id="509be-117">这可能不是问题，但是如果您需要创建代理实例 （例如，以便延迟加载或代理更改跟踪将起作用），可以实现使用 DbSet 的 Create 方法。</span><span class="sxs-lookup"><span data-stu-id="509be-117">This may not be a problem, but if you need to create a proxy instance (for example, so that lazy loading or proxy change tracking will work) then you can do so using the Create method of DbSet.</span></span> <span data-ttu-id="509be-118">例如：</span><span class="sxs-lookup"><span data-stu-id="509be-118">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

<span data-ttu-id="509be-119">可以使用创建的泛型版本，如果你想要创建派生的实体类型的实例。</span><span class="sxs-lookup"><span data-stu-id="509be-119">The generic version of Create can be used if you want to create an instance of a derived entity type.</span></span> <span data-ttu-id="509be-120">例如：</span><span class="sxs-lookup"><span data-stu-id="509be-120">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

<span data-ttu-id="509be-121">请注意，Create 方法不会不添加或创建的实体附加到上下文。</span><span class="sxs-lookup"><span data-stu-id="509be-121">Note that the Create method does not add or attach the created entity to the context.</span></span>  

<span data-ttu-id="509be-122">请注意，如果创建代理类型的实体，Create 方法将只需创建实体类型本身的实例必须没有值，因为它不会执行任何操作。</span><span class="sxs-lookup"><span data-stu-id="509be-122">Note that the Create method will just create an instance of the entity type itself if creating a proxy type for the entity would have no value because it would not do anything.</span></span> <span data-ttu-id="509be-123">例如，如果实体类型密封的并且/或者没有虚拟属性创建只需将创建实体类型的实例。</span><span class="sxs-lookup"><span data-stu-id="509be-123">For example, if the entity type is sealed and/or has no virtual properties then Create will just create an instance of the entity type.</span></span>  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a><span data-ttu-id="509be-124">从代理类型中获取的实际实体类型</span><span class="sxs-lookup"><span data-stu-id="509be-124">Getting the actual entity type from a proxy type</span></span>  

<span data-ttu-id="509be-125">代理类型具有名称如下所示：</span><span class="sxs-lookup"><span data-stu-id="509be-125">Proxy types have names that look something like this:</span></span>  

<span data-ttu-id="509be-126">System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span><span class="sxs-lookup"><span data-stu-id="509be-126">System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span></span>  

<span data-ttu-id="509be-127">您可以找到使用 GetObjectType 方法从 ObjectContext 此代理类型的实体类型。</span><span class="sxs-lookup"><span data-stu-id="509be-127">You can find the entity type for this proxy type using the GetObjectType method from ObjectContext.</span></span> <span data-ttu-id="509be-128">例如：</span><span class="sxs-lookup"><span data-stu-id="509be-128">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

<span data-ttu-id="509be-129">请注意，如果将类型传递给 GetObjectType 仍返回不是代理类型的实体类型的实体类型的一个实例。</span><span class="sxs-lookup"><span data-stu-id="509be-129">Note that if the type passed to GetObjectType is an instance of an entity type that is not a proxy type then the type of entity is still returned.</span></span> <span data-ttu-id="509be-130">这意味着你始终可以使用此方法以获取实际的实体类型，而无需任何其他检查以查看是否类型为代理类型。</span><span class="sxs-lookup"><span data-stu-id="509be-130">This means you can always use this method to get the actual entity type without any other checking to see if the type is a proxy type or not.</span></span>  
