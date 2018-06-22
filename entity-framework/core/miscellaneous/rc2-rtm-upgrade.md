---
title: 从 EF 升级核心到 RTM-1.0 RC2 EF 核心
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
ms.technology: entity-framework-core
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 4bb4c5736708413f6581cad250b089b7bc22a559
ms.sourcegitcommit: 90139dbd6f485473afda0788a5a314c9aa601ea0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2018
ms.locfileid: "30151035"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a>从 EF 核心 1.0 RC2 升级到 RTM

本文提供了将用到 1.0.0 RC2 包生成应用程序迁移指南 RTM。

## <a name="package-versions"></a>包版本

你通常将安装到应用程序的最上层包的名称未 RC2 和 RTM 之间发生更改。

**你需要升级到 RTM 版本，安装的程序包：**

* 运行时包 (例如`Microsoft.EntityFrameworkCore.SqlServer`) 更改，不再是`1.0.0-rc2-final`到`1.0.0`。

* `Microsoft.EntityFrameworkCore.Tools`包更改从`1.0.0-preview1-final`到`1.0.0-preview2-final`。 请注意，工具仍预发行版。

## <a name="existing-migrations-may-need-maxlength-added"></a>现有的迁移可能需要添加的 maxLength

在 RC2，在迁移中的列定义如下所示`table.Column<string>(nullable: true)`和列的长度在我们将存储迁移背后的代码在某些元数据中进行查找。 在 RTM，长度现在包含在基架代码`table.Column<string>(maxLength: 450, nullable: true)`。

已在使用 RTM 之前基架的任何现有迁移不会`maxLength`指定参数。 这意味着将使用数据库支持的最大长度 (`nvarchar(max)` SQL Server 上)。 这可能是相当不错的某些列，但键外, 键的一部分的列或索引需要更新，以包含最大长度。 按照约定，450 是最大长度用于密钥外, 键和索引列。 如果模型中，具有显式配置长度，然后应改为使用该长度。

**ASP.NET 标识**

此更改会影响使用 ASP.NET 标识，并且已从预先创建的项目的 RTM 项目模板。 项目模板包括用于创建数据库的迁移。 必须编辑此迁移，以指定最大长度为`256`为以下列。

*  **AspNetRoles**

    * 名称

    * NormalizedName

*  **AspNetUsers**

   * 电子邮件

   * NormalizedEmail

   * NormalizedUserName

   * UserName

若要进行此更改将导致以下异常时初始迁移已应用到数据库。

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a>.NET 核心： project.json 中删除"导入"

如果你已面向.NET 核心与 RC2，需要添加`imports`到临时的解决方法不支持.NET 标准的 EF 核心依赖关系的某些 project.json。 现在即可移除这些。

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

> [!NOTE]  
> 截至版本 1.0 RTM， [.NET 核心 SDK](https://www.microsoft.com/net/download/core)不再支持`project.json`或开发.NET 核心应用程序使用 Visual Studio 2015。 我们建议你[从 project.json 迁移到 csproj](https://docs.microsoft.com/dotnet/articles/core/migration/)。 如果使用 Visual Studio，我们建议你升级到[Visual Studio 2017](https://www.visualstudio.com/downloads/)。

## <a name="uwp-add-binding-redirects"></a>UWP： 添加绑定重定向

尝试运行 EF 命令在通用 Windows 平台 (UWP) 项目导致以下错误：

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

你需要手动将绑定重定向添加到 UWP 项目。 创建名为的文件`App.config`在项目根文件夹并将重定向添加到正确的程序集版本。

``` xml
<configuration>
 <runtime>
   <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
     <dependentAssembly>
       <assemblyIdentity name="System.IO.FileSystem.Primitives"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
     <dependentAssembly>
       <assemblyIdentity name="System.Threading.Overlapped"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
   </assemblyBinding>
 </runtime>
</configuration>
```
