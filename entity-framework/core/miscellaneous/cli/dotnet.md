---
title: .NET 核心 CLI-EF 核心
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 396d31c9d0c0f47d299f49e82e557ed29b8420e7
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/16/2018
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="0ca62-102">EF 核心.NET 命令行工具</span><span class="sxs-lookup"><span data-stu-id="0ca62-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="0ca62-103">实体框架核心.NET 命令行工具是一种扩展到跨平台**dotnet**命令，是一部分的[.NET 核心 SDK][2]。</span><span class="sxs-lookup"><span data-stu-id="0ca62-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="0ca62-104">如果你使用 Visual Studio，我们建议[PMC 工具][ 1]相反因为它们提供了更多集成的体验。</span><span class="sxs-lookup"><span data-stu-id="0ca62-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="0ca62-105">安装工具</span><span class="sxs-lookup"><span data-stu-id="0ca62-105">Installing the tools</span></span>
--------------------
<span data-ttu-id="0ca62-106">安装使用这些步骤的 EF 核心.NET 命令行工具：</span><span class="sxs-lookup"><span data-stu-id="0ca62-106">Install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="0ca62-107">编辑项目文件并将 Microsoft.EntityFrameworkCore.Tools.DotNet 添加为 DotNetCliToolReference 项 （见下文）</span><span class="sxs-lookup"><span data-stu-id="0ca62-107">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="0ca62-108">运行以下命令：</span><span class="sxs-lookup"><span data-stu-id="0ca62-108">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore


