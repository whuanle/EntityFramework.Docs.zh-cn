---
title: 程序包管理器控制台 (Visual Studio)-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 0799b0cb7c5d837fdbb7a4af510a9a4d9d34ec1a
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949033"
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="ab921-102">EF Core 程序包管理器控制台工具</span><span class="sxs-lookup"><span data-stu-id="ab921-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="ab921-103">EF Core 程序包管理器控制台 (PMC) 工具使用的 NuGet 在 Visual Studio 内运行[程序包管理器控制台][2]。</span><span class="sxs-lookup"><span data-stu-id="ab921-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="ab921-104">这些工具同时适用于 .NET Framework 和 .NET Core 项目。</span><span class="sxs-lookup"><span data-stu-id="ab921-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="ab921-105">未使用 Visual Studio？</span><span class="sxs-lookup"><span data-stu-id="ab921-105">Not using Visual Studio?</span></span> <span data-ttu-id="ab921-106">[EF Core 命令行工具][ 1]是跨平台，且在命令提示符下运行。</span><span class="sxs-lookup"><span data-stu-id="ab921-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="ab921-107">安装工具</span><span class="sxs-lookup"><span data-stu-id="ab921-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="ab921-108">通过安装 Microsoft.EntityFrameworkCore.Tools NuGet 包安装 EF Core 程序包管理器控制台 Tools。</span><span class="sxs-lookup"><span data-stu-id="ab921-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="ab921-109">可以通过执行以下命令安装它[程序包管理器控制台][2]。</span><span class="sxs-lookup"><span data-stu-id="ab921-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="ab921-110">如果一切运行正常，您应能够运行此命令：</span><span class="sxs-lookup"><span data-stu-id="ab921-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="ab921-111">如果你的启动项目面向.NET Standard[跨目标的受支持的框架][ 3]之前使用的工具。</span><span class="sxs-lookup"><span data-stu-id="ab921-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ab921-112">如果您使用的**通用 Windows**或**Xamarin**，将在 EF 代码移动到.NET Standard 类库和[交叉定向受支持的框架][ 3]之前使用的工具。</span><span class="sxs-lookup"><span data-stu-id="ab921-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="ab921-113">指定为启动项目的类库。</span><span class="sxs-lookup"><span data-stu-id="ab921-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="ab921-114">使用的工具</span><span class="sxs-lookup"><span data-stu-id="ab921-114">Using the tools</span></span>
---------------
<span data-ttu-id="ab921-115">每当调用命令时，有两个项目所涉及：</span><span class="sxs-lookup"><span data-stu-id="ab921-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="ab921-116">目标项目是在其中添加任何文件（或在某些情况下删除文件）的项目。</span><span class="sxs-lookup"><span data-stu-id="ab921-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="ab921-117">目标项目的默认值为**默认项目**选择在包管理器控制台中，但也可以通过使用指定的项目参数。</span><span class="sxs-lookup"><span data-stu-id="ab921-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="ab921-118">启动项目是执行项目代码时由工具模拟的项目。</span><span class="sxs-lookup"><span data-stu-id="ab921-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="ab921-119">它将默认为一个**设为启动项目**在解决方案资源管理器。</span><span class="sxs-lookup"><span data-stu-id="ab921-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="ab921-120">它可以还使用指定的 StartupProject 参数。</span><span class="sxs-lookup"><span data-stu-id="ab921-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="ab921-121">常见参数：</span><span class="sxs-lookup"><span data-stu-id="ab921-121">Common parameters:</span></span>

