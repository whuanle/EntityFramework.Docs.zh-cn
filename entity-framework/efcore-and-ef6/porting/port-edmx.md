---
title: 从 EF6 移植到 EF Core 技术-移植了基于 EDMX 的模型
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: c999d2114c76ee3a7615ae897b42ee3376cff14e
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812685"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>移植到 EF Core 了从 EF6 基于 EDMX 的模型

EF Core 不支持的模型的 EDMX 文件格式。 若要移植这些模型中，最佳选择是从你的应用程序数据库中生成新的基于代码的模型。

## <a name="install-ef-core-nuget-packages"></a>安装 EF Core NuGet 包

安装`Microsoft.EntityFrameworkCore.Tools`NuGet 包。

## <a name="regenerate-the-model"></a>重新生成模型

反向工程功能现在可用于创建基于现有数据库的模型。

在 Package Manager Console 中运行以下命令 (工具 –> NuGet 包管理器 –> 程序包管理器控制台)。 请参阅[Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md)有关命令选项搭建基架的表等等的子集。

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

例如，下面是要从 SQL Server LocalDB 实例上的博客数据库模型搭建基架命令。

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>删除从 EF6 模型

现在，你将删除从 EF6 模型从你的应用程序。

最好将从 EF6 NuGet 包 (EntityFramework) 安装，因为 EF Core 和 ef6 更高版本可以是在同一应用程序并行使用。 但是，如果你不想要在你的应用程序的任何区域中使用 ef6 更高版本，然后卸载程序包将有助于为提供上的代码片段，需要注意的编译错误。

## <a name="update-your-code"></a>更新你的代码

此时，它是一个寻址编译错误和查看代码，以查看你会影响从 EF6 和 EF Core 之间的行为更改。

## <a name="test-the-port"></a>测试端口

只是因为你的应用程序将编译，并不意味着它成功移植到 EF Core。 你将需要测试应用程序，以确保任何行为更改产生负面影响你的应用程序的所有区域。

> [!TIP]
> 请参阅[Getting Started with ASP.NET Core 使用现有的数据库上的 EF Core](xref:core/get-started/aspnetcore/existing-db)有关现有数据库，使用方式的其他参考 
