---
title: 日志记录-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: 65501b5ac03ae544c51b7fc1a07fa9eea849f1e3
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022140"
---
# <a name="logging"></a><span data-ttu-id="b2acc-102">日志记录</span><span class="sxs-lookup"><span data-stu-id="b2acc-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="b2acc-103">可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging)。</span><span class="sxs-lookup"><span data-stu-id="b2acc-103">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="b2acc-104">ASP.NET Core 应用程序</span><span class="sxs-lookup"><span data-stu-id="b2acc-104">ASP.NET Core applications</span></span>

<span data-ttu-id="b2acc-105">与 ASP.NET Core 的日志记录机制自动集成，EF Core 每当`AddDbContext`或`AddDbContextPool`使用。</span><span class="sxs-lookup"><span data-stu-id="b2acc-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="b2acc-106">因此，在使用 ASP.NET Core，日志记录应配置中所述[ASP.NET Core 文档](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)。</span><span class="sxs-lookup"><span data-stu-id="b2acc-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="b2acc-107">其他应用程序</span><span class="sxs-lookup"><span data-stu-id="b2acc-107">Other applications</span></span>

<span data-ttu-id="b2acc-108">当前日志记录的 EF Core 要求 ILoggerFactory 这是配置了一个或多个 ILoggerProvider 本身。</span><span class="sxs-lookup"><span data-stu-id="b2acc-108">EF Core logging currently requires an ILoggerFactory which is itself configured with one or more ILoggerProvider.</span></span> <span data-ttu-id="b2acc-109">在以下包中随附常见提供程序：</span><span class="sxs-lookup"><span data-stu-id="b2acc-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="b2acc-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/)： 简单的控制台记录器。</span><span class="sxs-lookup"><span data-stu-id="b2acc-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="b2acc-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/)： 支持 Azure 应用服务诊断日志和日志流功能。</span><span class="sxs-lookup"><span data-stu-id="b2acc-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="b2acc-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/)： 使用 System.Diagnostics.Debug.WriteLine() 调试器监视的日志。</span><span class="sxs-lookup"><span data-stu-id="b2acc-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="b2acc-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/)： 记录到 Windows 事件日志。</span><span class="sxs-lookup"><span data-stu-id="b2acc-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="b2acc-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/)： 支持 EventSource/EventListener。</span><span class="sxs-lookup"><span data-stu-id="b2acc-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="b2acc-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/)： 使用 System.Diagnostics.TraceSource.TraceEvent() 的跟踪侦听器的日志。</span><span class="sxs-lookup"><span data-stu-id="b2acc-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using System.Diagnostics.TraceSource.TraceEvent().</span></span>

<span data-ttu-id="b2acc-116">安装相应的程序包之后, 该应用程序应创建 LoggerFactory 的单一实例/全局实例。</span><span class="sxs-lookup"><span data-stu-id="b2acc-116">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="b2acc-117">例如，使用控制台记录器：</span><span class="sxs-lookup"><span data-stu-id="b2acc-117">For example, using the console logger:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

<span data-ttu-id="b2acc-118">此 singleton/全局实例应然后注册与 EF Core 上`DbContextOptionsBuilder`。</span><span class="sxs-lookup"><span data-stu-id="b2acc-118">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="b2acc-119">例如：</span><span class="sxs-lookup"><span data-stu-id="b2acc-119">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="b2acc-120">它是非常重要的应用程序不会创建新的 ILoggerFactory 实例为每个上下文实例。</span><span class="sxs-lookup"><span data-stu-id="b2acc-120">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="b2acc-121">执行此操作将导致内存泄漏和性能不佳。</span><span class="sxs-lookup"><span data-stu-id="b2acc-121">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="b2acc-122">筛选记录的内容</span><span class="sxs-lookup"><span data-stu-id="b2acc-122">Filtering what is logged</span></span>

<span data-ttu-id="b2acc-123">筛选记录的内容的最简单方法是注册 ILoggerProvider 时对其进行配置。</span><span class="sxs-lookup"><span data-stu-id="b2acc-123">The easiest way to filter what is logged is to configure it when registering the ILoggerProvider.</span></span> <span data-ttu-id="b2acc-124">例如：</span><span class="sxs-lookup"><span data-stu-id="b2acc-124">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

<span data-ttu-id="b2acc-125">在此示例中，筛选日志将仅消息返回：</span><span class="sxs-lookup"><span data-stu-id="b2acc-125">In this example, the log is filtered to return only messages:</span></span>
 * <span data-ttu-id="b2acc-126">在 Microsoft.EntityFrameworkCore.Database.Command 类别中</span><span class="sxs-lookup"><span data-stu-id="b2acc-126">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
 * <span data-ttu-id="b2acc-127">在信息级别</span><span class="sxs-lookup"><span data-stu-id="b2acc-127">at the 'Information' level</span></span>

<span data-ttu-id="b2acc-128">对于 EF Core 记录器类别在中定义`DbLoggerCategory`类，以便可以方便地查找类别，但这些解析为简单的字符串。</span><span class="sxs-lookup"><span data-stu-id="b2acc-128">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="b2acc-129">在基础的日志记录基础结构上的更多详细信息可在[ASP.NET Core 日志记录文档](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)。</span><span class="sxs-lookup"><span data-stu-id="b2acc-129">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
