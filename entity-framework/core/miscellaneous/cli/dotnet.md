---
title: ".NET 核心 CLI-EF 核心"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 8a52cb8259bb381729a33a8161aec4b73f69f45d
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2018
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="1852c-102">EF 核心.NET 命令行工具</span><span class="sxs-lookup"><span data-stu-id="1852c-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="1852c-103">实体框架核心.NET 命令行工具是一种扩展到跨平台**dotnet**命令，是一部分的[.NET 核心 SDK][2]。</span><span class="sxs-lookup"><span data-stu-id="1852c-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="1852c-104">如果你使用 Visual Studio，我们建议[PMC 工具][ 1]相反因为它们提供了更多集成的体验。</span><span class="sxs-lookup"><span data-stu-id="1852c-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="1852c-105">安装工具</span><span class="sxs-lookup"><span data-stu-id="1852c-105">Installing the tools</span></span>
--------------------
<span data-ttu-id="1852c-106">安装使用这些步骤的 EF 核心.NET 命令行工具：</span><span class="sxs-lookup"><span data-stu-id="1852c-106">Install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="1852c-107">编辑项目文件并将 Microsoft.EntityFrameworkCore.Tools.DotNet 添加为 DotNetCliToolReference 项 （见下文）</span><span class="sxs-lookup"><span data-stu-id="1852c-107">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="1852c-108">运行以下命令：</span><span class="sxs-lookup"><span data-stu-id="1852c-108">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore


<span data-ttu-id="1852c-109">生成的项目应如下所示：</span><span class="sxs-lookup"><span data-stu-id="1852c-109">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="1852c-110">一个具有的包引用`PrivateAssets="All"`意味着它不公开给引用此项目，这是非常适合通常仅在开发期间使用的包的项目。</span><span class="sxs-lookup"><span data-stu-id="1852c-110">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="1852c-111">如果您还没有完全正确，你应能够成功的命令提示中运行以下命令。</span><span class="sxs-lookup"><span data-stu-id="1852c-111">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="1852c-112">使用的工具</span><span class="sxs-lookup"><span data-stu-id="1852c-112">Using the tools</span></span>
---------------
<span data-ttu-id="1852c-113">每当调用某命令时，有两个项目涉及：</span><span class="sxs-lookup"><span data-stu-id="1852c-113">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="1852c-114">目标项目是在其中添加任何文件（或在某些情况下删除文件）的项目。</span><span class="sxs-lookup"><span data-stu-id="1852c-114">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="1852c-115">目标项目默认为当前目录中的项目，但可以使用更改<nobr> **-项目**</nobr>选项。</span><span class="sxs-lookup"><span data-stu-id="1852c-115">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="1852c-116">启动项目是执行项目代码时由工具模拟的项目。</span><span class="sxs-lookup"><span data-stu-id="1852c-116">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="1852c-117">它也默认为当前目录中的项目，但可以使用更改**-启动项目**选项。</span><span class="sxs-lookup"><span data-stu-id="1852c-117">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

