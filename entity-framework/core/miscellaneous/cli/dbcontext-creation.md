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
<a name="design-time-dbcontext-creation"></a>设计时 DbContext 创建
==============================
一些命令都要求在设计要创建的 DbContext 实例的 EF 工具的时间 （例如，当运行迁移命令）。 有多种工具尝试创建它的方法。

<a name="from-application-services"></a>从应用程序服务
-------------------------
如果你的启动项目，ASP.NET Core 应用工具将尝试从应用程序的服务提供程序获取 DbContext 对象。 它们通过调用获取该`Program.BuildWebHost()`和访问`IWebHost.Services`属性。 使用任何 DbContext 注册`IServiceCollection.AddDbContext<TContext>()`可以找出并创建这种方式。 此模式已[在 ASP.NET 核心 2.0 中引入][1]

<a name="using-the-default-constructor"></a>使用默认构造函数
-----------------------------
如果无法从应用程序服务提供程序获取 DbContext，工具查找该项目中的 DbContext 类型。 它们尝试使用其默认构造函数创建它。

<a name="from-a-design-time-factory"></a>从设计时工厂
--------------------------
您还可以判断工具如何通过实现来创建你 DbContext `IDesignTimeDbContextFactory`。 如果你的项目中找到了实现此接口的类，这些工具绕过的其他方式创建的 DbContext。
它们始终在设计时使用的工厂。 工厂是如果你需要为设计时比在运行时以不同的方式配置 DbContext 特别有用。

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
> `args`参数是当前未使用。 没有[问题][ 2]跟踪指定的工具的设计时自变量的功能。

  [1]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [2]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
