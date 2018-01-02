---
title: "使用 Entity Framework 测试组件 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/index
ms.openlocfilehash: c82c25da393c39cf5e2deb46c7322e7395051937
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="testing"></a><span data-ttu-id="be3ce-102">测试</span><span class="sxs-lookup"><span data-stu-id="be3ce-102">Testing</span></span>

<span data-ttu-id="be3ce-103">你可能希望使用类似于连接真实数据库的方式来测试组件，但不产生实际数据库 I/O 操作的开销。</span><span class="sxs-lookup"><span data-stu-id="be3ce-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="be3ce-104">可通过以下两种主要方法执行此操作：</span><span class="sxs-lookup"><span data-stu-id="be3ce-104">There are two main options for doing this:</span></span>
 * <span data-ttu-id="be3ce-105">通过 [SQLite 内存中模式](sqlite.md)，可针对行为类似于关系数据库的提供程序编写有效测试。</span><span class="sxs-lookup"><span data-stu-id="be3ce-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
 * <span data-ttu-id="be3ce-106">[InMemory 提供程序](in-memory.md)是一个具有极少依赖项的轻量提供程序，但其行为不会始终与关系数据库相似。</span><span class="sxs-lookup"><span data-stu-id="be3ce-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
