---
title: "包管理器控制台 (Visual Studio) - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: b4ecb27edf94e7b9ad6c7fe65a891dcbf1593309
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="7be5b-102">EF Core 包管理器控制台工具</span><span class="sxs-lookup"><span data-stu-id="7be5b-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="7be5b-103">EF Core 包管理器控制台 (PMC) 工具使用 NuGet 的[包管理器控制台][2]在 Visual Studio 中运行。</span><span class="sxs-lookup"><span data-stu-id="7be5b-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="7be5b-104">这些工具处理的.NET Framework 和.NET 核心项目。</span><span class="sxs-lookup"><span data-stu-id="7be5b-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="7be5b-105">未使用 Visual Studio？</span><span class="sxs-lookup"><span data-stu-id="7be5b-105">Not using Visual Studio?</span></span> <span data-ttu-id="7be5b-106">[EF Core 命令行工具][1]是跨平台的工具，可在命令提示符下运行。</span><span class="sxs-lookup"><span data-stu-id="7be5b-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="7be5b-107">安装工具</span><span class="sxs-lookup"><span data-stu-id="7be5b-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="7be5b-108">通过安装 Microsoft.EntityFrameworkCore.Tools NuGet 包安装 EF 核心程序包管理器控制台 Tools。</span><span class="sxs-lookup"><span data-stu-id="7be5b-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="7be5b-109">可以通过执行以下命令内的安装[程序包管理器控制台][2]。</span><span class="sxs-lookup"><span data-stu-id="7be5b-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="7be5b-110">如果一切运行正常，你应能够运行此命令：</span><span class="sxs-lookup"><span data-stu-id="7be5b-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="7be5b-111">如果你的启动项目面向.NET 标准[跨目标支持的框架][ 3]之前使用的工具。</span><span class="sxs-lookup"><span data-stu-id="7be5b-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7be5b-112">如果你使用**通用 Windows**或**Xamarin**，将 EF 代码移动到标准.NET 类库和[跨目标支持的框架][ 3]之前使用的工具。</span><span class="sxs-lookup"><span data-stu-id="7be5b-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="7be5b-113">指定为启动项目的类库。</span><span class="sxs-lookup"><span data-stu-id="7be5b-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="7be5b-114">使用的工具</span><span class="sxs-lookup"><span data-stu-id="7be5b-114">Using the tools</span></span>
---------------
<span data-ttu-id="7be5b-115">每当调用某命令时，有两个项目涉及：</span><span class="sxs-lookup"><span data-stu-id="7be5b-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="7be5b-116">目标项目将添加的任何文件的位置 （或在某些情况下删除）。</span><span class="sxs-lookup"><span data-stu-id="7be5b-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="7be5b-117">目标项目默认为**默认项目**选择在程序包管理器控制台中，但也可以通过使用指定的项目参数。</span><span class="sxs-lookup"><span data-stu-id="7be5b-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="7be5b-118">启动项目是仿真的工具，执行你的项目代码时。</span><span class="sxs-lookup"><span data-stu-id="7be5b-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="7be5b-119">它将默认为一个**设为启动项目**在解决方案资源管理器。</span><span class="sxs-lookup"><span data-stu-id="7be5b-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="7be5b-120">它可以还指定使用-StartupProject 参数。</span><span class="sxs-lookup"><span data-stu-id="7be5b-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="7be5b-121">通用参数：</span><span class="sxs-lookup"><span data-stu-id="7be5b-121">Common parameters:</span></span>

