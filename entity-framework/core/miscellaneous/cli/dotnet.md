---
title: .NET Core CLI-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 721235b07e695efd8df43294e1f4e90c28ae83d7
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2018
ms.locfileid: "34754491"
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="614a8-102">EF Core.NET 命令行工具</span><span class="sxs-lookup"><span data-stu-id="614a8-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="614a8-103">实体框架核心.NET 命令行工具是一种扩展到跨平台**dotnet**命令，是一部分的[.NET Core SDK][2]。</span><span class="sxs-lookup"><span data-stu-id="614a8-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="614a8-104">如果你使用 Visual Studio，我们建议[PMC 工具][ 1]相反因为它们提供了更多集成的体验。</span><span class="sxs-lookup"><span data-stu-id="614a8-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="614a8-105">安装工具</span><span class="sxs-lookup"><span data-stu-id="614a8-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="614a8-106">.NET Core SDK 版本 2.1.300 和更高版本包括**dotnet ef**与 EF Core 2.0 和更高版本兼容的命令。</span><span class="sxs-lookup"><span data-stu-id="614a8-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="614a8-107">因此如果你使用的最新版本的.NET Core SDK 和 EF Core运行时，需要进行任何安装，你可以忽略本部分的其余部分。</span><span class="sxs-lookup"><span data-stu-id="614a8-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="614a8-108">另一方面， **dotnet ef**工具包含在.NET Core SDK 版本 2.1.300 和更高版本不兼容与 EF Core 版本 1.0 和 1.1。</span><span class="sxs-lookup"><span data-stu-id="614a8-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="614a8-109">你可以使用.NET Core sdk 2.1.300 的计算机使用 EF Core这些早期版本的项目或更高版本安装之前，你还必须安装版本 2.1.200 或更低版本的 sdk 和配置应用程序通过修改使用该旧版本其 [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json)文件。</span><span class="sxs-lookup"><span data-stu-id="614a8-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="614a8-110">此文件通常包含在解决方案目录 （一个上面项目）。</span><span class="sxs-lookup"><span data-stu-id="614a8-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="614a8-111">然后，您可以继续下面 installlation 指令。</span><span class="sxs-lookup"><span data-stu-id="614a8-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="614a8-112">对于.NET Core sdk 的早期版本，你可以安装使用这些步骤的 EF Core.NET 命令行工具：</span><span class="sxs-lookup"><span data-stu-id="614a8-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="614a8-113">编辑项目文件并将 Microsoft.EntityFrameworkCore.Tools.DotNet 添加为 DotNetCliToolReference 项 （见下文）</span><span class="sxs-lookup"><span data-stu-id="614a8-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="614a8-114">运行以下命令：</span><span class="sxs-lookup"><span data-stu-id="614a8-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="614a8-115">生成的项目应如下所示：</span><span class="sxs-lookup"><span data-stu-id="614a8-115">The resulting project should look something like this:</span></span>

``` xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.EntityFrameworkCore.Design"
                      Version="2.0.0"
                      PrivateAssets="All" />
  </ItemGroup>
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
                            Version="2.0.0" />
  </ItemGroup>
</Project>
```

