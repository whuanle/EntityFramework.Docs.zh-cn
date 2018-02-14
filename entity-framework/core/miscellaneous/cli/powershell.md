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
<a name="ef-core-package-manager-console-tools"></a>EF Core 包管理器控制台工具
=====================================
EF Core 包管理器控制台 (PMC) 工具使用 NuGet 的[包管理器控制台][2]在 Visual Studio 中运行。
这些工具处理的.NET Framework 和.NET 核心项目。

> [!TIP]
> 未使用 Visual Studio？[EF Core 命令行工具][ 1]是跨平台的工具，可在命令提示符下运行。

<a name="installing-the-tools"></a>安装工具
--------------------
通过安装 Microsoft.EntityFrameworkCore.Tools NuGet 包安装 EF 核心程序包管理器控制台 Tools。
可以通过执行以下命令内的安装[程序包管理器控制台][2]。

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

如果一切运行正常，你应能够运行此命令：

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> 如果你的启动项目面向.NET 标准[跨目标支持的框架][ 3]之前使用的工具。

> [!IMPORTANT]
> 如果你使用**通用 Windows**或**Xamarin**，将 EF 代码移动到标准.NET 类库和[跨目标支持的框架][ 3]之前使用的工具。 指定为启动项目的类库。

<a name="using-the-tools"></a>使用的工具
---------------
每当调用某命令时，有两个项目涉及：

目标项目将添加的任何文件的位置 （或在某些情况下删除）。 目标项目默认为**默认项目**选择在程序包管理器控制台中，但也可以通过使用指定的项目参数。

启动项目是仿真的工具，执行你的项目代码时。 它将默认为一个**设为启动项目**在解决方案资源管理器。 它可以还指定使用-StartupProject 参数。

通用参数：

|                           |                             |
| ------------------------- | --------------------------- |
| -Context \<String>        | 要使用的 DbContext。|
| -Project \<String>        | 要使用的项目。|
| -StartupProject \<String> | 要使用的启动项目。|
| -Verbose                  | 显示详细输出。        |

若要显示有关命令的帮助信息，请使用 PowerShell 的`Get-Help`命令。

> [!TIP]
> Context、Project 和 StartupProject 参数支持选项卡扩展。

> [!TIP]
> 设置**env:ASPNETCORE_ENVIRONMENT**之前运行若要指定 ASP.NET Core 环境。

<a name="commands"></a>命令
--------

### <a name="add-migration"></a>Add-Migration

将添加一个新迁移。

参数：

|                                    |                                                                                 |
| ---------------------------------- | ------------------------------------------------------------------------------- |
| ***-Name*** \<String>              | 迁移的名称。|
| <nobr>-OutputDir \<String></nobr>  | 要使用的目录 （及其子命名空间）。路径是相对于项目目录的路径。默认值为“Migrations”。|

> [!NOTE]
>  **粗体**参数是必需的，**斜体参数取决于位置。

### <a name="drop-database"></a>Drop-Database

删除数据库。

参数：

|          |                                                          |
| -------- | -------------------------------------------------------- |
| -WhatIf  | 显示的数据库会被丢弃，但没有删除它。 |

### <a name="get-dbcontext"></a>Get-DbContext

获取有关 DbContext 类型的信息。

### <a name="remove-migration"></a>Remove-Migration

删除上次的迁移。

参数：

|        |                                                                       |
| ------ | --------------------------------------------------------------------- |
| -Force | 不检查以查看迁移是否已应用到数据库。 |

### <a name="scaffold-dbcontext"></a>Scaffold-DbContext

基架数据库类型 DbContext 和实体的类型。

参数：

|                                          |                                                                           |
| ---------------------------------------- | ------------------------------------------------------------------------- |
| <nobr>***-Connection*** \<String></nobr> | 数据库的连接字符串。|
| ***-Provider*** \<String>                | 要使用的提供程序。（例如， Microsoft.EntityFrameworkCore.SqlServer)|
| -OutputDir \<String>                     | 要将文件放入到其中的目录。路径是相对于项目目录。|
| -Context \<String>                       | 要生成的 DbContext 的名称。
| -Schemas \<String[]>                     | 要为其生成实体类型的表的架构。|
| -Tables \<String[]>                      | 要为其生成实体类型的表。|
| -DataAnnotations                         | 使用属性来配置该模型 （如果可能）。 如果省略，则使用仅 fluent API。 |
| -UseDatabaseNames                        | 使用直接从数据库表和列名称。                    |
| -Force                                   | 覆盖现有文件。                                                 |

### <a name="script-migration"></a>Script-Migration

从迁移中生成的 SQL 脚本。

参数：

|                   |                                                                    |
| ----------------- | ------------------------------------------------------------------ |
| *-From* \<String> | 初始迁移。默认值为 0（初始数据库）。|
| *-To* \<String>   | 结束迁移。默认为最后一次迁移。|
| -Idempotent       | 生成在进行任何迁移时都可以在数据库上使用的脚本。|
| -Output \<String> | 要将结果写入到其中的文件。|

> [!TIP]
> To、From 和 Output 参数支持选项卡扩展。

### <a name="update-database"></a>Update-Database

|                                     |                                                                                |
| ----------------------------------- | ------------------------------------------------------------------------------ |
| <nobr>*-Migration* \<String></nobr> | 目标迁移。如果为 "0"，将恢复所有迁移。默认为最后一次迁移。|

> [!TIP]
> Migration 参数支持选项卡扩展。


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