|                           |                             |
| ------------------------- | --------------------------- |
| <span data-ttu-id="7be5b-122">-Context \<String></span><span class="sxs-lookup"><span data-stu-id="7be5b-122">-Context \<String></span></span>        | <span data-ttu-id="7be5b-123">要使用的 DbContext。</span><span class="sxs-lookup"><span data-stu-id="7be5b-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="7be5b-124">-Project \<String></span><span class="sxs-lookup"><span data-stu-id="7be5b-124">-Project \<String></span></span>        | <span data-ttu-id="7be5b-125">要使用的项目。</span><span class="sxs-lookup"><span data-stu-id="7be5b-125">The project to use.</span></span>         |
| <span data-ttu-id="7be5b-126">-StartupProject \<String></span><span class="sxs-lookup"><span data-stu-id="7be5b-126">-StartupProject \<String></span></span> | <span data-ttu-id="7be5b-127">要使用的启动项目。</span><span class="sxs-lookup"><span data-stu-id="7be5b-127">The startup project to use.</span></span> |
| <span data-ttu-id="7be5b-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="7be5b-128">-Verbose</span></span>                  | <span data-ttu-id="7be5b-129">显示详细输出。</span><span class="sxs-lookup"><span data-stu-id="7be5b-129">Show verbose output.</span></span>        |

<span data-ttu-id="7be5b-130">若要显示有关命令的帮助信息，请使用 PowerShell 的`Get-Help`命令。</span><span class="sxs-lookup"><span data-stu-id="7be5b-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="7be5b-131">Context、Project 和 StartupProject 参数支持选项卡扩展。</span><span class="sxs-lookup"><span data-stu-id="7be5b-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="7be5b-132">设置**env:ASPNETCORE_ENVIRONMENT**之前运行若要指定 ASP.NET Core 环境。</span><span class="sxs-lookup"><span data-stu-id="7be5b-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="7be5b-133">命令</span><span class="sxs-lookup"><span data-stu-id="7be5b-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="7be5b-134">Add-Migration</span><span class="sxs-lookup"><span data-stu-id="7be5b-134">Add-Migration</span></span>

<span data-ttu-id="7be5b-135">将添加一个新迁移。</span><span class="sxs-lookup"><span data-stu-id="7be5b-135">Adds a new migration.</span></span>

<span data-ttu-id="7be5b-136">参数：</span><span class="sxs-lookup"><span data-stu-id="7be5b-136">Parameters:</span></span>

|                                    |                                                                                 |
| ---------------------------------- | ------------------------------------------------------------------------------- |
| <span data-ttu-id="7be5b-137">***-Name***\<String></span><span class="sxs-lookup"><span data-stu-id="7be5b-137">***-Name*** \<String></span></span>              | <span data-ttu-id="7be5b-138">迁移的名称。</span><span class="sxs-lookup"><span data-stu-id="7be5b-138">The name of the migration.</span></span>                                                      |
| <span data-ttu-id="7be5b-139"><nobr>-OutputDir\<String></nobr></span><span class="sxs-lookup"><span data-stu-id="7be5b-139"><nobr>-OutputDir \<String></nobr></span></span>  | <span data-ttu-id="7be5b-140">要使用的目录 （及其子命名空间）。</span><span class="sxs-lookup"><span data-stu-id="7be5b-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="7be5b-141">路径是相对于项目目录的路径。</span><span class="sxs-lookup"><span data-stu-id="7be5b-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="7be5b-142">默认值为“Migrations”。</span><span class="sxs-lookup"><span data-stu-id="7be5b-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="7be5b-143">**粗体**参数是必需的，斜体参数取决于位置。</span><span class="sxs-lookup"><span data-stu-id="7be5b-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="7be5b-144">Drop-Database</span><span class="sxs-lookup"><span data-stu-id="7be5b-144">Drop-Database</span></span>

<span data-ttu-id="7be5b-145">删除数据库。</span><span class="sxs-lookup"><span data-stu-id="7be5b-145">Drops the database.</span></span>

<span data-ttu-id="7be5b-146">参数：</span><span class="sxs-lookup"><span data-stu-id="7be5b-146">Parameters:</span></span>

|          |                                                          |
| -------- | -------------------------------------------------------- |
| <span data-ttu-id="7be5b-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="7be5b-147">-WhatIf</span></span>  | <span data-ttu-id="7be5b-148">显示的数据库会被丢弃，但没有删除它。</span><span class="sxs-lookup"><span data-stu-id="7be5b-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="7be5b-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="7be5b-149">Get-DbContext</span></span>

<span data-ttu-id="7be5b-150">获取有关 DbContext 类型的信息。</span><span class="sxs-lookup"><span data-stu-id="7be5b-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="7be5b-151">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="7be5b-151">Remove-Migration</span></span>

