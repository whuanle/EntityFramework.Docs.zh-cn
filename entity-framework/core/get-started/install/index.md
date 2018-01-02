---
title: "安装 EF Core"
author: divega
ms.author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
ms.technology: entity-framework-core
uid: core/get-started/install/index
ms.openlocfilehash: 31b96ebd0ae282b88be98988eff6263084dc5dd5
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
---
# <a name="installing-ef-core"></a><span data-ttu-id="aa528-102">安装 EF Core</span><span class="sxs-lookup"><span data-stu-id="aa528-102">Installing EF Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aa528-103">先决条件</span><span class="sxs-lookup"><span data-stu-id="aa528-103">Prerequisites</span></span>

<span data-ttu-id="aa528-104">为了开发 .NET Core 2.0 应用程序（包括面向 .NET Core 的 ASP.NET Core 2.0 应用程序），需要下载并安装适合所用平台的 [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core) 版本。</span><span class="sxs-lookup"><span data-stu-id="aa528-104">In order to develop .NET Core 2.0 applications (including ASP.NET Core 2.0 applications that target .NET Core) you will need to download and install a version of the [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core) that is appropriate to your platform.</span></span> <span data-ttu-id="aa528-105">即使已安装 Visual Studio 2017 版本 15.3，也是如此。</span><span class="sxs-lookup"><span data-stu-id="aa528-105">**This is true even if you have installed Visual Studio 2017 version 15.3.**</span></span>

<span data-ttu-id="aa528-106">为了将 EF Core 2.0 或任何其他 .NET Standard 2.0 库用于非 .NET Core 2.0 版本的 .NET 平台（例如 NET Framework 4.6.1 或更高版本），需要可识别 .NET Standard 2.0 及其兼容框架的 NuGet 版本。</span><span class="sxs-lookup"><span data-stu-id="aa528-106">In order to use EF Core 2.0 or any other .NET Standard 2.0 library with a .NET platforms besides .NET Core 2.0 (e.g. with .NET Framework 4.6.1 or greater) you will need a version of NuGet that is aware of the .NET Standard 2.0 and its compatible frameworks.</span></span> <span data-ttu-id="aa528-107">可通过几种方法获取此版本：</span><span class="sxs-lookup"><span data-stu-id="aa528-107">Here are a few ways you can obtain this:</span></span>

