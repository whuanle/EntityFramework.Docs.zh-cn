---
title: "Microsoft SQL Server 数据库提供程序 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/index
ms.openlocfilehash: b2faf932e0484da4df0c1774afa7ba7ae2d077a5
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Microsoft SQL Server EF Core 数据库提供程序

此数据库提供程序允许将 Entity Framework Core 与 Microsoft SQL Server（包括 SQL Azure）一起使用。 此提供程序作为 [EntityFramework GitHub 项目](https://github.com/aspnet/EntityFramework)的组成部分进行维护。

## <a name="install"></a>安装

安装 [Microsoft.EntityFrameworkCore.SqlServer NuGet 包](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)。

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a>开始操作

下列资源可帮助你开始使用此提供程序。
* [在 .NET Framework（控制台、WinForms、WPF 等）上开始使用](../../get-started/full-dotnet/index.md)

* [在 ASP.NET Core 上开始使用](../../get-started/aspnetcore/index.md)

* [UnicornStore 示例应用程序](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)

## <a name="supported-database-engines"></a>支持的数据库引擎

* Microsoft SQL Server（2008 及以上版本）

## <a name="supported-platforms"></a>支持的平台

* .NET Framework（4.5.1 及以上版本）

* .NET 核心

* Mono（4.2.0 及以上版本）

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
