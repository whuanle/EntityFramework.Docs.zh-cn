---
title: 测试以及 InMemory 的 EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 33690e3424d0777930d3cb8167575fb0f4ddd8f7
ms.sourcegitcommit: d096484dcf9eff73d9943fa60db7a418b10ca0b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2018
ms.locfileid: "27995582"
---
# <a name="testing-with-inmemory"></a>测试以及 InMemory

你想要测试使用的，它模拟连接到真实的数据库，而无需实际的数据库操作开销的组件时，InMemory 提供程序非常有用。

> [!TIP]  
> 可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing)。

## <a name="inmemory-is-not-a-relational-database"></a>InMemory 不是一个关系数据库

EF Core 数据库提供程序不需要是关系数据库。 InMemory 旨在作为一般用途数据库进行测试，而不是要模拟关系数据库。

包括一些示例：
* InMemory 将允许你保存将违反引用完整性约束关系数据库中的数据。

* 如果在模型中的属性使用 DefaultValueSql(string)，这是一个关系数据库 API，针对 InMemory 运行时将产生任何影响。

> [!TIP]  
> 对于许多测试目的这些差异将不起作用。 但是，如果你想要用作测试依据的表现得更像 true 的关系数据库内容，则可以考虑使用[SQLite 内存中模式](sqlite.md)。

## <a name="example-testing-scenario"></a>示例测试方案

请考虑以下允许应用程序代码执行与博客某些操作的服务。 它在内部使用`DbContext`连接到 SQL Server 数据库。 将发挥作用交换此上下文，以便我们可以编写高效测试此服务，而无需修改代码，或执行大量工作来创建测试连接到 InMemory 数据库上下文的双精度。

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>准备你的上下文

### <a name="avoid-configuring-two-database-providers"></a>避免配置两个数据库提供程序

在测试中要从外部配置要使用 InMemory 提供程序的上下文。 如果你要配置数据库提供程序通过重写`OnConfiguring`在您的上下文，然后你需要添加一些条件的代码，以确保你仅配置数据库提供程序，如果其中一个不已配置。

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> 如果你使用的 ASP.NET Core，则应不需要此代码由于外部 （会在 Startup.cs) 的上下文已配置数据库提供程序。

### <a name="add-a-constructor-for-testing"></a>添加用于测试的构造函数

若要启用针对不同的数据库的测试的最简单方法是修改你公开接受一个构造函数的上下文`DbContextOptions<TContext>`。

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>`告知所有其设置，例如要连接到的数据库上下文。 这是通过在您的上下文中运行 OnConfiguring 方法生成的相同对象。

## <a name="writing-tests"></a>编写测试

与此提供程序测试的关键是无法识别要使用的 InMemory 提供程序，并控制内存中数据库的作用域的上下文的能力。 通常您希望干净的数据库为每个测试方法。

下面是使用 InMemory 数据库测试类的一个示例。 每个测试方法指定一个唯一的数据库名称，这意味着每个方法都有自己 InMemory 数据库。

>[!TIP]
> 若要使用`.UseInMemoryDatabase()`扩展方法，引用的 NuGet 包`Microsoft.EntityFrameworkCore.InMemory`。

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
