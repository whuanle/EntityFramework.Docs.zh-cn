---
title: "MySQL 数据库提供程序 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4900b882-79c5-40d2-a44a-ccb0292f6ed9
ms.technology: entity-framework-core
uid: core/providers/mysql/index
ms.openlocfilehash: 1500d017cb463c3f394131a79b9063ff90cce5e2
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/22/2017
---
# <a name="mysql-ef-core-database-provider"></a>MySQL EF Core 数据库提供程序

此数据库提供程序允许将 Entity Framework Core 与 MySQL 一起使用。 此提供程序作为 [MySQL 项目](http://dev.mysql.com)的组成部分进行维护。

> [!WARNING]  
> 此提供程序为预发布版本。

> [!NOTE]  
> 此提供程序不作为 Entity Framework Core 项目的组成部分进行维护。 考虑使用第三方提供程序时，请务必评估质量、授权、支持等因素，确保其满足要求。

## <a name="install"></a>安装

安装 [MySql.Data.EntityFrameworkCore NuGet 包](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore)。

``` powershell
Install-Package MySql.Data.EntityFrameworkCore -Pre
```

## <a name="get-started"></a>开始操作

请参阅[以 MySQL EF Core 提供程序和 Connector/Net 7.0.4 开始](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/)。

## <a name="supported-database-engines"></a>支持的数据库引擎

* MySQL

## <a name="supported-platforms"></a>支持的平台

* .NET Framework（4.5.1 及以上版本）

* .NET 核心

请务必查阅[此处](https://dev.mysql.com/doc/connector-net/en/connector-net-versions.html)和[此处](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)的 MySQL 文档，获取有关版本兼容性的信息