<span data-ttu-id="0ca62-109">生成的项目应如下所示：</span><span class="sxs-lookup"><span data-stu-id="0ca62-109">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="0ca62-110">一个具有的包引用`PrivateAssets="All"`意味着它不公开给引用此项目，这是非常适合通常仅在开发期间使用的包的项目。</span><span class="sxs-lookup"><span data-stu-id="0ca62-110">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="0ca62-111">如果您还没有完全正确，你应能够成功的命令提示中运行以下命令。</span><span class="sxs-lookup"><span data-stu-id="0ca62-111">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="0ca62-112">使用的工具</span><span class="sxs-lookup"><span data-stu-id="0ca62-112">Using the tools</span></span>
---------------
<span data-ttu-id="0ca62-113">每当调用某命令时，有两个项目涉及：</span><span class="sxs-lookup"><span data-stu-id="0ca62-113">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="0ca62-114">目标项目是在其中添加任何文件（或在某些情况下删除文件）的项目。</span><span class="sxs-lookup"><span data-stu-id="0ca62-114">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="0ca62-115">目标项目默认为当前目录中的项目，但可以使用更改<nobr> **-项目**</nobr>选项。</span><span class="sxs-lookup"><span data-stu-id="0ca62-115">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="0ca62-116">启动项目是执行项目代码时由工具模拟的项目。</span><span class="sxs-lookup"><span data-stu-id="0ca62-116">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="0ca62-117">它也默认为当前目录中的项目，但可以使用更改**-启动项目**选项。</span><span class="sxs-lookup"><span data-stu-id="0ca62-117">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="0ca62-118">例如，更新的 web 应用程序具有不同的项目中安装的 EF 核心数据库将如下所示： `dotnet ef database update --project {project-path}` （从您的 web 应用程序目录）</span><span class="sxs-lookup"><span data-stu-id="0ca62-118">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="0ca62-119">常用的选项：</span><span class="sxs-lookup"><span data-stu-id="0ca62-119">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="0ca62-120">--json</span><span class="sxs-lookup"><span data-stu-id="0ca62-120">--json</span></span>                           | <span data-ttu-id="0ca62-121">显示 JSON 输出。</span><span class="sxs-lookup"><span data-stu-id="0ca62-121">Show JSON output.</span></span>           |
| <span data-ttu-id="0ca62-122">-c</span><span class="sxs-lookup"><span data-stu-id="0ca62-122">-c</span></span> | <span data-ttu-id="0ca62-123">-上下文\<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="0ca62-123">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="0ca62-124">若要使用 DbContext。</span><span class="sxs-lookup"><span data-stu-id="0ca62-124">The DbContext to use.</span></span>       |
| <span data-ttu-id="0ca62-125">-p</span><span class="sxs-lookup"><span data-stu-id="0ca62-125">-p</span></span> | <span data-ttu-id="0ca62-126">-项目\<项目 ></span><span class="sxs-lookup"><span data-stu-id="0ca62-126">--project \<PROJECT></span></span>             | <span data-ttu-id="0ca62-127">要使用的项目。</span><span class="sxs-lookup"><span data-stu-id="0ca62-127">The project to use.</span></span>         |
| <span data-ttu-id="0ca62-128">-s</span><span class="sxs-lookup"><span data-stu-id="0ca62-128">-s</span></span> | <span data-ttu-id="0ca62-129">-启动项目\<项目 ></span><span class="sxs-lookup"><span data-stu-id="0ca62-129">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="0ca62-130">要使用的启动项目。</span><span class="sxs-lookup"><span data-stu-id="0ca62-130">The startup project to use.</span></span> |
|    | <span data-ttu-id="0ca62-131">-framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="0ca62-131">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="0ca62-132">目标框架中。</span><span class="sxs-lookup"><span data-stu-id="0ca62-132">The target framework.</span></span>       |
|    | <span data-ttu-id="0ca62-133">-配置\<配置 ></span><span class="sxs-lookup"><span data-stu-id="0ca62-133">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="0ca62-134">要使用的配置。</span><span class="sxs-lookup"><span data-stu-id="0ca62-134">The configuration to use.</span></span>   |
|    | <span data-ttu-id="0ca62-135">-运行时\<标识符 ></span><span class="sxs-lookup"><span data-stu-id="0ca62-135">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="0ca62-136">若要使用运行时。</span><span class="sxs-lookup"><span data-stu-id="0ca62-136">The runtime to use.</span></span>         |
| <span data-ttu-id="0ca62-137">-h</span><span class="sxs-lookup"><span data-stu-id="0ca62-137">-h</span></span> | <span data-ttu-id="0ca62-138">-帮助</span><span class="sxs-lookup"><span data-stu-id="0ca62-138">--help</span></span>                           | <span data-ttu-id="0ca62-139">显示帮助信息。</span><span class="sxs-lookup"><span data-stu-id="0ca62-139">Show help information.</span></span>      |
| <span data-ttu-id="0ca62-140">-v</span><span class="sxs-lookup"><span data-stu-id="0ca62-140">-v</span></span> | <span data-ttu-id="0ca62-141">-verbose</span><span class="sxs-lookup"><span data-stu-id="0ca62-141">--verbose</span></span>                        | <span data-ttu-id="0ca62-142">显示详细输出。</span><span class="sxs-lookup"><span data-stu-id="0ca62-142">Show verbose output.</span></span>        |
|    | <span data-ttu-id="0ca62-143">--no-color</span><span class="sxs-lookup"><span data-stu-id="0ca62-143">--no-color</span></span>                       | <span data-ttu-id="0ca62-144">不为着色输出。</span><span class="sxs-lookup"><span data-stu-id="0ca62-144">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="0ca62-145">--prefix-output</span><span class="sxs-lookup"><span data-stu-id="0ca62-145">--prefix-output</span></span>                  | <span data-ttu-id="0ca62-146">输出与级别的前缀。</span><span class="sxs-lookup"><span data-stu-id="0ca62-146">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="0ca62-147">若要指定 ASP.NET Core 环境，设置**ASPNETCORE_ENVIRONMENT**之前运行的环境变量。</span><span class="sxs-lookup"><span data-stu-id="0ca62-147">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="0ca62-148">命令</span><span class="sxs-lookup"><span data-stu-id="0ca62-148">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="0ca62-149">dotnet ef 数据库除去</span><span class="sxs-lookup"><span data-stu-id="0ca62-149">dotnet ef database drop</span></span>

<span data-ttu-id="0ca62-150">删除数据库。</span><span class="sxs-lookup"><span data-stu-id="0ca62-150">Drops the database.</span></span>