* <span data-ttu-id="aa528-108">安装 Visual Studio 2017 版本 15.3</span><span class="sxs-lookup"><span data-stu-id="aa528-108">Install Visual Studio 2017 version 15.3</span></span>
* <span data-ttu-id="aa528-109">如果使用 Visual Studio 2015，请[下载 NuGet 客户端并升级至 3.6.0 版](https://www.nuget.org/downloads)</span><span class="sxs-lookup"><span data-stu-id="aa528-109">If you are using Visual Studio 2015, [download and upgrade NuGet client to version 3.6.0](https://www.nuget.org/downloads)</span></span>

<span data-ttu-id="aa528-110">为了与 .NET Framework 2.0 库兼容，使用以前版本的 Visual Studio 创建且面向 .NET Framework 的项目可能需要其他修改：</span><span class="sxs-lookup"><span data-stu-id="aa528-110">Projects created with previous versions of Visual Studio and targeting .NET Framework may need additional modifications in order to be compatible with .NET Standard 2.0 libraries:</span></span>

* <span data-ttu-id="aa528-111">编辑项目文件，并确保以下条目出现在初始属性组中：</span><span class="sxs-lookup"><span data-stu-id="aa528-111">Edit the project file and make sure the following entry appears in the initial property group:</span></span>
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* <span data-ttu-id="aa528-112">对于测试项目，还要确保存在以下条目：</span><span class="sxs-lookup"><span data-stu-id="aa528-112">For test projects, also make sure the following entry is present:</span></span>
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a><span data-ttu-id="aa528-113">获取相应工具</span><span class="sxs-lookup"><span data-stu-id="aa528-113">Getting the bits</span></span>
<span data-ttu-id="aa528-114">将 EF Core 运行时库添加到应用程序的推荐方法是，从 NuGet 安装 EF Core 数据库提供程序。</span><span class="sxs-lookup"><span data-stu-id="aa528-114">The recommended way to add EF Core runtime libraries into an application is to install an EF Core database provider from NuGet.</span></span>

<span data-ttu-id="aa528-115">除运行时库外，还可安装相应工具，以便在设计时更轻松地执行项目中的多个 EF Core 相关任务，例如创建和应用迁移以及基于现有数据库创建模型。</span><span class="sxs-lookup"><span data-stu-id="aa528-115">Besides the runtime libraries, you can install tools which make it easier to perform several EF Core-related tasks in your project at design time, such as creating and applying migrations, and creating a model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="aa528-116">如需更新使用第三方数据库提供程序的应用程序，请始终检查与要使用的 EF Core 版本兼容的提供程序有无更新。</span><span class="sxs-lookup"><span data-stu-id="aa528-116">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="aa528-117">例如</span><span class="sxs-lookup"><span data-stu-id="aa528-117">E.g.</span></span> <span data-ttu-id="aa528-118">早期版本的数据库提供程序不兼容 2.0 版 EF Core 运行时。</span><span class="sxs-lookup"><span data-stu-id="aa528-118">database providers for previous versions are not compatible with version 2.0 of the EF Core runtime.</span></span>  

> [!TIP]  
> <span data-ttu-id="aa528-119">面向 ASP.NET Core 2.0 的应用程序可以使用 EF Core 2.0，而不需要第三方数据库提供程序以外的其他依赖项。</span><span class="sxs-lookup"><span data-stu-id="aa528-119">Applications targeting ASP.NET Core 2.0 can use EF Core 2.0 without additional dependencies besides third party database providers.</span></span> <span data-ttu-id="aa528-120">面向 ASP.NET Core 早期版本的应用程序需升级至 ASP.NET Core 2.0 方可使用 EF Core 2.0。</span><span class="sxs-lookup"><span data-stu-id="aa528-120">Applications targeting previous versions of ASP.NET Core need to upgrade to ASP.NET Core 2.0 in order to use EF Core 2.0.</span></span>

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a><span data-ttu-id="aa528-121">使用 .NET Core 命令行接口 (CLI) 进行跨平台开发</span><span class="sxs-lookup"><span data-stu-id="aa528-121">Cross-platform development using the .NET Core Command Line Interface (CLI)</span></span>

<span data-ttu-id="aa528-122">要开发面向 [.NET Core](https://www.microsoft.com/net/download/core) 的应用程序，可以选择将 [`dotnet` CLI 命令](https://docs.microsoft.com/dotnet/core/tools/)与你喜爱的文本编辑器或集成开发环境 (IDE) 结合使用，例如 Visual Studio、Visual Studio for Mac 或 Visual Studio Code。</span><span class="sxs-lookup"><span data-stu-id="aa528-122">To develop applications that target [.NET Core](https://www.microsoft.com/net/download/core) you can choose to use the [`dotnet` CLI commands](https://docs.microsoft.com/dotnet/core/tools/) in combination with your favorite text editor, or an Integrated Development Environment (IDE) such as Visual Studio, Visual Studio for Mac or Visual Studio Code.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="aa528-123">面向 .NET Core 的应用程序需要使用特定版本的 Visual Studio，例如 .NET Core 1.x 开发需要使用 Visual Studio 2017，而 .NET Core 2.0 开发需要使用 Visual Studio 2017 版本 15.3。</span><span class="sxs-lookup"><span data-stu-id="aa528-123">Applications that target .NET Core require specific versions of Visual Studio, e.g. .NET Core 1.x development requires Visual Studio 2017, while .NET Core 2.0 development requires Visual Studio 2017 version 15.3.</span></span>

<span data-ttu-id="aa528-124">要在跨平台 .NET Core 应用程序中安装或升级 SQL Server 提供程序，请切换至应用程序的目录，并在命令行中运行以下命令：</span><span class="sxs-lookup"><span data-stu-id="aa528-124">To install or upgrade the SQL Server provider in a cross-platform .NET Core application, switch to the application's directory and run the following in a command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="aa528-125">可以使用 `-v` 修饰符在 `dotnet add package` 命令中指明特定的安装版本。</span><span class="sxs-lookup"><span data-stu-id="aa528-125">You can indicate a specific version install in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="aa528-126">例如</span><span class="sxs-lookup"><span data-stu-id="aa528-126">E.g.</span></span> <span data-ttu-id="aa528-127">要安装 EF Core 2.0 包，请将 `-v 2.0.0` 追加到此命令。</span><span class="sxs-lookup"><span data-stu-id="aa528-127">to install EF Core 2.0 packages, append `-v 2.0.0` to the command.</span></span>

<span data-ttu-id="aa528-128">EF Core 包含一套以 `dotnet ef` 开头的[附加 `dotnet` CLI 命令](../../miscellaneous/cli/dotnet.md)。</span><span class="sxs-lookup"><span data-stu-id="aa528-128">EF Core includes a set of [additional commands for the `dotnet` CLI](../../miscellaneous/cli/dotnet.md), starting with `dotnet ef`.</span></span> <span data-ttu-id="aa528-129">要使用 `dotnet ef` CLI 命令，应用程序的 `.csproj` 文件需包含以下条目：</span><span class="sxs-lookup"><span data-stu-id="aa528-129">In order to use the `dotnet ef` CLI commands, your application’s `.csproj` file needs to contain the following entry:</span></span>

``` xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
</ItemGroup>
```

<span data-ttu-id="aa528-130">用于 EF Core 的 .NET Core CLI 工具还需要一个名为 Microsoft.EntityFrameworkCore.Design 的单独包。</span><span class="sxs-lookup"><span data-stu-id="aa528-130">The .NET Core CLI tools for EF Core also require a separate package called Microsoft.EntityFrameworkCore.Design.</span></span> <span data-ttu-id="aa528-131">可使用以下命令将其轻松添加至项目：</span><span class="sxs-lookup"><span data-stu-id="aa528-131">You can simply add it to the project using:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Design
```

> [!IMPORTANT]  
> <span data-ttu-id="aa528-132">请务必使用与运行时包主版本相匹配的工具包版本。</span><span class="sxs-lookup"><span data-stu-id="aa528-132">Always use versions of the tools packages that match the major version of the runtime packages.</span></span>

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a><span data-ttu-id="aa528-133">Visual Studio 开发</span><span class="sxs-lookup"><span data-stu-id="aa528-133">Visual Studio development</span></span>

<span data-ttu-id="aa528-134">使用 Visual Studio 可以开发许多不同类型的应用程序，这些应用程序面向 .NET Core、.NET Framework 或受 EF Core 支持的其他平台。</span><span class="sxs-lookup"><span data-stu-id="aa528-134">You can develop many different types of applications that target .NET Core, .NET Framework, or other platforms supported by EF Core using Visual Studio.</span></span>

<span data-ttu-id="aa528-135">使用 Visual Studio 可通过两种方式在应用程序中安装 EF Core 数据库提供程序：</span><span class="sxs-lookup"><span data-stu-id="aa528-135">There are two ways you can install an EF Core database provider in your application from Visual Studio:</span></span>

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a><span data-ttu-id="aa528-136">使用 NuGet 的[包管理器用户界面](https://docs.microsoft.com/nuget/tools/package-manager-ui)</span><span class="sxs-lookup"><span data-stu-id="aa528-136">Using NuGet's [Package Manager User Interface](https://docs.microsoft.com/nuget/tools/package-manager-ui)</span></span>

* <span data-ttu-id="aa528-137">在菜单上选择“项目”>“管理 NuGet 程序包”</span><span class="sxs-lookup"><span data-stu-id="aa528-137">Select on the menu **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="aa528-138">单击“浏览”或“更新”选项卡</span><span class="sxs-lookup"><span data-stu-id="aa528-138">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="aa528-139">选择 `Microsoft.EntityFrameworkCore.SqlServer` 包及所需版本，然后确认</span><span class="sxs-lookup"><span data-stu-id="aa528-139">Select the `Microsoft.EntityFrameworkCore.SqlServer` package and the desired version and confirm</span></span>

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a><span data-ttu-id="aa528-140">使用 NuGet 的[包管理器控制台 (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)</span><span class="sxs-lookup"><span data-stu-id="aa528-140">Using NuGet's [Package Manager Console (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)</span></span>

* <span data-ttu-id="aa528-141">在菜单上选择“工具”>“NuGet 包管理器”>“包管理器控制台”</span><span class="sxs-lookup"><span data-stu-id="aa528-141">Select on the menu **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="aa528-142">在 PMC 中键入并运行以下命令：</span><span class="sxs-lookup"><span data-stu-id="aa528-142">Type and run the following command in the PMC:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="aa528-143">可以改为使用 `Update-Package` 命令将已安装的包更新至较新版本</span><span class="sxs-lookup"><span data-stu-id="aa528-143">You can use the `Update-Package` command instead to update a package that is already installed to a more recent  version</span></span>

* <span data-ttu-id="aa528-144">要指定特定版本，可使用 `-Version` 修饰符；例如，要安装 EF Core 2.0 包，可将 `-Version 2.0.0` 追加到该命令</span><span class="sxs-lookup"><span data-stu-id="aa528-144">To specify a specific version, you can use the `-Version` modifier, e.g. to install EF Core 2.0 packages, append `-Version 2.0.0` to the commands</span></span>

#### <a name="tools"></a><span data-ttu-id="aa528-145">工具</span><span class="sxs-lookup"><span data-stu-id="aa528-145">Tools</span></span>

<span data-ttu-id="aa528-146">Visual Studio 中还有 PowerShell 版本的[在 PMC 内运行的 EF Core 命令](../../miscellaneous/cli/powershell.md)，其功能与 `dotnet ef` 命令相似。</span><span class="sxs-lookup"><span data-stu-id="aa528-146">There is also a PowerShell version of the [EF Core commands which run inside the PMC](../../miscellaneous/cli/powershell.md) in Visual Studio, with similar capabilities to the `dotnet ef` commands.</span></span> <span data-ttu-id="aa528-147">要使用这些命令，请使用包管理器 UI 或 PMC 安装 `Microsoft.EntityFrameworkCore.Tools` 包。</span><span class="sxs-lookup"><span data-stu-id="aa528-147">In order to use these, install the `Microsoft.EntityFrameworkCore.Tools` package using either the Package Manager UI or the PMC.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="aa528-148">请务必使用与运行时包主版本相匹配的工具包版本。</span><span class="sxs-lookup"><span data-stu-id="aa528-148">Always use versions of the tools packages that match the major version of the runtime packages.</span></span>

> [!TIP]  
> <span data-ttu-id="aa528-149">尽管可以在 Visual Studio 中使用来自 PMC 的 `dotnet ef` 命令，但使用 PowerShell 版本要方便得多：</span><span class="sxs-lookup"><span data-stu-id="aa528-149">Although it is possible to use the `dotnet ef` commands from the PMC in Visual Studio, it is far more convenient to use the PowerShell version:</span></span>
> * <span data-ttu-id="aa528-150">它们会自动使用 PMC 中选择的当前项目，无需手动切换目录。</span><span class="sxs-lookup"><span data-stu-id="aa528-150">They automatically work with the current project selected in the PMC without requiring manually switching directories.</span></span>  
> * <span data-ttu-id="aa528-151">命令完成后，它们会自动在 Visual Studio 中打开命令所生成的文件。</span><span class="sxs-lookup"><span data-stu-id="aa528-151">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="aa528-152">**EF Core 2.0 中的弃用包：**如果将现有应用程序升级至 EF Core 2.0，可能需要手动删除一些对旧版 EF Core 包的引用。</span><span class="sxs-lookup"><span data-stu-id="aa528-152">**Deprecated packages in EF Core 2.0:** If you are upgrading an existing application to EF Core 2.0, some references to older EF Core packages may need to be removed manually.</span></span> <span data-ttu-id="aa528-153">具体而言，EF Core 2.0 不再需要或支持 `Microsoft.EntityFrameworkCore.SqlServer.Design` 等数据库提供程序设计时包，但在升级其他包后，它们不会被自动删除。</span><span class="sxs-lookup"><span data-stu-id="aa528-153">In particular, database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported in EF Core 2.0, but will not be automatically removed when upgrading the other packages.</span></span>
