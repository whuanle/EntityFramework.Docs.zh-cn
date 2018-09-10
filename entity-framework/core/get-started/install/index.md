---
title: 安装 Entity Framework Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 7831e6a54e885cf0b89ef3eef2cd81a9292df606
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250317"
---
# <a name="installing-entity-framework-core"></a>安装 Entity Framework Core

## <a name="prerequisites"></a>系统必备

* 若要开发面向.NET Core 2.1 的应用，请安装 [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core)。 即使有 Visual Studio 2017 的最新版本，也必须安装 SDK。

* 若要使用 Visual Studio 开发面向 .NET Core 2.1 的应用，请安装 Visual Studio 2017 版本 15.7 或更高版本。

* 若要在 ASP.NET Core 应用程序中使用 Entity Framework 2.1，请使用 ASP.NET Core 2.1。 使用早期版本的 ASP.NET Core 的应用程序必须更新到 2.1。

* 可以将 Visual Studio 2015 用于面向 .NET Framework 4.6.1 或更高版本的应用。 但需要可识别 .NET Standard 2.0 及其兼容框架的 NuGet 版本。 若要在 Visual Studio 2015 中获取此版本，[将 NuGet 客户端升级到版本 3.6.0](https://www.nuget.org/downloads)。

## <a name="get-the-entity-framework-core-runtime"></a>获取 Entity Framework Core 运行时

要将 EF Core 运行时库添加到应用程序，可以为要使用的数据库提供程序安装 NuGet 包。 要获取支持的提供程序及其 NuGet 包名称的列表，请参阅[数据库提供程序](../../providers/index.md)。

要安装或更新 NuGet 包，可以使用 .NET Core CLI、Visual Studio 包管理器对话框或 Visual Studio 包管理器控制台。

ASP.NET Core 2.1 应用程序自动随附内存中和 SQL Server 提供程序，因此无需单独安装。

> [!TIP]  
> 如需更新使用第三方数据库提供程序的应用程序，请始终检查与要使用的 EF Core 版本兼容的提供程序有无更新。 例如，旧版本使用的数据库提供程序与 2.1 版 EF Core 运行时不兼容。  

### <a name="net-core-cli"></a>.NET Core CLI

以下 .NET Core CLI 命令安装或更新 SQL Server 提供程序：

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

可以使用 `-v` 修饰符在 `dotnet add package` 命令中指明特定的版本。 例如，若要安装 EF Core 2.1.0 包，请将 `-v 2.1.0` 追加到命令中。

### <a name="visual-studio-nuget-package-manager-dialog"></a>Visual Studio NuGet 包管理器对话框

* 在菜单上选择“项目”>“管理 NuGet 程序包”

* 单击“浏览”或“更新”选项卡

* 若要安装或更新 SQL Server 提供程序，请选择 `Microsoft.EntityFrameworkCore.SqlServer` 包并确认。

有关详细信息，请参阅 [NuGet 包管理器对话框](https://docs.microsoft.com/nuget/tools/package-manager-ui)。

### <a name="visual-studio-nuget-package-manager-console"></a>Visual Studio NuGet 包管理器控制台

* 从菜单中，选择“工具”>“NuGet 包管理器”>“包管理器控制台”

* 若要安装 SQL Server 提供程序，请在包管理器控制台中运行以下命令：

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* 若要更新提供程序，使用 `Update-Package` 命令。

* 若要指定特定版本，可以使用 `-Version` 修饰符。 例如，若要安装 EF Core 2.1.0 包，请将 `-Version 2.1.0` 追加到命令中

有关详细信息，请参阅[包管理器控制台](https://docs.microsoft.com/nuget/tools/package-manager-console)。

## <a name="get-entity-framework-core-tools"></a>获取 Entity Framework Core 工具

除了运行时库，还可以安装设计时可在项目中执行一些与 EF Core 相关的任务的工具。 例如，可以创建迁移、应用迁移，并创建基于现有数据库的模型。

提供了两个工具集：
* .NET Core [命令行接口 (CLI) 工具](../../miscellaneous/cli/dotnet.md)可用于 Windows、Linux 或 macOS。 这些命令以 `dotnet ef` 开头。 
* [包管理器控制台工具](../../miscellaneous/cli/powershell.md)在 Windows 上的 Visual Studio 2017 中运行。 这些命令以动词开头，例如 `Add-Migration`、`Update-Database`。

虽然可在包管理器控制台中使用 `dotnet ef` 命令，但在使用 Visual Studio 时使用包管理器控制台工具更方便：
* 它们会自动使用包管理器控制台中选择的当前项目，无需手动切换目录。  
* 命令完成后，它们会自动在 Visual Studio 中打开命令所生成的文件。

<a name="cli"></a>

### <a name="get-the-cli-tools"></a>获取 CLI 工具

`dotnet ef` 命令随附在 .NET Core SDK 中，但若要启用这些命令，必须安装 `Microsoft.EntityFrameworkCore.Design` 包：

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

ASP.NET Core 2.1 应用自动随附此包。

如前面[先决条件](#prerequisites)所述，还需要安装 .NET Core 2.1 SDK。

> [!IMPORTANT]      
> 请务必使用与运行时包主版本匹配的工具包版本。

### <a name="get-the-package-manager-console-tools"></a>获取包管理器控制台工具

若要获取适用于 EF Core 的包管理器控制台工具，请安装 `Microsoft.EntityFrameworkCore.Tools` 包：

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Tools
``` 

ASP.NET Core 2.1 应用自动随附此包。

## <a name="upgrading-to-ef-core-21"></a>升级到 EF Core 2.1

若要将现有应用程序升级到 EF Core 2.1，可能需要手动删除一些对旧版 EF Core 包的引用：

* EF Core 2.1 不再需要或支持 `Microsoft.EntityFrameworkCore.SqlServer.Design` 等数据库提供程序设计时包，但在升级其他包后，它们不会被自动删除。

* .NET SDK 现包含 .NET CLI 工具，这样就可以从 .csproj 文件中删除对相应包的引用：

  ```
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  ```

对于面向 .NET Framework 且由 Visual Studio 早期版本创建的应用程序，请确保它们与 .NET Standard 2.0 库兼容：

  * 编辑项目文件，并确保以下条目出现在初始属性组中：

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * 对于测试项目，还要确保存在以下条目：

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
