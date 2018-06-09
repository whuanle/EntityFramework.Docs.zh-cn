---
title: 日志记录-EF 核心
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
ms.technology: entity-framework-core
uid: core/miscellaneous/logging
ms.openlocfilehash: 60d76bf3360eb47cdd9836494c1f135d1005a215
ms.sourcegitcommit: 3adf1267be92effc3c9daa893906a7f36834204f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/08/2018
ms.locfileid: "35232131"
---
# <a name="logging"></a>日志记录

> [!TIP]  
> 可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging)。

## <a name="aspnet-core-applications"></a>ASP.NET 核心应用程序

与 ASP.NET Core 的日志记录机制自动集成，EF 核心每当`AddDbContext`或`AddDbContextPool`使用。 因此，在使用 ASP.NET Core，日志记录应配置中所述[ASP.NET 核心文档](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)。

## <a name="other-applications"></a>其他应用程序

当前日志记录的 EF 核心要求 ILoggerFactory 这是配置了一个或多个 ILoggerProvider 本身。 常见的提供程序随附于下列包：

* [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/)： 简单的控制台记录器。
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/)： 支持 Azure 应用程序服务诊断日志和日志流功能。
* [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/)： 日志传输到使用 System.Diagnostics.Debug.WriteLine() 调试器监视器。
* [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/)： 到 Windows 事件日志的日志。
* [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/)： 支持 EventSource/EventListener。
* [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/)： 到使用 System.Diagnostics.TraceSource.TraceEvent() 的跟踪侦听器的日志。

在安装相应的程序包后, 应用程序应创建 LoggerFactory 的单一实例/全局实例。 例如，使用控制台记录器：

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

此 singleton/全局实例应然后注册与 EF 核心上`DbContextOptionsBuilder`。 例如：

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> 它是非常重要的应用程序不会创建每个上下文实例的新 ILoggerFactory 实例。 这样将导致内存泄漏和性能低下。

## <a name="filtering-what-is-logged"></a>筛选记录的内容，

筛选所记录内容的最简单方法是在注册 ILoggerProvider 时对其进行配置。 例如：

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

在此示例中，日志筛选以返回仅消息：
 * Microsoft.EntityFrameworkCore.Database.Command 类别中
 * 在信息级别

对于 EF 核心记录器类别在中定义`DbLoggerCategory`类，以便可以方便地查找类别，但这些解析为简单的字符串。

在基础的日志记录基础结构上的更多详细信息可在[ASP.NET 核心日志记录文档](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)。