<span data-ttu-id="7be5b-152">删除上次的迁移。</span><span class="sxs-lookup"><span data-stu-id="7be5b-152">Removes the last migration.</span></span>

<span data-ttu-id="7be5b-153">参数：</span><span class="sxs-lookup"><span data-stu-id="7be5b-153">Parameters:</span></span>

|        |                                                                       |
| ------ | --------------------------------------------------------------------- |
| <span data-ttu-id="7be5b-154">-Force</span><span class="sxs-lookup"><span data-stu-id="7be5b-154">-Force</span></span> | <span data-ttu-id="7be5b-155">不检查以查看迁移是否已应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="7be5b-155">Don't check to see if the migration has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="7be5b-156">Scaffold-DbContext</span><span class="sxs-lookup"><span data-stu-id="7be5b-156">Scaffold-DbContext</span></span>

<span data-ttu-id="7be5b-157">基架数据库类型 DbContext 和实体的类型。</span><span class="sxs-lookup"><span data-stu-id="7be5b-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="7be5b-158">参数：</span><span class="sxs-lookup"><span data-stu-id="7be5b-158">Parameters:</span></span>

|                                          |                                                                           |
| ---------------------------------------- | ------------------------------------------------------------------------- |
| <span data-ttu-id="7be5b-159"><nobr>***-Connection***\<String></nobr></span><span class="sxs-lookup"><span data-stu-id="7be5b-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="7be5b-160">数据库的连接字符串。</span><span class="sxs-lookup"><span data-stu-id="7be5b-160">The connection string to the database.</span></span>                                    |
| <span data-ttu-id="7be5b-161">***-Provider***\<String></span><span class="sxs-lookup"><span data-stu-id="7be5b-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="7be5b-162">要使用的提供程序。</span><span class="sxs-lookup"><span data-stu-id="7be5b-162">The provider to use.</span></span> <span data-ttu-id="7be5b-163">（例如，</span><span class="sxs-lookup"><span data-stu-id="7be5b-163">(E.g.</span></span> <span data-ttu-id="7be5b-164">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="7be5b-164">Microsoft.EntityFrameworkCore.SqlServer)</span></span>       |
| <span data-ttu-id="7be5b-165">-OutputDir\<String></span><span class="sxs-lookup"><span data-stu-id="7be5b-165">-OutputDir \<String></span></span>                     | <span data-ttu-id="7be5b-166">要将文件放入到其中的目录。</span><span class="sxs-lookup"><span data-stu-id="7be5b-166">The directory to put files in.</span></span> <span data-ttu-id="7be5b-167">路径是相对于项目目录的路径。</span><span class="sxs-lookup"><span data-stu-id="7be5b-167">Paths are relative to the project directory.</span></span> |
| <span data-ttu-id="7be5b-168">-Context \<String></span><span class="sxs-lookup"><span data-stu-id="7be5b-168">-Context \<String></span></span>                       | <span data-ttu-id="7be5b-169">要生成的 DbContext 的名称。</span><span class="sxs-lookup"><span data-stu-id="7be5b-169">The name of the DbContext to generate.</span></span>                                    |
| <span data-ttu-id="7be5b-170">-Schemas\<String [] ></span><span class="sxs-lookup"><span data-stu-id="7be5b-170">-Schemas \<String[]></span></span>                     | <span data-ttu-id="7be5b-171">要为其生成实体类型的表的架构。</span><span class="sxs-lookup"><span data-stu-id="7be5b-171">The schemas of tables to generate entity types for.</span></span>                       |
| <span data-ttu-id="7be5b-172">-Tables\<String [] ></span><span class="sxs-lookup"><span data-stu-id="7be5b-172">-Tables \<String[]></span></span>                      | <span data-ttu-id="7be5b-173">要为其生成实体类型的表。</span><span class="sxs-lookup"><span data-stu-id="7be5b-173">The tables to generate entity types for.</span></span>                                  |
| <span data-ttu-id="7be5b-174">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="7be5b-174">-DataAnnotations</span></span>                         | <span data-ttu-id="7be5b-175">使用属性来配置该模型 （如果可能）。</span><span class="sxs-lookup"><span data-stu-id="7be5b-175">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="7be5b-176">如果省略，则使用仅 fluent API。</span><span class="sxs-lookup"><span data-stu-id="7be5b-176">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="7be5b-177">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="7be5b-177">-UseDatabaseNames</span></span>                        | <span data-ttu-id="7be5b-178">使用直接从数据库表和列名称。</span><span class="sxs-lookup"><span data-stu-id="7be5b-178">Use table and column names directly from the database.</span></span>                    |
| <span data-ttu-id="7be5b-179">-Force</span><span class="sxs-lookup"><span data-stu-id="7be5b-179">-Force</span></span>                                   | <span data-ttu-id="7be5b-180">覆盖现有文件。</span><span class="sxs-lookup"><span data-stu-id="7be5b-180">Overwrite existing files.</span></span>                                                 |