<span data-ttu-id="1852c-118">常用的选项：</span><span class="sxs-lookup"><span data-stu-id="1852c-118">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="1852c-119">--json</span><span class="sxs-lookup"><span data-stu-id="1852c-119">--json</span></span>                           | <span data-ttu-id="1852c-120">显示 JSON 输出。</span><span class="sxs-lookup"><span data-stu-id="1852c-120">Show JSON output.</span></span>           |
| <span data-ttu-id="1852c-121">-c</span><span class="sxs-lookup"><span data-stu-id="1852c-121">-c</span></span> | <span data-ttu-id="1852c-122">--context \<DBCONTEXT></span><span class="sxs-lookup"><span data-stu-id="1852c-122">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="1852c-123">若要使用 DbContext。</span><span class="sxs-lookup"><span data-stu-id="1852c-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="1852c-124">-p</span><span class="sxs-lookup"><span data-stu-id="1852c-124">-p</span></span> | <span data-ttu-id="1852c-125">-项目\<项目 ></span><span class="sxs-lookup"><span data-stu-id="1852c-125">--project \<PROJECT></span></span>             | <span data-ttu-id="1852c-126">要使用的项目。</span><span class="sxs-lookup"><span data-stu-id="1852c-126">The project to use.</span></span>         |
| <span data-ttu-id="1852c-127">-s</span><span class="sxs-lookup"><span data-stu-id="1852c-127">-s</span></span> | <span data-ttu-id="1852c-128">-启动项目\<项目 ></span><span class="sxs-lookup"><span data-stu-id="1852c-128">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="1852c-129">要使用的启动项目。</span><span class="sxs-lookup"><span data-stu-id="1852c-129">The startup project to use.</span></span> |
|    | <span data-ttu-id="1852c-130">--framework \<FRAMEWORK></span><span class="sxs-lookup"><span data-stu-id="1852c-130">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="1852c-131">目标框架中。</span><span class="sxs-lookup"><span data-stu-id="1852c-131">The target framework.</span></span>       |
|    | <span data-ttu-id="1852c-132">-配置\<配置 ></span><span class="sxs-lookup"><span data-stu-id="1852c-132">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="1852c-133">要使用的配置。</span><span class="sxs-lookup"><span data-stu-id="1852c-133">The configuration to use.</span></span>   |
|    | <span data-ttu-id="1852c-134">-运行时\<标识符 ></span><span class="sxs-lookup"><span data-stu-id="1852c-134">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="1852c-135">若要使用运行时。</span><span class="sxs-lookup"><span data-stu-id="1852c-135">The runtime to use.</span></span>         |
| <span data-ttu-id="1852c-136">-h</span><span class="sxs-lookup"><span data-stu-id="1852c-136">-h</span></span> | <span data-ttu-id="1852c-137">--help</span><span class="sxs-lookup"><span data-stu-id="1852c-137">--help</span></span>                           | <span data-ttu-id="1852c-138">显示帮助信息。</span><span class="sxs-lookup"><span data-stu-id="1852c-138">Show help information.</span></span>      |
| <span data-ttu-id="1852c-139">-v</span><span class="sxs-lookup"><span data-stu-id="1852c-139">-v</span></span> | <span data-ttu-id="1852c-140">--verbose</span><span class="sxs-lookup"><span data-stu-id="1852c-140">--verbose</span></span>                        | <span data-ttu-id="1852c-141">显示详细输出。</span><span class="sxs-lookup"><span data-stu-id="1852c-141">Show verbose output.</span></span>        |
|    | <span data-ttu-id="1852c-142">--no-color</span><span class="sxs-lookup"><span data-stu-id="1852c-142">--no-color</span></span>                       | <span data-ttu-id="1852c-143">不为着色输出。</span><span class="sxs-lookup"><span data-stu-id="1852c-143">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="1852c-144">--prefix-output</span><span class="sxs-lookup"><span data-stu-id="1852c-144">--prefix-output</span></span>                  | <span data-ttu-id="1852c-145">输出与级别的前缀。</span><span class="sxs-lookup"><span data-stu-id="1852c-145">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="1852c-146">若要指定 ASP.NET Core 环境，设置**ASPNETCORE_ENVIRONMENT**之前运行的环境变量。</span><span class="sxs-lookup"><span data-stu-id="1852c-146">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="1852c-147">命令</span><span class="sxs-lookup"><span data-stu-id="1852c-147">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="1852c-148">dotnet ef 数据库除去</span><span class="sxs-lookup"><span data-stu-id="1852c-148">dotnet ef database drop</span></span>

<span data-ttu-id="1852c-149">删除数据库。</span><span class="sxs-lookup"><span data-stu-id="1852c-149">Drops the database.</span></span>

