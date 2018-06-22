---
title: 程序包管理器控制台 (Visual Studio)-EF 核心
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: a53455a78db4bc504c45abafdacf9a15381f608e
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812555"
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="661ff-102">EF 核心程序包管理器控制台工具</span><span class="sxs-lookup"><span data-stu-id="661ff-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="661ff-103">EF 核心程序包管理器控制台 (PMC) 工具使用的 NuGet 在 Visual Studio 内运行[程序包管理器控制台][2]。</span><span class="sxs-lookup"><span data-stu-id="661ff-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="661ff-104">这些工具同时适用于 .NET Framework 和 .NET Core 项目。</span><span class="sxs-lookup"><span data-stu-id="661ff-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="661ff-105">未使用 Visual Studio？</span><span class="sxs-lookup"><span data-stu-id="661ff-105">Not using Visual Studio?</span></span> <span data-ttu-id="661ff-106">[EF 核心命令行工具][ 1]是跨平台，且在命令提示符下运行。</span><span class="sxs-lookup"><span data-stu-id="661ff-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="661ff-107">安装工具</span><span class="sxs-lookup"><span data-stu-id="661ff-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="661ff-108">通过安装 Microsoft.EntityFrameworkCore.Tools NuGet 包安装 EF 核心程序包管理器控制台 Tools。</span><span class="sxs-lookup"><span data-stu-id="661ff-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="661ff-109">可以通过执行以下命令内的安装[程序包管理器控制台][2]。</span><span class="sxs-lookup"><span data-stu-id="661ff-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="661ff-110">如果一切运行正常，你应能够运行此命令：</span><span class="sxs-lookup"><span data-stu-id="661ff-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="661ff-111">如果你的启动项目面向.NET 标准[跨目标支持的框架][ 3]之前使用的工具。</span><span class="sxs-lookup"><span data-stu-id="661ff-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="661ff-112">如果你使用**通用 Windows**或**Xamarin**，将 EF 代码移动到标准.NET 类库和[跨目标支持的框架][ 3]之前使用的工具。</span><span class="sxs-lookup"><span data-stu-id="661ff-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="661ff-113">指定为启动项目的类库。</span><span class="sxs-lookup"><span data-stu-id="661ff-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="661ff-114">使用的工具</span><span class="sxs-lookup"><span data-stu-id="661ff-114">Using the tools</span></span>
---------------
<span data-ttu-id="661ff-115">每当调用某命令时，有两个项目涉及：</span><span class="sxs-lookup"><span data-stu-id="661ff-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="661ff-116">目标项目是在其中添加任何文件（或在某些情况下删除文件）的项目。</span><span class="sxs-lookup"><span data-stu-id="661ff-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="661ff-117">目标项目默认为**默认项目**选择在程序包管理器控制台中，但也可以通过使用指定的项目参数。</span><span class="sxs-lookup"><span data-stu-id="661ff-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="661ff-118">启动项目是执行项目代码时由工具模拟的项目。</span><span class="sxs-lookup"><span data-stu-id="661ff-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="661ff-119">它将默认为一个**设为启动项目**在解决方案资源管理器。</span><span class="sxs-lookup"><span data-stu-id="661ff-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="661ff-120">它可以还指定使用-StartupProject 参数。</span><span class="sxs-lookup"><span data-stu-id="661ff-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="661ff-121">通用参数：</span><span class="sxs-lookup"><span data-stu-id="661ff-121">Common parameters:</span></span>

