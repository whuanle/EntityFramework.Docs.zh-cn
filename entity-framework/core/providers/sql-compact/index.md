---
title: "Microsoft SQL Server Compact Edition 数据库提供程序 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 073f0004-3eb5-4618-ab93-0674910e1819
ms.technology: entity-framework-core
uid: core/providers/sql-compact/index
ms.openlocfilehash: d8b73621bdd363efec5bb7728886e0a0f6bdcf76
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
---
# <a name="microsoft-sql-server-compact-edition-ef-core-database-provider"></a>Microsoft SQL Server Compact Edition EF Core 数据库提供程序

此数据库提供程序允许将 Entity Framework Core 与 SQL Server Compact Edition 一起使用。 此提供程序作为 [ErikEJ/EntityFramework.SqlServerCompact GitHub 项目](https://github.com/ErikEJ/EntityFramework.SqlServerCompact)的组成部分进行维护。

> [!NOTE]  
> 此提供程序不作为 Entity Framework Core 项目的组成部分进行维护。 考虑使用第三方提供程序时，请务必评估质量、授权、支持等因素，确保其满足要求。

## <a name="install"></a>安装

要使用 SQL Server Compact Edition 4.0，请安装 [EntityFrameworkCore.SqlServerCompact40 NuGet 包](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40)。

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact40
```

要使用 SQL Server Compact Edition 3.5，请安装 [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35)。

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact35
```

## <a name="get-started"></a>开始操作

请参阅[项目网站上的入门文档](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)

## <a name="supported-database-engines"></a>支持的数据库引擎

* SQL Server Compact Edition 3.5

* SQL Server Compact Edition 4.0

## <a name="supported-platforms"></a>支持的平台

* .NET Framework（4.5.1 及以上版本）