|                           |                             |
|:--------------------------|:----------------------------|
| <span data-ttu-id="ab921-122">上下文\<字符串 ></span><span class="sxs-lookup"><span data-stu-id="ab921-122">-Context \<String></span></span>        | <span data-ttu-id="ab921-123">若要使用 DbContext。</span><span class="sxs-lookup"><span data-stu-id="ab921-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="ab921-124">-项目\<字符串 ></span><span class="sxs-lookup"><span data-stu-id="ab921-124">-Project \<String></span></span>        | <span data-ttu-id="ab921-125">要使用的项目。</span><span class="sxs-lookup"><span data-stu-id="ab921-125">The project to use.</span></span>         |
| <span data-ttu-id="ab921-126">-StartupProject\<字符串 ></span><span class="sxs-lookup"><span data-stu-id="ab921-126">-StartupProject \<String></span></span> | <span data-ttu-id="ab921-127">若要使用的启动项目。</span><span class="sxs-lookup"><span data-stu-id="ab921-127">The startup project to use.</span></span> |
| <span data-ttu-id="ab921-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="ab921-128">-Verbose</span></span>                  | <span data-ttu-id="ab921-129">显示详细输出。</span><span class="sxs-lookup"><span data-stu-id="ab921-129">Show verbose output.</span></span>        |

<span data-ttu-id="ab921-130">若要显示有关命令的帮助信息，请使用 PowerShell 的`Get-Help`命令。</span><span class="sxs-lookup"><span data-stu-id="ab921-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="ab921-131">上下文、 项目和 StartupProject 参数支持选项卡扩展。</span><span class="sxs-lookup"><span data-stu-id="ab921-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="ab921-132">设置**env:ASPNETCORE_ENVIRONMENT**之前运行若要指定 ASP.NET Core 环境。</span><span class="sxs-lookup"><span data-stu-id="ab921-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="ab921-133">命令</span><span class="sxs-lookup"><span data-stu-id="ab921-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="ab921-134">添加迁移</span><span class="sxs-lookup"><span data-stu-id="ab921-134">Add-Migration</span></span>

<span data-ttu-id="ab921-135">添加新的迁移。</span><span class="sxs-lookup"><span data-stu-id="ab921-135">Adds a new migration.</span></span>

<span data-ttu-id="ab921-136">参数：</span><span class="sxs-lookup"><span data-stu-id="ab921-136">Parameters:</span></span>

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="ab921-137">***-Name*** \<字符串 ></span><span class="sxs-lookup"><span data-stu-id="ab921-137">***-Name*** \<String></span></span>             | <span data-ttu-id="ab921-138">迁移的名称。</span><span class="sxs-lookup"><span data-stu-id="ab921-138">The name of the migration.</span></span>                                                                                       |
| <span data-ttu-id="ab921-139"><nobr>-OutputDir\<字符串 ></nobr></span><span class="sxs-lookup"><span data-stu-id="ab921-139"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="ab921-140">目录 （及其子命名空间） 来使用。</span><span class="sxs-lookup"><span data-stu-id="ab921-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="ab921-141">路径是相对于项目目录。</span><span class="sxs-lookup"><span data-stu-id="ab921-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="ab921-142">默认值为"迁移"。</span><span class="sxs-lookup"><span data-stu-id="ab921-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="ab921-143">中的参数**粗体**是必需的和中*斜体*是位置。</span><span class="sxs-lookup"><span data-stu-id="ab921-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="ab921-144">删除数据库</span><span class="sxs-lookup"><span data-stu-id="ab921-144">Drop-Database</span></span>

<span data-ttu-id="ab921-145">删除数据库。</span><span class="sxs-lookup"><span data-stu-id="ab921-145">Drops the database.</span></span>

<span data-ttu-id="ab921-146">参数：</span><span class="sxs-lookup"><span data-stu-id="ab921-146">Parameters:</span></span>

|         |                                                          |
|:--------|:---------------------------------------------------------|
| <span data-ttu-id="ab921-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="ab921-147">-WhatIf</span></span> | <span data-ttu-id="ab921-148">显示哪个数据库会被丢弃，但没有删除它。</span><span class="sxs-lookup"><span data-stu-id="ab921-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="ab921-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="ab921-149">Get-DbContext</span></span>

<span data-ttu-id="ab921-150">获取有关 DbContext 类型的信息。</span><span class="sxs-lookup"><span data-stu-id="ab921-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="ab921-151">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="ab921-151">Remove-Migration</span></span>

