---
title: .NET Core CLI-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 81427b010f63bdd591ffb9117c1556722313c3fa
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995292"
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="fe996-102">EF Core.NET 命令行工具</span><span class="sxs-lookup"><span data-stu-id="fe996-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="fe996-103">实体框架核心.NET 命令行工具是一种扩展到跨平台**dotnet**命令，是一部分的[.NET Core SDK][2]。</span><span class="sxs-lookup"><span data-stu-id="fe996-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="fe996-104">如果使用 Visual Studio，我们建议[PMC 工具][ 1]而是因为它们提供更完整的体验。</span><span class="sxs-lookup"><span data-stu-id="fe996-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="fe996-105">安装工具</span><span class="sxs-lookup"><span data-stu-id="fe996-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="fe996-106">.NET Core SDK 版本 2.1.300 和更高版本包括**dotnet ef**与 EF Core 2.0 和更高版本兼容的命令。</span><span class="sxs-lookup"><span data-stu-id="fe996-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="fe996-107">因此如果你使用的最新版本的.NET Core SDK 和 EF Core 运行时，需要进行任何安装，你可以忽略本部分的其余部分。</span><span class="sxs-lookup"><span data-stu-id="fe996-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="fe996-108">另一方面， **dotnet ef**工具包含在.NET Core SDK 版本 2.1.300 和更高版本不兼容与 EF Core 版本 1.0 和 1.1。</span><span class="sxs-lookup"><span data-stu-id="fe996-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="fe996-109">你可以使用.NET Core sdk 2.1.300 的计算机使用 EF Core 这些早期版本的项目或更高版本安装之前，你还必须安装版本 2.1.200 或更低版本的 sdk 和配置应用程序通过修改使用该旧版本其 [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json)文件。</span><span class="sxs-lookup"><span data-stu-id="fe996-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="fe996-110">此文件通常包含在解决方案目录 （一个项目上方）。</span><span class="sxs-lookup"><span data-stu-id="fe996-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="fe996-111">然后，您可以继续下面 installlation 指令。</span><span class="sxs-lookup"><span data-stu-id="fe996-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="fe996-112">对于.NET Core sdk 的早期版本，你可以安装使用这些步骤的 EF Core.NET 命令行工具：</span><span class="sxs-lookup"><span data-stu-id="fe996-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="fe996-113">编辑项目文件并添加 Microsoft.EntityFrameworkCore.Tools.DotNet 作为 DotNetCliToolReference 项 （见下文）</span><span class="sxs-lookup"><span data-stu-id="fe996-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="fe996-114">运行以下命令：</span><span class="sxs-lookup"><span data-stu-id="fe996-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="fe996-115">生成的项目应如下所示：</span><span class="sxs-lookup"><span data-stu-id="fe996-115">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="fe996-116">使用的包引用`PrivateAssets="All"`意味着它不公开到引用此项目，特别适合用于在开发过程中通常仅使用的包的项目。</span><span class="sxs-lookup"><span data-stu-id="fe996-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="fe996-117">如果您还没有完全正确，您应能够成功在命令提示符处运行以下命令。</span><span class="sxs-lookup"><span data-stu-id="fe996-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="fe996-118">使用的工具</span><span class="sxs-lookup"><span data-stu-id="fe996-118">Using the tools</span></span>
---------------
<span data-ttu-id="fe996-119">每当调用命令时，有两个项目所涉及：</span><span class="sxs-lookup"><span data-stu-id="fe996-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="fe996-120">目标项目是在其中添加任何文件（或在某些情况下删除文件）的项目。</span><span class="sxs-lookup"><span data-stu-id="fe996-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="fe996-121">目标项目默认为当前目录中的项目，但可以使用更改<nobr> **-项目**</nobr>选项。</span><span class="sxs-lookup"><span data-stu-id="fe996-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="fe996-122">启动项目是执行项目代码时由工具模拟的项目。</span><span class="sxs-lookup"><span data-stu-id="fe996-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="fe996-123">它还将默认为当前目录中的项目，但可以使用更改 **-启动项目**选项。</span><span class="sxs-lookup"><span data-stu-id="fe996-123">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="fe996-124">例如，更新的 web 应用程序具有不同的项目中安装的 EF Core 数据库将如下所示： `dotnet ef database update --project {project-path}` （从您的 web 应用程序目录）</span><span class="sxs-lookup"><span data-stu-id="fe996-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="fe996-125">常用选项：</span><span class="sxs-lookup"><span data-stu-id="fe996-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="fe996-126">--json</span><span class="sxs-lookup"><span data-stu-id="fe996-126">--json</span></span>                           | <span data-ttu-id="fe996-127">显示 JSON 输出。</span><span class="sxs-lookup"><span data-stu-id="fe996-127">Show JSON output.</span></span>           |
| <span data-ttu-id="fe996-128">-c</span><span class="sxs-lookup"><span data-stu-id="fe996-128">-c</span></span> | <span data-ttu-id="fe996-129">-上下文\<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="fe996-129">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="fe996-130">若要使用 DbContext。</span><span class="sxs-lookup"><span data-stu-id="fe996-130">The DbContext to use.</span></span>       |
| <span data-ttu-id="fe996-131">-p</span><span class="sxs-lookup"><span data-stu-id="fe996-131">-p</span></span> | <span data-ttu-id="fe996-132">-项目\<项目 ></span><span class="sxs-lookup"><span data-stu-id="fe996-132">--project \<PROJECT></span></span>             | <span data-ttu-id="fe996-133">要使用的项目。</span><span class="sxs-lookup"><span data-stu-id="fe996-133">The project to use.</span></span>         |
| <span data-ttu-id="fe996-134">-s</span><span class="sxs-lookup"><span data-stu-id="fe996-134">-s</span></span> | <span data-ttu-id="fe996-135">-启动项目\<项目 ></span><span class="sxs-lookup"><span data-stu-id="fe996-135">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="fe996-136">若要使用的启动项目。</span><span class="sxs-lookup"><span data-stu-id="fe996-136">The startup project to use.</span></span> |
|    | <span data-ttu-id="fe996-137">-framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="fe996-137">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="fe996-138">目标框架。</span><span class="sxs-lookup"><span data-stu-id="fe996-138">The target framework.</span></span>       |
|    | <span data-ttu-id="fe996-139">-配置\<配置 ></span><span class="sxs-lookup"><span data-stu-id="fe996-139">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="fe996-140">要使用的配置。</span><span class="sxs-lookup"><span data-stu-id="fe996-140">The configuration to use.</span></span>   |
|    | <span data-ttu-id="fe996-141">-运行时\<标识符 ></span><span class="sxs-lookup"><span data-stu-id="fe996-141">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="fe996-142">运行时使用。</span><span class="sxs-lookup"><span data-stu-id="fe996-142">The runtime to use.</span></span>         |
| <span data-ttu-id="fe996-143">-h</span><span class="sxs-lookup"><span data-stu-id="fe996-143">-h</span></span> | <span data-ttu-id="fe996-144">-帮助</span><span class="sxs-lookup"><span data-stu-id="fe996-144">--help</span></span>                           | <span data-ttu-id="fe996-145">显示帮助信息。</span><span class="sxs-lookup"><span data-stu-id="fe996-145">Show help information.</span></span>      |
| <span data-ttu-id="fe996-146">-v</span><span class="sxs-lookup"><span data-stu-id="fe996-146">-v</span></span> | <span data-ttu-id="fe996-147">-verbose</span><span class="sxs-lookup"><span data-stu-id="fe996-147">--verbose</span></span>                        | <span data-ttu-id="fe996-148">显示详细输出。</span><span class="sxs-lookup"><span data-stu-id="fe996-148">Show verbose output.</span></span>        |
|    | <span data-ttu-id="fe996-149">--no-color</span><span class="sxs-lookup"><span data-stu-id="fe996-149">--no-color</span></span>                       | <span data-ttu-id="fe996-150">不为着色输出。</span><span class="sxs-lookup"><span data-stu-id="fe996-150">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="fe996-151">--prefix-output</span><span class="sxs-lookup"><span data-stu-id="fe996-151">--prefix-output</span></span>                  | <span data-ttu-id="fe996-152">输出级别使用的前缀。</span><span class="sxs-lookup"><span data-stu-id="fe996-152">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="fe996-153">若要指定 ASP.NET Core 环境，设置**ASPNETCORE_ENVIRONMENT**之前运行的环境变量。</span><span class="sxs-lookup"><span data-stu-id="fe996-153">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="fe996-154">命令</span><span class="sxs-lookup"><span data-stu-id="fe996-154">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="fe996-155">dotnet ef 数据库拖放</span><span class="sxs-lookup"><span data-stu-id="fe996-155">dotnet ef database drop</span></span>

