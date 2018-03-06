---
title: "IBM 数据服务器 (DB2) 数据库提供程序 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 02/15/2017
ms.assetid: 825e5332-5aa3-4600-9efb-ab71aaff59ec
ms.technology: entity-framework-core
uid: core/providers/ibm/index
ms.openlocfilehash: a9caa8df63850d4f6b5f2164dad7ac5af7504076
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2018
---
# <a name="ibm-data-server-db2-ef-core-database-providers"></a>IBM 数据服务器 (DB2) EF Core 数据库提供程序

此数据库提供程序允许将 Entity Framework Core 与 IBM 数据服务器一起使用。 此提供程序由 IBM 进行维护。

> [!NOTE]  
> 此提供程序不作为 Entity Framework Core 项目的组成部分进行维护。 考虑使用第三方提供程序时，请务必评估质量、授权、支持等因素，确保其满足要求。

## <a name="install"></a>安装

要在 Windows 上使用 IBM 数据服务器，请安装 [IBM.EntityFrameworkCore NuGet 包](https://www.nuget.org/packages/IBM.EntityFrameworkCore)。

``` powershell
Install-Package IBM.EntityFrameworkCore
```

要在 Linux 上使用 IBM 数据服务器，请安装 [IBM.EntityFrameworkCore-lnx NuGet 包](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx)。

``` powershell
Install-Package IBM.EntityFrameworkCore-lnx
```

## <a name="get-started"></a>开始操作

[开始使用适用于 .NET Core 的 IBM .NET 提供程序](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/DB2DotnetCore?lang=en)

## <a name="supported-database-engines"></a>支持的数据库引擎

* zOS
* LUW（包括 IBM dashDB）
* IBM I
* Informix

## <a name="supported-platforms"></a>支持的平台

* .NET Framework（4.6 及以上版本）
* .NET 核心