<span data-ttu-id="ab921-152">删除最后一个迁移。</span><span class="sxs-lookup"><span data-stu-id="ab921-152">Removes the last migration.</span></span>

<span data-ttu-id="ab921-153">参数：</span><span class="sxs-lookup"><span data-stu-id="ab921-153">Parameters:</span></span>

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| <span data-ttu-id="ab921-154">-Force</span><span class="sxs-lookup"><span data-stu-id="ab921-154">-Force</span></span> | <span data-ttu-id="ab921-155">如果它已应用到数据库，请还原迁移。</span><span class="sxs-lookup"><span data-stu-id="ab921-155">Revert the migration if it has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="ab921-156">DbContext 基架</span><span class="sxs-lookup"><span data-stu-id="ab921-156">Scaffold-DbContext</span></span>

<span data-ttu-id="ab921-157">搭建基架以数据库的 DbContext 和实体类型。</span><span class="sxs-lookup"><span data-stu-id="ab921-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="ab921-158">参数：</span><span class="sxs-lookup"><span data-stu-id="ab921-158">Parameters:</span></span>

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="ab921-159"><nobr>***-连接***\<字符串 ></nobr></span><span class="sxs-lookup"><span data-stu-id="ab921-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="ab921-160">数据库的连接字符串。</span><span class="sxs-lookup"><span data-stu-id="ab921-160">The connection string to the database.</span></span>                                                           |
| <span data-ttu-id="ab921-161">***-Provider*** \<String></span><span class="sxs-lookup"><span data-stu-id="ab921-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="ab921-162">要使用的提供程序。</span><span class="sxs-lookup"><span data-stu-id="ab921-162">The provider to use.</span></span> <span data-ttu-id="ab921-163">（例如 Microsoft.EntityFrameworkCore.SqlServer）</span><span class="sxs-lookup"><span data-stu-id="ab921-163">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span>                      |
| <span data-ttu-id="ab921-164">-OutputDir\<字符串 ></span><span class="sxs-lookup"><span data-stu-id="ab921-164">-OutputDir \<String></span></span>                     | <span data-ttu-id="ab921-165">要将文件放入的目录。</span><span class="sxs-lookup"><span data-stu-id="ab921-165">The directory to put files in.</span></span> <span data-ttu-id="ab921-166">路径是相对于项目目录。</span><span class="sxs-lookup"><span data-stu-id="ab921-166">Paths are relative to the project directory.</span></span>                      |
| <span data-ttu-id="ab921-167">-ContextDir\<字符串 ></span><span class="sxs-lookup"><span data-stu-id="ab921-167">-ContextDir \<String></span></span>                    | <span data-ttu-id="ab921-168">要将 DbContext 文件放入的目录。</span><span class="sxs-lookup"><span data-stu-id="ab921-168">The directory to put DbContext file in.</span></span> <span data-ttu-id="ab921-169">路径是相对于项目目录。</span><span class="sxs-lookup"><span data-stu-id="ab921-169">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="ab921-170">上下文\<字符串 ></span><span class="sxs-lookup"><span data-stu-id="ab921-170">-Context \<String></span></span>                       | <span data-ttu-id="ab921-171">若要生成的 dbcontext 名称。</span><span class="sxs-lookup"><span data-stu-id="ab921-171">The name of the DbContext to generate.</span></span>                                                           |
| <span data-ttu-id="ab921-172">-架构\<String [] ></span><span class="sxs-lookup"><span data-stu-id="ab921-172">-Schemas \<String[]></span></span>                     | <span data-ttu-id="ab921-173">要生成的实体类型的表架构。</span><span class="sxs-lookup"><span data-stu-id="ab921-173">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="ab921-174">-表\<String [] ></span><span class="sxs-lookup"><span data-stu-id="ab921-174">-Tables \<String[]></span></span>                      | <span data-ttu-id="ab921-175">要生成的实体类型的表。</span><span class="sxs-lookup"><span data-stu-id="ab921-175">The tables to generate entity types for.</span></span>                                                         |
| <span data-ttu-id="ab921-176">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="ab921-176">-DataAnnotations</span></span>                         | <span data-ttu-id="ab921-177">特性用于将模型配置 （如果可能）。</span><span class="sxs-lookup"><span data-stu-id="ab921-177">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="ab921-178">如果省略，则使用仅 fluent API。</span><span class="sxs-lookup"><span data-stu-id="ab921-178">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="ab921-179">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="ab921-179">-UseDatabaseNames</span></span>                        | <span data-ttu-id="ab921-180">使用直接从数据库表和列名称。</span><span class="sxs-lookup"><span data-stu-id="ab921-180">Use table and column names directly from the database.</span></span>                                           |
| <span data-ttu-id="ab921-181">-Force</span><span class="sxs-lookup"><span data-stu-id="ab921-181">-Force</span></span>                                   | <span data-ttu-id="ab921-182">覆盖现有文件。</span><span class="sxs-lookup"><span data-stu-id="ab921-182">Overwrite existing files.</span></span>                                                                        |

