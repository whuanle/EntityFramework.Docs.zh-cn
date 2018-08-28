---
title: 安装 EF Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 30ca81a0ede65506a6684d2322d31332115b1ed3
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996923"
---
# <a name="installing-ef-core"></a><span data-ttu-id="81e2f-102">安装 EF Core</span><span class="sxs-lookup"><span data-stu-id="81e2f-102">Installing EF Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="81e2f-103">系统必备</span><span class="sxs-lookup"><span data-stu-id="81e2f-103">Prerequisites</span></span>

<span data-ttu-id="81e2f-104">必须下载并安装适合所用平台的 [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core) 版本，才能开发 .NET Core 2.1 应用程序（包括定目标到 .NET Core 的 ASP.NET Core 2.1 应用程序）。</span><span class="sxs-lookup"><span data-stu-id="81e2f-104">In order to develop .NET Core 2.1 applications (including ASP.NET Core 2.1 applications that target .NET Core) you will need to download and install a version of the [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core) that is appropriate to your platform.</span></span> <span data-ttu-id="81e2f-105">**即使已安装 Visual Studio 2017 版本 15.7，也不例外。**</span><span class="sxs-lookup"><span data-stu-id="81e2f-105">**This is true even if you have installed Visual Studio 2017 version 15.7.**</span></span>

<span data-ttu-id="81e2f-106">必须有可感知 .NET Standard 2.0 及其兼容框架的 NuGet 版本，才能将 EF Core 2.1 或其他任何 .NET Standard 2.0 库用于除 .NET Core 2.1 以外的 .NET 平台（例如 .NET Framework 4.6.1 或更高版本）。</span><span class="sxs-lookup"><span data-stu-id="81e2f-106">In order to use EF Core 2.1 or any other .NET Standard 2.0 library with a .NET platform besides .NET Core 2.1 (for example, with .NET Framework 4.6.1 or greater) you will need a version of NuGet that is aware of the .NET Standard 2.0 and its compatible frameworks.</span></span> <span data-ttu-id="81e2f-107">可通过几种方法获取此版本：</span><span class="sxs-lookup"><span data-stu-id="81e2f-107">Here are a few ways you can obtain this:</span></span>

