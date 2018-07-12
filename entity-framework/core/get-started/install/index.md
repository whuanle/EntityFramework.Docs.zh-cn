---
title: 安装 EF Core
author: divega
ms.author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
ms.technology: entity-framework-core
uid: core/get-started/install/index
ms.openlocfilehash: 7bb2ee11940a4fd5736c7a23c16533ef53018f7b
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949187"
---
# <a name="installing-ef-core"></a>安装 EF Core

## <a name="prerequisites"></a>系统必备

为了开发 .NET Core 2.0 应用程序（包括面向 .NET Core 的 ASP.NET Core 2.0 应用程序），需要下载并安装适合所用平台的 [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core) 版本。 即使已安装 Visual Studio 2017 版本 15.3，也是如此。

为了将 EF Core 2.0 或任何其他 .NET Standard 2.0 库用于非 .NET Core 2.0 版本的 .NET 平台（例如 .NET Framework 4.6.1 或更高版本），需要可识别 .NET Standard 2.0 及其兼容框架的 NuGet 版本。 可通过几种方法获取此版本：

* 安装 Visual Studio 2017 版本 15.3
* 如果使用 Visual Studio 2015，请[下载 NuGet 客户端并升级至 3.6.0 版](https://www.nuget.org/downloads)

为了与 .NET Framework 2.0 库兼容，使用以前版本的 Visual Studio 创建且面向 .NET Framework 的项目可能需要其他修改：

* 编辑项目文件，并确保以下条目出现在初始属性组中：
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* 对于测试项目，还要确保存在以下条目：
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a>获取相应工具
将 EF Core 运行时库添加到应用程序的推荐方法是，从 NuGet 安装 EF Core 数据库提供程序。

除运行时库外，还可安装相应工具，以便在设计时更轻松地执行项目中的多个 EF Core 相关任务，例如创建和应用迁移以及基于现有数据库创建模型。

> [!TIP]  
> 如需更新使用第三方数据库提供程序的应用程序，请始终检查与要使用的 EF Core 版本兼容的提供程序有无更新。 例如，早期版本的数据库提供程序不兼容 2.0 版 EF Core 运行时。  

> [!TIP]  
> 面向 ASP.NET Core 2.0 的应用程序可以使用 EF Core 2.0，而不需要第三方数据库提供程序以外的其他依赖项。 面向 ASP.NET Core 早期版本的应用程序需升级至 ASP.NET Core 2.0 方可使用 EF Core 2.0。

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a>使用 .NET Core 命令行接口 (CLI) 进行跨平台开发

要开发面向 [.NET Core](https://www.microsoft.com/net/download/core) 的应用程序，可以选择将 [`dotnet` CLI 命令](https://docs.microsoft.com/dotnet/core/tools/)与你喜爱的文本编辑器或集成开发环境 (IDE) 结合使用，例如 Visual Studio、Visual Studio for Mac 或 Visual Studio Code。

> [!IMPORTANT]  
> 面向 .NET Core 的应用程序需要特定版本的 Visual Studio。 例如，.NET Core 1.x 开发需要 Visual Studio 2017，而 .NET Core 2.0 开发需要 Visual Studio 2017 版本 15.3。

要在跨平台 .NET Core 应用程序中安装或升级 SQL Server 提供程序，请切换至应用程序的目录，并在命令行中运行以下命令：

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

可以使用 `-v` 修饰符在 `dotnet add package` 命令中指明特定的安装版本。 例如，要安装 EF Core 2.0 包，请将 `-v 2.0.0` 追加到此命令。

EF Core 包含一套以 `dotnet ef` 开头的[附加 `dotnet` CLI 命令](../../miscellaneous/cli/dotnet.md)。 要使用 `dotnet ef` CLI 命令，应用程序的 `.csproj` 文件需包含以下条目：

``` xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
</ItemGroup>
```

用于 EF Core 的 .NET Core CLI 工具还需要一个名为 Microsoft.EntityFrameworkCore.Design 的单独包。 可使用以下命令将其轻松添加至项目：

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Design
```

> [!IMPORTANT]  
> 请务必使用与运行时包主版本相匹配的工具包版本。

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a>Visual Studio 开发

使用 Visual Studio 可以开发许多不同类型的应用程序，这些应用程序面向 .NET Core、.NET Framework 或受 EF Core 支持的其他平台。

使用 Visual Studio 可通过两种方式在应用程序中安装 EF Core 数据库提供程序：

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a>使用 NuGet 的[包管理器用户界面](https://docs.microsoft.com/nuget/tools/package-manager-ui)

* 在菜单上选择“项目”>“管理 NuGet 程序包”

* 单击“浏览”或“更新”选项卡

* 选择 `Microsoft.EntityFrameworkCore.SqlServer` 包及所需版本，然后确认

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a>使用 NuGet 的[包管理器控制台 (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)

* 在菜单上选择“工具”>“NuGet 包管理器”>“包管理器控制台”

* 在 PMC 中键入并运行以下命令：

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* 可以改为使用 `Update-Package` 命令将已安装的包更新至较新版本

* 若要指定特定版本，可以使用 `-Version` 修饰符。 例如，要安装 EF Core 2.0 包，请将 `-Version 2.0.0` 追加到此命令

#### <a name="tools"></a>工具

Visual Studio 中还有 PowerShell 版本的[在 PMC 内运行的 EF Core 命令](../../miscellaneous/cli/powershell.md)，其功能与 `dotnet ef` 命令相似。 要使用这些命令，请使用包管理器 UI 或 PMC 安装 `Microsoft.EntityFrameworkCore.Tools` 包。

> [!IMPORTANT]  
> 请务必使用与运行时包主版本相匹配的工具包版本。

> [!TIP]  
> 尽管可以在 Visual Studio 中使用来自 PMC 的 `dotnet ef` 命令，但使用 PowerShell 版本要方便得多：
> * 它们会自动使用 PMC 中选择的当前项目，无需手动切换目录。  
> * 命令完成后，它们会自动在 Visual Studio 中打开命令所生成的文件。

> [!IMPORTANT]  
> **EF Core 2.0 中的弃用包**：如果将现有应用程序升级至 EF Core 2.0，可能需要手动删除一些对旧版 EF Core 包的引用。 具体而言，EF Core 2.0 不再需要或支持 `Microsoft.EntityFrameworkCore.SqlServer.Design` 等数据库提供程序设计时包，但在升级其他包后，它们不会被自动删除。