### <a name="script-migration"></a><span data-ttu-id="ab921-183">Script-Migration</span><span class="sxs-lookup"><span data-stu-id="ab921-183">Script-Migration</span></span>

<span data-ttu-id="ab921-184">从迁移中生成的 SQL 脚本。</span><span class="sxs-lookup"><span data-stu-id="ab921-184">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="ab921-185">参数：</span><span class="sxs-lookup"><span data-stu-id="ab921-185">Parameters:</span></span>

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| <span data-ttu-id="ab921-186">*-从*\<字符串 ></span><span class="sxs-lookup"><span data-stu-id="ab921-186">*-From* \<String></span></span> | <span data-ttu-id="ab921-187">开始迁移。</span><span class="sxs-lookup"><span data-stu-id="ab921-187">The starting migration.</span></span> <span data-ttu-id="ab921-188">默认值为 0 （初始数据库）。</span><span class="sxs-lookup"><span data-stu-id="ab921-188">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="ab921-189">*-To* \<字符串 ></span><span class="sxs-lookup"><span data-stu-id="ab921-189">*-To* \<String></span></span>   | <span data-ttu-id="ab921-190">结束的迁移。</span><span class="sxs-lookup"><span data-stu-id="ab921-190">The ending migration.</span></span> <span data-ttu-id="ab921-191">默认的最后一个迁移。</span><span class="sxs-lookup"><span data-stu-id="ab921-191">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="ab921-192">幂等</span><span class="sxs-lookup"><span data-stu-id="ab921-192">-Idempotent</span></span>       | <span data-ttu-id="ab921-193">生成脚本，可以在任何迁移的数据库上使用。</span><span class="sxs-lookup"><span data-stu-id="ab921-193">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="ab921-194">-输出\<字符串 ></span><span class="sxs-lookup"><span data-stu-id="ab921-194">-Output \<String></span></span> | <span data-ttu-id="ab921-195">要将结果写入的文件。</span><span class="sxs-lookup"><span data-stu-id="ab921-195">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="ab921-196">收件人、、 和输出参数支持选项卡扩展。</span><span class="sxs-lookup"><span data-stu-id="ab921-196">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="ab921-197">更新数据库</span><span class="sxs-lookup"><span data-stu-id="ab921-197">Update-Database</span></span>

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <span data-ttu-id="ab921-198"><nobr>*-迁移*\<字符串 ></nobr></span><span class="sxs-lookup"><span data-stu-id="ab921-198"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="ab921-199">目标迁移。</span><span class="sxs-lookup"><span data-stu-id="ab921-199">The target migration.</span></span> <span data-ttu-id="ab921-200">如果为"0"，将还原所有迁移。</span><span class="sxs-lookup"><span data-stu-id="ab921-200">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="ab921-201">默认的最后一个迁移。</span><span class="sxs-lookup"><span data-stu-id="ab921-201">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="ab921-202">迁移参数支持选项卡扩展。</span><span class="sxs-lookup"><span data-stu-id="ab921-202">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
