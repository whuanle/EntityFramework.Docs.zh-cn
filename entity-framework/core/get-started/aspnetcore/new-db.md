---
title: ASP.NET Core入门 - 新数据库 - EF Core
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: c6a86dd943dc7fe6f600455fe6743ea01a062aab
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996059"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a>使用新数据库在 ASP.NET Core 上开始使用 EF Core

在本教程中，你将使用 Entity Framework Core 构建执行基本数据访问的 ASP.NET Core MVC 应用程序。 使用迁移功能从 EF Core 模型创建数据库。

[在 GitHub 上查看此文章的示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb)。

## <a name="prerequisites"></a>系统必备

安装以下软件：

* 具有以下工作负载的 [Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/)：
  * “ASP.NET 和 Web 开发”（位于“Web 和云”下）
  * “.NET Core 跨平台开发”（位于“其他工具集”下）
* [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).

## <a name="create-a-new-project-in-visual-studio-2017"></a>在 Visual Studio 2017 中创建新项目

* 打开 Visual Studio 2017
* “文件”>“新建”>“项目”
* 从左菜单中选择“已安装”>“Visual C#”>“.NET Core”。
* 选择“ASP.NET Core Web 应用程序”。
* 输入“EFGetStarted.AspNetCore.NewDb”作为名称，然后单击“确定”。
* 在“新建 ASP.NET Core Web 应用程序”对话框中：
  * 确保在下拉列表中选择“.NET Core”和“ASP.NET Core 2.1”选项
  * 选择“Web 应用程序(模型视图控制器)”项目模板
  * 确保将“身份验证”设置为“无身份验证”
  * 单击“确定” 

警告：如果你使用“个人用户帐户”而不是“无身份验证”，则会将 Entity Framework Core 模型添加到 `Models\IdentityModel.cs` 中的项目中。 使用在本教程中学习的技巧，可以选择添加第二个模型，或者扩展此现有模型以包含实体类。

## <a name="install-entity-framework-core"></a>安装 Entity Framework Core

要安装 EF Core，请为要作为目标对象的 EF Core 数据库提供程序安装程序包。 有关可用提供程序的列表，请参阅[数据库提供程序](../../providers/index.md)。 

对于本教程，无需安装提供程序包，因为本教程使用 SQL Server。 SQL Server 提供程序包包含在 [Microsoft.AspnetCore.App 元包](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1)中。

## <a name="create-the-model"></a>创建模型

定义构成模型的上下文类和实体类：

* 右键单击“Models”文件夹，然后选择“添加”>“类”。
* 输入“Model.cs”作为名称，然后单击“确定”。
* 将此文件的内容替换为以下代码：

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

在真正的应用程序中，通常会将模型中的每个类放在单独的文件中。 为简单起见，本教程将所有类放在一个文件中。

## <a name="register-your-context-with-dependency-injection"></a>通过依赖关系注入注册上下文

服务（例如 `BloggingContext`）在应用程序启动期间通过[依赖关系注入](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html)进行注册。 然后，通过构造函数参数或属性为需要这些服务的组件（如 MVC 控制器）提供相应服务。

若要使 `BloggingContext` 对 MVC 控制器可用，请将其注册为服务。

* 打开 Startup.cs
* 添加以下 `using` 语句：

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

调用 `AddDbContext` 方法，将上下文注册为服务。

* 将以下突出显示的代码添加到 `ConfigureServices` 方法：

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=13-14)]

注意：真正的应用程序通常会将连接字符串放在配置文件或环境变量中。 为简单起见，本教程在代码中定义它。 有关详细信息，请参阅[连接字符串 ](../../miscellaneous/connection-strings.md)。

## <a name="create-the-database"></a>创建数据库

有了模型后，你可以使用[迁移](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)创建数据库。

* “工具”>“NuGet 包管理器”>“包管理器控制台”
* 运行 `Add-Migration InitialCreate` 来为迁移搭建基架，从而为模型创建一组初始表。 如果收到错误，指出 `The term 'add-migration' is not recognized as the name of a cmdlet`，请关闭并重新打开 Visual Studio。
* 运行 `Update-Database` 以将新迁移应用到数据库。 在应用迁移之前，此命令可创建数据库。

## <a name="create-a-controller"></a>创建控制器

生成 `Blog` 实体控制器和视图的基架。

* 在“解决方案资源管理器”中，右键单击“控制器”文件夹，然后选择“添加”>“控制器”。
* 选择“视图使用 Entity Framework 的 MVC 控制器”，然后单击“添加”。
* 将“模型类”设置为“Blog”，将“数据上下文类”设置为“BloggingContext”。
* 单击 **添加**。


## <a name="run-the-application"></a>运行此应用程序

按 F5 以运行和测试应用程序。

* 导航到 `/Blogs`
* 使用创建链接创建一些博客条目。 测试“详细信息”和“删除”链接。

![图像](_static/create.png)

![图像](_static/index-new-db.png)

## <a name="additional-resources"></a>其他资源

* [EF - 通过 SQLite 新建数据库](xref:core/get-started/netcore/new-db-sqlite) - 跨平台的控制台 EF 教程。
* [Mac 或 Linux 上的 ASP.NET Core MVC 简介](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [具有 Visual Studio 的 ASP.NET Core MVC 简介](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [借助 Visual Studio 使用 ASP.NET Core 和 Entity Framework Core 入门](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
