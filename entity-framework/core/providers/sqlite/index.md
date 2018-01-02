---
title: "SQLite 数据库提供程序 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
ms.technology: entity-framework-core
uid: core/providers/sqlite/index
ms.openlocfilehash: 0f3905a491fb5f7a657ce9037d166771e1f326d8
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
---
# <a name="sqlite-ef-core-database-provider"></a>SQLite EF Core 数据库提供程序

此数据库提供程序允许将 Entity Framework Core 与 SQLite 一起使用。 此提供程序作为 [EntityFramework GitHub 项目](https://github.com/aspnet/EntityFramework)的组成部分进行维护。

## <a name="install"></a>安装

安装 [Microsoft.EntityFrameworkCore.Sqlite NuGet 包](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/)。

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a>开始操作

下列资源可帮助你开始使用此提供程序。
* [UWP 上的本地 SQLite ](../../get-started/uwp/getting-started.md)

* [.NET Core 应用程序到新 SQLite 数据库](../../get-started/netcore/new-db-sqlite.md)

* [Unicorn Clicker 示例应用程序](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [Unicorn Packer 示例应用程序](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a>支持的数据库引擎

* SQLite（3.7 及以上版本）

## <a name="supported-platforms"></a>支持的平台

* .NET Framework（4.5.1 及以上版本）

* .NET 核心

* Mono（4.2.0 及以上版本）

* 通用 Windows 平台

## <a name="limitations"></a>限制

有关 SQLite 提供程序的一些重要限制，请参阅 [SQLite 限制](limitations.md)。
