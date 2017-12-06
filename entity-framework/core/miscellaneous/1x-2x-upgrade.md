---
title: "从以前的版本升级到 EF 核心 2-EF 核心"
author: divega
ms.author: divega
ms.date: 8/13/2017
ms.assetid: 8BD43C8C-63D9-4F3A-B954-7BC518A1B7DB
ms.technology: entity-framework-core
uid: core/miscellaneous/1x-2x-upgrade
ms.openlocfilehash: 0bd1ea2476621f826cca7d4a526a49a1b902acf8
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2017
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a>从以前版本的应用程序升级到 EF 核心 2.0

## <a name="procedures-common-to-all-applications"></a>对所有应用程序都通用的过程

更新现有应用程序到 EF 核心 2.0 可能还需要：

1. 升级到另一个支持.NET 标准 2.0 应用程序的目标.NET 平台。 请参阅[支持的平台](../platforms/index.md)有关详细信息。

2. 标识的提供程序与 EF 核心 2.0 兼容的目标数据库。 请参阅[EF 核心 2.0 需要 2.0 版数据库提供程序](#ef-core-20-requires-a-20-database-provider)下面。

3. 升级到 2.0 所有 EF 核心包 （运行时和工具）。 请参阅[安装 EF 核心](../get-started/install/index.md)有关详细信息。

4. 进行任何必要的代码更改，以弥补的重大更改。 请参阅[的重大更改](#breaking-changes)下面部分以了解更多详细信息。

## <a name="aspnet-core-applications"></a>ASP.NET 核心应用程序

1. 请参阅尤其[初始化应用程序的服务提供程序的新模式](#new-way-of-getting-application-services)如下所述。

> [!TIP]  
> 此新模式时更新应用程序迁移到 2.0 强烈建议和 Entity Framework 核心迁移等的产品功能使起作用所需的采用率。 其他常见的替代方法是[实现*IDesignTimeDbContextFactory\<TContext >*](configuring-dbcontext.md#using-idesigntimedbcontextfactorytcontext)。

2. 面向 ASP.NET 核心 2.0 应用程序可以使用不带除了第三方数据库提供程序的其他依赖项的 EF 核心 2.0。 但是，面向以前版本的 ASP.NET Core 应用程序需要为了使用 EF 核心 2.0 升级到 ASP.NET 核心 2.0。 有关升级到 2.0 的 ASP.NET Core 应用程序的详细信息，请参阅[有关该主题的 ASP.NET 核心文档](https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/)。

## <a name="breaking-changes"></a>重大更改

我们执行了显著改进我们的现有的 Api 和 2.0 中的行为的机会。 有几个可能需要修改现有应用程序代码的改进，虽然我们认为对于大多数应用程序的影响将很低，在大多数情况下要求只需重新编译和最小的引导式的更改，以替换已过时 Api。

### <a name="new-way-of-getting-application-services"></a>获取应用程序服务的新方法

已为 2.0 中断 1.x 中使用的 EF 核心的设计时逻辑的方式更新的建议的模式为 ASP.NET 核心 web 应用程序。 以前在设计时，EF 核心会尝试调用`Startup.ConfigureServices`直接才能访问应用程序的服务提供程序。 在 ASP.NET 核心 2.0 中，配置初始化外部`Startup`类。 通常使用 EF 核心应用程序配置中，从访问其连接字符串因此`Startup`本身已不再够用。 如果升级 ASP.NET Core 1.x 应用程序，你可能会收到以下错误，使用 EF 核心工具时。

> ApplicationContext 上发现了没有无参数构造函数。 将无参数构造函数添加到 ApplicationContext 或添加的实现 IDesignTimeDbContextFactory&lt;ApplicationContext&gt;中 ApplicationContext 相同的程序集中

ASP.NET 核心 2.0 的默认模板中已添加新的设计时挂钩。 静态`Program.BuildWebHost`方法使 EF 核心以在设计时访问应用程序的服务提供程序。 如果你正在升级 ASP.NET Core 1.x 应用程序，你将需要更新你`Program`类如下所示。

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

### <a name="idbcontextfactory-renamed"></a>重命名的 IDbContextFactory

为了支持不同的应用程序模式，并为用户提供更好地控制如何其`DbContext`使用在设计时，我们，在过去，提供了`IDbContextFactory<TContext>`接口。 EF 核心工具将在设计时发现你的项目中的此接口的实现并使用它来创建`DbContext`对象。

此接口具有一个非常一般名，它会误导某些用户尝试重新将其用于其他`DbContext`-创建方案。 它们时 EF 工具然后尝试在设计时使用其实现感到措手不及和导致等命令`Update-Database`或`dotnet ef database update`失败。

以便进行通信的此接口的强设计时语义，我们已重命名到`IDesignTimeDbContextFactory<TContext>`。

为 2.0 版本`IDbContextFactory<TContext>`仍存在，但标记为过时。

### <a name="dbcontextfactoryoptions-removed"></a>DbContextFactoryOptions 删除

由于上面所述的 ASP.NET 核心 2.0 更改，我们发现，`DbContextFactoryOptions`已不再需要在新`IDesignTimeDbContextFactory<TContext>`接口。 以下是应改为使用你选择的选项。

DbContextFactoryOptions | 替代项
--- | ---
ApplicationBasePath | AppContext.BaseDirectory
ContentRootPath | Directory.GetCurrentDirectory()
EnvironmentName | Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT")

### <a name="design-time-working-directory-changed"></a>设计时更改的工作目录

ASP.NET 核心 2.0 更改也需要使用的工作目录`dotnet ef`为了符合运行你的应用程序时，由 Visual Studio 使用的工作目录。 这样的一个明显的副作用是该 SQLite 像以前一样，文件名现相对于项目目录而不是输出目录。

### <a name="ef-core-20-requires-a-20-database-provider"></a>EF 核心 2.0 需要 2.0 版数据库提供程序

为使用 EF 核心 2.0 我们所做工作许多简化和方式的数据库提供程序中的改进。 这意味着 1.0.x 版和 1.1.x 提供程序不会使用 EF 核心 2.0。

在 SQL Server 和 SQLite 提供程序交付的 EF 团队和 2.0 版本将可作为 2.0 版的一部分发布。 开放源代码第三方提供程序[SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact)， [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL)，和[MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql)正在更新为 2.0。 对于所有其他提供程序，请联系提供程序编写器。

### <a name="logging-and-diagnostics-events-have-changed"></a>日志记录和诊断事件已发生了更改

注意： 这些更改不应影响大多数应用程序代码。

消息发送到事件 Id [ILogger](https://github.com/aspnet/Logging/blob/dev/src/Microsoft.Extensions.Logging.Abstractions/ILogger.cs) 2.0 中已更改。 事件 Id 是现在唯一的 EF 核心代码。 这些消息现在还可以遵循结构化日志记录由，例如，MVC 的标准模式。

记录器类别也已更改。 现在通过访问一组已知的类别有如此多[DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs)。

[DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md)事件现在使用相同的事件 ID 名称作为相应`ILogger`消息。 事件负载是派生自的所有名义类型[EventData](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/EventData.cs)。

事件 Id、 负载类型和类别均记录在[CoreEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/CoreEventId.cs)和[RelationalEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore.Relational/Diagnostics/RelationalEventId.cs)类。

Id 还从移动 Microsoft.EntityFrameworkCore.Infraestructure 到新 Microsoft.EntityFrameworkCore.Diagnostics 命名空间。

### <a name="ef-core-relational-metadata-api-changes"></a>EF 核心关系元数据 API 更改

EF 核心 2.0 现在将生成不同[IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs)正在使用的每个不同的提供程序。 这是通常是透明的应用程序。 具有可帮助实现该较低级别的元数据 Api 的简化形式，任何访问到_常见关系的元数据的概念，_始终通过调用建立`.Relational`而不是`.SqlServer`， `.Sqlite`，等等。例如，1.1.x 代码如下：

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

现在应编写如下：

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

而不是使用等方法`ForSqlServerToTable`，扩展方法现在都可用于编写基于当前的提供程序中使用的条件代码。 例如：

```C#
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

请注意，此更改仅适用于 Api/元数据为定义_所有_关系的提供程序。 API 和元数据保持不变，当它是特定于单个提供程序。 例如，聚集的索引都是特定于 SQL Sever，因此`ForSqlServerIsClustered`和`.SqlServer().IsClustered()`仍必须使用。

### <a name="dont-take-control-of-the-ef-service-provider"></a>不能控制 EF 服务提供程序

EF 核心使用内部`IServiceProvider`（即依赖关系注入容器） 用于内部实现。 应用程序应允许 EF 核心以创建和管理在特殊情况下此提供程序除外。 强烈建议考虑删除对任何调用`UseInternalServiceProvider`。 如果应用程序需要调用`UseInternalServiceProvider`，请考虑[填写问题](https://github.com/aspnet/EntityFramework/Issues)以便我们可以调查其他方法来处理你的方案。

调用`AddEntityFramework`， `AddEntityFrameworkSqlServer`，等不需要的应用程序代码，除非`UseInternalServiceProvider`也被称为。 删除任何现有调用`AddEntityFramework`或`AddEntityFrameworkSqlServer`等`AddDbContext`仍应使用相同的方式和前面一样。

### <a name="in-memory-databases-must-be-named"></a>必须命名为内存中数据库

已删除全局未命名的内存中数据库，而是必须命名为内存中的所有数据库。 例如：

``` csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

这创建/使用具有名称"MyDatabase"的数据库。 如果`UseInMemoryDatabase`会再次调用具有相同的名称，则将使用相同的内存中数据库，从而使其可以由多个上下文实例共享。

### <a name="read-only-api-changes"></a>只读的 API 更改

`IsReadOnlyBeforeSave``IsReadOnlyAferSave`，和`IsStoreGeneratedAlways`已过时，替换[BeforeSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L39)和[AfterSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L55)。 这些行为应用于任何属性 （不只由存储生成的属性），并确定将插入到数据库行时应如何使用属性的值 (`BeforeSaveBehavior`) 或更新现有数据库时行 (`AfterSaveBehavior`)。

属性标记为[ValueGenerated.OnAddOrUpdate](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/ValueGenerated.cs) （例如，对于计算列） 将默认情况下忽略当前的属性上设置任何值。 这意味着无论是否已设置或的被跟踪实体上修改任何值将始终获取由存储生成的值。 这可以通过设置为其他更改`Before\AfterSaveBehavior`。

### <a name="new-clientsetnull-delete-behavior"></a>新 ClientSetNull 删除行为

在以前版本中， [DeleteBehavior.Restrict](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/DeleteBehavior.cs)具有的实体的行为由上下文跟踪的详细信息关闭匹配`SetNull`语义。 在 EF 核心 2.0 中，新`ClientSetNull`作为的默认值为可选关系引入了行为。 此行为具有`SetNull`语义跟踪的实体和`Restrict`创建使用 EF 核心的数据库的行为。 在我们的经验，它们是跟踪的实体和数据库最预期/有用的行为。 `DeleteBehavior.Restrict`现在会遵循的跟踪时设置的可选关系的实体。

### <a name="provider-design-time-packages-removed"></a>删除的提供程序设计时包

`Microsoft.EntityFrameworkCore.Relational.Design`删除包。 它的内容已合并到`Microsoft.EntityFrameworkCore.Relational`和`Microsoft.EntityFrameworkCore.Design`。

这将传播到提供程序的设计时包。 这些包 (`Microsoft.EntityFrameworkCore.Sqlite.Design`，`Microsoft.EntityFrameworkCore.SqlServer.Design`等) 已删除和其内容合并到主提供程序的包。

若要启用`Scaffold-DbContext`或`dotnet ef dbcontext scaffold`在 EF 核心 2.0 中，你只需引用单个提供程序包：

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="2.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools"
    Version="2.0.0"
    PrivateAssets="All" />
<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
    Version="2.0.0" />
```
