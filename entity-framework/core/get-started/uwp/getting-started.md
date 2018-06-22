---
title: UWP 入门 - 新数据库 - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.topic: get-started-article
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
ms.technology: entity-framework-core
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: f743ff5392d1f30283a13d2e7fb8029be88387aa
ms.sourcegitcommit: 96324e58c02b97277395ed43173bf13ac80d2012
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2017
ms.locfileid: "26049830"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a>通过新数据库在通用 Windows 平台 (UWP) 上开始使用 EF Core

> [!NOTE]
> 本教程使用 EF Core 2.0.1（与 ASP.NET Core 和 .NET Core SDK 2.0.3 一起发布）。 EF Core 2.0.0 缺少一些重要的 bug 修复，因而无法提供良好的 UWP 体验。

在本演练中，你将构建一个通用 Windows 平台 (UWP) 应用程序，该程序使用 Entity Framework 对本地 SQLite 数据库执行基本数据访问。

> [!IMPORTANT]
> 考虑在针对 UWP 的 LINQ 查询中避免使用匿名类型。 将 UWP 应用程序部署到应用商店要求使用 .NET Native 编译应用程序。 使用匿名类型的查询在 .NET Native 上性能较差。

> [!TIP]
> 可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite)。

## <a name="prerequisites"></a>先决条件

完成本演练需要具有以下各项：

* [Windows 10 Fall Creators Update](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0)

* [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 或更高版本。

* 具有**通用 Windows 平台开发**工作负载的 [Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.4 版或更高版本。

## <a name="create-a-new-model-project"></a>创建新模型项目

> [!WARNING]
> 由于 .NET Core 工具与 UWP 项目交互的方式受到限制，因此该模型需要放在非 UWP 项目中才能在包管理器控制台中运行迁移命令

* 打开 Visual Studio

* “文件”>“新建”>“项目...”

* 从左侧菜单中选择“模板”>“Visual C#”

* 选择“类库(.NET Standard)”项目模板。

* 为项目提供名称，然后单击“确定”

## <a name="install-entity-framework"></a>安装 Entity Framework

要使用 EF Core，请为要作为目标对象的数据库提供程序安装程序包。 本演练使用 SQLite。 有关可用提供程序的列表，请参阅[数据库提供程序](../../providers/index.md)。

* “工具”>“NuGet 包管理器”>“包管理器控制台”

* 运行 `Install-Package Microsoft.EntityFrameworkCore.Sqlite`

在本演练的后面部分，我们还将一些 Entity Framework Tools 用于维护数据库。 因此，我们也会安装此工具包。

* 运行 `Install-Package Microsoft.EntityFrameworkCore.Tools`

* 编辑 .csproj 文件，并将 `<TargetFramework>netstandard2.0</TargetFramework>` 替换为 `<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`

## <a name="create-your-model"></a>创建你自己的模型

现在是时候定义构成模型的上下文和实体类了。

* “项目”>“添加类...”

* 输入“Model.cs”作为名称，然后单击“确定”

* 将此文件的内容替换为以下代码

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a>创建新的 UWP 项目

* 打开 Visual Studio

* “文件”>“新建”>“项目...”

* 从左侧菜单中选择“模板”>“Visual C#”>“Windows 通用”

* 选择“空白应用(通用 Windows)”项目模板

* 为项目提供名称，然后单击“确定”

* 将目标版本和最低版本至少设置为 `Windows 10 Fall Creators Update (10.0; build 16299.0)`

## <a name="create-your-database"></a>创建数据库

现在你已经有了模型，可以使用迁移为自己创建数据库。

* “工具”–>“NuGet 包管理器”–>“包管理器控制台”

* 选择作为默认项目的模型项目并将其设置为启动项目

* 运行 `Add-Migration MyFirstMigration` 来为迁移搭建基架，从而为模型创建一组初始表。

我们希望在运行该应用程序的设备上创建数据库，因此，我们将添加一些代码，以便在应用程序启动时将任何待定迁移应用到本地数据库。 应用程序第一次运行时，该操作将为我们创建本地数据库。

* 在“解决方案资源管理器”中，右键单击“App.xaml”，然后选择“查看代码”。

* 将突出显示的 using 添加到文件的开头

* 添加突出显示的代码，以应用任何待定迁移

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/App.xaml.cs?highlight=1,25-28)]

> [!TIP]  
> 如果将来对模型进行更改，则可以使用 `Add-Migration` 命令为新迁移搭建基架，以便将相应更改应用到数据库。 当应用程序启动时，任何待定迁移都将应用到每个设备上的本地数据库。
>
>EF 使用数据库中的 `__EFMigrationsHistory` 表来跟踪哪些迁移已经应用到数据库。

## <a name="use-your-model"></a>使用模型

你现在可以使用模型执行数据访问。

* 打开 MainPage.xaml

* 添加下面突出显示的网页加载处理程序和 UI 内容

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml?highlight=9,11-23)]

现在我们将添加代码来连接 UI 和数据库

* 在“解决方案资源管理器”中，右键单击“MainPage.xaml”，然后选择“查看代码”。

* 从以下列表中添加突出显示的代码

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml.cs?highlight=30-48)]

现在可以运行应用程序来查看其实际运行情况。

* “调试”>“开始执行(不调试)”

* 应用程序将生成并启动

* 输入 URL，然后单击“添加”按钮

![图像](_static/create.png)

![图像](_static/list.png)

## <a name="next-steps"></a>后续步骤

> [!TIP]
> 通过实现实体类型中的 [`INotifyPropertyChanged`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx)、[`INotifyPropertyChanging`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx)、[`INotifyCollectionChanged`](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) 并使用 `ChangeTrackingStrategy.ChangingAndChangedNotifications` 可改进 `SaveChanges()` 性能。

哇哦！ 你现在已经有了一个运行 Entity Framework 的简单 UWP 应用了。

查阅本文档中的其他文章，详细了解关于 Entity Framework 功能的信息。
