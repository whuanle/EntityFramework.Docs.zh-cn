---
title: ".NET 核心 CLI-EF 核心"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 26b5fb326d20575ed2f3c6955c699e0c3757bf57
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="51ab7-102">EF 核心.NET 命令行工具</span><span class="sxs-lookup"><span data-stu-id="51ab7-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="51ab7-103">实体框架核心.NET 命令行工具是一种扩展到跨平台**dotnet**命令，是一部分的[.NET 核心 SDK][2]。</span><span class="sxs-lookup"><span data-stu-id="51ab7-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="51ab7-104">如果你使用 Visual Studio，我们建议[PMC 工具][ 1]相反因为它们提供了更多集成的体验。</span><span class="sxs-lookup"><span data-stu-id="51ab7-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="51ab7-105">安装工具</span><span class="sxs-lookup"><span data-stu-id="51ab7-105">Installing the tools</span></span>
--------------------
<span data-ttu-id="51ab7-106">安装使用这些步骤的 EF 核心.NET 命令行工具：</span><span class="sxs-lookup"><span data-stu-id="51ab7-106">Install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="51ab7-107">编辑项目文件并将 Microsoft.EntityFrameworkCore.Tools.DotNet 添加为 DotNetCliToolReference 项 （见下文）</span><span class="sxs-lookup"><span data-stu-id="51ab7-107">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="51ab7-108">运行以下命令：</span><span class="sxs-lookup"><span data-stu-id="51ab7-108">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore


<span data-ttu-id="51ab7-109">生成的项目应如下所示：</span><span class="sxs-lookup"><span data-stu-id="51ab7-109">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="51ab7-110">一个具有的包引用`PrivateAssets="All"`意味着它不公开给引用此项目，这是非常适合通常仅在开发期间使用的包的项目。</span><span class="sxs-lookup"><span data-stu-id="51ab7-110">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="51ab7-111">如果您还没有完全正确，你应能够成功的命令提示中运行以下命令。</span><span class="sxs-lookup"><span data-stu-id="51ab7-111">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="51ab7-112">使用的工具</span><span class="sxs-lookup"><span data-stu-id="51ab7-112">Using the tools</span></span>
---------------
<span data-ttu-id="51ab7-113">每当调用某命令时，有两个项目涉及：</span><span class="sxs-lookup"><span data-stu-id="51ab7-113">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="51ab7-114">目标项目将添加的任何文件的位置 （或在某些情况下删除）。</span><span class="sxs-lookup"><span data-stu-id="51ab7-114">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="51ab7-115">目标项目默认为当前目录中的项目，但可以使用更改<nobr> **-项目**</nobr>选项。</span><span class="sxs-lookup"><span data-stu-id="51ab7-115">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="51ab7-116">启动项目是仿真的工具，执行你的项目代码时。</span><span class="sxs-lookup"><span data-stu-id="51ab7-116">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="51ab7-117">它也默认为当前目录中的项目，但可以使用更改**-启动项目**选项。</span><span class="sxs-lookup"><span data-stu-id="51ab7-117">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

<span data-ttu-id="51ab7-118">常用的选项：</span><span class="sxs-lookup"><span data-stu-id="51ab7-118">Common options:</span></span>

