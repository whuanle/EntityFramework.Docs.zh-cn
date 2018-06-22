---
title: InMemory 数据库提供程序 - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: 356af9390a8aafa5afe35f333cd1e6ac1988390d
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678981"
---
# <a name="ef-core-in-memory-database-provider"></a>EF Core In-Memory 数据库提供程序

此数据库提供程序允许将 Entity Framework Core 和内存数据库一起使用。 这对测试非常有用，尽管内存中模式下的 SQLite 提供程序可能是针对关系数据库的更合适的测试替代。 该提供程序作为 [Entity Framework Core 项目](https://github.com/aspnet/EntityFrameworkCore)的组成部分进行维护。

## <a name="install"></a>安装

安装 [Microsoft.EntityFrameworkCore.InMemory NuGet 包](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/)。

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a>开始操作

下列资源可帮助你开始使用此提供程序。
* [使用 InMemory 进行测试](../../miscellaneous/testing/in-memory.md)

* [UnicornStore 示例应用程序测试](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a>支持的数据库引擎

* 内置的内存中数据库（仅用于测试目的）

## <a name="supported-platforms"></a>支持的平台

* .NET Framework（4.5.1 及以上版本）

* .NET 核心

* Mono（4.2.0 及以上版本）

* 通用 Windows 平台
