---
title: 安装 Entity Framework Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 455eccbb149f0980cefa250ef5db443c73e66603
ms.sourcegitcommit: ae399f9f3d1bae2c446b552247bd3af3ca5a2cf9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2018
ms.locfileid: "48575634"
---
# <a name="installing-entity-framework-core"></a><span data-ttu-id="82768-102">安装 Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="82768-102">Installing Entity Framework Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="82768-103">系统必备</span><span class="sxs-lookup"><span data-stu-id="82768-103">Prerequisites</span></span>

* <span data-ttu-id="82768-104">若要开发面向.NET Core 2.1 的应用，请安装 [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core)。</span><span class="sxs-lookup"><span data-stu-id="82768-104">To develop apps that target .NET Core 2.1, install [the .NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span> <span data-ttu-id="82768-105">即使有 Visual Studio 2017 的最新版本，也必须安装 SDK。</span><span class="sxs-lookup"><span data-stu-id="82768-105">The SDK has to be installed even if you have the latest version of Visual Studio 2017.</span></span>

* <span data-ttu-id="82768-106">若要使用 Visual Studio 开发面向 .NET Core 2.1 的应用，请安装 Visual Studio 2017 版本 15.7 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="82768-106">To use Visual Studio for development of apps that target .NET Core 2.1, install Visual Studio 2017 version 15.7 or later.</span></span>

* <span data-ttu-id="82768-107">若要在 ASP.NET Core 应用程序中使用 Entity Framework 2.1，请使用 ASP.NET Core 2.1。</span><span class="sxs-lookup"><span data-stu-id="82768-107">To use Entity Framework 2.1 in ASP.NET Core applications, use ASP.NET Core 2.1.</span></span> <span data-ttu-id="82768-108">使用早期版本的 ASP.NET Core 的应用程序必须更新到 2.1。</span><span class="sxs-lookup"><span data-stu-id="82768-108">Applications that use earlier versions of ASP.NET Core must be updated to 2.1.</span></span>

* <span data-ttu-id="82768-109">可以将 Visual Studio 2015 用于面向 .NET Framework 4.6.1 或更高版本的应用。</span><span class="sxs-lookup"><span data-stu-id="82768-109">You can use Visual Studio 2015 for apps that target the .NET Framework 4.6.1 or later.</span></span> <span data-ttu-id="82768-110">但需要可识别 .NET Standard 2.0 及其兼容框架的 NuGet 版本。</span><span class="sxs-lookup"><span data-stu-id="82768-110">But you need a version of NuGet that is aware of the .NET Standard 2.0 and its compatible frameworks.</span></span> <span data-ttu-id="82768-111">若要在 Visual Studio 2015 中获取此版本，[将 NuGet 客户端升级到版本 3.6.0](https://www.nuget.org/downloads)。</span><span class="sxs-lookup"><span data-stu-id="82768-111">To get that in Visual Studio 2015, [upgrade the NuGet client to version 3.6.0](https://www.nuget.org/downloads).</span></span>

## <a name="get-the-entity-framework-core-runtime"></a><span data-ttu-id="82768-112">获取 Entity Framework Core 运行时</span><span class="sxs-lookup"><span data-stu-id="82768-112">Get the Entity Framework Core runtime</span></span>

<span data-ttu-id="82768-113">要将 EF Core 运行时库添加到应用程序，可以为要使用的数据库提供程序安装 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="82768-113">To add EF Core runtime libraries to an application, install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="82768-114">要获取支持的提供程序及其 NuGet 包名称的列表，请参阅[数据库提供程序](../../providers/index.md)。</span><span class="sxs-lookup"><span data-stu-id="82768-114">For a list of supported providers and their NuGet package names, see [Database providers](../../providers/index.md).</span></span>

<span data-ttu-id="82768-115">要安装或更新 NuGet 包，可以使用 .NET Core CLI、Visual Studio 包管理器对话框或 Visual Studio 包管理器控制台。</span><span class="sxs-lookup"><span data-stu-id="82768-115">To install or update NuGet packages, use the .NET Core CLI, the Visual Studio Package Manager Dialog, or the Visual Studio Package Manager Console.</span></span>

<span data-ttu-id="82768-116">ASP.NET Core 2.1 应用程序自动随附内存中和 SQL Server 提供程序，因此无需单独安装。</span><span class="sxs-lookup"><span data-stu-id="82768-116">For ASP.NET Core 2.1 applications, the in-memory and SQL Server providers are automatically included, so there's no need to install them separately.</span></span>

> [!TIP]  
> <span data-ttu-id="82768-117">如需更新使用第三方数据库提供程序的应用程序，请始终检查与要使用的 EF Core 版本兼容的提供程序有无更新。</span><span class="sxs-lookup"><span data-stu-id="82768-117">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="82768-118">例如，旧版本使用的数据库提供程序与 2.1 版 EF Core 运行时不兼容。</span><span class="sxs-lookup"><span data-stu-id="82768-118">For example, database providers for previous versions are not compatible with version 2.1 of the EF Core runtime.</span></span>  

### <a name="net-core-cli"></a><span data-ttu-id="82768-119">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="82768-119">.NET Core CLI</span></span>

<span data-ttu-id="82768-120">以下 .NET Core CLI 命令安装或更新 SQL Server 提供程序：</span><span class="sxs-lookup"><span data-stu-id="82768-120">The following .NET Core CLI command installs or updates the SQL Server provider:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="82768-121">可以使用 `-v` 修饰符在 `dotnet add package` 命令中指明特定的版本。</span><span class="sxs-lookup"><span data-stu-id="82768-121">You can indicate a specific version in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="82768-122">例如，若要安装 EF Core 2.1.0 包，请将 `-v 2.1.0` 追加到命令中。</span><span class="sxs-lookup"><span data-stu-id="82768-122">For example, to install EF Core 2.1.0 packages, append `-v 2.1.0` to the command.</span></span>

### <a name="visual-studio-nuget-package-manager-dialog"></a><span data-ttu-id="82768-123">Visual Studio NuGet 包管理器对话框</span><span class="sxs-lookup"><span data-stu-id="82768-123">Visual Studio NuGet Package Manager Dialog</span></span>

* <span data-ttu-id="82768-124">在菜单上选择“项目”>“管理 NuGet 程序包”</span><span class="sxs-lookup"><span data-stu-id="82768-124">From the menu, select **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="82768-125">单击“浏览”或“更新”选项卡</span><span class="sxs-lookup"><span data-stu-id="82768-125">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="82768-126">若要安装或更新 SQL Server 提供程序，请选择 `Microsoft.EntityFrameworkCore.SqlServer` 包并确认。</span><span class="sxs-lookup"><span data-stu-id="82768-126">To install or update the SQL Server provider, select the `Microsoft.EntityFrameworkCore.SqlServer` package, and confirm.</span></span>

<span data-ttu-id="82768-127">有关详细信息，请参阅 [NuGet 包管理器对话框](https://docs.microsoft.com/nuget/tools/package-manager-ui)。</span><span class="sxs-lookup"><span data-stu-id="82768-127">For more information, see [NuGet Package Manager Dialog](https://docs.microsoft.com/nuget/tools/package-manager-ui).</span></span>

### <a name="visual-studio-nuget-package-manager-console"></a><span data-ttu-id="82768-128">Visual Studio NuGet 包管理器控制台</span><span class="sxs-lookup"><span data-stu-id="82768-128">Visual Studio NuGet Package Manager Console</span></span>

* <span data-ttu-id="82768-129">从菜单中，选择“工具”>“NuGet 包管理器”>“包管理器控制台”</span><span class="sxs-lookup"><span data-stu-id="82768-129">From the menu, select **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="82768-130">若要安装 SQL Server 提供程序，请在包管理器控制台中运行以下命令：</span><span class="sxs-lookup"><span data-stu-id="82768-130">To install the SQL Server provider, run the following command in the Package Manager Console:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="82768-131">若要更新提供程序，使用 `Update-Package` 命令。</span><span class="sxs-lookup"><span data-stu-id="82768-131">To update the provider, use the `Update-Package` command.</span></span>

* <span data-ttu-id="82768-132">若要指定特定版本，可以使用 `-Version` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="82768-132">To specify a specific version, use the `-Version` modifier.</span></span> <span data-ttu-id="82768-133">例如，若要安装 EF Core 2.1.0 包，请将 `-Version 2.1.0` 追加到命令中</span><span class="sxs-lookup"><span data-stu-id="82768-133">For example, to install EF Core 2.1.0 packages, append `-Version 2.1.0` to the commands</span></span>

<span data-ttu-id="82768-134">有关详细信息，请参阅[包管理器控制台](https://docs.microsoft.com/nuget/tools/package-manager-console)。</span><span class="sxs-lookup"><span data-stu-id="82768-134">For more information, see [Package Manager Console](https://docs.microsoft.com/nuget/tools/package-manager-console).</span></span>

## <a name="get-entity-framework-core-tools"></a><span data-ttu-id="82768-135">获取 Entity Framework Core 工具</span><span class="sxs-lookup"><span data-stu-id="82768-135">Get Entity Framework Core tools</span></span>

<span data-ttu-id="82768-136">除了运行时库，还可以安装设计时可在项目中执行一些与 EF Core 相关的任务的工具。</span><span class="sxs-lookup"><span data-stu-id="82768-136">Besides the runtime libraries, you can install tools that can perform some EF Core-related tasks in your project at design time.</span></span> <span data-ttu-id="82768-137">例如，可以创建迁移、应用迁移，并创建基于现有数据库的模型。</span><span class="sxs-lookup"><span data-stu-id="82768-137">For example, you can create migrations, apply migrations, and create a model based on an existing database.</span></span>

<span data-ttu-id="82768-138">提供了两个工具集：</span><span class="sxs-lookup"><span data-stu-id="82768-138">Two sets of tools are available:</span></span>
* <span data-ttu-id="82768-139">.NET Core [命令行接口 (CLI) 工具](../../miscellaneous/cli/dotnet.md)可用于 Windows、Linux 或 macOS。</span><span class="sxs-lookup"><span data-stu-id="82768-139">The .NET Core [command-line interface (CLI) tools](../../miscellaneous/cli/dotnet.md) can be used on Windows, Linux, or macOS.</span></span> <span data-ttu-id="82768-140">这些命令以 `dotnet ef` 开头。</span><span class="sxs-lookup"><span data-stu-id="82768-140">These commands begin with `dotnet ef`.</span></span> 
* <span data-ttu-id="82768-141">[包管理器控制台工具](../../miscellaneous/cli/powershell.md)在 Windows 上的 Visual Studio 2017 中运行。</span><span class="sxs-lookup"><span data-stu-id="82768-141">The [Package Manager Console tools](../../miscellaneous/cli/powershell.md)  run in Visual Studio 2017 on Windows.</span></span> <span data-ttu-id="82768-142">这些命令以动词开头，例如 `Add-Migration`、`Update-Database`。</span><span class="sxs-lookup"><span data-stu-id="82768-142">These commands start with a verb, for example `Add-Migration`, `Update-Database`.</span></span>

<span data-ttu-id="82768-143">虽然可在包管理器控制台中使用 `dotnet ef` 命令，但在使用 Visual Studio 时使用包管理器控制台工具更方便：</span><span class="sxs-lookup"><span data-stu-id="82768-143">Although you can use the `dotnet ef` commands from the Package Manager Console, it's more convenient to use the Package Manager Console tools when you're using Visual Studio:</span></span>
* <span data-ttu-id="82768-144">它们会自动使用包管理器控制台中选择的当前项目，无需手动切换目录。</span><span class="sxs-lookup"><span data-stu-id="82768-144">They automatically work with the current project selected in the Package Manager Console without requiring manually switching directories.</span></span>  
* <span data-ttu-id="82768-145">命令完成后，它们会自动在 Visual Studio 中打开命令所生成的文件。</span><span class="sxs-lookup"><span data-stu-id="82768-145">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

<a name="cli"></a>

### <a name="get-the-cli-tools"></a><span data-ttu-id="82768-146">获取 CLI 工具</span><span class="sxs-lookup"><span data-stu-id="82768-146">Get the CLI tools</span></span>

<span data-ttu-id="82768-147">`dotnet ef` 命令随附在 .NET Core SDK 中，但若要启用这些命令，必须安装 `Microsoft.EntityFrameworkCore.Design` 包：</span><span class="sxs-lookup"><span data-stu-id="82768-147">The `dotnet ef` commands are included in the .NET Core SDK, but to enable the commands you have to install the `Microsoft.EntityFrameworkCore.Design` package:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

<span data-ttu-id="82768-148">ASP.NET Core 2.1 应用自动随附此包。</span><span class="sxs-lookup"><span data-stu-id="82768-148">For ASP.NET Core 2.1 apps, this package is included automatically.</span></span>

<span data-ttu-id="82768-149">如前面[先决条件](#prerequisites)所述，还需要安装 .NET Core 2.1 SDK。</span><span class="sxs-lookup"><span data-stu-id="82768-149">As explained earlier in [Prerequisites](#prerequisites), you also need to install the .NET Core 2.1 SDK.</span></span>

> [!IMPORTANT]      
> <span data-ttu-id="82768-150">请务必使用与运行时包主版本匹配的工具包版本。</span><span class="sxs-lookup"><span data-stu-id="82768-150">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

### <a name="get-the-package-manager-console-tools"></a><span data-ttu-id="82768-151">获取包管理器控制台工具</span><span class="sxs-lookup"><span data-stu-id="82768-151">Get the Package Manager Console tools</span></span>

<span data-ttu-id="82768-152">若要获取适用于 EF Core 的包管理器控制台工具，请安装 `Microsoft.EntityFrameworkCore.Tools` 包：</span><span class="sxs-lookup"><span data-stu-id="82768-152">To get the Package Manager Console tools for EF Core, install the `Microsoft.EntityFrameworkCore.Tools` package:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Tools
``` 

<span data-ttu-id="82768-153">ASP.NET Core 2.1 应用自动随附此包。</span><span class="sxs-lookup"><span data-stu-id="82768-153">For ASP.NET Core 2.1 apps, this package is included automatically.</span></span>

## <a name="upgrading-to-ef-core-21"></a><span data-ttu-id="82768-154">升级到 EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="82768-154">Upgrading to EF Core 2.1</span></span>

<span data-ttu-id="82768-155">若要将现有应用程序升级到 EF Core 2.1，可能需要手动删除一些对旧版 EF Core 包的引用：</span><span class="sxs-lookup"><span data-stu-id="82768-155">If you're upgrading an existing application to EF Core 2.1, some references to older EF Core packages may need to be removed manually:</span></span>

* <span data-ttu-id="82768-156">EF Core 2.1 不再需要或支持 `Microsoft.EntityFrameworkCore.SqlServer.Design` 等数据库提供程序设计时包，但在升级其他包后，它们不会被自动删除。</span><span class="sxs-lookup"><span data-stu-id="82768-156">Database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported in EF Core 2.1, but aren't automatically removed when upgrading the other packages.</span></span>

* <span data-ttu-id="82768-157">.NET SDK 现包含 .NET CLI 工具，这样就可以从 .csproj 文件中删除对相应包的引用：</span><span class="sxs-lookup"><span data-stu-id="82768-157">The .NET CLI tools are now included in the .NET SDK, so the reference to that package can be removed from the *.csproj* file:</span></span>

  ```
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  ```

<span data-ttu-id="82768-158">对于面向 .NET Framework 且由 Visual Studio 早期版本创建的应用程序，请确保它们与 .NET Standard 2.0 库兼容：</span><span class="sxs-lookup"><span data-stu-id="82768-158">For applications that target the .NET Framework and were created by earlier versions of Visual Studio, make sure that they are compatible with .NET Standard 2.0 libraries:</span></span>

  * <span data-ttu-id="82768-159">编辑项目文件，并确保以下条目出现在初始属性组中：</span><span class="sxs-lookup"><span data-stu-id="82768-159">Edit the project file and make sure the following entry appears in the initial property group:</span></span>

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * <span data-ttu-id="82768-160">对于测试项目，还要确保存在以下条目：</span><span class="sxs-lookup"><span data-stu-id="82768-160">For test projects, also make sure the following entry is present:</span></span>

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
