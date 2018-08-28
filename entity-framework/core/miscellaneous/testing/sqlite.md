---
title: 使用 SQLite 的 EF Core测试
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: bc9d6768a90ce17160c4126d2a68fddaa30d63de
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996863"
---
# <a name="testing-with-sqlite"></a>使用 SQLite 进行测试

SQLite 具有内存中模式，您可以使用 SQLite 针对关系数据库中，而无需实际的数据库操作的开销编写测试。

> [!TIP]  
> 您可以查看此文章[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing)GitHub 上

## <a name="example-testing-scenario"></a>示例测试方案

请考虑以下服务，允许应用程序代码来执行某些与博客相关的操作。 在内部使用`DbContext`连接到 SQL Server 数据库。 可以用来交换此上下文，以便我们可以编写此服务的有效测试，而无需修改代码，或执行大量工作来创建测试连接到内存中 SQLite 数据库上下文的双精度。

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>准备您的上下文

### <a name="avoid-configuring-two-database-providers"></a>避免配置两个数据库提供程序

在测试中要从外部配置要使用 InMemory 提供程序的上下文。 如果要配置数据库提供程序通过重写`OnConfiguring`在上下文中，则需要添加一些条件的代码，以确保，仅当其中一个已尚未配置配置的数据库提供程序。

> [!TIP]  
> 如果使用的 ASP.NET Core，则应该不需要此代码由于数据库提供程序配置 （在 Startup.cs) 的上下文之外。

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>添加用于测试的构造函数

若要启用针对不同的数据库的测试的最简单方法是修改上下文，以公开一个构造函数接受`DbContextOptions<TContext>`。

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` 告诉所有其设置，例如要连接到的数据库上下文。 这是通过在您的上下文中运行 OnConfiguring 方法生成的相同对象。

## <a name="writing-tests"></a>编写测试

使用此提供程序进行测试的关键是能够告诉地使用 SQLite，并控制的内存中数据库作用域的上下文。 数据库作用域控制打开和关闭连接。 数据库为作用域为该连接处于打开状态的持续时间。 通常情况下想干净的数据库的每个测试方法。

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
