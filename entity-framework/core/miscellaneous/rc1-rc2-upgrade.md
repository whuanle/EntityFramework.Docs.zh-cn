---
title: 从 EF Core 1.0 RC1 升级到 RC2-EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
ms.technology: entity-framework-core
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: e76886729248101ccac024568cf9abcd945fca33
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678622"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a><span data-ttu-id="bd630-102">从 EF Core 1.0 RC1 升级到 1.0 RC2</span><span class="sxs-lookup"><span data-stu-id="bd630-102">Upgrading from EF Core 1.0 RC1 to 1.0 RC2</span></span>

<span data-ttu-id="bd630-103">本文提供了将用到 RC2 RC1 包生成应用程序迁移指南。</span><span class="sxs-lookup"><span data-stu-id="bd630-103">This article provides guidance for moving an application built with the RC1 packages to RC2.</span></span>

## <a name="package-names-and-versions"></a><span data-ttu-id="bd630-104">包名称和版本</span><span class="sxs-lookup"><span data-stu-id="bd630-104">Package Names and Versions</span></span>

<span data-ttu-id="bd630-105">RC1，RC2，我们从更改"实体框架 7"为"实体框架 Core"。</span><span class="sxs-lookup"><span data-stu-id="bd630-105">Between RC1 and RC2, we changed from "Entity Framework 7" to "Entity Framework Core".</span></span> <span data-ttu-id="bd630-106">你可以阅读更多有关的更改的原因[由 Scott Hanselman 的此博客文章](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx)。</span><span class="sxs-lookup"><span data-stu-id="bd630-106">You can read more about the reasons for the change in [this post by Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span></span> <span data-ttu-id="bd630-107">由于此更改，我们包名称更改从`EntityFramework.*`到`Microsoft.EntityFrameworkCore.*`和我们版本的`7.0.0-rc1-final`到`1.0.0-rc2-final`(或`1.0.0-preview1-final`程序工具)。</span><span class="sxs-lookup"><span data-stu-id="bd630-107">Because of this change, our package names changed from `EntityFramework.*` to `Microsoft.EntityFrameworkCore.*` and our versions from `7.0.0-rc1-final` to `1.0.0-rc2-final` (or `1.0.0-preview1-final` for tooling).</span></span>

<span data-ttu-id="bd630-108">**你将需要完全删除 RC1 包，然后安装 RC2 的。**</span><span class="sxs-lookup"><span data-stu-id="bd630-108">**You will need to completely remove the RC1 packages and then install the RC2 ones.**</span></span> <span data-ttu-id="bd630-109">下面是一些常见的包的映射。</span><span class="sxs-lookup"><span data-stu-id="bd630-109">Here is the mapping for some common packages.</span></span>

| <span data-ttu-id="bd630-110">RC1 包</span><span class="sxs-lookup"><span data-stu-id="bd630-110">RC1 Package</span></span>                                               | <span data-ttu-id="bd630-111">RC2 等效项</span><span class="sxs-lookup"><span data-stu-id="bd630-111">RC2 Equivalent</span></span>                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| <span data-ttu-id="bd630-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="bd630-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span></span> | <span data-ttu-id="bd630-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="bd630-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="bd630-114">EntityFramework.SQLite                    7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="bd630-114">EntityFramework.SQLite                    7.0.0-rc1-final</span></span> | <span data-ttu-id="bd630-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="bd630-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="bd630-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span><span class="sxs-lookup"><span data-stu-id="bd630-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span></span>     | <span data-ttu-id="bd630-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span><span class="sxs-lookup"><span data-stu-id="bd630-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span></span>      |
| <span data-ttu-id="bd630-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="bd630-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span></span> | <span data-ttu-id="bd630-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="bd630-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="bd630-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="bd630-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span></span> | <span data-ttu-id="bd630-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="bd630-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="bd630-122">EntityFramework.InMemory                  7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="bd630-122">EntityFramework.InMemory                  7.0.0-rc1-final</span></span> | <span data-ttu-id="bd630-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="bd630-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="bd630-124">EntityFramework.IBMDataServer             7.0.0-beta1</span><span class="sxs-lookup"><span data-stu-id="bd630-124">EntityFramework.IBMDataServer             7.0.0-beta1</span></span>     | <span data-ttu-id="bd630-125">Rc2 尚不可用</span><span class="sxs-lookup"><span data-stu-id="bd630-125">Not yet available for RC2</span></span>                                            |
| <span data-ttu-id="bd630-126">EntityFramework.Commands                  7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="bd630-126">EntityFramework.Commands                  7.0.0-rc1-final</span></span> | <span data-ttu-id="bd630-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span><span class="sxs-lookup"><span data-stu-id="bd630-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span></span> |
| <span data-ttu-id="bd630-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="bd630-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span></span> | <span data-ttu-id="bd630-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="bd630-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span></span>      |

## <a name="namespaces"></a><span data-ttu-id="bd630-130">命名空间</span><span class="sxs-lookup"><span data-stu-id="bd630-130">Namespaces</span></span>

<span data-ttu-id="bd630-131">命名空间从包名称，以及更改`Microsoft.Data.Entity.*`到`Microsoft.EntityFrameworkCore.*`。</span><span class="sxs-lookup"><span data-stu-id="bd630-131">Along with package names, namespaces changed from `Microsoft.Data.Entity.*` to `Microsoft.EntityFrameworkCore.*`.</span></span> <span data-ttu-id="bd630-132">你可以处理此更改与查找/替换的`using Microsoft.Data.Entity`与`using Microsoft.EntityFrameworkCore`。</span><span class="sxs-lookup"><span data-stu-id="bd630-132">You can handle this change with a find/replace of `using Microsoft.Data.Entity` with `using Microsoft.EntityFrameworkCore`.</span></span>

## <a name="table-naming-convention-changes"></a><span data-ttu-id="bd630-133">命名约定更改的表</span><span class="sxs-lookup"><span data-stu-id="bd630-133">Table Naming Convention Changes</span></span>

<span data-ttu-id="bd630-134">我们在 RC2 中所用的一个重要的功能变化是使用的名称`DbSet<TEntity>`属性为给定实体为表名它将映射到，而不是只是类的名称。</span><span class="sxs-lookup"><span data-stu-id="bd630-134">A significant functional change we took in RC2 was to use the name of the `DbSet<TEntity>` property for a given entity as the table name it maps to, rather than just the class name.</span></span> <span data-ttu-id="bd630-135">你可以阅读更多有关这一更改[相关的公告问题](https://github.com/aspnet/Announcements/issues/167)。</span><span class="sxs-lookup"><span data-stu-id="bd630-135">You can read more about this change in [the related announcement issue](https://github.com/aspnet/Announcements/issues/167).</span></span>

<span data-ttu-id="bd630-136">对于现有 RC1 应用程序，我们建议将以下代码添加到开始你`OnModelCreating`方法以保持 RC1 命名策略：</span><span class="sxs-lookup"><span data-stu-id="bd630-136">For existing RC1 applications, we recommend adding the following code to the start of your `OnModelCreating` method to keep the RC1 naming strategy:</span></span>

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

<span data-ttu-id="bd630-137">如果你想要采用新的命名策略，我们建议成功完成的升级步骤然后删除代码并创建要应用的表的迁移的其余部分将重命名。</span><span class="sxs-lookup"><span data-stu-id="bd630-137">If you want to adopt the new naming strategy, we would recommend successfully completing the rest of the upgrade steps and then removing the code and creating a migration to apply the table renames.</span></span>

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a><span data-ttu-id="bd630-138">AddDbContext / Startup.cs 更改 （仅适用于 ASP.NET Core 项目）</span><span class="sxs-lookup"><span data-stu-id="bd630-138">AddDbContext / Startup.cs Changes (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="bd630-139">在 RC1，你必须将实体框架服务添加到应用程序服务提供商，在`Startup.ConfigureServices(...)`:</span><span class="sxs-lookup"><span data-stu-id="bd630-139">In RC1, you had to add Entity Framework services to the application service provider - in `Startup.ConfigureServices(...)`:</span></span>

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="bd630-140">在 RC2，你可以删除对的调用`AddEntityFramework()`， `AddSqlServer()`，等等。:</span><span class="sxs-lookup"><span data-stu-id="bd630-140">In RC2, you can remove the calls to `AddEntityFramework()`, `AddSqlServer()`, etc.:</span></span>

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="bd630-141">你还需要添加一个构造函数，为您派生的上下文，，它接受上下文选项并将其传递给基构造函数。</span><span class="sxs-lookup"><span data-stu-id="bd630-141">You also need to add a constructor, to your derived context, that takes context options and passes them to the base constructor.</span></span> <span data-ttu-id="bd630-142">这被必须的因为我们删除了一些在后台 snuck 在可怕幻数：</span><span class="sxs-lookup"><span data-stu-id="bd630-142">This is needed because we removed some of the scary magic that snuck them in behind the scenes:</span></span>

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a><span data-ttu-id="bd630-143">IServiceProvider 传入</span><span class="sxs-lookup"><span data-stu-id="bd630-143">Passing in an IServiceProvider</span></span>

<span data-ttu-id="bd630-144">如果必须通过的 RC1 代码`IServiceProvider`到上下文，这现在已移到`DbContextOptions`，而不是单独的构造函数参数。</span><span class="sxs-lookup"><span data-stu-id="bd630-144">If you have RC1 code that passes an `IServiceProvider` to the context, this has now moved to `DbContextOptions`, rather than being a separate constructor parameter.</span></span> <span data-ttu-id="bd630-145">使用`DbContextOptionsBuilder.UseInternalServiceProvider(...)`设置服务提供程序。</span><span class="sxs-lookup"><span data-stu-id="bd630-145">Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` to set the service provider.</span></span>

### <a name="testing"></a><span data-ttu-id="bd630-146">正在测试</span><span class="sxs-lookup"><span data-stu-id="bd630-146">Testing</span></span>

<span data-ttu-id="bd630-147">执行此操作的最常见方案是在测试时控制 InMemory 数据库的范围。</span><span class="sxs-lookup"><span data-stu-id="bd630-147">The most common scenario for doing this was to control the scope of an InMemory database when testing.</span></span> <span data-ttu-id="bd630-148">请参阅更新[测试](testing/index.md)文，以执行此操作使用 RC2 的示例。</span><span class="sxs-lookup"><span data-stu-id="bd630-148">See the updated [Testing](testing/index.md) article for an example of doing this with RC2.</span></span>

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a><span data-ttu-id="bd630-149">解析内部服务从应用程序服务提供商 （仅适用于 ASP.NET Core 项目）</span><span class="sxs-lookup"><span data-stu-id="bd630-149">Resolving Internal Services from Application Service Provider (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="bd630-150">如果你有一个 ASP.NET Core 应用并且想 EF 若要解决应用程序服务提供商提供的内部服务，则的重载`AddDbContext`，可以对此进行配置：</span><span class="sxs-lookup"><span data-stu-id="bd630-150">If you have an ASP.NET Core application and you want EF to resolve internal services from the application service provider, there is an overload of `AddDbContext` that allows you to configure this:</span></span>

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> <span data-ttu-id="bd630-151">我们建议允许 EF 内部管理其自身的服务，除非你有其他原因将内部 EF 服务合并到你的应用程序服务提供商。</span><span class="sxs-lookup"><span data-stu-id="bd630-151">We recommend allowing EF to internally manage its own services, unless you have a reason to combine the internal EF services into your application service provider.</span></span> <span data-ttu-id="bd630-152">你可能想要执行此操作的主要原因是使用你的应用程序服务提供商来替换 EF 在内部使用的服务</span><span class="sxs-lookup"><span data-stu-id="bd630-152">The main reason you may want to do this is to use your application service provider to replace services that EF uses internally</span></span>

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a><span data-ttu-id="bd630-153">DNX 命令 = >.NET CLI （仅适用于 ASP.NET Core 项目）</span><span class="sxs-lookup"><span data-stu-id="bd630-153">DNX Commands => .NET CLI (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="bd630-154">如果你以前使用`dnx ef`对于 ASP.NET 5 项目的命令，这些现在已移到`dotnet ef`命令。</span><span class="sxs-lookup"><span data-stu-id="bd630-154">If you previously used the `dnx ef` commands for ASP.NET 5 projects, these have now moved to `dotnet ef` commands.</span></span> <span data-ttu-id="bd630-155">相同的命令语法仍然适用。</span><span class="sxs-lookup"><span data-stu-id="bd630-155">The same command syntax still applies.</span></span> <span data-ttu-id="bd630-156">你可以使用`dotnet ef --help`语法信息。</span><span class="sxs-lookup"><span data-stu-id="bd630-156">You can use `dotnet ef --help` for syntax information.</span></span>

<span data-ttu-id="bd630-157">注册命令的方式已更改 RC2，由于.NET CLI 被取代的 DNX 中。</span><span class="sxs-lookup"><span data-stu-id="bd630-157">The way commands are registered has changed in RC2, due to DNX being replaced by .NET CLI.</span></span> <span data-ttu-id="bd630-158">在现在注册命令`tools`主题中`project.json`:</span><span class="sxs-lookup"><span data-stu-id="bd630-158">Commands are now registered in a `tools` section in `project.json`:</span></span>

``` json
"tools": {
  "Microsoft.EntityFrameworkCore.Tools": {
    "version": "1.0.0-preview1-final",
    "imports": [
      "portable-net45+win8+dnxcore50",
      "portable-net45+win8"
    ]
  }
}
```

> [!TIP]  
> <span data-ttu-id="bd630-159">如果你使用 Visual Studio，你现在可以使用程序包管理器控制台运行 ASP.NET Core项目 （这不支持在 RC1 中） 的 EF 命令。</span><span class="sxs-lookup"><span data-stu-id="bd630-159">If you use Visual Studio, you can now use Package Manager Console to run EF commands for ASP.NET Core projects (this was not supported in RC1).</span></span> <span data-ttu-id="bd630-160">你仍需要注册中的命令`tools`部分`project.json`要这样做。</span><span class="sxs-lookup"><span data-stu-id="bd630-160">You still need to register the commands in the `tools` section of `project.json` to do this.</span></span>

## <a name="package-manager-commands-require-powershell-5"></a><span data-ttu-id="bd630-161">包管理器命令需要 PowerShell 5</span><span class="sxs-lookup"><span data-stu-id="bd630-161">Package Manager Commands Require PowerShell 5</span></span>

<span data-ttu-id="bd630-162">如果在 Visual Studio 中的包管理器控制台中使用实体框架命令，你将需要确保已安装的 PowerShell 5。</span><span class="sxs-lookup"><span data-stu-id="bd630-162">If you use the Entity Framework commands in Package Manager Console in Visual Studio, then you will need to ensure you have PowerShell 5 installed.</span></span> <span data-ttu-id="bd630-163">这是在下一个版本中将删除的临时要求 (请参阅[发出 #5327](https://github.com/aspnet/EntityFramework/issues/5327)有关详细信息)。</span><span class="sxs-lookup"><span data-stu-id="bd630-163">This is a temporary requirement that will be removed in the next release (see [issue #5327](https://github.com/aspnet/EntityFramework/issues/5327) for more details).</span></span>

## <a name="using-imports-in-projectjson"></a><span data-ttu-id="bd630-164">Project.json 中使用"导入"</span><span class="sxs-lookup"><span data-stu-id="bd630-164">Using "imports" in project.json</span></span>

<span data-ttu-id="bd630-165">一些 EF Core依赖关系不支持.NET 标准尚未。</span><span class="sxs-lookup"><span data-stu-id="bd630-165">Some of EF Core's dependencies do not support .NET Standard yet.</span></span> <span data-ttu-id="bd630-166">标准.NET 和.NET Core项目中的 EF Core可能会要求添加"导入"到 project.json 临时的解决方法。</span><span class="sxs-lookup"><span data-stu-id="bd630-166">EF Core in .NET Standard and .NET Core projects may require adding "imports" to project.json as a temporary workaround.</span></span>

<span data-ttu-id="bd630-167">在添加 EF，NuGet 还原将显示此错误消息：</span><span class="sxs-lookup"><span data-stu-id="bd630-167">When adding EF, NuGet restore will display this error message:</span></span>

``` Console
Package Ix-Async 1.2.5 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Ix-Async 1.2.5 supports:
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8 (.NETPortable,Version=v0.0,Profile=Profile78)
Package Remotion.Linq 2.0.2 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Remotion.Linq 2.0.2 supports:
  - net35 (.NETFramework,Version=v3.5)
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8+wpa81 (.NETPortable,Version=v0.0,Profile=Profile259)
```

<span data-ttu-id="bd630-168">解决方法是手动导入"可移植 net451 + win8"的可移植配置文件。</span><span class="sxs-lookup"><span data-stu-id="bd630-168">The workaround is to manually import the portable profile "portable-net451+win8".</span></span> <span data-ttu-id="bd630-169">此强制将匹配此此二进制文件的 NuGet 提供作为.NET standard 兼容框架，即使它们不是。</span><span class="sxs-lookup"><span data-stu-id="bd630-169">This forces NuGet to treat this binaries that match this provide as a compatible framework with .NET Standard, even though they are not.</span></span> <span data-ttu-id="bd630-170">虽然"可移植 net451 + win8"不是 100%兼容.NET 标准，它是兼容的 PCL 从转换为.NET 标准。</span><span class="sxs-lookup"><span data-stu-id="bd630-170">Although "portable-net451+win8" is not 100% compatible with .NET Standard, it is compatible enough for the transition from PCL to .NET Standard.</span></span> <span data-ttu-id="bd630-171">EF 的依赖关系最终将升级到.NET Standard 时，可能会删除导入。</span><span class="sxs-lookup"><span data-stu-id="bd630-171">Imports can be removed when EF's dependencies eventually upgrade to .NET Standard.</span></span>

<span data-ttu-id="bd630-172">数组语法中，可以将多个框架添加到"导入"。</span><span class="sxs-lookup"><span data-stu-id="bd630-172">Multiple frameworks can be added to "imports" in array syntax.</span></span> <span data-ttu-id="bd630-173">如果将其他库添加到你的项目，则可能需要采用其他导入。</span><span class="sxs-lookup"><span data-stu-id="bd630-173">Other imports may be necessary if you add additional libraries to your project.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

<span data-ttu-id="bd630-174">请参阅[发出 #5176](https://github.com/aspnet/EntityFramework/issues/5176)。</span><span class="sxs-lookup"><span data-stu-id="bd630-174">See [Issue #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span></span>
