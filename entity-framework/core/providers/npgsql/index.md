---
title: "适用于 PostgreSQL 的 Npgsql 数据库提供程序 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 5ecd1b22-c68e-4d87-ba39-b0761f4d5b90
ms.technology: entity-framework-core
uid: core/providers/npgsql/index
ms.openlocfilehash: acf2e18d7a608b0d75b571a5ac0199d84f86066b
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
---
# <a name="npgsql-ef-core-database-provider-for-postgresql"></a>适用于 PostgreSQL 的 Npgsql EF Core 数据库提供程序

此数据库提供程序允许将 Entity Framework Core 与 PostgreSQL 一起使用。 此提供程序作为 [Npgsql 项目](http://www.npgsql.org)的组成部分进行维护。

> [!NOTE]  
> 此提供程序不作为 Entity Framework Core 项目的组成部分进行维护。 考虑使用第三方提供程序时，请务必评估质量、授权、支持等因素，确保其满足要求。

## <a name="install"></a>安装

安装 [Npgsql.EntityFrameworkCore.PostgreSQL NuGet 包](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL)。

``` powershell
Install-Package Npgsql.EntityFrameworkCore.PostgreSQL
```

## <a name="get-started"></a>开始操作

请参阅 [Npgsql 文档](http://www.npgsql.org/efcore/index.html)，了解如何开始操作。

## <a name="supported-database-engines"></a>支持的数据库引擎

* postgresql

## <a name="supported-platforms"></a>支持的平台

* .NET Framework（4.5.1 及以上版本）

* .NET 核心

* Mono（4.2.0 及以上版本）