|                           |                             |
|:--------------------------|:----------------------------|
| <span data-ttu-id="661ff-122">-上下文\<字符串 ></span><span class="sxs-lookup"><span data-stu-id="661ff-122">-Context \<String></span></span>        | <span data-ttu-id="661ff-123">若要使用 DbContext。</span><span class="sxs-lookup"><span data-stu-id="661ff-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="661ff-124">-项目\<字符串 ></span><span class="sxs-lookup"><span data-stu-id="661ff-124">-Project \<String></span></span>        | <span data-ttu-id="661ff-125">要使用的项目。</span><span class="sxs-lookup"><span data-stu-id="661ff-125">The project to use.</span></span>         |
| <span data-ttu-id="661ff-126">-StartupProject\<字符串 ></span><span class="sxs-lookup"><span data-stu-id="661ff-126">-StartupProject \<String></span></span> | <span data-ttu-id="661ff-127">要使用的启动项目。</span><span class="sxs-lookup"><span data-stu-id="661ff-127">The startup project to use.</span></span> |
| <span data-ttu-id="661ff-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="661ff-128">-Verbose</span></span>                  | <span data-ttu-id="661ff-129">显示详细输出。</span><span class="sxs-lookup"><span data-stu-id="661ff-129">Show verbose output.</span></span>        |

<span data-ttu-id="661ff-130">若要显示有关命令的帮助信息，请使用 PowerShell 的`Get-Help`命令。</span><span class="sxs-lookup"><span data-stu-id="661ff-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="661ff-131">上下文、 Project 和 StartupProject 参数支持选项卡扩展。</span><span class="sxs-lookup"><span data-stu-id="661ff-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="661ff-132">设置**env:ASPNETCORE_ENVIRONMENT**之前运行若要指定 ASP.NET Core 环境。</span><span class="sxs-lookup"><span data-stu-id="661ff-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="661ff-133">命令</span><span class="sxs-lookup"><span data-stu-id="661ff-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="661ff-134">添加迁移</span><span class="sxs-lookup"><span data-stu-id="661ff-134">Add-Migration</span></span>

<span data-ttu-id="661ff-135">将添加一个新迁移。</span><span class="sxs-lookup"><span data-stu-id="661ff-135">Adds a new migration.</span></span>

<span data-ttu-id="661ff-136">参数：</span><span class="sxs-lookup"><span data-stu-id="661ff-136">Parameters:</span></span>

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="661ff-137">***-名称***\<字符串 ></span><span class="sxs-lookup"><span data-stu-id="661ff-137">***-Name*** \<String></span></span>             | <span data-ttu-id="661ff-138">迁移的名称。</span><span class="sxs-lookup"><span data-stu-id="661ff-138">The name of the migration.</span></span>                                                                                       |
| <span data-ttu-id="661ff-139"><nobr>-OutputDir\<字符串 ></nobr></span><span class="sxs-lookup"><span data-stu-id="661ff-139"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="661ff-140">目录 （及其子命名空间） 使用。</span><span class="sxs-lookup"><span data-stu-id="661ff-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="661ff-141">路径是相对于项目目录。</span><span class="sxs-lookup"><span data-stu-id="661ff-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="661ff-142">默认值为"迁移"。</span><span class="sxs-lookup"><span data-stu-id="661ff-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="661ff-143">中的参数**粗体**是必需的以及在发生*斜体*是位置。</span><span class="sxs-lookup"><span data-stu-id="661ff-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="661ff-144">删除数据库</span><span class="sxs-lookup"><span data-stu-id="661ff-144">Drop-Database</span></span>

<span data-ttu-id="661ff-145">删除数据库。</span><span class="sxs-lookup"><span data-stu-id="661ff-145">Drops the database.</span></span>

<span data-ttu-id="661ff-146">参数：</span><span class="sxs-lookup"><span data-stu-id="661ff-146">Parameters:</span></span>

|         |                                                          |
|:--------|:---------------------------------------------------------|
| <span data-ttu-id="661ff-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="661ff-147">-WhatIf</span></span> | <span data-ttu-id="661ff-148">显示的数据库会被丢弃，但没有删除它。</span><span class="sxs-lookup"><span data-stu-id="661ff-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="661ff-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="661ff-149">Get-DbContext</span></span>

<span data-ttu-id="661ff-150">获取有关 DbContext 类型的信息。</span><span class="sxs-lookup"><span data-stu-id="661ff-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="661ff-151">删除迁移</span><span class="sxs-lookup"><span data-stu-id="661ff-151">Remove-Migration</span></span>