|    |                                  |                             |
| -- | -------------------------------- | --------------------------- |
|    | <span data-ttu-id="51ab7-119">--json</span><span class="sxs-lookup"><span data-stu-id="51ab7-119">--json</span></span>                           | <span data-ttu-id="51ab7-120">显示 JSON 输出。</span><span class="sxs-lookup"><span data-stu-id="51ab7-120">Show JSON output.</span></span>           |
| <span data-ttu-id="51ab7-121">-c</span><span class="sxs-lookup"><span data-stu-id="51ab7-121">-c</span></span> | <span data-ttu-id="51ab7-122">-上下文\<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="51ab7-122">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="51ab7-123">若要使用 DbContext。</span><span class="sxs-lookup"><span data-stu-id="51ab7-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="51ab7-124">-P</span><span class="sxs-lookup"><span data-stu-id="51ab7-124">-p</span></span> | <span data-ttu-id="51ab7-125">-项目\<项目 ></span><span class="sxs-lookup"><span data-stu-id="51ab7-125">--project \<PROJECT></span></span>             | <span data-ttu-id="51ab7-126">要使用的项目。</span><span class="sxs-lookup"><span data-stu-id="51ab7-126">The project to use.</span></span>         |
| <span data-ttu-id="51ab7-127">-s</span><span class="sxs-lookup"><span data-stu-id="51ab7-127">-s</span></span> | <span data-ttu-id="51ab7-128">-启动项目\<项目 ></span><span class="sxs-lookup"><span data-stu-id="51ab7-128">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="51ab7-129">要使用的启动项目。</span><span class="sxs-lookup"><span data-stu-id="51ab7-129">The startup project to use.</span></span> |
|    | <span data-ttu-id="51ab7-130">-framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="51ab7-130">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="51ab7-131">目标框架中。</span><span class="sxs-lookup"><span data-stu-id="51ab7-131">The target framework.</span></span>       |
|    | <span data-ttu-id="51ab7-132">-配置\<配置 ></span><span class="sxs-lookup"><span data-stu-id="51ab7-132">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="51ab7-133">要使用的配置。</span><span class="sxs-lookup"><span data-stu-id="51ab7-133">The configuration to use.</span></span>   |
|    | <span data-ttu-id="51ab7-134">-运行时\<标识符 ></span><span class="sxs-lookup"><span data-stu-id="51ab7-134">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="51ab7-135">若要使用运行时。</span><span class="sxs-lookup"><span data-stu-id="51ab7-135">The runtime to use.</span></span>         |
| <span data-ttu-id="51ab7-136">-h</span><span class="sxs-lookup"><span data-stu-id="51ab7-136">-h</span></span> | <span data-ttu-id="51ab7-137">-帮助</span><span class="sxs-lookup"><span data-stu-id="51ab7-137">--help</span></span>                           | <span data-ttu-id="51ab7-138">显示帮助信息。</span><span class="sxs-lookup"><span data-stu-id="51ab7-138">Show help information.</span></span>      |
| <span data-ttu-id="51ab7-139">-v</span><span class="sxs-lookup"><span data-stu-id="51ab7-139">-v</span></span> | <span data-ttu-id="51ab7-140">-verbose</span><span class="sxs-lookup"><span data-stu-id="51ab7-140">--verbose</span></span>                        | <span data-ttu-id="51ab7-141">显示详细输出。</span><span class="sxs-lookup"><span data-stu-id="51ab7-141">Show verbose output.</span></span>        |
|    | <span data-ttu-id="51ab7-142">-无颜色</span><span class="sxs-lookup"><span data-stu-id="51ab7-142">--no-color</span></span>                       | <span data-ttu-id="51ab7-143">不为着色输出。</span><span class="sxs-lookup"><span data-stu-id="51ab7-143">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="51ab7-144">-前缀输出</span><span class="sxs-lookup"><span data-stu-id="51ab7-144">--prefix-output</span></span>                  | <span data-ttu-id="51ab7-145">输出与级别的前缀。</span><span class="sxs-lookup"><span data-stu-id="51ab7-145">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="51ab7-146">若要指定 ASP.NET Core 环境，设置**ASPNETCORE_ENVIRONMENT**之前运行的环境变量。</span><span class="sxs-lookup"><span data-stu-id="51ab7-146">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="51ab7-147">命令</span><span class="sxs-lookup"><span data-stu-id="51ab7-147">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="51ab7-148">dotnet ef 数据库除去</span><span class="sxs-lookup"><span data-stu-id="51ab7-148">dotnet ef database drop</span></span>

<span data-ttu-id="51ab7-149">删除数据库。</span><span class="sxs-lookup"><span data-stu-id="51ab7-149">Drops the database.</span></span>

<span data-ttu-id="51ab7-150">选项:</span><span class="sxs-lookup"><span data-stu-id="51ab7-150">Options:</span></span>

|    |           |                                                          |
| -- | --------- | -------------------------------------------------------- |
| <span data-ttu-id="51ab7-151">-f</span><span class="sxs-lookup"><span data-stu-id="51ab7-151">-f</span></span> | <span data-ttu-id="51ab7-152">-强制</span><span class="sxs-lookup"><span data-stu-id="51ab7-152">--force</span></span>   | <span data-ttu-id="51ab7-153">不确认。</span><span class="sxs-lookup"><span data-stu-id="51ab7-153">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="51ab7-154">-试运行</span><span class="sxs-lookup"><span data-stu-id="51ab7-154">--dry-run</span></span> | <span data-ttu-id="51ab7-155">显示的数据库会被丢弃，但没有删除它。</span><span class="sxs-lookup"><span data-stu-id="51ab7-155">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="51ab7-156">dotnet ef 数据库更新</span><span class="sxs-lookup"><span data-stu-id="51ab7-156">dotnet ef database update</span></span>

