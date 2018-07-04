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
<a name="design-time-dbcontext-creation"></a>设计时 DbContext 创建
==============================
某些 EF Core工具命令 (例如，[迁移][ 1]命令) 需要派生`DbContext`实例以收集有关应用程序的详细信息在设计时创建实体类型和它们如何映射到数据库架构。 在大多数情况下，最好的`DbContext`从而创建配置如何将以类似方式[在运行时配置][2]。

有多种方法工具尝试创建`DbContext`:

<a name="from-application-services"></a>从应用程序服务
-------------------------
如果你的启动项目，ASP.NET Core 应用工具将尝试从应用程序的服务提供程序获取 DbContext 对象。

该工具第一次尝试通过调用获取服务提供商`Program.BuildWebHost()`和访问`IWebHost.Services`属性。

> [!NOTE]
> 在创建新的 ASP.NET Core 2.0 应用程序时，默认情况下包含此挂钩。 在以前版本的 EF Core 和 ASP.NET Core，工具尝试调用`Startup.ConfigureServices`直接以获取应用程序的服务提供商，但此模式不能再正常 ASP.NET Core 2.0 应用程序中。 如果您正在升级到 2.0 ASP.NET Core 1.x 应用程序，则可以[修改你`Program`类遵循新模式][3]。

`DbContext`本身以及任何依赖项在其构造函数中的需要注册为在应用程序的服务提供程序的服务。 这可以轻松实现通过让[上的构造函数`DbContext`采用的实例`DbContextOptions<TContext>`作为自变量][ 4]和使用[`AddDbContext<TContext>`方法][5].

<a name="using-a-constructor-with-no-parameters"></a>使用不带任何参数的构造函数
--------------------------------------
如果不能从应用程序服务提供程序获得 DbContext，工具查找派生`DbContext`项目中的类型。 然后，它们尝试创建使用不带任何参数的构造函数实例。 这可以是默认构造函数，如果`DbContext`使用配置[ `OnConfiguring` ] [ 6]方法。

<a name="from-a-design-time-factory"></a>从设计时工厂
--------------------------
您还可以判断工具如何通过实现来创建你 DbContext`IDesignTimeDbContextFactory<TContext>`接口： 如果找到实现此接口的类中派生的同一项目`DbContext`或在应用程序的启动项目中，工具绕过改为创建的 DbContext 和使用设计时工厂的其他方法。

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
> `args`参数是当前未使用。 没有[问题][ 7]跟踪指定的工具的设计时自变量的功能。

设计时工厂可能特别有用，如果你需要配置 DbContext 以不同方式为设计时比在运行时，如果`DbContext`构造函数采用其他参数在 DI 中, 不会进行注册，如果你根本不使用 DI 或如果由于某些原因不希望让`BuildWebHost`ASP.NET Core 应用程序中的方法  
`Main` 类。

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
