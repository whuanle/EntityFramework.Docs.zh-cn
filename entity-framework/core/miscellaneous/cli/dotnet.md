---
title: .NET Core CLI-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: c70003fc7e2c5381a22d26baacd3d76f32489328
ms.sourcegitcommit: 0cef7d448e1e47bdb333002e2254ed42d57b45b6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/29/2018
ms.locfileid: "43152409"
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="5e5d4-102">EF Core.NET 命令行工具</span><span class="sxs-lookup"><span data-stu-id="5e5d4-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="5e5d4-103">实体框架核心.NET 命令行工具是一种扩展到跨平台**dotnet**命令，是一部分的[.NET Core SDK][2]。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="5e5d4-104">如果使用 Visual Studio，我们建议[PMC 工具][ 1]而是因为它们提供更完整的体验。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="5e5d4-105">安装工具</span><span class="sxs-lookup"><span data-stu-id="5e5d4-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="5e5d4-106">.NET Core SDK 版本 2.1.300 和更高版本包括**dotnet ef**与 EF Core 2.0 和更高版本兼容的命令。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="5e5d4-107">因此如果你使用的最新版本的.NET Core SDK 和 EF Core 运行时，需要进行任何安装，你可以忽略本部分的其余部分。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="5e5d4-108">另一方面， **dotnet ef**工具包含在.NET Core SDK 版本 2.1.300 和更高版本不兼容与 EF Core 版本 1.0 和 1.1。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="5e5d4-109">你可以使用.NET Core sdk 2.1.300 的计算机使用 EF Core 这些早期版本的项目或更高版本安装之前，你还必须安装版本 2.1.200 或更低版本的 sdk 和配置应用程序通过修改使用该旧版本其 [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json)文件。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="5e5d4-110">此文件通常包含在解决方案目录 （一个项目上方）。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="5e5d4-111">然后，您可以继续下面 installlation 指令。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="5e5d4-112">对于.NET Core sdk 的早期版本，你可以安装使用这些步骤的 EF Core.NET 命令行工具：</span><span class="sxs-lookup"><span data-stu-id="5e5d4-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="5e5d4-113">编辑项目文件并添加 Microsoft.EntityFrameworkCore.Tools.DotNet 作为 DotNetCliToolReference 项 （见下文）</span><span class="sxs-lookup"><span data-stu-id="5e5d4-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="5e5d4-114">运行以下命令：</span><span class="sxs-lookup"><span data-stu-id="5e5d4-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="5e5d4-115">生成的项目应如下所示：</span><span class="sxs-lookup"><span data-stu-id="5e5d4-115">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="5e5d4-116">使用的包引用`PrivateAssets="All"`意味着它不公开到引用此项目，特别适合用于在开发过程中通常仅使用的包的项目。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="5e5d4-117">如果您还没有完全正确，您应能够成功在命令提示符处运行以下命令。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="5e5d4-118">使用的工具</span><span class="sxs-lookup"><span data-stu-id="5e5d4-118">Using the tools</span></span>
---------------
<span data-ttu-id="5e5d4-119">每当调用命令时，有两个项目所涉及：</span><span class="sxs-lookup"><span data-stu-id="5e5d4-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="5e5d4-120">目标项目是在其中添加任何文件（或在某些情况下删除文件）的项目。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="5e5d4-121">目标项目默认为当前目录中的项目，但可以使用更改<nobr> **`--project`** </nobr>选项。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**`--project`**</nobr> option.</span></span>

<span data-ttu-id="5e5d4-122">启动项目是执行项目代码时由工具模拟的项目。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="5e5d4-123">它还将默认为当前目录中的项目，但可以使用更改**`--startup-project`** 选项。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-123">It also defaults to the project in the current directory, but can be changed using the **`--startup-project`** option.</span></span>