<span data-ttu-id="51ab7-157">到指定的迁移更新数据库。</span><span class="sxs-lookup"><span data-stu-id="51ab7-157">Updates the database to a specified migration.</span></span>

<span data-ttu-id="51ab7-158">自变量：</span><span class="sxs-lookup"><span data-stu-id="51ab7-158">Arguments:</span></span>

|              |                                                                                              |
| ------------ | ---------------------------------------------------------------------------------------------|
| <span data-ttu-id="51ab7-159">\<迁移 ></span><span class="sxs-lookup"><span data-stu-id="51ab7-159">\<MIGRATION></span></span> | <span data-ttu-id="51ab7-160">目标迁移。</span><span class="sxs-lookup"><span data-stu-id="51ab7-160">The target migration.</span></span> <span data-ttu-id="51ab7-161">如果为 0，将恢复所有迁移。</span><span class="sxs-lookup"><span data-stu-id="51ab7-161">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="51ab7-162">默认到最后一个迁移。</span><span class="sxs-lookup"><span data-stu-id="51ab7-162">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="51ab7-163">dotnet ef dbcontext 信息</span><span class="sxs-lookup"><span data-stu-id="51ab7-163">dotnet ef dbcontext info</span></span>

<span data-ttu-id="51ab7-164">获取有关 DbContext 类型的信息。</span><span class="sxs-lookup"><span data-stu-id="51ab7-164">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="51ab7-165">dotnet ef dbcontext 列表</span><span class="sxs-lookup"><span data-stu-id="51ab7-165">dotnet ef dbcontext list</span></span>

<span data-ttu-id="51ab7-166">列出可用的 DbContext 类型。</span><span class="sxs-lookup"><span data-stu-id="51ab7-166">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="51ab7-167">dotnet ef dbcontext 基架</span><span class="sxs-lookup"><span data-stu-id="51ab7-167">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="51ab7-168">基架数据库类型 DbContext 和实体的类型。</span><span class="sxs-lookup"><span data-stu-id="51ab7-168">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="51ab7-169">自变量：</span><span class="sxs-lookup"><span data-stu-id="51ab7-169">Arguments:</span></span>