<span data-ttu-id="1852c-150">选项:</span><span class="sxs-lookup"><span data-stu-id="1852c-150">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="1852c-151">-f</span><span class="sxs-lookup"><span data-stu-id="1852c-151">-f</span></span> | <span data-ttu-id="1852c-152">--force</span><span class="sxs-lookup"><span data-stu-id="1852c-152">--force</span></span>   | <span data-ttu-id="1852c-153">不确认。</span><span class="sxs-lookup"><span data-stu-id="1852c-153">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="1852c-154">-试运行</span><span class="sxs-lookup"><span data-stu-id="1852c-154">--dry-run</span></span> | <span data-ttu-id="1852c-155">显示的数据库会被丢弃，但没有删除它。</span><span class="sxs-lookup"><span data-stu-id="1852c-155">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="1852c-156">dotnet ef 数据库更新</span><span class="sxs-lookup"><span data-stu-id="1852c-156">dotnet ef database update</span></span>

<span data-ttu-id="1852c-157">到指定的迁移更新数据库。</span><span class="sxs-lookup"><span data-stu-id="1852c-157">Updates the database to a specified migration.</span></span>

<span data-ttu-id="1852c-158">自变量：</span><span class="sxs-lookup"><span data-stu-id="1852c-158">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="1852c-159">\<迁移 ></span><span class="sxs-lookup"><span data-stu-id="1852c-159">\<MIGRATION></span></span> | <span data-ttu-id="1852c-160">目标迁移。</span><span class="sxs-lookup"><span data-stu-id="1852c-160">The target migration.</span></span> <span data-ttu-id="1852c-161">如果为 0，将恢复所有迁移。</span><span class="sxs-lookup"><span data-stu-id="1852c-161">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="1852c-162">默认到最后一个迁移。</span><span class="sxs-lookup"><span data-stu-id="1852c-162">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="1852c-163">dotnet ef dbcontext 信息</span><span class="sxs-lookup"><span data-stu-id="1852c-163">dotnet ef dbcontext info</span></span>

<span data-ttu-id="1852c-164">获取有关 DbContext 类型的信息。</span><span class="sxs-lookup"><span data-stu-id="1852c-164">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="1852c-165">dotnet ef dbcontext 列表</span><span class="sxs-lookup"><span data-stu-id="1852c-165">dotnet ef dbcontext list</span></span>

<span data-ttu-id="1852c-166">列出可用的 DbContext 类型。</span><span class="sxs-lookup"><span data-stu-id="1852c-166">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="1852c-167">dotnet ef dbcontext 基架</span><span class="sxs-lookup"><span data-stu-id="1852c-167">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="1852c-168">基架数据库类型 DbContext 和实体的类型。</span><span class="sxs-lookup"><span data-stu-id="1852c-168">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="1852c-169">自变量：</span><span class="sxs-lookup"><span data-stu-id="1852c-169">Arguments:</span></span>

