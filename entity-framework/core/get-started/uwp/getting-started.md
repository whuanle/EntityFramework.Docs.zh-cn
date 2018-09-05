---
title: UWP 入门 - 新数据库 - EF Core
author: rowanmiller
ms.date: 08/08/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: c243ef2a1940af9bf4f4b32f17acfcce7f972862
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996905"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a>通过新数据库在通用 Windows 平台 (UWP) 上开始使用 EF Core

在本教程中，将构建一个通用 Windows 平台 (UWP) 应用程序，该应用程序使用 Entity Framework Core 对本地 SQLite 数据库执行基本数据访问。

[在 GitHub 上查看此文章的示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP)。

## <a name="prerequisites"></a>系统必备

* [Windows 10 Fall Creators Update（10.0；内部版本 16299）或更改版本](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10)。

* 具有通用 Windows 平台开发工作负载的 [Visual Studio 2017 15.7 版或更高版本](https://www.visualstudio.com/downloads/)。

* [.NET Core 2.1 SDK 或更高版本](https://www.microsoft.com/net/core)。

## <a name="create-a-model-project"></a>创建模型项目

> [!IMPORTANT]
> 由于 .NET Core 工具与 UWP 项目交互的方式受到限制，因此该模型需要放在非 UWP 项目中才能在包管理器控制台 (PMC) 中运行迁移命令

* 打开 Visual Studio

* “文件”>“新建”>“项目”

* 从左侧菜单中选择“已安装”>“Visual C#”>“.NET Standard”。

* 选择“类库(.NET Standard)”模板。

* 将该项目命名为 Blogging.Model。

* 将该解决方案命名为 Blogging。

* 单击 **“确定”**。

## <a name="install-entity-framework-core"></a>安装 Entity Framework Core

要使用 EF Core，请为要作为目标对象的数据库提供程序安装程序包。 本教程使用 SQLite。 有关可用提供程序的列表，请参阅[数据库提供程序](../../providers/index.md)。

* “工具”>“NuGet 包管理器”>“包管理器控制台”。

* 运行 `Install-Package Microsoft.EntityFrameworkCore.Sqlite`

在本教程的后部分，将使用某些 Entity Framework Core 工具维护数据库。 因此，请同时安装该工具包。

* 运行 `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="create-the-model"></a>创建模型

现在是时候定义构成模型的上下文和实体类了。

* 删除 Class1.cs。

* 用以下代码创建 Model.cs：

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a>创建新的 UWP 项目

* 在解决方案资源管理器中，右键单击该解决方案，然后选择“添加”>“新项目”。

* 从左侧菜单中选择“已安装”>“Visual C#”>“Windows Universal”。

* 选择“空白应用(通用 Windows)”项目模板。

* 将项目命名为 Blogging.UWP ，然后单击“确定”

* 将目标和最低版本设置为至少“Windows 10 Fall Creators Update (10.0；内部版本 16299.0)”。

## <a name="create-the-initial-migration"></a>创建初始迁移

现已有一个模型，可设置应用以在第一次运行时创建数据库。 在本部分中，将创建初始迁移。 在接下来的部分中，将添加在应用启动时应用此迁移的代码。

迁移工具需要非 UWP 启动项目，因此需先创建该项目。

* 在解决方案资源管理器中，右键单击该解决方案，然后选择“添加”>“新项目”。

* 从左菜单中选择“已安装”>“Visual C#”>“.NET Core”。

* 选择“控制台应用程序(.NET Core)”项目模板。

* 将该项目命名为 Blogging.Migrations.Startup ，然后单击“确定”。

* 将 Blogging.Migrations.Startup 项目中的项目引用添加到 Blogging.Model 项目。

现在，可创建初始迁移。

* “工具”>“NuGet 包管理器”>“包管理器控制台”

* 选择 Blogging.Model 项目作为“默认项目”。

* 在解决方案资源管理器中，将 Blogging.Migrations.Startup 项目设置为启动项目。

* 运行 `Add-Migration InitialCreate`。

  此命令为迁移搭建基架，该迁移为模型创建一组初始表。

## <a name="create-the-database-on-app-startup"></a>在应用启动时创建数据库

由于希望在运行该应用的设备上创建数据库，因此，将添加代码，以便在应用程序启动时将任何待定迁移应用到本地数据库。 应用第一次运行时，该操作将创建本地数据库。

* 将 Blogging.UWP 项目中的项目引用添加到 Blogging.Model 项目。

* 打开 App.xaml.cs。

* 添加突出显示的代码，应用任何待定迁移。

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> 如果对模型进行了更改，则请使用 `Add-Migration` 命令为新迁移搭建基架，以便将相应更改应用到数据库。 当应用程序启动时，任何待定迁移都将应用到每个设备上的本地数据库。
>
>EF 使用数据库中的 `__EFMigrationsHistory` 表来跟踪哪些迁移已经应用到数据库。

## <a name="use-the-model"></a>使用模型

现在可以使用模型执行数据访问。

* 打开 MainPage.xaml.

* 添加下面突出显示的网页加载处理程序和 UI 内容

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

现在，添加代码来连接 UI 和数据库

* 打开 MainPage.xaml.cs.

* 从以下列表中添加突出显示的代码：

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

现在可以运行应用程序来查看其实际运行情况。

* 在解决方案资源管理器中，右键单击Blogging.UWP 项目，然后选择“部署”。

* 将 Blogging.UWP 设置为启动项目。

* “调试”>“开始执行(不调试)”

  将生成并运行测试应用。

* 输入 URL，然后单击“添加”按钮

  ![图像](_static/create.png)

  ![图像](_static/list.png)

  哇哦！ 现在便有了一个运行 Entity Framework Core 的简单 UWP 应用。

## <a name="next-steps"></a>后续步骤

有关结合使用 EF Core 和 UWP 时应了解的兼容性和性能信息，请参阅 [EF Core 支持的 .NET 实现](../../platforms/index.md#universal-windows-platform)。

查阅本文档中的其他文章，深入了解 Entity Framework Core 功能。