> [!NOTE]
> <span data-ttu-id="614a8-116">一个具有的包引用`PrivateAssets="All"`意味着它不公开给引用此项目，这是非常适合通常仅在开发期间使用的包的项目。</span><span class="sxs-lookup"><span data-stu-id="614a8-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="614a8-117">如果您还没有完全正确，你应能够成功的命令提示中运行以下命令。</span><span class="sxs-lookup"><span data-stu-id="614a8-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="614a8-118">使用的工具</span><span class="sxs-lookup"><span data-stu-id="614a8-118">Using the tools</span></span>
---------------
<span data-ttu-id="614a8-119">每当调用某命令时，有两个项目涉及：</span><span class="sxs-lookup"><span data-stu-id="614a8-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="614a8-120">目标项目是在其中添加任何文件（或在某些情况下删除文件）的项目。</span><span class="sxs-lookup"><span data-stu-id="614a8-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="614a8-121">目标项目默认为当前目录中的项目，但可以使用更改<nobr> **-项目**</nobr>选项。</span><span class="sxs-lookup"><span data-stu-id="614a8-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="614a8-122">启动项目是执行项目代码时由工具模拟的项目。</span><span class="sxs-lookup"><span data-stu-id="614a8-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="614a8-123">它也默认为当前目录中的项目，但可以使用更改 **-启动项目**选项。</span><span class="sxs-lookup"><span data-stu-id="614a8-123">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="614a8-124">例如，更新的 web 应用程序具有不同的项目中安装的 EF Core数据库将如下所示： `dotnet ef database update --project {project-path}` （从您的 web 应用程序目录）</span><span class="sxs-lookup"><span data-stu-id="614a8-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="614a8-125">常用的选项：</span><span class="sxs-lookup"><span data-stu-id="614a8-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="614a8-126">--json</span><span class="sxs-lookup"><span data-stu-id="614a8-126">--json</span></span>                           | <span data-ttu-id="614a8-127">显示 JSON 输出。</span><span class="sxs-lookup"><span data-stu-id="614a8-127">Show JSON output.</span></span>           |
| <span data-ttu-id="614a8-128">-c</span><span class="sxs-lookup"><span data-stu-id="614a8-128">-c</span></span> | <span data-ttu-id="614a8-129">-上下文\<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="614a8-129">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="614a8-130">若要使用 DbContext。</span><span class="sxs-lookup"><span data-stu-id="614a8-130">The DbContext to use.</span></span>       |
| <span data-ttu-id="614a8-131">-p</span><span class="sxs-lookup"><span data-stu-id="614a8-131">-p</span></span> | <span data-ttu-id="614a8-132">-项目\<项目 ></span><span class="sxs-lookup"><span data-stu-id="614a8-132">--project \<PROJECT></span></span>             | <span data-ttu-id="614a8-133">要使用的项目。</span><span class="sxs-lookup"><span data-stu-id="614a8-133">The project to use.</span></span>         |
| <span data-ttu-id="614a8-134">-s</span><span class="sxs-lookup"><span data-stu-id="614a8-134">-s</span></span> | <span data-ttu-id="614a8-135">-启动项目\<项目 ></span><span class="sxs-lookup"><span data-stu-id="614a8-135">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="614a8-136">要使用的启动项目。</span><span class="sxs-lookup"><span data-stu-id="614a8-136">The startup project to use.</span></span> |
|    | <span data-ttu-id="614a8-137">-framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="614a8-137">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="614a8-138">目标框架中。</span><span class="sxs-lookup"><span data-stu-id="614a8-138">The target framework.</span></span>       |
|    | <span data-ttu-id="614a8-139">-配置\<配置 ></span><span class="sxs-lookup"><span data-stu-id="614a8-139">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="614a8-140">要使用的配置。</span><span class="sxs-lookup"><span data-stu-id="614a8-140">The configuration to use.</span></span>   |
|    | <span data-ttu-id="614a8-141">-运行时\<标识符 ></span><span class="sxs-lookup"><span data-stu-id="614a8-141">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="614a8-142">若要使用运行时。</span><span class="sxs-lookup"><span data-stu-id="614a8-142">The runtime to use.</span></span>         |
| <span data-ttu-id="614a8-143">-h</span><span class="sxs-lookup"><span data-stu-id="614a8-143">-h</span></span> | <span data-ttu-id="614a8-144">-帮助</span><span class="sxs-lookup"><span data-stu-id="614a8-144">--help</span></span>                           | <span data-ttu-id="614a8-145">显示帮助信息。</span><span class="sxs-lookup"><span data-stu-id="614a8-145">Show help information.</span></span>      |
| <span data-ttu-id="614a8-146">-v</span><span class="sxs-lookup"><span data-stu-id="614a8-146">-v</span></span> | <span data-ttu-id="614a8-147">-verbose</span><span class="sxs-lookup"><span data-stu-id="614a8-147">--verbose</span></span>                        | <span data-ttu-id="614a8-148">显示详细输出。</span><span class="sxs-lookup"><span data-stu-id="614a8-148">Show verbose output.</span></span>        |
|    | <span data-ttu-id="614a8-149">--no-color</span><span class="sxs-lookup"><span data-stu-id="614a8-149">--no-color</span></span>                       | <span data-ttu-id="614a8-150">不为着色输出。</span><span class="sxs-lookup"><span data-stu-id="614a8-150">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="614a8-151">--prefix-output</span><span class="sxs-lookup"><span data-stu-id="614a8-151">--prefix-output</span></span>                  | <span data-ttu-id="614a8-152">输出与级别的前缀。</span><span class="sxs-lookup"><span data-stu-id="614a8-152">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="614a8-153">若要指定 ASP.NET Core 环境，设置**ASPNETCORE_ENVIRONMENT**之前运行的环境变量。</span><span class="sxs-lookup"><span data-stu-id="614a8-153">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="614a8-154">命令</span><span class="sxs-lookup"><span data-stu-id="614a8-154">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="614a8-155">dotnet ef 数据库除去</span><span class="sxs-lookup"><span data-stu-id="614a8-155">dotnet ef database drop</span></span>