> [!NOTE]
> <span data-ttu-id="5e5d4-124">例如，更新的 web 应用程序具有不同的项目中安装的 EF Core 数据库将如下所示： `dotnet ef database update --project {project-path}` （从您的 web 应用程序目录）</span><span class="sxs-lookup"><span data-stu-id="5e5d4-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="5e5d4-125">常用选项：</span><span class="sxs-lookup"><span data-stu-id="5e5d4-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | `--json`                           | <span data-ttu-id="5e5d4-126">显示 JSON 输出。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-126">Show JSON output.</span></span>           |
| <span data-ttu-id="5e5d4-127">-c</span><span class="sxs-lookup"><span data-stu-id="5e5d4-127">-c</span></span> | `--context <DBCONTEXT>`           | <span data-ttu-id="5e5d4-128">若要使用 DbContext。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-128">The DbContext to use.</span></span>       |
| <span data-ttu-id="5e5d4-129">-p</span><span class="sxs-lookup"><span data-stu-id="5e5d4-129">-p</span></span> | `--project <PROJECT>`             | <span data-ttu-id="5e5d4-130">要使用的项目。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-130">The project to use.</span></span>         |
| <span data-ttu-id="5e5d4-131">-s</span><span class="sxs-lookup"><span data-stu-id="5e5d4-131">-s</span></span> | `--startup-project <PROJECT>`     | <span data-ttu-id="5e5d4-132">若要使用的启动项目。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-132">The startup project to use.</span></span> |
|    | `--framework <FRAMEWORK>`         | <span data-ttu-id="5e5d4-133">目标框架。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-133">The target framework.</span></span>       |
|    | `--configuration <CONFIGURATION>` | <span data-ttu-id="5e5d4-134">要使用的配置。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-134">The configuration to use.</span></span>   |
|    | `--runtime <IDENTIFIER>`          | <span data-ttu-id="5e5d4-135">运行时使用。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-135">The runtime to use.</span></span>         |
| <span data-ttu-id="5e5d4-136">-h</span><span class="sxs-lookup"><span data-stu-id="5e5d4-136">-h</span></span> | `--help`                           | <span data-ttu-id="5e5d4-137">显示帮助信息。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-137">Show help information.</span></span>      |
| <span data-ttu-id="5e5d4-138">-v</span><span class="sxs-lookup"><span data-stu-id="5e5d4-138">-v</span></span> | `--verbose`                        | <span data-ttu-id="5e5d4-139">显示详细输出。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-139">Show verbose output.</span></span>        |
|    | `--no-color`                       | <span data-ttu-id="5e5d4-140">不为着色输出。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-140">Don't colorize output.</span></span>      |
|    | `--prefix-output`                  | <span data-ttu-id="5e5d4-141">输出级别使用的前缀。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-141">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="5e5d4-142">若要指定 ASP.NET Core 环境，设置**ASPNETCORE_ENVIRONMENT**之前运行的环境变量。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-142">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="5e5d4-143">命令</span><span class="sxs-lookup"><span data-stu-id="5e5d4-143">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="5e5d4-144">dotnet ef 数据库拖放</span><span class="sxs-lookup"><span data-stu-id="5e5d4-144">dotnet ef database drop</span></span>

<span data-ttu-id="5e5d4-145">删除数据库。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-145">Drops the database.</span></span>

<span data-ttu-id="5e5d4-146">选项:</span><span class="sxs-lookup"><span data-stu-id="5e5d4-146">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="5e5d4-147">-f</span><span class="sxs-lookup"><span data-stu-id="5e5d4-147">-f</span></span> | `--force`   | <span data-ttu-id="5e5d4-148">不确认。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-148">Don't confirm.</span></span>                                           |
|    | `--dry-run` | <span data-ttu-id="5e5d4-149">显示哪个数据库会被丢弃，但没有删除它。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-149">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="5e5d4-150">dotnet ef 数据库更新</span><span class="sxs-lookup"><span data-stu-id="5e5d4-150">dotnet ef database update</span></span>

<span data-ttu-id="5e5d4-151">指定迁移到更新数据库。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-151">Updates the database to a specified migration.</span></span>

