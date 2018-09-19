---
title: 从以前的版本升级到 EF Core 2-EF Core
author: divega
ms.date: 08/13/2017
ms.assetid: 8BD43C8C-63D9-4F3A-B954-7BC518A1B7DB
uid: core/miscellaneous/1x-2x-upgrade
ms.openlocfilehash: 5371c8f3b7c6102c621296bbae145d13779e0c6e
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283766"
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a><span data-ttu-id="3d69c-102">从以前版本的应用程序升级到 EF Core 2.0</span><span class="sxs-lookup"><span data-stu-id="3d69c-102">Upgrading applications from previous versions to EF Core 2.0</span></span>

<span data-ttu-id="3d69c-103">我们已经采取了显著改进我们的现有 Api 和 2.0 中的行为的机会。</span><span class="sxs-lookup"><span data-stu-id="3d69c-103">We have taken the opportunity to significantly refine our existing APIs and behaviors in 2.0.</span></span> <span data-ttu-id="3d69c-104">可能需要修改现有应用程序代码的一些改进，尽管我们认为大多数应用程序的影响很低，在大多数情况下需要只需重新编译和最小的引导式的更改来替换已过时的 Api。</span><span class="sxs-lookup"><span data-stu-id="3d69c-104">There are a few improvements that can require modifying existing application code, although we believe that for the majority of applications the impact will be low, in most cases requiring just recompilation and minimal guided changes to replace obsolete APIs.</span></span>

<span data-ttu-id="3d69c-105">更新现有应用程序到 EF Core 2.0 可能还需要：</span><span class="sxs-lookup"><span data-stu-id="3d69c-105">Updating an existing application to EF Core 2.0 may require:</span></span>

1. <span data-ttu-id="3d69c-106">升级为支持.NET Standard 2.0 应用程序的目标.NET 实现。</span><span class="sxs-lookup"><span data-stu-id="3d69c-106">Upgrading the target .NET implementation of the application to one that supports .NET Standard 2.0.</span></span> <span data-ttu-id="3d69c-107">请参阅[支持的.NET 实现](../platforms/index.md)的更多详细信息。</span><span class="sxs-lookup"><span data-stu-id="3d69c-107">See [Supported .NET Implementations](../platforms/index.md) for more details.</span></span>

