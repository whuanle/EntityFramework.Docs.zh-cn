---
title: 使用 SQLite 的 EF 核心测试
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: 8fcb4b1a37da6f52219ebe3c672160627fe28fab
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052697"
---
# <a name="testing-with-sqlite"></a>使用 SQLite 测试

SQLite 具有内存中模式，您可以使用 SQLite 来编写针对关系数据库，而无需实际的数据库操作开销的测试。

> [!TIP]  
> 你可以查看这篇文章[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing)GitHub 上

## <a name="example-testing-scenario"></a>示例测试方案

请考虑以下允许应用程序代码执行与博客某些操作的服务。 它在内部使用`DbContext`连接到 SQL Server 数据库。 它会有用要交换此上下文，以便我们可以编写高效测试此服务，而无需修改代码，或执行大量工作来创建测试连接到内存中 SQLite 数据库上下文的双精度。

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>准备你的上下文

### <a name="avoid-configuring-two-database-providers"></a>避免配置两个数据库提供程序

在测试中要从外部配置要使用 InMemory 提供程序的上下文。 如果你要配置数据库提供程序通过重写`OnConfiguring`在您的上下文，然后你需要添加一些条件的代码，以确保你仅配置数据库提供程序，如果其中一个不已配置。

> [!TIP]  
> 如果你使用的 ASP.NET 核心，则应不需要此代码由于数据库提供程序配置 （会在 Startup.cs) 的上下文之外。

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>添加用于测试的构造函数

若要启用针对不同的数据库的测试的最简单方法是修改你公开接受一个构造函数的上下文`DbContextOptions<TContext>`。

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>`告知所有其设置，例如要连接到的数据库上下文。 这是通过在您的上下文中运行 OnConfiguring 方法生成的相同对象。

## <a name="writing-tests"></a>编写测试

与此提供程序测试的关键是无法识别要使用 SQLite，并控制内存中数据库的作用域的上下文的能力。 数据库范围的受打开和关闭连接。 数据库的连接已打开的持续时间限定。 通常您希望干净的数据库为每个测试方法。

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