### <a name="script-migration"></a><span data-ttu-id="7be5b-181">Script-Migration</span><span class="sxs-lookup"><span data-stu-id="7be5b-181">Script-Migration</span></span>

<span data-ttu-id="7be5b-182">从迁移中生成的 SQL 脚本。</span><span class="sxs-lookup"><span data-stu-id="7be5b-182">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="7be5b-183">参数：</span><span class="sxs-lookup"><span data-stu-id="7be5b-183">Parameters:</span></span>

|                   |                                                                    |
| ----------------- | ------------------------------------------------------------------ |
| <span data-ttu-id="7be5b-184">*-From*\<String></span><span class="sxs-lookup"><span data-stu-id="7be5b-184">*-From* \<String></span></span> | <span data-ttu-id="7be5b-185">初始迁移。</span><span class="sxs-lookup"><span data-stu-id="7be5b-185">The starting migration.</span></span> <span data-ttu-id="7be5b-186">默认值为 0（初始数据库）。</span><span class="sxs-lookup"><span data-stu-id="7be5b-186">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="7be5b-187">*-To*\<String></span><span class="sxs-lookup"><span data-stu-id="7be5b-187">*-To* \<String></span></span>   | <span data-ttu-id="7be5b-188">结束迁移。</span><span class="sxs-lookup"><span data-stu-id="7be5b-188">The ending migration.</span></span> <span data-ttu-id="7be5b-189">默认为最后一次迁移。</span><span class="sxs-lookup"><span data-stu-id="7be5b-189">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="7be5b-190">-Idempotent</span><span class="sxs-lookup"><span data-stu-id="7be5b-190">-Idempotent</span></span>       | <span data-ttu-id="7be5b-191">生成在进行任何迁移时都可以在数据库上使用的脚本。</span><span class="sxs-lookup"><span data-stu-id="7be5b-191">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="7be5b-192">-Output\<String></span><span class="sxs-lookup"><span data-stu-id="7be5b-192">-Output \<String></span></span> | <span data-ttu-id="7be5b-193">要将结果写入到其中的文件。</span><span class="sxs-lookup"><span data-stu-id="7be5b-193">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="7be5b-194">To、From 和 Output 参数支持选项卡扩展。</span><span class="sxs-lookup"><span data-stu-id="7be5b-194">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="7be5b-195">Update-Database</span><span class="sxs-lookup"><span data-stu-id="7be5b-195">Update-Database</span></span>

|                                     |                                                                                |
| ----------------------------------- | ------------------------------------------------------------------------------ |
| <span data-ttu-id="7be5b-196"><nobr>*-Migration*\<String></nobr></span><span class="sxs-lookup"><span data-stu-id="7be5b-196"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="7be5b-197">目标迁移。</span><span class="sxs-lookup"><span data-stu-id="7be5b-197">The target migration.</span></span> <span data-ttu-id="7be5b-198">如果为 "0"，将恢复所有迁移。</span><span class="sxs-lookup"><span data-stu-id="7be5b-198">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="7be5b-199">默认为最后一次迁移。</span><span class="sxs-lookup"><span data-stu-id="7be5b-199">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="7be5b-200">Migration 参数支持选项卡扩展。</span><span class="sxs-lookup"><span data-stu-id="7be5b-200">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