<span data-ttu-id="0ca62-151">选项:</span><span class="sxs-lookup"><span data-stu-id="0ca62-151">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="0ca62-152">-f</span><span class="sxs-lookup"><span data-stu-id="0ca62-152">-f</span></span> | <span data-ttu-id="0ca62-153">--force</span><span class="sxs-lookup"><span data-stu-id="0ca62-153">--force</span></span>   | <span data-ttu-id="0ca62-154">不确认。</span><span class="sxs-lookup"><span data-stu-id="0ca62-154">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="0ca62-155">-试运行</span><span class="sxs-lookup"><span data-stu-id="0ca62-155">--dry-run</span></span> | <span data-ttu-id="0ca62-156">显示的数据库会被丢弃，但没有删除它。</span><span class="sxs-lookup"><span data-stu-id="0ca62-156">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="0ca62-157">dotnet ef 数据库更新</span><span class="sxs-lookup"><span data-stu-id="0ca62-157">dotnet ef database update</span></span>

<span data-ttu-id="0ca62-158">到指定的迁移更新数据库。</span><span class="sxs-lookup"><span data-stu-id="0ca62-158">Updates the database to a specified migration.</span></span>

<span data-ttu-id="0ca62-159">自变量：</span><span class="sxs-lookup"><span data-stu-id="0ca62-159">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="0ca62-160">\<迁移 &GT;</span><span class="sxs-lookup"><span data-stu-id="0ca62-160">\<MIGRATION></span></span> | <span data-ttu-id="0ca62-161">目标迁移。</span><span class="sxs-lookup"><span data-stu-id="0ca62-161">The target migration.</span></span> <span data-ttu-id="0ca62-162">如果为 0，将恢复所有迁移。</span><span class="sxs-lookup"><span data-stu-id="0ca62-162">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="0ca62-163">默认到最后一个迁移。</span><span class="sxs-lookup"><span data-stu-id="0ca62-163">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="0ca62-164">dotnet ef dbcontext 信息</span><span class="sxs-lookup"><span data-stu-id="0ca62-164">dotnet ef dbcontext info</span></span>

<span data-ttu-id="0ca62-165">获取有关 DbContext 类型的信息。</span><span class="sxs-lookup"><span data-stu-id="0ca62-165">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="0ca62-166">dotnet ef dbcontext 列表</span><span class="sxs-lookup"><span data-stu-id="0ca62-166">dotnet ef dbcontext list</span></span>

<span data-ttu-id="0ca62-167">列出可用的 DbContext 类型。</span><span class="sxs-lookup"><span data-stu-id="0ca62-167">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="0ca62-168">dotnet ef dbcontext 基架</span><span class="sxs-lookup"><span data-stu-id="0ca62-168">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="0ca62-169">基架数据库类型 DbContext 和实体的类型。</span><span class="sxs-lookup"><span data-stu-id="0ca62-169">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="0ca62-170">自变量：</span><span class="sxs-lookup"><span data-stu-id="0ca62-170">Arguments:</span></span>