<span data-ttu-id="661ff-152">删除上次的迁移。</span><span class="sxs-lookup"><span data-stu-id="661ff-152">Removes the last migration.</span></span>

<span data-ttu-id="661ff-153">参数：</span><span class="sxs-lookup"><span data-stu-id="661ff-153">Parameters:</span></span>

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| <span data-ttu-id="661ff-154">-Force</span><span class="sxs-lookup"><span data-stu-id="661ff-154">-Force</span></span> | <span data-ttu-id="661ff-155">如果它已应用到数据库，请还原迁移。</span><span class="sxs-lookup"><span data-stu-id="661ff-155">Revert the migration if it has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="661ff-156">基架 DbContext</span><span class="sxs-lookup"><span data-stu-id="661ff-156">Scaffold-DbContext</span></span>

<span data-ttu-id="661ff-157">基架数据库类型 DbContext 和实体的类型。</span><span class="sxs-lookup"><span data-stu-id="661ff-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="661ff-158">参数：</span><span class="sxs-lookup"><span data-stu-id="661ff-158">Parameters:</span></span>

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="661ff-159"><nobr>***连接***\<字符串 ></nobr></span><span class="sxs-lookup"><span data-stu-id="661ff-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="661ff-160">数据库的连接字符串。</span><span class="sxs-lookup"><span data-stu-id="661ff-160">The connection string to the database.</span></span>                                                           |
| <span data-ttu-id="661ff-161">***-Provider*** \<String></span><span class="sxs-lookup"><span data-stu-id="661ff-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="661ff-162">要使用的提供程序。</span><span class="sxs-lookup"><span data-stu-id="661ff-162">The provider to use.</span></span> <span data-ttu-id="661ff-163">（例如，</span><span class="sxs-lookup"><span data-stu-id="661ff-163">(E.g.</span></span> <span data-ttu-id="661ff-164">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="661ff-164">Microsoft.EntityFrameworkCore.SqlServer)</span></span>                              |
| <span data-ttu-id="661ff-165">-OutputDir\<字符串 ></span><span class="sxs-lookup"><span data-stu-id="661ff-165">-OutputDir \<String></span></span>                     | <span data-ttu-id="661ff-166">要将文件放入的目录。</span><span class="sxs-lookup"><span data-stu-id="661ff-166">The directory to put files in.</span></span> <span data-ttu-id="661ff-167">路径是相对于项目目录。</span><span class="sxs-lookup"><span data-stu-id="661ff-167">Paths are relative to the project directory.</span></span>                      |
| <span data-ttu-id="661ff-168">-ContextDir\<字符串 ></span><span class="sxs-lookup"><span data-stu-id="661ff-168">-ContextDir \<String></span></span>                    | <span data-ttu-id="661ff-169">要将 DbContext 文件放入的目录。</span><span class="sxs-lookup"><span data-stu-id="661ff-169">The directory to put DbContext file in.</span></span> <span data-ttu-id="661ff-170">路径是相对于项目目录。</span><span class="sxs-lookup"><span data-stu-id="661ff-170">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="661ff-171">-上下文\<字符串 ></span><span class="sxs-lookup"><span data-stu-id="661ff-171">-Context \<String></span></span>                       | <span data-ttu-id="661ff-172">若要生成的 dbcontext 名称。</span><span class="sxs-lookup"><span data-stu-id="661ff-172">The name of the DbContext to generate.</span></span>                                                           |
| <span data-ttu-id="661ff-173">-架构\<String [] ></span><span class="sxs-lookup"><span data-stu-id="661ff-173">-Schemas \<String[]></span></span>                     | <span data-ttu-id="661ff-174">要生成实体类型的表架构。</span><span class="sxs-lookup"><span data-stu-id="661ff-174">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="661ff-175">-表\<String [] ></span><span class="sxs-lookup"><span data-stu-id="661ff-175">-Tables \<String[]></span></span>                      | <span data-ttu-id="661ff-176">要生成实体类型的表。</span><span class="sxs-lookup"><span data-stu-id="661ff-176">The tables to generate entity types for.</span></span>                                                         |
| <span data-ttu-id="661ff-177">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="661ff-177">-DataAnnotations</span></span>                         | <span data-ttu-id="661ff-178">使用属性来配置该模型 （如果可能）。</span><span class="sxs-lookup"><span data-stu-id="661ff-178">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="661ff-179">如果省略，则使用仅 fluent API。</span><span class="sxs-lookup"><span data-stu-id="661ff-179">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="661ff-180">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="661ff-180">-UseDatabaseNames</span></span>                        | <span data-ttu-id="661ff-181">使用直接从数据库表和列名称。</span><span class="sxs-lookup"><span data-stu-id="661ff-181">Use table and column names directly from the database.</span></span>                                           |
| <span data-ttu-id="661ff-182">-Force</span><span class="sxs-lookup"><span data-stu-id="661ff-182">-Force</span></span>                                   | <span data-ttu-id="661ff-183">覆盖现有文件。</span><span class="sxs-lookup"><span data-stu-id="661ff-183">Overwrite existing files.</span></span>                                                                        |