<span data-ttu-id="5e5d4-152">参数：</span><span class="sxs-lookup"><span data-stu-id="5e5d4-152">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| `<MIGRATION>` | <span data-ttu-id="5e5d4-153">目标迁移。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-153">The target migration.</span></span> <span data-ttu-id="5e5d4-154">如果为 0，将还原所有迁移。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-154">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="5e5d4-155">默认的最后一个迁移。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-155">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="5e5d4-156">dotnet ef dbcontext 信息</span><span class="sxs-lookup"><span data-stu-id="5e5d4-156">dotnet ef dbcontext info</span></span>

<span data-ttu-id="5e5d4-157">获取有关 DbContext 类型的信息。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-157">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="5e5d4-158">dotnet ef dbcontext 列表</span><span class="sxs-lookup"><span data-stu-id="5e5d4-158">dotnet ef dbcontext list</span></span>

<span data-ttu-id="5e5d4-159">列出了可用的 DbContext 类型。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-159">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="5e5d4-160">dotnet ef dbcontext 基架</span><span class="sxs-lookup"><span data-stu-id="5e5d4-160">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="5e5d4-161">搭建基架以数据库的 DbContext 和实体类型。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-161">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="5e5d4-162">参数：</span><span class="sxs-lookup"><span data-stu-id="5e5d4-162">Arguments:</span></span>

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| `<CONNECTION>` | <span data-ttu-id="5e5d4-163">数据库的连接字符串。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-163">The connection string to the database.</span></span>                                      |
| `<PROVIDER>`   | <span data-ttu-id="5e5d4-164">要使用的提供程序。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-164">The provider to use.</span></span> <span data-ttu-id="5e5d4-165">（例如 Microsoft.EntityFrameworkCore.SqlServer）</span><span class="sxs-lookup"><span data-stu-id="5e5d4-165">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="5e5d4-166">选项:</span><span class="sxs-lookup"><span data-stu-id="5e5d4-166">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="5e5d4-167"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="5e5d4-167"><nobr>-d</nobr></span></span> | `--data-annotations`                      | <span data-ttu-id="5e5d4-168">特性用于将模型配置 （如果可能）。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-168">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="5e5d4-169">如果省略，则使用仅 fluent API。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-169">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="5e5d4-170">-c</span><span class="sxs-lookup"><span data-stu-id="5e5d4-170">-c</span></span>              | `--context <NAME>`                       | <span data-ttu-id="5e5d4-171">DbContext 的名称。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-171">The name of the DbContext.</span></span>                                                                       |
|                 | `--context-dir <PATH>`                   | <span data-ttu-id="5e5d4-172">要将 DbContext 文件放入的目录。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-172">The directory to put DbContext file in.</span></span> <span data-ttu-id="5e5d4-173">路径是相对于项目目录。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-173">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="5e5d4-174">-f</span><span class="sxs-lookup"><span data-stu-id="5e5d4-174">-f</span></span>              | `--force`                                 | <span data-ttu-id="5e5d4-175">覆盖现有文件。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-175">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="5e5d4-176">-o</span><span class="sxs-lookup"><span data-stu-id="5e5d4-176">-o</span></span>              | `--output-dir <PATH>`                    | <span data-ttu-id="5e5d4-177">要将文件放入的目录。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-177">The directory to put files in.</span></span> <span data-ttu-id="5e5d4-178">路径是相对于项目目录。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-178">Paths are relative to the project directory.</span></span>                      |
|                 | <nobr>`--schema <SCHEMA_NAME>...`</nobr> | <span data-ttu-id="5e5d4-179">要生成的实体类型的表架构。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-179">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="5e5d4-180">-t</span><span class="sxs-lookup"><span data-stu-id="5e5d4-180">-t</span></span>              | <span data-ttu-id="5e5d4-181">`--table <TABLE_NAME>`...</span><span class="sxs-lookup"><span data-stu-id="5e5d4-181">`--table <TABLE_NAME>`...</span></span>                | <span data-ttu-id="5e5d4-182">要生成的实体类型的表。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-182">The tables to generate entity types for.</span></span>                                                         |
|                 | `--use-database-names`                    | <span data-ttu-id="5e5d4-183">使用直接从数据库表和列名称。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-183">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="5e5d4-184">添加 dotnet ef 迁移</span><span class="sxs-lookup"><span data-stu-id="5e5d4-184">dotnet ef migrations add</span></span>

