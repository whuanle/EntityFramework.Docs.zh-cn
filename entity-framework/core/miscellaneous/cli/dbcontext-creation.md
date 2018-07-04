---
title: 设计时 DbContext 创建-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: 8b38d300d31038bdf5cd877aa3c42b7f5f97eabc
ms.sourcegitcommit: 7113e8675f26cbb546200824512078bf360225df
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
ms.locfileid: "30202479"
---
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="34fa1-102">设计时 DbContext 创建</span><span class="sxs-lookup"><span data-stu-id="34fa1-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="34fa1-103">某些 EF Core工具命令 (例如，[迁移][ 1]命令) 需要派生`DbContext`实例以收集有关应用程序的详细信息在设计时创建实体类型和它们如何映射到数据库架构。</span><span class="sxs-lookup"><span data-stu-id="34fa1-103">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="34fa1-104">在大多数情况下，最好的`DbContext`从而创建配置如何将以类似方式[在运行时配置][2]。</span><span class="sxs-lookup"><span data-stu-id="34fa1-104">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="34fa1-105">有多种方法工具尝试创建`DbContext`:</span><span class="sxs-lookup"><span data-stu-id="34fa1-105">There are various ways the tools try to create the `DbContext`:</span></span>

<a name="from-application-services"></a><span data-ttu-id="34fa1-106">从应用程序服务</span><span class="sxs-lookup"><span data-stu-id="34fa1-106">From application services</span></span>
-------------------------
<span data-ttu-id="34fa1-107">如果你的启动项目，ASP.NET Core 应用工具将尝试从应用程序的服务提供程序获取 DbContext 对象。</span><span class="sxs-lookup"><span data-stu-id="34fa1-107">If your startup project is an ASP.NET Core app, the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="34fa1-108">该工具第一次尝试通过调用获取服务提供商`Program.BuildWebHost()`和访问`IWebHost.Services`属性。</span><span class="sxs-lookup"><span data-stu-id="34fa1-108">The tool first try to obtain the service provider by invoking `Program.BuildWebHost()` and accessing the `IWebHost.Services` property.</span></span>

> [!NOTE]
> <span data-ttu-id="34fa1-109">在创建新的 ASP.NET Core 2.0 应用程序时，默认情况下包含此挂钩。</span><span class="sxs-lookup"><span data-stu-id="34fa1-109">When you create a new ASP.NET Core 2.0 application, this hook is included by default.</span></span> <span data-ttu-id="34fa1-110">在以前版本的 EF Core 和 ASP.NET Core，工具尝试调用`Startup.ConfigureServices`直接以获取应用程序的服务提供商，但此模式不能再正常 ASP.NET Core 2.0 应用程序中。</span><span class="sxs-lookup"><span data-stu-id="34fa1-110">In previous versions of EF Core and ASP.NET Core, the tools try to invoke `Startup.ConfigureServices` directly in order to obtain the application's service provider, but this pattern no longer works correctly in ASP.NET Core 2.0 applications.</span></span> <span data-ttu-id="34fa1-111">如果您正在升级到 2.0 ASP.NET Core 1.x 应用程序，则可以[修改你`Program`类遵循新模式][3]。</span><span class="sxs-lookup"><span data-stu-id="34fa1-111">If you are upgrading an ASP.NET Core 1.x application to 2.0, you can [modify your `Program` class to follow the new pattern][3].</span></span>

<span data-ttu-id="34fa1-112">`DbContext`本身以及任何依赖项在其构造函数中的需要注册为在应用程序的服务提供程序的服务。</span><span class="sxs-lookup"><span data-stu-id="34fa1-112">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="34fa1-113">这可以轻松实现通过让[上的构造函数`DbContext`采用的实例`DbContextOptions<TContext>`作为自变量][ 4]和使用[`AddDbContext<TContext>`方法][5].</span><span class="sxs-lookup"><span data-stu-id="34fa1-113">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as a argument][4] and using the [`AddDbContext<TContext>` method][5].</span></span>

<a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="34fa1-114">使用不带任何参数的构造函数</span><span class="sxs-lookup"><span data-stu-id="34fa1-114">Using a constructor with no parameters</span></span>
--------------------------------------
<span data-ttu-id="34fa1-115">如果不能从应用程序服务提供程序获得 DbContext，工具查找派生`DbContext`项目中的类型。</span><span class="sxs-lookup"><span data-stu-id="34fa1-115">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="34fa1-116">然后，它们尝试创建使用不带任何参数的构造函数实例。</span><span class="sxs-lookup"><span data-stu-id="34fa1-116">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="34fa1-117">这可以是默认构造函数，如果`DbContext`使用配置[ `OnConfiguring` ] [ 6]方法。</span><span class="sxs-lookup"><span data-stu-id="34fa1-117">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][6] method.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="34fa1-118">从设计时工厂</span><span class="sxs-lookup"><span data-stu-id="34fa1-118">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="34fa1-119">您还可以判断工具如何通过实现来创建你 DbContext`IDesignTimeDbContextFactory<TContext>`接口： 如果找到实现此接口的类中派生的同一项目`DbContext`或在应用程序的启动项目中，工具绕过改为创建的 DbContext 和使用设计时工厂的其他方法。</span><span class="sxs-lookup"><span data-stu-id="34fa1-119">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Design;
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
> <span data-ttu-id="34fa1-120">`args`参数是当前未使用。</span><span class="sxs-lookup"><span data-stu-id="34fa1-120">The `args` parameter is currently unused.</span></span> <span data-ttu-id="34fa1-121">没有[问题][ 7]跟踪指定的工具的设计时自变量的功能。</span><span class="sxs-lookup"><span data-stu-id="34fa1-121">There is [an issue][7] tracking the ability to specify design-time arguments from the tools.</span></span>

<span data-ttu-id="34fa1-122">设计时工厂可能特别有用，如果你需要配置 DbContext 以不同方式为设计时比在运行时，如果`DbContext`构造函数采用其他参数在 DI 中, 不会进行注册，如果你根本不使用 DI 或如果由于某些原因不希望让`BuildWebHost`ASP.NET Core 应用程序中的方法</span><span class="sxs-lookup"><span data-stu-id="34fa1-122">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `BuildWebHost` method in your ASP.NET Core application's</span></span>  
<span data-ttu-id="34fa1-123">`Main` 类。</span><span class="sxs-lookup"><span data-stu-id="34fa1-123">`Main` class.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