<span data-ttu-id="fe996-156">删除数据库。</span><span class="sxs-lookup"><span data-stu-id="fe996-156">Drops the database.</span></span>

<span data-ttu-id="fe996-157">选项:</span><span class="sxs-lookup"><span data-stu-id="fe996-157">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="fe996-158">-f</span><span class="sxs-lookup"><span data-stu-id="fe996-158">-f</span></span> | <span data-ttu-id="fe996-159">--force</span><span class="sxs-lookup"><span data-stu-id="fe996-159">--force</span></span>   | <span data-ttu-id="fe996-160">不确认。</span><span class="sxs-lookup"><span data-stu-id="fe996-160">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="fe996-161">--dry-run</span><span class="sxs-lookup"><span data-stu-id="fe996-161">--dry-run</span></span> | <span data-ttu-id="fe996-162">显示哪个数据库会被丢弃，但没有删除它。</span><span class="sxs-lookup"><span data-stu-id="fe996-162">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="fe996-163">dotnet ef 数据库更新</span><span class="sxs-lookup"><span data-stu-id="fe996-163">dotnet ef database update</span></span>

<span data-ttu-id="fe996-164">指定迁移到更新数据库。</span><span class="sxs-lookup"><span data-stu-id="fe996-164">Updates the database to a specified migration.</span></span>

