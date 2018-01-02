---
title: "InMemory 数据库提供程序 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: a8e05f50837f3da554b338475d24215706dfa2ec
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
---
# <a name="ef-core-in-memory-database-provider"></a>EF Core In-Memory 数据库提供程序

此数据库提供程序允许将 Entity Framework Core 和内存数据库一起使用。 测试使用 Entity Framework Core 的代码时，这非常有用。 此提供程序作为 [EntityFramework GitHub 项目](https://github.com/aspnet/EntityFramework)的组成部分进行维护。

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