2. <span data-ttu-id="3d69c-108">标识的提供程序与 EF Core 2.0 兼容的目标数据库。</span><span class="sxs-lookup"><span data-stu-id="3d69c-108">Identify a provider for the target database which is compatible with EF Core 2.0.</span></span> <span data-ttu-id="3d69c-109">请参阅[EF Core 2.0 需要 2.0 版数据库提供程序](#ef-core-20-requires-a-20-database-provider)下面。</span><span class="sxs-lookup"><span data-stu-id="3d69c-109">See [EF Core 2.0 requires a 2.0 database provider](#ef-core-20-requires-a-20-database-provider) below.</span></span>

3. <span data-ttu-id="3d69c-110">升级到 2.0 所有 EF Core 包 （运行时和工具）。</span><span class="sxs-lookup"><span data-stu-id="3d69c-110">Upgrading all the EF Core packages (runtime and tooling) to 2.0.</span></span> <span data-ttu-id="3d69c-111">请参阅[安装 EF Core](../get-started/install/index.md) 有关详细信息。</span><span class="sxs-lookup"><span data-stu-id="3d69c-111">Refer to [Installing EF Core](../get-started/install/index.md) for more details.</span></span>

4. <span data-ttu-id="3d69c-112">进行任何必要的代码更改，以弥补此文档的其余部分中所述的重大更改。</span><span class="sxs-lookup"><span data-stu-id="3d69c-112">Make any necessary code changes to compensate for the breaking changes described in the rest of this document.</span></span>

## <a name="aspnet-core-now-includes-ef-core"></a><span data-ttu-id="3d69c-113">ASP.NET Core 现在包括 EF Core</span><span class="sxs-lookup"><span data-stu-id="3d69c-113">ASP.NET Core now includes EF Core</span></span>

<span data-ttu-id="3d69c-114">面向 ASP.NET Core 2.0 的应用程序可以使用 EF Core 2.0，而不需要第三方数据库提供程序以外的其他依赖项。</span><span class="sxs-lookup"><span data-stu-id="3d69c-114">Applications targeting ASP.NET Core 2.0 can use EF Core 2.0 without additional dependencies besides third party database providers.</span></span> <span data-ttu-id="3d69c-115">但是，面向以前版本的 ASP.NET Core 应用程序需要为了使用 EF Core 2.0 升级到 ASP.NET Core 2.0。</span><span class="sxs-lookup"><span data-stu-id="3d69c-115">However, applications targeting previous versions of ASP.NET Core need to upgrade to ASP.NET Core 2.0 in order to use EF Core 2.0.</span></span> <span data-ttu-id="3d69c-116">有关升级到 2.0 的 ASP.NET Core 应用程序的详细信息，请参阅[有关该主题的 ASP.NET Core 文档](https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/)。</span><span class="sxs-lookup"><span data-stu-id="3d69c-116">For more details on upgrading ASP.NET Core applications to 2.0 see [the ASP.NET Core documentation on the subject](https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/).</span></span>

## <a name="new-way-of-getting-application-services-in-aspnet-core"></a><span data-ttu-id="3d69c-117">获取在 ASP.NET Core 中的应用程序服务的新方法</span><span class="sxs-lookup"><span data-stu-id="3d69c-117">New way of getting application services in ASP.NET Core</span></span>

<span data-ttu-id="3d69c-118">已为 2.0 中断 1.x 中使用的 EF Core 的设计时逻辑的方式更新的建议的模式为 ASP.NET Core web 应用程序。</span><span class="sxs-lookup"><span data-stu-id="3d69c-118">The recommended pattern for ASP.NET Core web applications has been updated for 2.0 in a way that broke the design-time logic EF Core used in 1.x.</span></span> <span data-ttu-id="3d69c-119">以前在设计时，EF Core 会尝试调用`Startup.ConfigureServices`直接才能访问应用程序的服务提供程序。</span><span class="sxs-lookup"><span data-stu-id="3d69c-119">Previously at design-time, EF Core would try to invoke `Startup.ConfigureServices` directly in order to access the application's service provider.</span></span> <span data-ttu-id="3d69c-120">在 ASP.NET Core 2.0 中，配置初始化外部`Startup`类。</span><span class="sxs-lookup"><span data-stu-id="3d69c-120">In ASP.NET Core 2.0, Configuration is initialized outside of the `Startup` class.</span></span> <span data-ttu-id="3d69c-121">通常使用 EF Core 应用程序配置中，从访问其连接字符串因此`Startup`本身已不再够用。</span><span class="sxs-lookup"><span data-stu-id="3d69c-121">Applications using EF Core typically access their connection string from Configuration, so `Startup` by itself is no longer sufficient.</span></span> <span data-ttu-id="3d69c-122">如果升级 ASP.NET Core 1.x 应用程序，你可能会收到以下错误，使用 EF Core 工具时。</span><span class="sxs-lookup"><span data-stu-id="3d69c-122">If you upgrade an ASP.NET Core 1.x application, you may receive the following error when using the EF Core tools.</span></span>

> <span data-ttu-id="3d69c-123">没有无参数构造函数 ApplicationContext 上找到。</span><span class="sxs-lookup"><span data-stu-id="3d69c-123">No parameterless constructor was found on 'ApplicationContext'.</span></span> <span data-ttu-id="3d69c-124">将无参数构造函数添加到 ApplicationContext 或添加的实现 IDesignTimeDbContextFactory&lt;ApplicationContext&gt;作为 ApplicationContext 在同一程序集中</span><span class="sxs-lookup"><span data-stu-id="3d69c-124">Either add a parameterless constructor to 'ApplicationContext' or add an implementation of 'IDesignTimeDbContextFactory&lt;ApplicationContext&gt;' in the same assembly as 'ApplicationContext'</span></span>

<span data-ttu-id="3d69c-125">ASP.NET Core 2.0 的默认模板中已添加新的设计时挂钩。</span><span class="sxs-lookup"><span data-stu-id="3d69c-125">A new design-time hook has been added in ASP.NET Core 2.0's default template.</span></span> <span data-ttu-id="3d69c-126">静态`Program.BuildWebHost`方法使 EF Core 以在设计时访问应用程序的服务提供程序。</span><span class="sxs-lookup"><span data-stu-id="3d69c-126">The static `Program.BuildWebHost` method enables EF Core to access the application's service provider at design time.</span></span> <span data-ttu-id="3d69c-127">如果要升级的 ASP.NET Core 1.x 应用程序，你将需要更新`Program`类如下所示。</span><span class="sxs-lookup"><span data-stu-id="3d69c-127">If you are upgrading an ASP.NET Core 1.x application, you will need to update the `Program` class to resemble the following.</span></span>

``` csharp
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;

namespace AspNetCoreDotNetCore2._0App
{
    public class Program
    {
        public static void Main(string[] args)
        {
            BuildWebHost(args).Run();
        }

        public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseStartup<Startup>()
                .Build();
    }
}
```

<span data-ttu-id="3d69c-128">此新模式时正在更新应用程序到 2.0 强烈建议和 Entity Framework Core 迁移等产品功能才能工作所需的采用。</span><span class="sxs-lookup"><span data-stu-id="3d69c-128">The adoption of this new pattern when updating applications to 2.0 is highly recommended and is required in order for product features like Entity Framework Core Migrations to work.</span></span> <span data-ttu-id="3d69c-129">其他常见的替代方法是[实现*IDesignTimeDbContextFactory\<TContext >*](xref:core/miscellaneous/cli/dbcontext-creation#from-a-design-time-factory)。</span><span class="sxs-lookup"><span data-stu-id="3d69c-129">The other common alternative is to [implement *IDesignTimeDbContextFactory\<TContext>*](xref:core/miscellaneous/cli/dbcontext-creation#from-a-design-time-factory).</span></span>

## <a name="idbcontextfactory-renamed"></a><span data-ttu-id="3d69c-130">IDbContextFactory 重命名</span><span class="sxs-lookup"><span data-stu-id="3d69c-130">IDbContextFactory renamed</span></span>

<span data-ttu-id="3d69c-131">为了支持不同的应用程序模式，并为用户提供更好地控制如何他们`DbContext`用于在设计时，我们，在过去，提供了`IDbContextFactory<TContext>`接口。</span><span class="sxs-lookup"><span data-stu-id="3d69c-131">In order to support diverse application patterns and give users more control over how their `DbContext` is used at design time, we have, in the past, provided the `IDbContextFactory<TContext>` interface.</span></span> <span data-ttu-id="3d69c-132">EF Core 工具将在设计时发现你的项目中的此接口的实现并使用它来创建`DbContext`对象。</span><span class="sxs-lookup"><span data-stu-id="3d69c-132">At design-time, the EF Core tools will discover implementations of this interface in your project and use it to create `DbContext` objects.</span></span>

<span data-ttu-id="3d69c-133">此接口有一个非常常规名称，这会误导某些用户，请尝试重新将其用于其他`DbContext`-创建方案。</span><span class="sxs-lookup"><span data-stu-id="3d69c-133">This interface had a very general name which mislead some users to try re-using it for other `DbContext`-creating scenarios.</span></span> <span data-ttu-id="3d69c-134">它们时 EF 工具，则尝试在设计时使用它们的实现和导致之类的命令`Update-Database`或`dotnet ef database update`失败。</span><span class="sxs-lookup"><span data-stu-id="3d69c-134">They were caught off guard when the EF Tools then tried to use their implementation at design-time and caused commands like `Update-Database` or `dotnet ef database update` to fail.</span></span>

<span data-ttu-id="3d69c-135">为了进行通信的强的设计时语义的此接口，我们已重命名到`IDesignTimeDbContextFactory<TContext>`。</span><span class="sxs-lookup"><span data-stu-id="3d69c-135">In order to communicate the strong design-time semantics of this interface, we have renamed it to `IDesignTimeDbContextFactory<TContext>`.</span></span>

<span data-ttu-id="3d69c-136">对于在 2.0 发行`IDbContextFactory<TContext>`仍然存在，但已标记为过时。</span><span class="sxs-lookup"><span data-stu-id="3d69c-136">For the 2.0 release the `IDbContextFactory<TContext>` still exists but is marked as obsolete.</span></span>

## <a name="dbcontextfactoryoptions-removed"></a><span data-ttu-id="3d69c-137">DbContextFactoryOptions 删除</span><span class="sxs-lookup"><span data-stu-id="3d69c-137">DbContextFactoryOptions removed</span></span>

<span data-ttu-id="3d69c-138">由于上面所述的 ASP.NET Core 2.0 更改，我们发现，`DbContextFactoryOptions`已不再需要在新`IDesignTimeDbContextFactory<TContext>`接口。</span><span class="sxs-lookup"><span data-stu-id="3d69c-138">Because of the ASP.NET Core 2.0 changes described above, we found that `DbContextFactoryOptions` was no longer needed on the new `IDesignTimeDbContextFactory<TContext>` interface.</span></span> <span data-ttu-id="3d69c-139">以下是应改为使用你的备选方案。</span><span class="sxs-lookup"><span data-stu-id="3d69c-139">Here are the alternatives you should be using instead.</span></span>

| <span data-ttu-id="3d69c-140">DbContextFactoryOptions</span><span class="sxs-lookup"><span data-stu-id="3d69c-140">DbContextFactoryOptions</span></span> | <span data-ttu-id="3d69c-141">替代项</span><span class="sxs-lookup"><span data-stu-id="3d69c-141">Alternative</span></span>                                                  |
|:------------------------|:-------------------------------------------------------------|
| <span data-ttu-id="3d69c-142">ApplicationBasePath</span><span class="sxs-lookup"><span data-stu-id="3d69c-142">ApplicationBasePath</span></span>     | <span data-ttu-id="3d69c-143">AppContext.BaseDirectory</span><span class="sxs-lookup"><span data-stu-id="3d69c-143">AppContext.BaseDirectory</span></span>                                     |
| <span data-ttu-id="3d69c-144">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="3d69c-144">ContentRootPath</span></span>         | <span data-ttu-id="3d69c-145">Directory.GetCurrentDirectory()</span><span class="sxs-lookup"><span data-stu-id="3d69c-145">Directory.GetCurrentDirectory()</span></span>                              |
| <span data-ttu-id="3d69c-146">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="3d69c-146">EnvironmentName</span></span>         | <span data-ttu-id="3d69c-147">Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT")</span><span class="sxs-lookup"><span data-stu-id="3d69c-147">Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT")</span></span> |

## <a name="design-time-working-directory-changed"></a><span data-ttu-id="3d69c-148">设计时更改的工作目录</span><span class="sxs-lookup"><span data-stu-id="3d69c-148">Design-time working directory changed</span></span>

<span data-ttu-id="3d69c-149">ASP.NET Core 2.0 更改也需要使用的工作目录`dotnet ef`为了符合运行你的应用程序时，由 Visual Studio 使用的工作目录。</span><span class="sxs-lookup"><span data-stu-id="3d69c-149">The ASP.NET Core 2.0 changes also required the working directory used by `dotnet ef` to align with the working directory used by Visual Studio when running your application.</span></span> <span data-ttu-id="3d69c-150">这一个可观察的副作用是像以前一样，文件名现相对于项目目录中并不是输出目录的 SQLite。</span><span class="sxs-lookup"><span data-stu-id="3d69c-150">One observable side effect of this is that SQLite filenames are now relative to the project directory and not the output directory like they used to be.</span></span>

## <a name="ef-core-20-requires-a-20-database-provider"></a><span data-ttu-id="3d69c-151">EF Core 2.0 需要 2.0 版数据库提供程序</span><span class="sxs-lookup"><span data-stu-id="3d69c-151">EF Core 2.0 requires a 2.0 database provider</span></span>

<span data-ttu-id="3d69c-152">为使用 EF Core 2.0 我们所做工作许多简化和方式的数据库提供程序中的改进。</span><span class="sxs-lookup"><span data-stu-id="3d69c-152">For EF Core 2.0 we have made many simplifications and improvements in the way database providers work.</span></span> <span data-ttu-id="3d69c-153">这意味着 1.0.x 版和 1.1.x 提供程序不会使用 EF Core 2.0。</span><span class="sxs-lookup"><span data-stu-id="3d69c-153">This means that 1.0.x and 1.1.x providers will not work with EF Core 2.0.</span></span>

<span data-ttu-id="3d69c-154">EF 团队提供的 SQL Server 和 SQLite 提供程序和 2.0 版本将可作为 2.0 版的一部分发布。</span><span class="sxs-lookup"><span data-stu-id="3d69c-154">The SQL Server and SQLite providers are shipped by the EF team and 2.0 versions will be available as part of the 2.0 release.</span></span> <span data-ttu-id="3d69c-155">开放源代码第三方提供程序[SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact)， [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL)，并[MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql)正在更新 2.0。</span><span class="sxs-lookup"><span data-stu-id="3d69c-155">The open-source third party providers for [SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), and [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) are being updated for 2.0.</span></span> <span data-ttu-id="3d69c-156">对于所有其他提供程序，请联系提供程序编写器。</span><span class="sxs-lookup"><span data-stu-id="3d69c-156">For all other providers, please contact the provider writer.</span></span>

## <a name="logging-and-diagnostics-events-have-changed"></a><span data-ttu-id="3d69c-157">日志记录和诊断事件已更改</span><span class="sxs-lookup"><span data-stu-id="3d69c-157">Logging and Diagnostics events have changed</span></span>

<span data-ttu-id="3d69c-158">注意： 这些更改不应影响大多数应用程序代码。</span><span class="sxs-lookup"><span data-stu-id="3d69c-158">Note: these changes should not impact most application code.</span></span>

<span data-ttu-id="3d69c-159">事件 Id 发送到的消息[ILogger](https://github.com/aspnet/Logging/blob/dev/src/Microsoft.Extensions.Logging.Abstractions/ILogger.cs) 2.0 中已更改。</span><span class="sxs-lookup"><span data-stu-id="3d69c-159">The event IDs for messages sent to an [ILogger](https://github.com/aspnet/Logging/blob/dev/src/Microsoft.Extensions.Logging.Abstractions/ILogger.cs) have changed in 2.0.</span></span> <span data-ttu-id="3d69c-160">现在，事件 ID 在 EF Core 代码内具有唯一性。</span><span class="sxs-lookup"><span data-stu-id="3d69c-160">The event IDs are now unique across EF Core code.</span></span> <span data-ttu-id="3d69c-161">这些消息现在还遵循 MVC 等所用的结构化日志记录的标准模式。</span><span class="sxs-lookup"><span data-stu-id="3d69c-161">These messages now also follow the standard pattern for structured logging used by, for example, MVC.</span></span>

<span data-ttu-id="3d69c-162">记录器类别也已更改。</span><span class="sxs-lookup"><span data-stu-id="3d69c-162">Logger categories have also changed.</span></span> <span data-ttu-id="3d69c-163">现提供通过 [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs) 访问的熟知类别集。</span><span class="sxs-lookup"><span data-stu-id="3d69c-163">There is now a well-known set of categories accessed through [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).</span></span>

<span data-ttu-id="3d69c-164">[DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md)事件现在使用相同的事件 ID 名称与相应`ILogger`消息。</span><span class="sxs-lookup"><span data-stu-id="3d69c-164">[DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) events now use the same event ID names as the corresponding `ILogger` messages.</span></span> <span data-ttu-id="3d69c-165">事件负载是所有的名义类型派生自[EventData](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/EventData.cs)。</span><span class="sxs-lookup"><span data-stu-id="3d69c-165">The event payloads are all nominal types derived from [EventData](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/EventData.cs).</span></span>

<span data-ttu-id="3d69c-166">在中记录事件 Id、 有效负载类型和类别[CoreEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/CoreEventId.cs)并[RelationalEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore.Relational/Diagnostics/RelationalEventId.cs)类。</span><span class="sxs-lookup"><span data-stu-id="3d69c-166">Event IDs, payload types, and categories are documented in the [CoreEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/CoreEventId.cs) and the [RelationalEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore.Relational/Diagnostics/RelationalEventId.cs) classes.</span></span>

<span data-ttu-id="3d69c-167">Id 也已从 Microsoft.EntityFrameworkCore.Infraestructure 到新 Microsoft.EntityFrameworkCore.Diagnostics 命名空间。</span><span class="sxs-lookup"><span data-stu-id="3d69c-167">IDs have also moved from Microsoft.EntityFrameworkCore.Infraestructure to the new Microsoft.EntityFrameworkCore.Diagnostics namespace.</span></span>

## <a name="ef-core-relational-metadata-api-changes"></a><span data-ttu-id="3d69c-168">EF Core 关系元数据 API 更改</span><span class="sxs-lookup"><span data-stu-id="3d69c-168">EF Core relational metadata API changes</span></span>

<span data-ttu-id="3d69c-169">EF Core 2.0 现将对所用的每个不同提供程序生成不同的 [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs)。</span><span class="sxs-lookup"><span data-stu-id="3d69c-169">EF Core 2.0 will now build a different [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) for each different provider being used.</span></span> <span data-ttu-id="3d69c-170">这对应用程序而言通常是透明的。</span><span class="sxs-lookup"><span data-stu-id="3d69c-170">This is usually transparent to the application.</span></span> <span data-ttu-id="3d69c-171">这有助于简化较低级别的元数据 API，从而始终通过调用 `.Relational`（而不是 `.SqlServer`、`.Sqlite` 等）来访问常见关系元数据概念。例如，1.1.x 代码如下：</span><span class="sxs-lookup"><span data-stu-id="3d69c-171">This has facilitated a simplification of lower-level metadata APIs such that any access to _common relational metadata concepts_ is always made through a call to `.Relational` instead of `.SqlServer`, `.Sqlite`, etc. For example, 1.1.x code like this:</span></span>

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

<span data-ttu-id="3d69c-172">现在应编写如下代码：</span><span class="sxs-lookup"><span data-stu-id="3d69c-172">Should now be written like this:</span></span>

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

<span data-ttu-id="3d69c-173">而不是使用方法，如`ForSqlServerToTable`，扩展方法现在都可用于编写基于当前提供程序中使用的条件代码。</span><span class="sxs-lookup"><span data-stu-id="3d69c-173">Instead of using methods like `ForSqlServerToTable`, extension methods are now available to write conditional code based on the current provider in use.</span></span> <span data-ttu-id="3d69c-174">例如：</span><span class="sxs-lookup"><span data-stu-id="3d69c-174">For example:</span></span>

```C#
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

<span data-ttu-id="3d69c-175">请注意，此更改仅适用于为定义的 Api/metadata_所有_关系提供程序。</span><span class="sxs-lookup"><span data-stu-id="3d69c-175">Note that this change only applies to APIs/metadata that is defined for _all_ relational providers.</span></span> <span data-ttu-id="3d69c-176">API 和元数据保持不变，当它是特定于单个提供程序。</span><span class="sxs-lookup"><span data-stu-id="3d69c-176">The API and metadata remains the same when it is specific to only a single provider.</span></span> <span data-ttu-id="3d69c-177">例如，聚集的索引都是特定于 SQL Sever，因此`ForSqlServerIsClustered`和`.SqlServer().IsClustered()`仍必须使用。</span><span class="sxs-lookup"><span data-stu-id="3d69c-177">For example, clustered indexes are specific to SQL Sever, so `ForSqlServerIsClustered` and  `.SqlServer().IsClustered()` must still be used.</span></span>

## <a name="dont-take-control-of-the-ef-service-provider"></a><span data-ttu-id="3d69c-178">不控制 EF 服务提供程序</span><span class="sxs-lookup"><span data-stu-id="3d69c-178">Don’t take control of the EF service provider</span></span>

<span data-ttu-id="3d69c-179">EF Core 使用内部`IServiceProvider`（依赖关系注入容器） 为其内部实现。</span><span class="sxs-lookup"><span data-stu-id="3d69c-179">EF Core uses an internal `IServiceProvider` (a dependency injection container) for its internal implementation.</span></span> <span data-ttu-id="3d69c-180">应用程序应允许 EF Core 以创建和管理在特殊情况下此提供程序除外。</span><span class="sxs-lookup"><span data-stu-id="3d69c-180">Applications should allow EF Core to create and manage this provider except in special cases.</span></span> <span data-ttu-id="3d69c-181">强烈建议考虑删除对任何调用`UseInternalServiceProvider`。</span><span class="sxs-lookup"><span data-stu-id="3d69c-181">Strongly consider removing any calls to `UseInternalServiceProvider`.</span></span> <span data-ttu-id="3d69c-182">如果应用程序确实需要调用`UseInternalServiceProvider`，请考虑[上报](https://github.com/aspnet/EntityFramework/Issues)以便我们可以调查其他方式来处理你的方案。</span><span class="sxs-lookup"><span data-stu-id="3d69c-182">If an application does need to call `UseInternalServiceProvider`, then please consider [filing an issue](https://github.com/aspnet/EntityFramework/Issues) so we can investigate other ways to handle your scenario.</span></span>

<span data-ttu-id="3d69c-183">调用`AddEntityFramework`， `AddEntityFrameworkSqlServer`，等不需要由应用程序代码，除非`UseInternalServiceProvider`也称为。</span><span class="sxs-lookup"><span data-stu-id="3d69c-183">Calling `AddEntityFramework`, `AddEntityFrameworkSqlServer`, etc. is not required by application code unless `UseInternalServiceProvider` is also called.</span></span> <span data-ttu-id="3d69c-184">删除对任何现有调用`AddEntityFramework`或`AddEntityFrameworkSqlServer`等`AddDbContext`仍需使用相同的方式与前面。</span><span class="sxs-lookup"><span data-stu-id="3d69c-184">Remove any existing calls to `AddEntityFramework` or `AddEntityFrameworkSqlServer`, etc. `AddDbContext` should still be used in the same way as before.</span></span>

## <a name="in-memory-databases-must-be-named"></a><span data-ttu-id="3d69c-185">内存中数据库必须命名为</span><span class="sxs-lookup"><span data-stu-id="3d69c-185">In-memory databases must be named</span></span>

<span data-ttu-id="3d69c-186">未命名的全局内存中数据库已删除，而是必须命名为内存中的所有数据库。</span><span class="sxs-lookup"><span data-stu-id="3d69c-186">The global unnamed in-memory database has been removed and instead all in-memory databases must be named.</span></span> <span data-ttu-id="3d69c-187">例如：</span><span class="sxs-lookup"><span data-stu-id="3d69c-187">For example:</span></span>

``` csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

<span data-ttu-id="3d69c-188">这将创建/使用具有名称"MyDatabase"的数据库。</span><span class="sxs-lookup"><span data-stu-id="3d69c-188">This creates/uses a database with the name “MyDatabase”.</span></span> <span data-ttu-id="3d69c-189">如果`UseInMemoryDatabase`会再次调用具有相同的名称，则将使用相同的内存中数据库，从而使其可以由多个上下文实例共享。</span><span class="sxs-lookup"><span data-stu-id="3d69c-189">If `UseInMemoryDatabase` is called again with the same name, then the same in-memory database will be used, allowing it to be shared by multiple context instances.</span></span>

## <a name="read-only-api-changes"></a><span data-ttu-id="3d69c-190">只读的 API 更改</span><span class="sxs-lookup"><span data-stu-id="3d69c-190">Read-only API changes</span></span>

<span data-ttu-id="3d69c-191">`IsReadOnlyBeforeSave``IsReadOnlyAfterSave`，并`IsStoreGeneratedAlways`已过时，替换[BeforeSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L39)并[AfterSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L55)。</span><span class="sxs-lookup"><span data-stu-id="3d69c-191">`IsReadOnlyBeforeSave`, `IsReadOnlyAfterSave`, and `IsStoreGeneratedAlways` have been obsoleted and replaced with [BeforeSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L39) and [AfterSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L55).</span></span> <span data-ttu-id="3d69c-192">这些行为将应用于任何属性 （不只存储生成的属性），并确定将插入到数据库行时应如何使用属性的值 (`BeforeSaveBehavior`) 或更新现有数据库行时 (`AfterSaveBehavior`)。</span><span class="sxs-lookup"><span data-stu-id="3d69c-192">These behaviors apply to any property (not only store-generated properties) and determine how the value of the property should be used when inserting into a database row (`BeforeSaveBehavior`) or when updating an existing database row (`AfterSaveBehavior`).</span></span>

<span data-ttu-id="3d69c-193">属性标记为[ValueGenerated.OnAddOrUpdate](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/ValueGenerated.cs) （例如，对于计算列） 将默认情况下忽略当前的属性上设置任何值。</span><span class="sxs-lookup"><span data-stu-id="3d69c-193">Properties marked as [ValueGenerated.OnAddOrUpdate](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/ValueGenerated.cs) (for example, for computed columns) will by default ignore any value currently set on the property.</span></span> <span data-ttu-id="3d69c-194">这意味着，而不考虑任何值已设置或修改所跟踪实体上是否将始终获取由存储生成的值。</span><span class="sxs-lookup"><span data-stu-id="3d69c-194">This means that a store-generated value will always be obtained regardless of whether any value has been set or modified on the tracked entity.</span></span> <span data-ttu-id="3d69c-195">这可以通过设置其他更改`Before\AfterSaveBehavior`。</span><span class="sxs-lookup"><span data-stu-id="3d69c-195">This can be changed by setting a different `Before\AfterSaveBehavior`.</span></span>

## <a name="new-clientsetnull-delete-behavior"></a><span data-ttu-id="3d69c-196">新 ClientSetNull 删除行为</span><span class="sxs-lookup"><span data-stu-id="3d69c-196">New ClientSetNull delete behavior</span></span>

<span data-ttu-id="3d69c-197">在上一版本中， [DeleteBehavior.Restrict](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/DeleteBehavior.cs)必须为实体行为由上下文跟踪的详细信息已关闭的匹配`SetNull`语义。</span><span class="sxs-lookup"><span data-stu-id="3d69c-197">In previous releases, [DeleteBehavior.Restrict](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/DeleteBehavior.cs) had a behavior for entities tracked by the context that more closed matched `SetNull` semantics.</span></span> <span data-ttu-id="3d69c-198">在 EF Core 2.0 中，新`ClientSetNull`作为的默认值为可选关系引入了行为。</span><span class="sxs-lookup"><span data-stu-id="3d69c-198">In EF Core 2.0, a new `ClientSetNull` behavior has been introduced as the default for optional relationships.</span></span> <span data-ttu-id="3d69c-199">此行为具有`SetNull`语义跟踪的实体和`Restrict`创建使用 EF Core 的数据库的行为。</span><span class="sxs-lookup"><span data-stu-id="3d69c-199">This behavior has `SetNull` semantics for tracked entities and `Restrict` behavior for databases created using EF Core.</span></span> <span data-ttu-id="3d69c-200">在我们的经验，它们是跟踪的实体和数据库的最预期/有用行为。</span><span class="sxs-lookup"><span data-stu-id="3d69c-200">In our experience, these are the most expected/useful behaviors for tracked entities and the database.</span></span> <span data-ttu-id="3d69c-201">`DeleteBehavior.Restrict` 现在为跟踪的实体的可选关系设置时采用。</span><span class="sxs-lookup"><span data-stu-id="3d69c-201">`DeleteBehavior.Restrict` is now honored for tracked entities when set for optional relationships.</span></span>

## <a name="provider-design-time-packages-removed"></a><span data-ttu-id="3d69c-202">删除提供程序设计时包</span><span class="sxs-lookup"><span data-stu-id="3d69c-202">Provider design-time packages removed</span></span>

<span data-ttu-id="3d69c-203">`Microsoft.EntityFrameworkCore.Relational.Design`删除包。</span><span class="sxs-lookup"><span data-stu-id="3d69c-203">The `Microsoft.EntityFrameworkCore.Relational.Design` package has been removed.</span></span> <span data-ttu-id="3d69c-204">它的内容已合并到`Microsoft.EntityFrameworkCore.Relational`和`Microsoft.EntityFrameworkCore.Design`。</span><span class="sxs-lookup"><span data-stu-id="3d69c-204">It's contents were consolidated into `Microsoft.EntityFrameworkCore.Relational` and `Microsoft.EntityFrameworkCore.Design`.</span></span>

<span data-ttu-id="3d69c-205">这将传播到提供程序设计时包。</span><span class="sxs-lookup"><span data-stu-id="3d69c-205">This propagates into the provider design-time packages.</span></span> <span data-ttu-id="3d69c-206">这些包 (`Microsoft.EntityFrameworkCore.Sqlite.Design`，`Microsoft.EntityFrameworkCore.SqlServer.Design`等) 已删除和其内容合并到主要提供程序包。</span><span class="sxs-lookup"><span data-stu-id="3d69c-206">Those packages (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`, etc.) were removed and their contents consolidated into the main provider packages.</span></span>

<span data-ttu-id="3d69c-207">若要启用`Scaffold-DbContext`或`dotnet ef dbcontext scaffold`在 EF Core 2.0 中，你只需引用单个提供程序包：</span><span class="sxs-lookup"><span data-stu-id="3d69c-207">To enable `Scaffold-DbContext` or `dotnet ef dbcontext scaffold` in EF Core 2.0, you only need to reference the single provider package:</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="2.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools"
    Version="2.0.0"
    PrivateAssets="All" />
<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
    Version="2.0.0" />
```