* <span data-ttu-id="81e2f-108">安装 Visual Studio 2017 版本 15.7</span><span class="sxs-lookup"><span data-stu-id="81e2f-108">Install Visual Studio 2017 version 15.7</span></span>
* <span data-ttu-id="81e2f-109">如果使用 Visual Studio 2015，请[下载 NuGet 客户端并升级至 3.6.0 版](https://www.nuget.org/downloads)</span><span class="sxs-lookup"><span data-stu-id="81e2f-109">If you are using Visual Studio 2015, [download and upgrade NuGet client to version 3.6.0](https://www.nuget.org/downloads)</span></span>

<span data-ttu-id="81e2f-110">为了与 .NET Framework 2.0 库兼容，使用以前版本的 Visual Studio 创建且面向 .NET Framework 的项目可能需要其他修改：</span><span class="sxs-lookup"><span data-stu-id="81e2f-110">Projects created with previous versions of Visual Studio and targeting .NET Framework may need additional modifications in order to be compatible with .NET Standard 2.0 libraries:</span></span>

* <span data-ttu-id="81e2f-111">编辑项目文件，并确保以下条目出现在初始属性组中：</span><span class="sxs-lookup"><span data-stu-id="81e2f-111">Edit the project file and make sure the following entry appears in the initial property group:</span></span>
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* <span data-ttu-id="81e2f-112">对于测试项目，还要确保存在以下条目：</span><span class="sxs-lookup"><span data-stu-id="81e2f-112">For test projects, also make sure the following entry is present:</span></span>
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a><span data-ttu-id="81e2f-113">获取相应工具</span><span class="sxs-lookup"><span data-stu-id="81e2f-113">Getting the bits</span></span>
<span data-ttu-id="81e2f-114">将 EF Core 运行时库添加到应用程序的推荐方法是，从 NuGet 安装 EF Core 数据库提供程序。</span><span class="sxs-lookup"><span data-stu-id="81e2f-114">The recommended way to add EF Core runtime libraries into an application is to install an EF Core database provider from NuGet.</span></span>

<span data-ttu-id="81e2f-115">除运行时库外，还可安装相应工具，以便在设计时更轻松地执行项目中的多个 EF Core 相关任务，例如创建和应用迁移以及基于现有数据库创建模型。</span><span class="sxs-lookup"><span data-stu-id="81e2f-115">Besides the runtime libraries, you can install tools which make it easier to perform several EF Core-related tasks in your project at design time, such as creating and applying migrations, and creating a model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="81e2f-116">如需更新使用第三方数据库提供程序的应用程序，请始终检查与要使用的 EF Core 版本兼容的提供程序有无更新。</span><span class="sxs-lookup"><span data-stu-id="81e2f-116">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="81e2f-117">例如，旧版本使用的数据库提供程序与 2.1 版 EF Core 运行时不兼容。</span><span class="sxs-lookup"><span data-stu-id="81e2f-117">For example, database providers for previous versions are not compatible with version 2.1 of the EF Core runtime.</span></span>  

> [!TIP]  
> <span data-ttu-id="81e2f-118">定目标到 ASP.NET Core 2.1 的应用程序可以使用 EF Core 2.1，无需除第三方数据库提供程序以外的其他依赖项。</span><span class="sxs-lookup"><span data-stu-id="81e2f-118">Applications targeting ASP.NET Core 2.1 can use EF Core 2.1 without additional dependencies besides third party database providers.</span></span> <span data-ttu-id="81e2f-119">定目标到旧版 ASP.NET Core 的应用程序必须升级到 ASP.NET Core 2.1，才能使用 EF Core 2.1。</span><span class="sxs-lookup"><span data-stu-id="81e2f-119">Applications targeting previous versions of ASP.NET Core need to upgrade to ASP.NET Core 2.1 in order to use EF Core 2.1.</span></span>

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a><span data-ttu-id="81e2f-120">使用 .NET Core 命令行接口 (CLI) 进行跨平台开发</span><span class="sxs-lookup"><span data-stu-id="81e2f-120">Cross-platform development using the .NET Core Command Line Interface (CLI)</span></span>

<span data-ttu-id="81e2f-121">若要开发定目标到 [.NET Core](https://www.microsoft.com/net/download/core) 的应用程序，可以选择结合使用 [`dotnet` CLI 命令](https://docs.microsoft.com/dotnet/core/tools/)与常用文本编辑器或集成开发环境 (IDE)（如 Visual Studio、Visual Studio for Mac 或 Visual Studio Code）。</span><span class="sxs-lookup"><span data-stu-id="81e2f-121">To develop applications that target [.NET Core](https://www.microsoft.com/net/download/core) you can choose to use the [`dotnet` CLI commands](https://docs.microsoft.com/dotnet/core/tools/) in combination with your favorite text editor, or an Integrated Development Environment (IDE) such as Visual Studio, Visual Studio for Mac, or Visual Studio Code.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="81e2f-122">面向 .NET Core 的应用程序需要特定版本的 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="81e2f-122">Applications that target .NET Core require specific versions of Visual Studio.</span></span> <span data-ttu-id="81e2f-123">例如，.NET Core 1.x 开发需要 Visual Studio 2017，而 .NET Core 2.1 开发则需要 Visual Studio 2017 版本 15.7。</span><span class="sxs-lookup"><span data-stu-id="81e2f-123">For example, .NET Core 1.x development requires Visual Studio 2017, while .NET Core 2.1 development requires Visual Studio 2017 version 15.7.</span></span>

<span data-ttu-id="81e2f-124">要在跨平台 .NET Core 应用程序中安装或升级 SQL Server 提供程序，请切换至应用程序的目录，并在命令行中运行以下命令：</span><span class="sxs-lookup"><span data-stu-id="81e2f-124">To install or upgrade the SQL Server provider in a cross-platform .NET Core application, switch to the application's directory and run the following in a command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="81e2f-125">可以使用 `-v` 修饰符在 `dotnet add package` 命令中指明特定的安装版本。</span><span class="sxs-lookup"><span data-stu-id="81e2f-125">You can indicate a specific version install in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="81e2f-126">例如，若要安装 EF Core 2.1 包，请将 `-v 2.1.0` 追加到命令中。</span><span class="sxs-lookup"><span data-stu-id="81e2f-126">For example, to install EF Core 2.1 packages, append `-v 2.1.0` to the command.</span></span>

<span data-ttu-id="81e2f-127">EF Core 包含一套以 `dotnet ef` 开头的[附加 `dotnet` CLI 命令](../../miscellaneous/cli/dotnet.md)。</span><span class="sxs-lookup"><span data-stu-id="81e2f-127">EF Core includes a set of [additional commands for the `dotnet` CLI](../../miscellaneous/cli/dotnet.md), starting with `dotnet ef`.</span></span> <span data-ttu-id="81e2f-128">适用于 EF Core 的 .NET Core CLI 工具需要包 `Microsoft.EntityFrameworkCore.Design`。</span><span class="sxs-lookup"><span data-stu-id="81e2f-128">The .NET Core CLI tools for EF Core require a package called `Microsoft.EntityFrameworkCore.Design`.</span></span> <span data-ttu-id="81e2f-129">可以运行下面的命令，将它添加到项目中：</span><span class="sxs-lookup"><span data-stu-id="81e2f-129">You can add it to the project using:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

> [!IMPORTANT]      
> <span data-ttu-id="81e2f-130">请务必使用与运行时包主版本匹配的工具包版本。</span><span class="sxs-lookup"><span data-stu-id="81e2f-130">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a><span data-ttu-id="81e2f-131">Visual Studio 开发</span><span class="sxs-lookup"><span data-stu-id="81e2f-131">Visual Studio development</span></span>

<span data-ttu-id="81e2f-132">使用 Visual Studio 可以开发许多不同类型的应用程序，这些应用程序面向 .NET Core、.NET Framework 或受 EF Core 支持的其他平台。</span><span class="sxs-lookup"><span data-stu-id="81e2f-132">You can develop many different types of applications that target .NET Core, .NET Framework, or other platforms supported by EF Core using Visual Studio.</span></span>

<span data-ttu-id="81e2f-133">使用 Visual Studio 可通过两种方式在应用程序中安装 EF Core 数据库提供程序：</span><span class="sxs-lookup"><span data-stu-id="81e2f-133">There are two ways you can install an EF Core database provider in your application from Visual Studio:</span></span>

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a><span data-ttu-id="81e2f-134">使用 NuGet 的[包管理器用户界面](https://docs.microsoft.com/nuget/tools/package-manager-ui)</span><span class="sxs-lookup"><span data-stu-id="81e2f-134">Using NuGet's [Package Manager User Interface](https://docs.microsoft.com/nuget/tools/package-manager-ui)</span></span>

* <span data-ttu-id="81e2f-135">在菜单上选择“项目”>“管理 NuGet 程序包”</span><span class="sxs-lookup"><span data-stu-id="81e2f-135">Select on the menu **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="81e2f-136">单击“浏览”或“更新”选项卡</span><span class="sxs-lookup"><span data-stu-id="81e2f-136">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="81e2f-137">选择 `Microsoft.EntityFrameworkCore.SqlServer` 包及所需版本，然后确认</span><span class="sxs-lookup"><span data-stu-id="81e2f-137">Select the `Microsoft.EntityFrameworkCore.SqlServer` package and the desired version and confirm</span></span>

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a><span data-ttu-id="81e2f-138">使用 NuGet 的[包管理器控制台 (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)</span><span class="sxs-lookup"><span data-stu-id="81e2f-138">Using NuGet's [Package Manager Console (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)</span></span>

* <span data-ttu-id="81e2f-139">在菜单上选择“工具”>“NuGet 包管理器”>“包管理器控制台”</span><span class="sxs-lookup"><span data-stu-id="81e2f-139">Select on the menu **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="81e2f-140">在 PMC 中键入并运行以下命令：</span><span class="sxs-lookup"><span data-stu-id="81e2f-140">Type and run the following command in the PMC:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="81e2f-141">可以改为使用 `Update-Package` 命令将已安装的包更新至较新版本</span><span class="sxs-lookup"><span data-stu-id="81e2f-141">You can use the `Update-Package` command instead to update a package that is already installed to a more recent  version</span></span>

* <span data-ttu-id="81e2f-142">若要指定特定版本，可以使用 `-Version` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="81e2f-142">To specify a specific version, you can use the `-Version` modifier.</span></span> <span data-ttu-id="81e2f-143">例如，若要安装 EF Core 2.1 包，请将 `-Version 2.1.0` 追加到命令中</span><span class="sxs-lookup"><span data-stu-id="81e2f-143">For example, to install EF Core 2.1 packages, append `-Version 2.1.0` to the commands</span></span>

#### <a name="tools"></a><span data-ttu-id="81e2f-144">工具</span><span class="sxs-lookup"><span data-stu-id="81e2f-144">Tools</span></span>

<span data-ttu-id="81e2f-145">Visual Studio 中还有 PowerShell 版本的[在 PMC 内运行的 EF Core 命令](../../miscellaneous/cli/powershell.md)，其功能与 `dotnet ef` 命令相似。</span><span class="sxs-lookup"><span data-stu-id="81e2f-145">There is also a PowerShell version of the [EF Core commands which run inside the PMC](../../miscellaneous/cli/powershell.md) in Visual Studio, with similar capabilities to the `dotnet ef` commands.</span></span> 

> [!TIP]  
> <span data-ttu-id="81e2f-146">尽管可以在 Visual Studio 中使用来自 PMC 的 `dotnet ef` 命令，但使用 PowerShell 版本要方便得多：</span><span class="sxs-lookup"><span data-stu-id="81e2f-146">Although it is possible to use the `dotnet ef` commands from the PMC in Visual Studio, it is far more convenient to use the PowerShell version:</span></span>
> * <span data-ttu-id="81e2f-147">它们会自动使用 PMC 中选择的当前项目，无需手动切换目录。</span><span class="sxs-lookup"><span data-stu-id="81e2f-147">They automatically work with the current project selected in the PMC without requiring manually switching directories.</span></span>  
> * <span data-ttu-id="81e2f-148">命令完成后，它们会自动在 Visual Studio 中打开命令所生成的文件。</span><span class="sxs-lookup"><span data-stu-id="81e2f-148">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="81e2f-149">**EF Core 2.1 中的弃用包：** 若要将现有应用程序升级到 EF Core 2.1，可能需要手动删除一些对旧版 EF Core 包的引用：</span><span class="sxs-lookup"><span data-stu-id="81e2f-149">**Deprecated packages in EF Core 2.1:** If you're upgrading an existing application to EF Core 2.1, some references to older EF Core packages may need to be removed manually:</span></span>
> * <span data-ttu-id="81e2f-150">EF Core 2.1 不再需要或支持 `Microsoft.EntityFrameworkCore.SqlServer.Design` 等数据库提供程序设计时包，但在升级其他包后，它们不会被自动删除。</span><span class="sxs-lookup"><span data-stu-id="81e2f-150">Database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported in EF Core 2.1, but will not be automatically removed when upgrading the other packages.</span></span>
> * <span data-ttu-id="81e2f-151">.NET SDK 现包含 .NET CLI 工具，这样就可以从 .csproj 文件中删除对相应包的引用：</span><span class="sxs-lookup"><span data-stu-id="81e2f-151">The .NET CLI tools are now included in the .NET SDK, so the reference to that package can be removed from the *.csproj* file:</span></span>
>   ```
>   <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
>   ```