<span data-ttu-id="fe996-165">参数：</span><span class="sxs-lookup"><span data-stu-id="fe996-165">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="fe996-166">\<迁移 &GT;</span><span class="sxs-lookup"><span data-stu-id="fe996-166">\<MIGRATION></span></span> | <span data-ttu-id="fe996-167">目标迁移。</span><span class="sxs-lookup"><span data-stu-id="fe996-167">The target migration.</span></span> <span data-ttu-id="fe996-168">如果为 0，将还原所有迁移。</span><span class="sxs-lookup"><span data-stu-id="fe996-168">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="fe996-169">默认的最后一个迁移。</span><span class="sxs-lookup"><span data-stu-id="fe996-169">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="fe996-170">dotnet ef dbcontext 信息</span><span class="sxs-lookup"><span data-stu-id="fe996-170">dotnet ef dbcontext info</span></span>

<span data-ttu-id="fe996-171">获取有关 DbContext 类型的信息。</span><span class="sxs-lookup"><span data-stu-id="fe996-171">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="fe996-172">dotnet ef dbcontext 列表</span><span class="sxs-lookup"><span data-stu-id="fe996-172">dotnet ef dbcontext list</span></span>

<span data-ttu-id="fe996-173">列出了可用的 DbContext 类型。</span><span class="sxs-lookup"><span data-stu-id="fe996-173">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="fe996-174">dotnet ef dbcontext 基架</span><span class="sxs-lookup"><span data-stu-id="fe996-174">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="fe996-175">搭建基架以数据库的 DbContext 和实体类型。</span><span class="sxs-lookup"><span data-stu-id="fe996-175">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="fe996-176">参数：</span><span class="sxs-lookup"><span data-stu-id="fe996-176">Arguments:</span></span>

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| <span data-ttu-id="fe996-177">\<连接 &GT;</span><span class="sxs-lookup"><span data-stu-id="fe996-177">\<CONNECTION></span></span> | <span data-ttu-id="fe996-178">数据库的连接字符串。</span><span class="sxs-lookup"><span data-stu-id="fe996-178">The connection string to the database.</span></span>                                      |
| <span data-ttu-id="fe996-179">\<提供程序 &GT;</span><span class="sxs-lookup"><span data-stu-id="fe996-179">\<PROVIDER></span></span>   | <span data-ttu-id="fe996-180">要使用的提供程序。</span><span class="sxs-lookup"><span data-stu-id="fe996-180">The provider to use.</span></span> <span data-ttu-id="fe996-181">（例如 Microsoft.EntityFrameworkCore.SqlServer）</span><span class="sxs-lookup"><span data-stu-id="fe996-181">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="fe996-182">选项:</span><span class="sxs-lookup"><span data-stu-id="fe996-182">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fe996-183"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="fe996-183"><nobr>-d</nobr></span></span> | <span data-ttu-id="fe996-184">-数据注释</span><span class="sxs-lookup"><span data-stu-id="fe996-184">--data-annotations</span></span>                      | <span data-ttu-id="fe996-185">特性用于将模型配置 （如果可能）。</span><span class="sxs-lookup"><span data-stu-id="fe996-185">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="fe996-186">如果省略，则使用仅 fluent API。</span><span class="sxs-lookup"><span data-stu-id="fe996-186">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="fe996-187">-c</span><span class="sxs-lookup"><span data-stu-id="fe996-187">-c</span></span>              | <span data-ttu-id="fe996-188">-上下文\<名称 ></span><span class="sxs-lookup"><span data-stu-id="fe996-188">--context \<NAME></span></span>                       | <span data-ttu-id="fe996-189">DbContext 的名称。</span><span class="sxs-lookup"><span data-stu-id="fe996-189">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="fe996-190">-上下文 dir\<路径 ></span><span class="sxs-lookup"><span data-stu-id="fe996-190">--context-dir \<PATH></span></span>                   | <span data-ttu-id="fe996-191">要将 DbContext 文件放入的目录。</span><span class="sxs-lookup"><span data-stu-id="fe996-191">The directory to put DbContext file in.</span></span> <span data-ttu-id="fe996-192">路径是相对于项目目录。</span><span class="sxs-lookup"><span data-stu-id="fe996-192">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="fe996-193">-f</span><span class="sxs-lookup"><span data-stu-id="fe996-193">-f</span></span>              | <span data-ttu-id="fe996-194">--force</span><span class="sxs-lookup"><span data-stu-id="fe996-194">--force</span></span>                                 | <span data-ttu-id="fe996-195">覆盖现有文件。</span><span class="sxs-lookup"><span data-stu-id="fe996-195">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="fe996-196">-o</span><span class="sxs-lookup"><span data-stu-id="fe996-196">-o</span></span>              | <span data-ttu-id="fe996-197">-输出-dir\<路径 ></span><span class="sxs-lookup"><span data-stu-id="fe996-197">--output-dir \<PATH></span></span>                    | <span data-ttu-id="fe996-198">要将文件放入的目录。</span><span class="sxs-lookup"><span data-stu-id="fe996-198">The directory to put files in.</span></span> <span data-ttu-id="fe996-199">路径是相对于项目目录。</span><span class="sxs-lookup"><span data-stu-id="fe996-199">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="fe996-200"><nobr>-架构\<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="fe996-200"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="fe996-201">要生成的实体类型的表架构。</span><span class="sxs-lookup"><span data-stu-id="fe996-201">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="fe996-202">-t</span><span class="sxs-lookup"><span data-stu-id="fe996-202">-t</span></span>              | <span data-ttu-id="fe996-203">-表\<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="fe996-203">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="fe996-204">要生成的实体类型的表。</span><span class="sxs-lookup"><span data-stu-id="fe996-204">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="fe996-205">-使用数据库名称</span><span class="sxs-lookup"><span data-stu-id="fe996-205">--use-database-names</span></span>                    | <span data-ttu-id="fe996-206">使用直接从数据库表和列名称。</span><span class="sxs-lookup"><span data-stu-id="fe996-206">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="fe996-207">添加 dotnet ef 迁移</span><span class="sxs-lookup"><span data-stu-id="fe996-207">dotnet ef migrations add</span></span>