<span data-ttu-id="5e5d4-185">添加新的迁移。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-185">Adds a new migration.</span></span>

<span data-ttu-id="5e5d4-186">参数：</span><span class="sxs-lookup"><span data-stu-id="5e5d4-186">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| `<NAME>` | <span data-ttu-id="5e5d4-187">迁移的名称。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-187">The name of the migration.</span></span> |

<span data-ttu-id="5e5d4-188">选项:</span><span class="sxs-lookup"><span data-stu-id="5e5d4-188">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="5e5d4-189"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="5e5d4-189"><nobr>-o</nobr></span></span> | <span data-ttu-id="5e5d4-190"><nobr> `--output-dir <PATH>` </nobr></span><span class="sxs-lookup"><span data-stu-id="5e5d4-190"><nobr> `--output-dir <PATH>` </nobr></span></span> | <span data-ttu-id="5e5d4-191">目录 （及其子命名空间） 来使用。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-191">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="5e5d4-192">路径是相对于项目目录。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-192">Paths are relative to the project directory.</span></span> <span data-ttu-id="5e5d4-193">默认值为"迁移"。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-193">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="5e5d4-194">dotnet ef 迁移列表</span><span class="sxs-lookup"><span data-stu-id="5e5d4-194">dotnet ef migrations list</span></span>

<span data-ttu-id="5e5d4-195">列出了可用的迁移。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-195">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="5e5d4-196">dotnet ef 迁移删除</span><span class="sxs-lookup"><span data-stu-id="5e5d4-196">dotnet ef migrations remove</span></span>

<span data-ttu-id="5e5d4-197">删除最后一个迁移。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-197">Removes the last migration.</span></span>

<span data-ttu-id="5e5d4-198">选项:</span><span class="sxs-lookup"><span data-stu-id="5e5d4-198">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="5e5d4-199">-f</span><span class="sxs-lookup"><span data-stu-id="5e5d4-199">-f</span></span> | `--force` | <span data-ttu-id="5e5d4-200">如果它已应用到数据库，请还原迁移。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-200">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="5e5d4-201">dotnet ef 迁移脚本</span><span class="sxs-lookup"><span data-stu-id="5e5d4-201">dotnet ef migrations script</span></span>

<span data-ttu-id="5e5d4-202">从迁移中生成的 SQL 脚本。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-202">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="5e5d4-203">参数：</span><span class="sxs-lookup"><span data-stu-id="5e5d4-203">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| `<FROM>` | <span data-ttu-id="5e5d4-204">开始迁移。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-204">The starting migration.</span></span> <span data-ttu-id="5e5d4-205">默认值为 0 （初始数据库）。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-205">Defaults to 0 (the initial database).</span></span> |
| `<TO>`   | <span data-ttu-id="5e5d4-206">结束的迁移。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-206">The ending migration.</span></span> <span data-ttu-id="5e5d4-207">默认的最后一个迁移。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-207">Defaults to the last migration.</span></span>         |

<span data-ttu-id="5e5d4-208">选项:</span><span class="sxs-lookup"><span data-stu-id="5e5d4-208">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="5e5d4-209">-o</span><span class="sxs-lookup"><span data-stu-id="5e5d4-209">-o</span></span> | `--output <FILE>` | <span data-ttu-id="5e5d4-210">要将结果写入的文件。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-210">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="5e5d4-211">-i</span><span class="sxs-lookup"><span data-stu-id="5e5d4-211">-i</span></span> | `--idempotent`     | <span data-ttu-id="5e5d4-212">生成脚本，可以在任何迁移的数据库上使用。</span><span class="sxs-lookup"><span data-stu-id="5e5d4-212">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