|               |                                                                     |
|:--------------|:--------------------------------------------------------------------|
| <span data-ttu-id="1852c-170">\<连接 ></span><span class="sxs-lookup"><span data-stu-id="1852c-170">\<CONNECTION></span></span> | <span data-ttu-id="1852c-171">数据库的连接字符串。</span><span class="sxs-lookup"><span data-stu-id="1852c-171">The connection string to the database.</span></span>                              |
| <span data-ttu-id="1852c-172">\<PROVIDER></span><span class="sxs-lookup"><span data-stu-id="1852c-172">\<PROVIDER></span></span>   | <span data-ttu-id="1852c-173">要使用的提供程序。</span><span class="sxs-lookup"><span data-stu-id="1852c-173">The provider to use.</span></span> <span data-ttu-id="1852c-174">（例如</span><span class="sxs-lookup"><span data-stu-id="1852c-174">(E.g.</span></span> <span data-ttu-id="1852c-175">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="1852c-175">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="1852c-176">选项:</span><span class="sxs-lookup"><span data-stu-id="1852c-176">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="1852c-177"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="1852c-177"><nobr>-d</nobr></span></span> | <span data-ttu-id="1852c-178">--data-annotations</span><span class="sxs-lookup"><span data-stu-id="1852c-178">--data-annotations</span></span>                      | <span data-ttu-id="1852c-179">使用属性来配置该模型 （如果可能）。</span><span class="sxs-lookup"><span data-stu-id="1852c-179">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="1852c-180">如果省略，则使用仅 fluent API。</span><span class="sxs-lookup"><span data-stu-id="1852c-180">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="1852c-181">-c</span><span class="sxs-lookup"><span data-stu-id="1852c-181">-c</span></span>              | <span data-ttu-id="1852c-182">-上下文\<名称 ></span><span class="sxs-lookup"><span data-stu-id="1852c-182">--context \<NAME></span></span>                       | <span data-ttu-id="1852c-183">Dbcontext 名称。</span><span class="sxs-lookup"><span data-stu-id="1852c-183">The name of the DbContext.</span></span>                                                                       |
| <span data-ttu-id="1852c-184">-f</span><span class="sxs-lookup"><span data-stu-id="1852c-184">-f</span></span>              | <span data-ttu-id="1852c-185">--force</span><span class="sxs-lookup"><span data-stu-id="1852c-185">--force</span></span>                                 | <span data-ttu-id="1852c-186">覆盖现有文件。</span><span class="sxs-lookup"><span data-stu-id="1852c-186">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="1852c-187">-o</span><span class="sxs-lookup"><span data-stu-id="1852c-187">-o</span></span>              | <span data-ttu-id="1852c-188">-输出 dir\<路径 ></span><span class="sxs-lookup"><span data-stu-id="1852c-188">--output-dir \<PATH></span></span>                    | <span data-ttu-id="1852c-189">要将文件放入的目录。</span><span class="sxs-lookup"><span data-stu-id="1852c-189">The directory to put files in.</span></span> <span data-ttu-id="1852c-190">路径是相对于项目目录。</span><span class="sxs-lookup"><span data-stu-id="1852c-190">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="1852c-191"><nobr>-架构\<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="1852c-191"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="1852c-192">要生成实体类型的表架构。</span><span class="sxs-lookup"><span data-stu-id="1852c-192">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="1852c-193">-t</span><span class="sxs-lookup"><span data-stu-id="1852c-193">-t</span></span>              | <span data-ttu-id="1852c-194">-表\<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="1852c-194">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="1852c-195">要生成实体类型的表。</span><span class="sxs-lookup"><span data-stu-id="1852c-195">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="1852c-196">--use-database-names</span><span class="sxs-lookup"><span data-stu-id="1852c-196">--use-database-names</span></span>                    | <span data-ttu-id="1852c-197">使用直接从数据库表和列名称。</span><span class="sxs-lookup"><span data-stu-id="1852c-197">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="1852c-198">dotnet ef 迁移添加</span><span class="sxs-lookup"><span data-stu-id="1852c-198">dotnet ef migrations add</span></span>

<span data-ttu-id="1852c-199">将添加一个新迁移。</span><span class="sxs-lookup"><span data-stu-id="1852c-199">Adds a new migration.</span></span>

<span data-ttu-id="1852c-200">自变量：</span><span class="sxs-lookup"><span data-stu-id="1852c-200">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="1852c-201">\<NAME></span><span class="sxs-lookup"><span data-stu-id="1852c-201">\<NAME></span></span> | <span data-ttu-id="1852c-202">迁移的名称。</span><span class="sxs-lookup"><span data-stu-id="1852c-202">The name of the migration.</span></span> |

<span data-ttu-id="1852c-203">选项:</span><span class="sxs-lookup"><span data-stu-id="1852c-203">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="1852c-204"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="1852c-204"><nobr>-o</nobr></span></span> | <span data-ttu-id="1852c-205"><nobr>-输出 dir\<路径 ></nobr></span><span class="sxs-lookup"><span data-stu-id="1852c-205"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="1852c-206">目录 （及其子命名空间） 使用。</span><span class="sxs-lookup"><span data-stu-id="1852c-206">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="1852c-207">路径是相对于项目目录。</span><span class="sxs-lookup"><span data-stu-id="1852c-207">Paths are relative to the project directory.</span></span> <span data-ttu-id="1852c-208">默认值为"迁移"。</span><span class="sxs-lookup"><span data-stu-id="1852c-208">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="1852c-209">dotnet ef 迁移列表</span><span class="sxs-lookup"><span data-stu-id="1852c-209">dotnet ef migrations list</span></span>

<span data-ttu-id="1852c-210">列出可用的迁移。</span><span class="sxs-lookup"><span data-stu-id="1852c-210">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="1852c-211">dotnet ef 迁移删除</span><span class="sxs-lookup"><span data-stu-id="1852c-211">dotnet ef migrations remove</span></span>

<span data-ttu-id="1852c-212">删除上次的迁移。</span><span class="sxs-lookup"><span data-stu-id="1852c-212">Removes the last migration.</span></span>

<span data-ttu-id="1852c-213">选项:</span><span class="sxs-lookup"><span data-stu-id="1852c-213">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="1852c-214">-f</span><span class="sxs-lookup"><span data-stu-id="1852c-214">-f</span></span> | <span data-ttu-id="1852c-215">--force</span><span class="sxs-lookup"><span data-stu-id="1852c-215">--force</span></span> | <span data-ttu-id="1852c-216">不检查以查看迁移是否已应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="1852c-216">Don't check to see if the migration has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="1852c-217">dotnet ef 迁移脚本</span><span class="sxs-lookup"><span data-stu-id="1852c-217">dotnet ef migrations script</span></span>

<span data-ttu-id="1852c-218">从迁移中生成的 SQL 脚本。</span><span class="sxs-lookup"><span data-stu-id="1852c-218">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="1852c-219">自变量：</span><span class="sxs-lookup"><span data-stu-id="1852c-219">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="1852c-220">\<FROM></span><span class="sxs-lookup"><span data-stu-id="1852c-220">\<FROM></span></span> | <span data-ttu-id="1852c-221">开始迁移。</span><span class="sxs-lookup"><span data-stu-id="1852c-221">The starting migration.</span></span> <span data-ttu-id="1852c-222">默认值为 0 （初始数据库）。</span><span class="sxs-lookup"><span data-stu-id="1852c-222">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="1852c-223">\<TO></span><span class="sxs-lookup"><span data-stu-id="1852c-223">\<TO></span></span>   | <span data-ttu-id="1852c-224">结束的迁移。</span><span class="sxs-lookup"><span data-stu-id="1852c-224">The ending migration.</span></span> <span data-ttu-id="1852c-225">默认到最后一个迁移。</span><span class="sxs-lookup"><span data-stu-id="1852c-225">Defaults to the last migration.</span></span>         |

<span data-ttu-id="1852c-226">选项:</span><span class="sxs-lookup"><span data-stu-id="1852c-226">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="1852c-227">-o</span><span class="sxs-lookup"><span data-stu-id="1852c-227">-o</span></span> | <span data-ttu-id="1852c-228">-输出\<文件 ></span><span class="sxs-lookup"><span data-stu-id="1852c-228">--output \<FILE></span></span> | <span data-ttu-id="1852c-229">要将结果写入的文件。</span><span class="sxs-lookup"><span data-stu-id="1852c-229">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="1852c-230">-i</span><span class="sxs-lookup"><span data-stu-id="1852c-230">-i</span></span> | <span data-ttu-id="1852c-231">-幂等</span><span class="sxs-lookup"><span data-stu-id="1852c-231">--idempotent</span></span>     | <span data-ttu-id="1852c-232">生成可以在任何迁移的数据库使用的脚本。</span><span class="sxs-lookup"><span data-stu-id="1852c-232">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