### <a name="script-migration"></a><span data-ttu-id="661ff-184">Script-Migration</span><span class="sxs-lookup"><span data-stu-id="661ff-184">Script-Migration</span></span>

<span data-ttu-id="661ff-185">从迁移中生成的 SQL 脚本。</span><span class="sxs-lookup"><span data-stu-id="661ff-185">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="661ff-186">参数：</span><span class="sxs-lookup"><span data-stu-id="661ff-186">Parameters:</span></span>

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| <span data-ttu-id="661ff-187">*-从*\<字符串 ></span><span class="sxs-lookup"><span data-stu-id="661ff-187">*-From* \<String></span></span> | <span data-ttu-id="661ff-188">开始迁移。</span><span class="sxs-lookup"><span data-stu-id="661ff-188">The starting migration.</span></span> <span data-ttu-id="661ff-189">默认值为 0 （初始数据库）。</span><span class="sxs-lookup"><span data-stu-id="661ff-189">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="661ff-190">*到*\<字符串 ></span><span class="sxs-lookup"><span data-stu-id="661ff-190">*-To* \<String></span></span>   | <span data-ttu-id="661ff-191">结束的迁移。</span><span class="sxs-lookup"><span data-stu-id="661ff-191">The ending migration.</span></span> <span data-ttu-id="661ff-192">默认到最后一个迁移。</span><span class="sxs-lookup"><span data-stu-id="661ff-192">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="661ff-193">幂等性</span><span class="sxs-lookup"><span data-stu-id="661ff-193">-Idempotent</span></span>       | <span data-ttu-id="661ff-194">生成可以在任何迁移的数据库使用的脚本。</span><span class="sxs-lookup"><span data-stu-id="661ff-194">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="661ff-195">-输出\<字符串 ></span><span class="sxs-lookup"><span data-stu-id="661ff-195">-Output \<String></span></span> | <span data-ttu-id="661ff-196">要将结果写入的文件。</span><span class="sxs-lookup"><span data-stu-id="661ff-196">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="661ff-197">收件人、 从，和输出参数支持选项卡扩展。</span><span class="sxs-lookup"><span data-stu-id="661ff-197">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="661ff-198">更新数据库</span><span class="sxs-lookup"><span data-stu-id="661ff-198">Update-Database</span></span>

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <span data-ttu-id="661ff-199"><nobr>*迁移*\<字符串 ></nobr></span><span class="sxs-lookup"><span data-stu-id="661ff-199"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="661ff-200">目标迁移。</span><span class="sxs-lookup"><span data-stu-id="661ff-200">The target migration.</span></span> <span data-ttu-id="661ff-201">如果为"0"，将恢复所有迁移。</span><span class="sxs-lookup"><span data-stu-id="661ff-201">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="661ff-202">默认到最后一个迁移。</span><span class="sxs-lookup"><span data-stu-id="661ff-202">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="661ff-203">迁移参数支持选项卡扩展。</span><span class="sxs-lookup"><span data-stu-id="661ff-203">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
