---
title: 自动检测到更改-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a8d1488d-9a54-4623-a76b-e81329ff2756
ms.openlocfilehash: 9af85fd7ca48a14432a1f33c59079fc438ef8810
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490982"
---
# <a name="automatic-detect-changes"></a><span data-ttu-id="4d63b-102">自动检测更改</span><span class="sxs-lookup"><span data-stu-id="4d63b-102">Automatic detect changes</span></span>
<span data-ttu-id="4d63b-103">使用大多数的 POCO 实体时由检测更改算法处理的实体的更改方式 （并因此需要发送到数据库的更新） 确定。</span><span class="sxs-lookup"><span data-stu-id="4d63b-103">When using most POCO entities the determination of how an entity has changed (and therefore which updates need to be sent to the database) is handled by the Detect Changes algorithm.</span></span> <span data-ttu-id="4d63b-104">通过检测该实体的当前属性值和当查询或附加该实体已在快照中存储的原始属性值之间的差异来检测更改的工作原理。</span><span class="sxs-lookup"><span data-stu-id="4d63b-104">Detect Changes works by detecting the differences between the current property values of the entity and the original property values that are stored in a snapshot when the entity was queried or attached.</span></span> <span data-ttu-id="4d63b-105">本主题所介绍的方法同样适用于查询使用 Code First 和 EF 设计器创建的模型。</span><span class="sxs-lookup"><span data-stu-id="4d63b-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="4d63b-106">默认情况下，实体框架会自动执行检测更改，当调用以下方法：</span><span class="sxs-lookup"><span data-stu-id="4d63b-106">By default, Entity Framework performs Detect Changes automatically when the following methods are called:</span></span>  

- <span data-ttu-id="4d63b-107">DbSet.Find</span><span class="sxs-lookup"><span data-stu-id="4d63b-107">DbSet.Find</span></span>  
- <span data-ttu-id="4d63b-108">DbSet.Local</span><span class="sxs-lookup"><span data-stu-id="4d63b-108">DbSet.Local</span></span>  
- <span data-ttu-id="4d63b-109">DbSet.Add</span><span class="sxs-lookup"><span data-stu-id="4d63b-109">DbSet.Add</span></span>  
- <span data-ttu-id="4d63b-110">DbSet.AddRange</span><span class="sxs-lookup"><span data-stu-id="4d63b-110">DbSet.AddRange</span></span>
- <span data-ttu-id="4d63b-111">DbSet.Remove</span><span class="sxs-lookup"><span data-stu-id="4d63b-111">DbSet.Remove</span></span>  
- <span data-ttu-id="4d63b-112">DbSet.RemoveRange</span><span class="sxs-lookup"><span data-stu-id="4d63b-112">DbSet.RemoveRange</span></span>
- <span data-ttu-id="4d63b-113">DbSet.Attach</span><span class="sxs-lookup"><span data-stu-id="4d63b-113">DbSet.Attach</span></span>  
- <span data-ttu-id="4d63b-114">DbContext.SaveChanges</span><span class="sxs-lookup"><span data-stu-id="4d63b-114">DbContext.SaveChanges</span></span>  
- <span data-ttu-id="4d63b-115">DbContext.GetValidationErrors</span><span class="sxs-lookup"><span data-stu-id="4d63b-115">DbContext.GetValidationErrors</span></span>  
- <span data-ttu-id="4d63b-116">DbContext.Entry</span><span class="sxs-lookup"><span data-stu-id="4d63b-116">DbContext.Entry</span></span>  
- <span data-ttu-id="4d63b-117">DbChangeTracker.Entries</span><span class="sxs-lookup"><span data-stu-id="4d63b-117">DbChangeTracker.Entries</span></span>  

## <a name="disabling-automatic-detection-of-changes"></a><span data-ttu-id="4d63b-118">禁用自动检测更改</span><span class="sxs-lookup"><span data-stu-id="4d63b-118">Disabling automatic detection of changes</span></span>  

<span data-ttu-id="4d63b-119">如果您正在跟踪大量实体在上下文中并调用下列方法之一很多时候在循环中，然后可能会显著的性能改进通过关闭更改检测循环的持续时间。</span><span class="sxs-lookup"><span data-stu-id="4d63b-119">If you are tracking a lot of entities in your context and you call one of these methods many times in a loop, then you may get significant performance improvements by turning off detection of changes for the duration of the loop.</span></span> <span data-ttu-id="4d63b-120">例如：</span><span class="sxs-lookup"><span data-stu-id="4d63b-120">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    try
    {
        context.Configuration.AutoDetectChangesEnabled = false;

        // Make many calls in a loop
        foreach (var blog in aLotOfBlogs)
        {
            context.Blogs.Add(blog);
        }
    }
    finally
    {
        context.Configuration.AutoDetectChangesEnabled = true;
    }
}
```  

<span data-ttu-id="4d63b-121">不要忘记重新启用更改检测循环后的，我们使用了 try/finally 来确保始终重新启用即使循环中的代码引发异常。</span><span class="sxs-lookup"><span data-stu-id="4d63b-121">Don’t forget to re-enable detection of changes after the loop — We've used a try/finally to ensure it is always re-enabled even if code in the loop throws an exception.</span></span>  

<span data-ttu-id="4d63b-122">除了禁用和重新启用是将自动在所有时间和任一调用上下文已关闭的更改检测。ChangeTracker.DetectChanges 显式或使用更改跟踪代理努力。</span><span class="sxs-lookup"><span data-stu-id="4d63b-122">An alternative to disabling and re-enabling is to leave automatic detection of changes turned off at all times and either call context.ChangeTracker.DetectChanges explicitly or use change tracking proxies diligently.</span></span> <span data-ttu-id="4d63b-123">这两种选项都高级和可以轻松地细微 bug 引入你的应用程序因此请谨慎使用它们。</span><span class="sxs-lookup"><span data-stu-id="4d63b-123">Both of these options are advanced and can easily introduce subtle bugs into your application so use them with care.</span></span>  

<span data-ttu-id="4d63b-124">如果需要添加或删除许多对象上下文中，请考虑使用 DbSet.AddRange 和 DbSet.RemoveRange。</span><span class="sxs-lookup"><span data-stu-id="4d63b-124">If you need to add or remove many objects from a context, consider using DbSet.AddRange and DbSet.RemoveRange.</span></span> <span data-ttu-id="4d63b-125">此方法会自动检测更改一次后添加或删除操作都已完成。</span><span class="sxs-lookup"><span data-stu-id="4d63b-125">This methods automatically detect changes only once after the add or remove operations are completed.</span></span> 