<span data-ttu-id="fe996-208">添加新的迁移。</span><span class="sxs-lookup"><span data-stu-id="fe996-208">Adds a new migration.</span></span>

<span data-ttu-id="fe996-209">参数：</span><span class="sxs-lookup"><span data-stu-id="fe996-209">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="fe996-210">\<名称 &GT;</span><span class="sxs-lookup"><span data-stu-id="fe996-210">\<NAME></span></span> | <span data-ttu-id="fe996-211">迁移的名称。</span><span class="sxs-lookup"><span data-stu-id="fe996-211">The name of the migration.</span></span> |

<span data-ttu-id="fe996-212">选项:</span><span class="sxs-lookup"><span data-stu-id="fe996-212">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fe996-213"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="fe996-213"><nobr>-o</nobr></span></span> | <span data-ttu-id="fe996-214"><nobr>-输出-dir\<路径 ></nobr></span><span class="sxs-lookup"><span data-stu-id="fe996-214"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="fe996-215">目录 （及其子命名空间） 来使用。</span><span class="sxs-lookup"><span data-stu-id="fe996-215">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="fe996-216">路径是相对于项目目录。</span><span class="sxs-lookup"><span data-stu-id="fe996-216">Paths are relative to the project directory.</span></span> <span data-ttu-id="fe996-217">默认值为"迁移"。</span><span class="sxs-lookup"><span data-stu-id="fe996-217">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="fe996-218">dotnet ef 迁移列表</span><span class="sxs-lookup"><span data-stu-id="fe996-218">dotnet ef migrations list</span></span>