<span data-ttu-id="614a8-156">删除数据库。</span><span class="sxs-lookup"><span data-stu-id="614a8-156">Drops the database.</span></span>

<span data-ttu-id="614a8-157">选项:</span><span class="sxs-lookup"><span data-stu-id="614a8-157">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="614a8-158">-f</span><span class="sxs-lookup"><span data-stu-id="614a8-158">-f</span></span> | <span data-ttu-id="614a8-159">--force</span><span class="sxs-lookup"><span data-stu-id="614a8-159">--force</span></span>   | <span data-ttu-id="614a8-160">不确认。</span><span class="sxs-lookup"><span data-stu-id="614a8-160">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="614a8-161">-试运行</span><span class="sxs-lookup"><span data-stu-id="614a8-161">--dry-run</span></span> | <span data-ttu-id="614a8-162">显示的数据库会被丢弃，但没有删除它。</span><span class="sxs-lookup"><span data-stu-id="614a8-162">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="614a8-163">dotnet ef 数据库更新</span><span class="sxs-lookup"><span data-stu-id="614a8-163">dotnet ef database update</span></span>

<span data-ttu-id="614a8-164">到指定的迁移更新数据库。</span><span class="sxs-lookup"><span data-stu-id="614a8-164">Updates the database to a specified migration.</span></span>

<span data-ttu-id="614a8-165">自变量：</span><span class="sxs-lookup"><span data-stu-id="614a8-165">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="614a8-166">\<迁移 &GT;</span><span class="sxs-lookup"><span data-stu-id="614a8-166">\<MIGRATION></span></span> | <span data-ttu-id="614a8-167">目标迁移。</span><span class="sxs-lookup"><span data-stu-id="614a8-167">The target migration.</span></span> <span data-ttu-id="614a8-168">如果为 0，将恢复所有迁移。</span><span class="sxs-lookup"><span data-stu-id="614a8-168">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="614a8-169">默认到最后一个迁移。</span><span class="sxs-lookup"><span data-stu-id="614a8-169">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="614a8-170">dotnet ef dbcontext 信息</span><span class="sxs-lookup"><span data-stu-id="614a8-170">dotnet ef dbcontext info</span></span>

<span data-ttu-id="614a8-171">获取有关 DbContext 类型的信息。</span><span class="sxs-lookup"><span data-stu-id="614a8-171">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="614a8-172">dotnet ef dbcontext 列表</span><span class="sxs-lookup"><span data-stu-id="614a8-172">dotnet ef dbcontext list</span></span>