|               |                                                                     |
| ------------- | ------------------------------------------------------------------- |
| <span data-ttu-id="51ab7-170">\<连接 ></span><span class="sxs-lookup"><span data-stu-id="51ab7-170">\<CONNECTION></span></span> | <span data-ttu-id="51ab7-171">数据库的连接字符串。</span><span class="sxs-lookup"><span data-stu-id="51ab7-171">The connection string to the database.</span></span>                              |
| <span data-ttu-id="51ab7-172">\<提供程序 ></span><span class="sxs-lookup"><span data-stu-id="51ab7-172">\<PROVIDER></span></span>   | <span data-ttu-id="51ab7-173">要使用的提供程序。</span><span class="sxs-lookup"><span data-stu-id="51ab7-173">The provider to use.</span></span> <span data-ttu-id="51ab7-174">（例如，</span><span class="sxs-lookup"><span data-stu-id="51ab7-174">(E.g.</span></span> <span data-ttu-id="51ab7-175">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="51ab7-175">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="51ab7-176">选项:</span><span class="sxs-lookup"><span data-stu-id="51ab7-176">Options:</span></span>

|                 |                                         |                                                          |
| --------------- | --------------------------------------- | -------------------------------------------------------- |
| <span data-ttu-id="51ab7-177"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="51ab7-177"><nobr>-d</nobr></span></span> |       <span data-ttu-id="51ab7-178">-数据批注</span><span class="sxs-lookup"><span data-stu-id="51ab7-178">--data-annotations</span></span>                | <span data-ttu-id="51ab7-179">使用属性来配置该模型 （如果可能）。</span><span class="sxs-lookup"><span data-stu-id="51ab7-179">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="51ab7-180">如果省略，则使用仅 fluent API。</span><span class="sxs-lookup"><span data-stu-id="51ab7-180">If omitted, only the fluent API is used.</span></span> |
|       <span data-ttu-id="51ab7-181">-c</span><span class="sxs-lookup"><span data-stu-id="51ab7-181">-c</span></span>        |       <span data-ttu-id="51ab7-182">-上下文\<名称 ></span><span class="sxs-lookup"><span data-stu-id="51ab7-182">--context \<NAME></span></span>                 | <span data-ttu-id="51ab7-183">Dbcontext 名称。</span><span class="sxs-lookup"><span data-stu-id="51ab7-183">The name of the DbContext.</span></span>                               |
|       <span data-ttu-id="51ab7-184">-f</span><span class="sxs-lookup"><span data-stu-id="51ab7-184">-f</span></span>        |       <span data-ttu-id="51ab7-185">-强制</span><span class="sxs-lookup"><span data-stu-id="51ab7-185">--force</span></span>                           | <span data-ttu-id="51ab7-186">覆盖现有文件。</span><span class="sxs-lookup"><span data-stu-id="51ab7-186">Overwrite existing files.</span></span>                                |
|       <span data-ttu-id="51ab7-187">-o</span><span class="sxs-lookup"><span data-stu-id="51ab7-187">-o</span></span>        |       <span data-ttu-id="51ab7-188">-输出 dir\<路径 ></span><span class="sxs-lookup"><span data-stu-id="51ab7-188">--output-dir \<PATH></span></span>              | <span data-ttu-id="51ab7-189">要将文件放入的目录。</span><span class="sxs-lookup"><span data-stu-id="51ab7-189">The directory to put files in.</span></span> <span data-ttu-id="51ab7-190">路径是相对于项目目录。</span><span class="sxs-lookup"><span data-stu-id="51ab7-190">Paths are relative to the project directory.</span></span> |
|                 | <span data-ttu-id="51ab7-191"><nobr>-架构\<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="51ab7-191"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="51ab7-192">要生成实体类型的表架构。</span><span class="sxs-lookup"><span data-stu-id="51ab7-192">The schemas of tables to generate entity types for.</span></span>      |
|       <span data-ttu-id="51ab7-193">-t</span><span class="sxs-lookup"><span data-stu-id="51ab7-193">-t</span></span>        |       <span data-ttu-id="51ab7-194">-表\<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="51ab7-194">--table \<TABLE_NAME>...</span></span>          | <span data-ttu-id="51ab7-195">要生成实体类型的表。</span><span class="sxs-lookup"><span data-stu-id="51ab7-195">The tables to generate entity types for.</span></span>                 |
|                 |       <span data-ttu-id="51ab7-196">-使用数据库名称</span><span class="sxs-lookup"><span data-stu-id="51ab7-196">--use-database-names</span></span>              | <span data-ttu-id="51ab7-197">使用直接从数据库表和列名称。</span><span class="sxs-lookup"><span data-stu-id="51ab7-197">Use table and column names directly from the database.</span></span>   |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="51ab7-198">dotnet ef 迁移添加</span><span class="sxs-lookup"><span data-stu-id="51ab7-198">dotnet ef migrations add</span></span>

<span data-ttu-id="51ab7-199">将添加一个新迁移。</span><span class="sxs-lookup"><span data-stu-id="51ab7-199">Adds a new migration.</span></span>

<span data-ttu-id="51ab7-200">自变量：</span><span class="sxs-lookup"><span data-stu-id="51ab7-200">Arguments:</span></span>

|         |                            |
| ------- | -------------------------- |
| <span data-ttu-id="51ab7-201">\<名称 ></span><span class="sxs-lookup"><span data-stu-id="51ab7-201">\<NAME></span></span> | <span data-ttu-id="51ab7-202">迁移的名称。</span><span class="sxs-lookup"><span data-stu-id="51ab7-202">The name of the migration.</span></span> |

<span data-ttu-id="51ab7-203">选项:</span><span class="sxs-lookup"><span data-stu-id="51ab7-203">Options:</span></span>

|                 |                                   |                                                                |
| --------------- |---------------------------------- | -------------------------------------------------------------- |
| <span data-ttu-id="51ab7-204"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="51ab7-204"><nobr>-o</nobr></span></span> | <span data-ttu-id="51ab7-205"><nobr>-输出 dir\<路径 ></nobr></span><span class="sxs-lookup"><span data-stu-id="51ab7-205"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="51ab7-206">目录 （及其子命名空间） 使用。</span><span class="sxs-lookup"><span data-stu-id="51ab7-206">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="51ab7-207">路径是相对于项目目录。</span><span class="sxs-lookup"><span data-stu-id="51ab7-207">Paths are relative to the project directory.</span></span> <span data-ttu-id="51ab7-208">默认值为"迁移"。</span><span class="sxs-lookup"><span data-stu-id="51ab7-208">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="51ab7-209">dotnet ef 迁移列表</span><span class="sxs-lookup"><span data-stu-id="51ab7-209">dotnet ef migrations list</span></span>

<span data-ttu-id="51ab7-210">列出可用的迁移。</span><span class="sxs-lookup"><span data-stu-id="51ab7-210">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="51ab7-211">dotnet ef 迁移删除</span><span class="sxs-lookup"><span data-stu-id="51ab7-211">dotnet ef migrations remove</span></span>

<span data-ttu-id="51ab7-212">删除上次的迁移。</span><span class="sxs-lookup"><span data-stu-id="51ab7-212">Removes the last migration.</span></span>

<span data-ttu-id="51ab7-213">选项:</span><span class="sxs-lookup"><span data-stu-id="51ab7-213">Options:</span></span>

|    |         |                                                                       |
| -- | ------- | --------------------------------------------------------------------- |
| <span data-ttu-id="51ab7-214">-f</span><span class="sxs-lookup"><span data-stu-id="51ab7-214">-f</span></span> | <span data-ttu-id="51ab7-215">-强制</span><span class="sxs-lookup"><span data-stu-id="51ab7-215">--force</span></span> | <span data-ttu-id="51ab7-216">不检查以查看迁移是否已应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="51ab7-216">Don't check to see if the migration has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="51ab7-217">dotnet ef 迁移脚本</span><span class="sxs-lookup"><span data-stu-id="51ab7-217">dotnet ef migrations script</span></span>

<span data-ttu-id="51ab7-218">从迁移中生成的 SQL 脚本。</span><span class="sxs-lookup"><span data-stu-id="51ab7-218">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="51ab7-219">自变量：</span><span class="sxs-lookup"><span data-stu-id="51ab7-219">Arguments:</span></span>

|         |                                                               |
| ------- | ------------------------------------------------------------- |
| <span data-ttu-id="51ab7-220">\<从 ></span><span class="sxs-lookup"><span data-stu-id="51ab7-220">\<FROM></span></span> | <span data-ttu-id="51ab7-221">开始迁移。</span><span class="sxs-lookup"><span data-stu-id="51ab7-221">The starting migration.</span></span> <span data-ttu-id="51ab7-222">默认值为 0 （初始数据库）。</span><span class="sxs-lookup"><span data-stu-id="51ab7-222">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="51ab7-223">\<到 ></span><span class="sxs-lookup"><span data-stu-id="51ab7-223">\<TO></span></span>   | <span data-ttu-id="51ab7-224">结束的迁移。</span><span class="sxs-lookup"><span data-stu-id="51ab7-224">The ending migration.</span></span> <span data-ttu-id="51ab7-225">默认到最后一个迁移。</span><span class="sxs-lookup"><span data-stu-id="51ab7-225">Defaults to the last migration.</span></span>         |

<span data-ttu-id="51ab7-226">选项:</span><span class="sxs-lookup"><span data-stu-id="51ab7-226">Options:</span></span>

|    |                  |                                                                    |
| -- | ---------------- | ------------------------------------------------------------------ |
| <span data-ttu-id="51ab7-227">-o</span><span class="sxs-lookup"><span data-stu-id="51ab7-227">-o</span></span> | <span data-ttu-id="51ab7-228">-输出\<文件 ></span><span class="sxs-lookup"><span data-stu-id="51ab7-228">--output \<FILE></span></span> | <span data-ttu-id="51ab7-229">要将结果写入的文件。</span><span class="sxs-lookup"><span data-stu-id="51ab7-229">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="51ab7-230">-i</span><span class="sxs-lookup"><span data-stu-id="51ab7-230">-i</span></span> | <span data-ttu-id="51ab7-231">-幂等</span><span class="sxs-lookup"><span data-stu-id="51ab7-231">--idempotent</span></span>     | <span data-ttu-id="51ab7-232">生成可以在任何迁移的数据库使用的脚本。</span><span class="sxs-lookup"><span data-stu-id="51ab7-232">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