<span data-ttu-id="fe996-219">列出了可用的迁移。</span><span class="sxs-lookup"><span data-stu-id="fe996-219">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="fe996-220">dotnet ef 迁移删除</span><span class="sxs-lookup"><span data-stu-id="fe996-220">dotnet ef migrations remove</span></span>

<span data-ttu-id="fe996-221">删除最后一个迁移。</span><span class="sxs-lookup"><span data-stu-id="fe996-221">Removes the last migration.</span></span>

<span data-ttu-id="fe996-222">选项:</span><span class="sxs-lookup"><span data-stu-id="fe996-222">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="fe996-223">-f</span><span class="sxs-lookup"><span data-stu-id="fe996-223">-f</span></span> | <span data-ttu-id="fe996-224">--force</span><span class="sxs-lookup"><span data-stu-id="fe996-224">--force</span></span> | <span data-ttu-id="fe996-225">如果它已应用到数据库，请还原迁移。</span><span class="sxs-lookup"><span data-stu-id="fe996-225">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="fe996-226">dotnet ef 迁移脚本</span><span class="sxs-lookup"><span data-stu-id="fe996-226">dotnet ef migrations script</span></span>

<span data-ttu-id="fe996-227">从迁移中生成的 SQL 脚本。</span><span class="sxs-lookup"><span data-stu-id="fe996-227">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="fe996-228">参数：</span><span class="sxs-lookup"><span data-stu-id="fe996-228">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="fe996-229">\<FROM></span><span class="sxs-lookup"><span data-stu-id="fe996-229">\<FROM></span></span> | <span data-ttu-id="fe996-230">开始迁移。</span><span class="sxs-lookup"><span data-stu-id="fe996-230">The starting migration.</span></span> <span data-ttu-id="fe996-231">默认值为 0 （初始数据库）。</span><span class="sxs-lookup"><span data-stu-id="fe996-231">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="fe996-232">\<到 &GT;</span><span class="sxs-lookup"><span data-stu-id="fe996-232">\<TO></span></span>   | <span data-ttu-id="fe996-233">结束的迁移。</span><span class="sxs-lookup"><span data-stu-id="fe996-233">The ending migration.</span></span> <span data-ttu-id="fe996-234">默认的最后一个迁移。</span><span class="sxs-lookup"><span data-stu-id="fe996-234">Defaults to the last migration.</span></span>         |

<span data-ttu-id="fe996-235">选项:</span><span class="sxs-lookup"><span data-stu-id="fe996-235">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="fe996-236">-o</span><span class="sxs-lookup"><span data-stu-id="fe996-236">-o</span></span> | <span data-ttu-id="fe996-237">-输出\<文件 ></span><span class="sxs-lookup"><span data-stu-id="fe996-237">--output \<FILE></span></span> | <span data-ttu-id="fe996-238">要将结果写入的文件。</span><span class="sxs-lookup"><span data-stu-id="fe996-238">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="fe996-239">-i</span><span class="sxs-lookup"><span data-stu-id="fe996-239">-i</span></span> | <span data-ttu-id="fe996-240">-幂等</span><span class="sxs-lookup"><span data-stu-id="fe996-240">--idempotent</span></span>     | <span data-ttu-id="fe996-241">生成脚本，可以在任何迁移的数据库上使用。</span><span class="sxs-lookup"><span data-stu-id="fe996-241">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