<span data-ttu-id="614a8-173">列出可用的 DbContext 类型。</span><span class="sxs-lookup"><span data-stu-id="614a8-173">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="614a8-174">dotnet ef dbcontext 基架</span><span class="sxs-lookup"><span data-stu-id="614a8-174">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="614a8-175">基架数据库类型 DbContext 和实体的类型。</span><span class="sxs-lookup"><span data-stu-id="614a8-175">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="614a8-176">自变量：</span><span class="sxs-lookup"><span data-stu-id="614a8-176">Arguments:</span></span>

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| <span data-ttu-id="614a8-177">\<连接 &GT;</span><span class="sxs-lookup"><span data-stu-id="614a8-177">\<CONNECTION></span></span> | <span data-ttu-id="614a8-178">数据库的连接字符串。</span><span class="sxs-lookup"><span data-stu-id="614a8-178">The connection string to the database.</span></span>                                      |
| <span data-ttu-id="614a8-179">\<提供程序 &GT;</span><span class="sxs-lookup"><span data-stu-id="614a8-179">\<PROVIDER></span></span>   | <span data-ttu-id="614a8-180">要使用的提供程序。</span><span class="sxs-lookup"><span data-stu-id="614a8-180">The provider to use.</span></span> <span data-ttu-id="614a8-181">（例如 Microsoft.EntityFrameworkCore.SqlServer）</span><span class="sxs-lookup"><span data-stu-id="614a8-181">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="614a8-182">选项:</span><span class="sxs-lookup"><span data-stu-id="614a8-182">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="614a8-183"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="614a8-183"><nobr>-d</nobr></span></span> | <span data-ttu-id="614a8-184">-数据批注</span><span class="sxs-lookup"><span data-stu-id="614a8-184">--data-annotations</span></span>                      | <span data-ttu-id="614a8-185">使用属性来配置该模型 （如果可能）。</span><span class="sxs-lookup"><span data-stu-id="614a8-185">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="614a8-186">如果省略，则使用仅 fluent API。</span><span class="sxs-lookup"><span data-stu-id="614a8-186">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="614a8-187">-c</span><span class="sxs-lookup"><span data-stu-id="614a8-187">-c</span></span>              | <span data-ttu-id="614a8-188">-上下文\<名称 ></span><span class="sxs-lookup"><span data-stu-id="614a8-188">--context \<NAME></span></span>                       | <span data-ttu-id="614a8-189">Dbcontext 名称。</span><span class="sxs-lookup"><span data-stu-id="614a8-189">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="614a8-190">-上下文 dir\<路径 ></span><span class="sxs-lookup"><span data-stu-id="614a8-190">--context-dir \<PATH></span></span>                   | <span data-ttu-id="614a8-191">要将 DbContext 文件放入的目录。</span><span class="sxs-lookup"><span data-stu-id="614a8-191">The directory to put DbContext file in.</span></span> <span data-ttu-id="614a8-192">路径是相对于项目目录。</span><span class="sxs-lookup"><span data-stu-id="614a8-192">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="614a8-193">-f</span><span class="sxs-lookup"><span data-stu-id="614a8-193">-f</span></span>              | <span data-ttu-id="614a8-194">--force</span><span class="sxs-lookup"><span data-stu-id="614a8-194">--force</span></span>                                 | <span data-ttu-id="614a8-195">覆盖现有文件。</span><span class="sxs-lookup"><span data-stu-id="614a8-195">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="614a8-196">-o</span><span class="sxs-lookup"><span data-stu-id="614a8-196">-o</span></span>              | <span data-ttu-id="614a8-197">-输出 dir\<路径 ></span><span class="sxs-lookup"><span data-stu-id="614a8-197">--output-dir \<PATH></span></span>                    | <span data-ttu-id="614a8-198">要将文件放入的目录。</span><span class="sxs-lookup"><span data-stu-id="614a8-198">The directory to put files in.</span></span> <span data-ttu-id="614a8-199">路径是相对于项目目录。</span><span class="sxs-lookup"><span data-stu-id="614a8-199">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="614a8-200"><nobr>-架构\<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="614a8-200"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="614a8-201">要生成实体类型的表架构。</span><span class="sxs-lookup"><span data-stu-id="614a8-201">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="614a8-202">-t</span><span class="sxs-lookup"><span data-stu-id="614a8-202">-t</span></span>              | <span data-ttu-id="614a8-203">-表\<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="614a8-203">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="614a8-204">要生成实体类型的表。</span><span class="sxs-lookup"><span data-stu-id="614a8-204">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="614a8-205">-使用数据库名称</span><span class="sxs-lookup"><span data-stu-id="614a8-205">--use-database-names</span></span>                    | <span data-ttu-id="614a8-206">使用直接从数据库表和列名称。</span><span class="sxs-lookup"><span data-stu-id="614a8-206">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="614a8-207">dotnet ef 迁移添加</span><span class="sxs-lookup"><span data-stu-id="614a8-207">dotnet ef migrations add</span></span>

<span data-ttu-id="614a8-208">将添加一个新迁移。</span><span class="sxs-lookup"><span data-stu-id="614a8-208">Adds a new migration.</span></span>

<span data-ttu-id="614a8-209">自变量：</span><span class="sxs-lookup"><span data-stu-id="614a8-209">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="614a8-210">\<名称 &GT;</span><span class="sxs-lookup"><span data-stu-id="614a8-210">\<NAME></span></span> | <span data-ttu-id="614a8-211">迁移的名称。</span><span class="sxs-lookup"><span data-stu-id="614a8-211">The name of the migration.</span></span> |

