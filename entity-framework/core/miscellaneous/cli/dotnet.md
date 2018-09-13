---
title: .NET Core CLI-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 3534336f1caeed96079b35c739d694a536919020
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489604"
---
<a name="ef-core-net-command-line-tools"></a>EF Core.NET 命令行工具
===============================
实体框架核心.NET 命令行工具是一种扩展到跨平台**dotnet**命令，是一部分的[.NET Core SDK][2]。

> [!TIP]
> 如果使用 Visual Studio，我们建议[PMC 工具][ 1]而是因为它们提供更完整的体验。

<a name="installing-the-tools"></a>安装工具
--------------------
> [!NOTE]
> .NET Core SDK 版本 2.1.300 和更高版本包括**dotnet ef**与 EF Core 2.0 和更高版本兼容的命令。 因此如果你使用的最新版本的.NET Core SDK 和 EF Core 运行时，需要进行任何安装，你可以忽略本部分的其余部分。
>
> 另一方面， **dotnet ef**工具包含在.NET Core SDK 版本 2.1.300 和更高版本不兼容与 EF Core 版本 1.0 和 1.1。 你可以使用.NET Core sdk 2.1.300 的计算机使用 EF Core 这些早期版本的项目或更高版本安装之前，你还必须安装版本 2.1.200 或更低版本的 sdk 和配置应用程序通过修改使用该旧版本其 [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json)文件。 此文件通常包含在解决方案目录 （一个项目上方）。 然后，您可以继续下面 installlation 指令。

对于.NET Core sdk 的早期版本，你可以安装使用这些步骤的 EF Core.NET 命令行工具：

1. 编辑项目文件并添加 Microsoft.EntityFrameworkCore.Tools.DotNet 作为 DotNetCliToolReference 项 （见下文）
2. 运行以下命令：

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

生成的项目应如下所示：

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
> 使用的包引用`PrivateAssets="All"`意味着它不公开到引用此项目，特别适合用于在开发过程中通常仅使用的包的项目。

如果您还没有完全正确，您应能够成功在命令提示符处运行以下命令。

``` Console
dotnet ef
```

<a name="using-the-tools"></a>使用的工具
---------------
每当调用命令时，有两个项目所涉及：

目标项目是在其中添加任何文件（或在某些情况下删除文件）的项目。 目标项目默认为当前目录中的项目，但可以使用更改<nobr> **`--project`** </nobr>选项。

启动项目是执行项目代码时由工具模拟的项目。 它还将默认为当前目录中的项目，但可以使用更改**`--startup-project`** 选项。

> [!NOTE]
> 例如，更新的 web 应用程序具有不同的项目中安装的 EF Core 数据库将如下所示： `dotnet ef database update --project {project-path}` （从您的 web 应用程序目录）

常用选项：

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | `--json`                           | 显示 JSON 输出。           |
| -c | `--context <DBCONTEXT>`           | 若要使用 DbContext。       |
| -p | `--project <PROJECT>`             | 要使用的项目。         |
| -s | `--startup-project <PROJECT>`     | 若要使用的启动项目。 |
|    | `--framework <FRAMEWORK>`         | 目标框架。       |
|    | `--configuration <CONFIGURATION>` | 要使用的配置。   |
|    | `--runtime <IDENTIFIER>`          | 运行时使用。         |
| -h | `--help`                           | 显示帮助信息。      |
| -v | `--verbose`                        | 显示详细输出。        |
|    | `--no-color`                       | 不为着色输出。      |
|    | `--prefix-output`                  | 输出级别使用的前缀。   |


> [!TIP]
> 若要指定 ASP.NET Core 环境，设置**ASPNETCORE_ENVIRONMENT**之前运行的环境变量。

<a name="commands"></a>命令
--------

### <a name="dotnet-ef-database-drop"></a>dotnet ef 数据库拖放

删除数据库。

选项:

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| -f | `--force`   | 不确认。                                           |
|    | `--dry-run` | 显示哪个数据库会被丢弃，但没有删除它。 |

### <a name="dotnet-ef-database-update"></a>dotnet ef 数据库更新

指定迁移到更新数据库。

参数：

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| `<MIGRATION>` | 目标迁移。 如果为 0，将还原所有迁移。 默认的最后一个迁移。 |

### <a name="dotnet-ef-dbcontext-info"></a>dotnet ef dbcontext 信息

获取有关 DbContext 类型的信息。

### <a name="dotnet-ef-dbcontext-list"></a>dotnet ef dbcontext 列表

列出了可用的 DbContext 类型。

### <a name="dotnet-ef-dbcontext-scaffold"></a>dotnet ef dbcontext 基架

搭建基架以数据库的 DbContext 和实体类型。

参数：

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| `<CONNECTION>` | 数据库的连接字符串。                                      |
| `<PROVIDER>`   | 要使用的提供程序。 （例如 Microsoft.EntityFrameworkCore.SqlServer） |

选项:

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr> | `--data-annotations`                      | 特性用于将模型配置 （如果可能）。 如果省略，则使用仅 fluent API。 |
| -c              | `--context <NAME>`                       | DbContext 的名称。                                                                       |
|                 | `--context-dir <PATH>`                   | 要将 DbContext 文件放入的目录。 路径是相对于项目目录。             |
| -f              | `--force`                                 | 覆盖现有文件。                                                                        |
| -o              | `--output-dir <PATH>`                    | 要将文件放入的目录。 路径是相对于项目目录。                      |
|                 | <nobr>`--schema <SCHEMA_NAME>...`</nobr> | 要生成的实体类型的表架构。                                              |
| -t              | `--table <TABLE_NAME>`...                | 要生成的实体类型的表。                                                         |
|                 | `--use-database-names`                    | 使用直接从数据库表和列名称。                                           |

### <a name="dotnet-ef-migrations-add"></a>添加 dotnet ef 迁移

添加新的迁移。

参数：

|         |                            |
|:--------|:---------------------------|
| `<NAME>` | 迁移的名称。 |

选项:

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <nobr>-o</nobr> | <nobr> `--output-dir <PATH>` </nobr> | 目录 （及其子命名空间） 来使用。 路径是相对于项目目录。 默认值为"迁移"。 |

### <a name="dotnet-ef-migrations-list"></a>dotnet ef 迁移列表

列出了可用的迁移。

### <a name="dotnet-ef-migrations-remove"></a>dotnet ef 迁移删除

删除最后一个迁移。

选项:

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| -f | `--force` | 如果它已应用到数据库，请还原迁移。 |

### <a name="dotnet-ef-migrations-script"></a>dotnet ef 迁移脚本

从迁移中生成的 SQL 脚本。

参数：

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| `<FROM>` | 开始迁移。 默认值为 0 （初始数据库）。 |
| `<TO>`   | 结束的迁移。 默认的最后一个迁移。         |

选项:

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| -o | `--output <FILE>` | 要将结果写入的文件。                                   |
| -i | `--idempotent`     | 生成脚本，可以在任何迁移的数据库上使用。 |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