|               |                                                                     |
|:--------------|:--------------------------------------------------------------------|
| <span data-ttu-id="0ca62-171">\<连接 &GT;</span><span class="sxs-lookup"><span data-stu-id="0ca62-171">\<CONNECTION></span></span> | <span data-ttu-id="0ca62-172">数据库的连接字符串。</span><span class="sxs-lookup"><span data-stu-id="0ca62-172">The connection string to the database.</span></span>                              |
| <span data-ttu-id="0ca62-173">\<提供程序 &GT;</span><span class="sxs-lookup"><span data-stu-id="0ca62-173">\<PROVIDER></span></span>   | <span data-ttu-id="0ca62-174">要使用的提供程序。</span><span class="sxs-lookup"><span data-stu-id="0ca62-174">The provider to use.</span></span> <span data-ttu-id="0ca62-175">（例如，</span><span class="sxs-lookup"><span data-stu-id="0ca62-175">(E.g.</span></span> <span data-ttu-id="0ca62-176">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="0ca62-176">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="0ca62-177">选项:</span><span class="sxs-lookup"><span data-stu-id="0ca62-177">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0ca62-178"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="0ca62-178"><nobr>-d</nobr></span></span> | <span data-ttu-id="0ca62-179">-数据批注</span><span class="sxs-lookup"><span data-stu-id="0ca62-179">--data-annotations</span></span>                      | <span data-ttu-id="0ca62-180">使用属性来配置该模型 （如果可能）。</span><span class="sxs-lookup"><span data-stu-id="0ca62-180">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="0ca62-181">如果省略，则使用仅 fluent API。</span><span class="sxs-lookup"><span data-stu-id="0ca62-181">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="0ca62-182">-c</span><span class="sxs-lookup"><span data-stu-id="0ca62-182">-c</span></span>              | <span data-ttu-id="0ca62-183">-上下文\<名称 ></span><span class="sxs-lookup"><span data-stu-id="0ca62-183">--context \<NAME></span></span>                       | <span data-ttu-id="0ca62-184">Dbcontext 名称。</span><span class="sxs-lookup"><span data-stu-id="0ca62-184">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="0ca62-185">-上下文 dir\<路径 ></span><span class="sxs-lookup"><span data-stu-id="0ca62-185">--context-dir \<PATH></span></span>                   | <span data-ttu-id="0ca62-186">要将 DbContext 文件放入的目录。</span><span class="sxs-lookup"><span data-stu-id="0ca62-186">The directory to put DbContext file in.</span></span> <span data-ttu-id="0ca62-187">路径是相对于项目目录。</span><span class="sxs-lookup"><span data-stu-id="0ca62-187">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="0ca62-188">-f</span><span class="sxs-lookup"><span data-stu-id="0ca62-188">-f</span></span>              | <span data-ttu-id="0ca62-189">--force</span><span class="sxs-lookup"><span data-stu-id="0ca62-189">--force</span></span>                                 | <span data-ttu-id="0ca62-190">覆盖现有文件。</span><span class="sxs-lookup"><span data-stu-id="0ca62-190">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="0ca62-191">-o</span><span class="sxs-lookup"><span data-stu-id="0ca62-191">-o</span></span>              | <span data-ttu-id="0ca62-192">-输出 dir\<路径 ></span><span class="sxs-lookup"><span data-stu-id="0ca62-192">--output-dir \<PATH></span></span>                    | <span data-ttu-id="0ca62-193">要将文件放入的目录。</span><span class="sxs-lookup"><span data-stu-id="0ca62-193">The directory to put files in.</span></span> <span data-ttu-id="0ca62-194">路径是相对于项目目录。</span><span class="sxs-lookup"><span data-stu-id="0ca62-194">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="0ca62-195"><nobr>-架构\<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="0ca62-195"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="0ca62-196">要生成实体类型的表架构。</span><span class="sxs-lookup"><span data-stu-id="0ca62-196">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="0ca62-197">-t</span><span class="sxs-lookup"><span data-stu-id="0ca62-197">-t</span></span>              | <span data-ttu-id="0ca62-198">-表\<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="0ca62-198">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="0ca62-199">要生成实体类型的表。</span><span class="sxs-lookup"><span data-stu-id="0ca62-199">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="0ca62-200">-使用数据库名称</span><span class="sxs-lookup"><span data-stu-id="0ca62-200">--use-database-names</span></span>                    | <span data-ttu-id="0ca62-201">使用直接从数据库表和列名称。</span><span class="sxs-lookup"><span data-stu-id="0ca62-201">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="0ca62-202">dotnet ef 迁移添加</span><span class="sxs-lookup"><span data-stu-id="0ca62-202">dotnet ef migrations add</span></span>

<span data-ttu-id="0ca62-203">将添加一个新迁移。</span><span class="sxs-lookup"><span data-stu-id="0ca62-203">Adds a new migration.</span></span>

<span data-ttu-id="0ca62-204">自变量：</span><span class="sxs-lookup"><span data-stu-id="0ca62-204">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="0ca62-205">\<名称 &GT;</span><span class="sxs-lookup"><span data-stu-id="0ca62-205">\<NAME></span></span> | <span data-ttu-id="0ca62-206">迁移的名称。</span><span class="sxs-lookup"><span data-stu-id="0ca62-206">The name of the migration.</span></span> |

<span data-ttu-id="0ca62-207">选项:</span><span class="sxs-lookup"><span data-stu-id="0ca62-207">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0ca62-208"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="0ca62-208"><nobr>-o</nobr></span></span> | <span data-ttu-id="0ca62-209"><nobr>-输出 dir\<路径 ></nobr></span><span class="sxs-lookup"><span data-stu-id="0ca62-209"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="0ca62-210">目录 （及其子命名空间） 使用。</span><span class="sxs-lookup"><span data-stu-id="0ca62-210">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="0ca62-211">路径是相对于项目目录。</span><span class="sxs-lookup"><span data-stu-id="0ca62-211">Paths are relative to the project directory.</span></span> <span data-ttu-id="0ca62-212">默认值为"迁移"。</span><span class="sxs-lookup"><span data-stu-id="0ca62-212">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="0ca62-213">dotnet ef 迁移列表</span><span class="sxs-lookup"><span data-stu-id="0ca62-213">dotnet ef migrations list</span></span>

<span data-ttu-id="0ca62-214">列出可用的迁移。</span><span class="sxs-lookup"><span data-stu-id="0ca62-214">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="0ca62-215">dotnet ef 迁移删除</span><span class="sxs-lookup"><span data-stu-id="0ca62-215">dotnet ef migrations remove</span></span>

<span data-ttu-id="0ca62-216">删除上次的迁移。</span><span class="sxs-lookup"><span data-stu-id="0ca62-216">Removes the last migration.</span></span>

<span data-ttu-id="0ca62-217">选项:</span><span class="sxs-lookup"><span data-stu-id="0ca62-217">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="0ca62-218">-f</span><span class="sxs-lookup"><span data-stu-id="0ca62-218">-f</span></span> | <span data-ttu-id="0ca62-219">--force</span><span class="sxs-lookup"><span data-stu-id="0ca62-219">--force</span></span> | <span data-ttu-id="0ca62-220">如果它已应用到数据库，请还原迁移。</span><span class="sxs-lookup"><span data-stu-id="0ca62-220">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="0ca62-221">dotnet ef 迁移脚本</span><span class="sxs-lookup"><span data-stu-id="0ca62-221">dotnet ef migrations script</span></span>

<span data-ttu-id="0ca62-222">从迁移中生成的 SQL 脚本。</span><span class="sxs-lookup"><span data-stu-id="0ca62-222">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="0ca62-223">自变量：</span><span class="sxs-lookup"><span data-stu-id="0ca62-223">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="0ca62-224">\<FROM></span><span class="sxs-lookup"><span data-stu-id="0ca62-224">\<FROM></span></span> | <span data-ttu-id="0ca62-225">开始迁移。</span><span class="sxs-lookup"><span data-stu-id="0ca62-225">The starting migration.</span></span> <span data-ttu-id="0ca62-226">默认值为 0 （初始数据库）。</span><span class="sxs-lookup"><span data-stu-id="0ca62-226">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="0ca62-227">\<到 &GT;</span><span class="sxs-lookup"><span data-stu-id="0ca62-227">\<TO></span></span>   | <span data-ttu-id="0ca62-228">结束的迁移。</span><span class="sxs-lookup"><span data-stu-id="0ca62-228">The ending migration.</span></span> <span data-ttu-id="0ca62-229">默认到最后一个迁移。</span><span class="sxs-lookup"><span data-stu-id="0ca62-229">Defaults to the last migration.</span></span>         |

<span data-ttu-id="0ca62-230">选项:</span><span class="sxs-lookup"><span data-stu-id="0ca62-230">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="0ca62-231">-o</span><span class="sxs-lookup"><span data-stu-id="0ca62-231">-o</span></span> | <span data-ttu-id="0ca62-232">-输出\<文件 ></span><span class="sxs-lookup"><span data-stu-id="0ca62-232">--output \<FILE></span></span> | <span data-ttu-id="0ca62-233">要将结果写入的文件。</span><span class="sxs-lookup"><span data-stu-id="0ca62-233">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="0ca62-234">-i</span><span class="sxs-lookup"><span data-stu-id="0ca62-234">-i</span></span> | <span data-ttu-id="0ca62-235">-幂等</span><span class="sxs-lookup"><span data-stu-id="0ca62-235">--idempotent</span></span>     | <span data-ttu-id="0ca62-236">生成可以在任何迁移的数据库使用的脚本。</span><span class="sxs-lookup"><span data-stu-id="0ca62-236">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