<span data-ttu-id="614a8-212">选项:</span><span class="sxs-lookup"><span data-stu-id="614a8-212">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="614a8-213"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="614a8-213"><nobr>-o</nobr></span></span> | <span data-ttu-id="614a8-214"><nobr>-输出 dir\<路径 ></nobr></span><span class="sxs-lookup"><span data-stu-id="614a8-214"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="614a8-215">目录 （及其子命名空间） 使用。</span><span class="sxs-lookup"><span data-stu-id="614a8-215">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="614a8-216">路径是相对于项目目录。</span><span class="sxs-lookup"><span data-stu-id="614a8-216">Paths are relative to the project directory.</span></span> <span data-ttu-id="614a8-217">默认值为"迁移"。</span><span class="sxs-lookup"><span data-stu-id="614a8-217">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="614a8-218">dotnet ef 迁移列表</span><span class="sxs-lookup"><span data-stu-id="614a8-218">dotnet ef migrations list</span></span>

<span data-ttu-id="614a8-219">列出可用的迁移。</span><span class="sxs-lookup"><span data-stu-id="614a8-219">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="614a8-220">dotnet ef 迁移删除</span><span class="sxs-lookup"><span data-stu-id="614a8-220">dotnet ef migrations remove</span></span>

<span data-ttu-id="614a8-221">删除上次的迁移。</span><span class="sxs-lookup"><span data-stu-id="614a8-221">Removes the last migration.</span></span>

<span data-ttu-id="614a8-222">选项:</span><span class="sxs-lookup"><span data-stu-id="614a8-222">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="614a8-223">-f</span><span class="sxs-lookup"><span data-stu-id="614a8-223">-f</span></span> | <span data-ttu-id="614a8-224">--force</span><span class="sxs-lookup"><span data-stu-id="614a8-224">--force</span></span> | <span data-ttu-id="614a8-225">如果它已应用到数据库，请还原迁移。</span><span class="sxs-lookup"><span data-stu-id="614a8-225">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="614a8-226">dotnet ef 迁移脚本</span><span class="sxs-lookup"><span data-stu-id="614a8-226">dotnet ef migrations script</span></span>

<span data-ttu-id="614a8-227">从迁移中生成的 SQL 脚本。</span><span class="sxs-lookup"><span data-stu-id="614a8-227">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="614a8-228">自变量：</span><span class="sxs-lookup"><span data-stu-id="614a8-228">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="614a8-229">\<FROM></span><span class="sxs-lookup"><span data-stu-id="614a8-229">\<FROM></span></span> | <span data-ttu-id="614a8-230">开始迁移。</span><span class="sxs-lookup"><span data-stu-id="614a8-230">The starting migration.</span></span> <span data-ttu-id="614a8-231">默认值为 0 （初始数据库）。</span><span class="sxs-lookup"><span data-stu-id="614a8-231">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="614a8-232">\<到 &GT;</span><span class="sxs-lookup"><span data-stu-id="614a8-232">\<TO></span></span>   | <span data-ttu-id="614a8-233">结束的迁移。</span><span class="sxs-lookup"><span data-stu-id="614a8-233">The ending migration.</span></span> <span data-ttu-id="614a8-234">默认到最后一个迁移。</span><span class="sxs-lookup"><span data-stu-id="614a8-234">Defaults to the last migration.</span></span>         |

<span data-ttu-id="614a8-235">选项:</span><span class="sxs-lookup"><span data-stu-id="614a8-235">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="614a8-236">-o</span><span class="sxs-lookup"><span data-stu-id="614a8-236">-o</span></span> | <span data-ttu-id="614a8-237">-输出\<文件 ></span><span class="sxs-lookup"><span data-stu-id="614a8-237">--output \<FILE></span></span> | <span data-ttu-id="614a8-238">要将结果写入的文件。</span><span class="sxs-lookup"><span data-stu-id="614a8-238">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="614a8-239">-i</span><span class="sxs-lookup"><span data-stu-id="614a8-239">-i</span></span> | <span data-ttu-id="614a8-240">-幂等</span><span class="sxs-lookup"><span data-stu-id="614a8-240">--idempotent</span></span>     | <span data-ttu-id="614a8-241">生成可以在任何迁移的数据库使用的脚本。</span><span class="sxs-lookup"><span data-stu-id="614a8-241">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
