---
title: "设计时 DbContext 创建-EF 核心"
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 5fcd9e362d76127e7acadd9e552ef3ac90967a37
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
---
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="93415-102">设计时 DbContext 创建</span><span class="sxs-lookup"><span data-stu-id="93415-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="93415-103">一些命令都要求在设计要创建的 DbContext 实例的 EF 工具的时间 （例如，当运行迁移命令）。</span><span class="sxs-lookup"><span data-stu-id="93415-103">Some of the EF Tools commands require a DbContext instance to be created at design time (for example, when running Migrations commands).</span></span> <span data-ttu-id="93415-104">有多种工具尝试创建它的方法。</span><span class="sxs-lookup"><span data-stu-id="93415-104">There are various ways the tools try to create it.</span></span>

<a name="from-application-services"></a><span data-ttu-id="93415-105">从应用程序服务</span><span class="sxs-lookup"><span data-stu-id="93415-105">From application services</span></span>
-------------------------
<span data-ttu-id="93415-106">如果你的启动项目，ASP.NET Core 应用工具将尝试从应用程序的服务提供程序获取 DbContext 对象。</span><span class="sxs-lookup"><span data-stu-id="93415-106">If your startup project is an ASP.NET Core app, the tools try to obtain the DbContext object from the application's service provider.</span></span> <span data-ttu-id="93415-107">它们通过调用获取该`Program.BuildWebHost()`和访问`IWebHost.Services`属性。</span><span class="sxs-lookup"><span data-stu-id="93415-107">They obtain it by invoking `Program.BuildWebHost()` and accessing the `IWebHost.Services` property.</span></span> <span data-ttu-id="93415-108">使用任何 DbContext 注册`IServiceCollection.AddDbContext<TContext>()`可以找出并创建这种方式。</span><span class="sxs-lookup"><span data-stu-id="93415-108">Any DbContext registered using `IServiceCollection.AddDbContext<TContext>()` can be found and created this way.</span></span> <span data-ttu-id="93415-109">此模式已[在 ASP.NET 核心 2.0 中引入][1]</span><span class="sxs-lookup"><span data-stu-id="93415-109">This pattern was [introduced in ASP.NET Core 2.0][1]</span></span>

<a name="using-the-default-constructor"></a><span data-ttu-id="93415-110">使用默认构造函数</span><span class="sxs-lookup"><span data-stu-id="93415-110">Using the default constructor</span></span>
-----------------------------
<span data-ttu-id="93415-111">如果无法从应用程序服务提供程序获取 DbContext，工具查找该项目中的 DbContext 类型。</span><span class="sxs-lookup"><span data-stu-id="93415-111">If the DbContext can't be obtained from the application service provider, the tools look for the DbContext type inside the project.</span></span> <span data-ttu-id="93415-112">它们尝试使用其默认构造函数创建它。</span><span class="sxs-lookup"><span data-stu-id="93415-112">They try to create it using its default constructor.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="93415-113">从设计时工厂</span><span class="sxs-lookup"><span data-stu-id="93415-113">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="93415-114">您还可以判断工具如何通过实现来创建你 DbContext `IDesignTimeDbContextFactory`。</span><span class="sxs-lookup"><span data-stu-id="93415-114">You can also tell the tools how to create your DbContext by implementing `IDesignTimeDbContextFactory`.</span></span> <span data-ttu-id="93415-115">如果你的项目中找到了实现此接口的类，这些工具绕过的其他方式创建的 DbContext。</span><span class="sxs-lookup"><span data-stu-id="93415-115">If a class implementing this interface is found inside your project, the tools bypass the other ways of creating the DbContext.</span></span>
<span data-ttu-id="93415-116">它们始终在设计时使用的工厂。</span><span class="sxs-lookup"><span data-stu-id="93415-116">They always use the factory at design time.</span></span> <span data-ttu-id="93415-117">工厂是如果你需要为设计时比在运行时以不同的方式配置 DbContext 特别有用。</span><span class="sxs-lookup"><span data-stu-id="93415-117">A factory is especially useful if you need to configure the DbContext differently for design time than at runtime.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Infrastructure;

namespace MyProject
{
    public class BloggingContextFactory : IDesignTimeDbContextFactory<BloggingContext>
    {
        public BloggingContext CreateDbContext(string[] args)
        {
            var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
            optionsBuilder.UseSqlite("Data Source=blog.db");

            return new BloggingContext(optionsBuilder.Options);
        }
    }
}
```

> [!NOTE]
> <span data-ttu-id="93415-118">`args`参数是当前未使用。</span><span class="sxs-lookup"><span data-stu-id="93415-118">The `args` parameter is currently unused.</span></span> <span data-ttu-id="93415-119">没有[问题][ 2]跟踪指定的工具的设计时自变量的功能。</span><span class="sxs-lookup"><span data-stu-id="93415-119">There is [an issue][2] tracking the ability to specify design-time arguments from the tools.</span></span>

  [1]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [2]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
