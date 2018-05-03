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
---
<a name="ef-core-package-manager-console-tools"></a>EF 核心程序包管理器控制台工具
=====================================
EF 核心程序包管理器控制台 (PMC) 工具使用的 NuGet 在 Visual Studio 内运行[程序包管理器控制台][2]。
这些工具同时适用于 .NET Framework 和 .NET Core 项目。

> [!TIP]
> 未使用 Visual Studio？ [EF 核心命令行工具][ 1]是跨平台，且在命令提示符下运行。

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

目标项目是在其中添加任何文件（或在某些情况下删除文件）的项目。 目标项目默认为**默认项目**选择在程序包管理器控制台中，但也可以通过使用指定的项目参数。

启动项目是执行项目代码时由工具模拟的项目。 它将默认为一个**设为启动项目**在解决方案资源管理器。 它可以还指定使用-StartupProject 参数。

通用参数：

|                           |                             |
|:--------------------------|:----------------------------|
| -上下文\<字符串 >        | 若要使用 DbContext。       |
| -项目\<字符串 >        | 要使用的项目。         |
| -StartupProject\<字符串 > | 要使用的启动项目。 |
| -Verbose                  | 显示详细输出。        |

若要显示有关命令的帮助信息，请使用 PowerShell 的`Get-Help`命令。

> [!TIP]
> 上下文、 Project 和 StartupProject 参数支持选项卡扩展。

> [!TIP]
> 设置**env:ASPNETCORE_ENVIRONMENT**之前运行若要指定 ASP.NET Core 环境。

<a name="commands"></a>命令
--------

### <a name="add-migration"></a>添加迁移

将添加一个新迁移。

参数：

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| ***-名称***\<字符串 >             | 迁移的名称。                                                                                       |
| <nobr>-OutputDir\<字符串 ></nobr> | 目录 （及其子命名空间） 使用。 路径是相对于项目目录。 默认值为"迁移"。 |

> [!NOTE]
> 中的参数**粗体**是必需的以及在发生*斜体*是位置。

### <a name="drop-database"></a>删除数据库

删除数据库。

参数：

|         |                                                          |
|:--------|:---------------------------------------------------------|
| -WhatIf | 显示的数据库会被丢弃，但没有删除它。 |

### <a name="get-dbcontext"></a>Get-DbContext

获取有关 DbContext 类型的信息。

### <a name="remove-migration"></a>删除迁移

删除上次的迁移。

参数：

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| -Force | 如果它已应用到数据库，请还原迁移。 |

### <a name="scaffold-dbcontext"></a>基架 DbContext

基架数据库类型 DbContext 和实体的类型。

参数：

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>***连接***\<字符串 ></nobr> | 数据库的连接字符串。                                                           |
| ***-Provider*** \<String>                | 要使用的提供程序。 （例如， Microsoft.EntityFrameworkCore.SqlServer)                              |
| -OutputDir\<字符串 >                     | 要将文件放入的目录。 路径是相对于项目目录。                      |
| -ContextDir\<字符串 >                    | 要将 DbContext 文件放入的目录。 路径是相对于项目目录。             |
| -上下文\<字符串 >                       | 若要生成的 dbcontext 名称。                                                           |
| -架构\<String [] >                     | 要生成实体类型的表架构。                                              |
| -表\<String [] >                      | 要生成实体类型的表。                                                         |
| -DataAnnotations                         | 使用属性来配置该模型 （如果可能）。 如果省略，则使用仅 fluent API。 |
| -UseDatabaseNames                        | 使用直接从数据库表和列名称。                                           |
| -Force                                   | 覆盖现有文件。                                                                        |

### <a name="script-migration"></a>Script-Migration

从迁移中生成的 SQL 脚本。

参数：

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| *-从*\<字符串 > | 开始迁移。 默认值为 0 （初始数据库）。      |
| *到*\<字符串 >   | 结束的迁移。 默认到最后一个迁移。              |
| 幂等性       | 生成可以在任何迁移的数据库使用的脚本。 |
| -输出\<字符串 > | 要将结果写入的文件。                                   |

> [!TIP]
> 收件人、 从，和输出参数支持选项卡扩展。

### <a name="update-database"></a>更新数据库

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <nobr>*迁移*\<字符串 ></nobr> | 目标迁移。 如果为"0"，将恢复所有迁移。 默认到最后一个迁移。 |

> [!TIP]
> 迁移参数支持选项卡扩展。


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
