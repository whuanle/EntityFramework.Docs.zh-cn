---
title: "EF6 和 EF Core - 在同一应用程序中使用它们"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: f6eb4bf7d99fbc61f8ffbd0dc7c6c17789395303
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a>在同一个应用程序中使用 EF Core 和 EF6

通过安装两个 NuGet 包可在同一个 .NET Framework 应用程序或库中使用 EF Core 和 EF6。 

某些类型在 EF Core 和 EF6 中具有相同的名称，并且仅命名空间有所不同，这可能会使在同一代码文件中同时使用 EF Core 和 EF6 变得复杂。 使用命名空间别名指令可以很容易地删除多义性，例如：

``` csharp
using Microsoft.EntityFrameworkCore;
using EF6 = System.Data.Entity; // e.g. EF6.DbContext
```

如果要迁移具有多个 EF 模型的现有应用程序，则可以将其中一些选择性地迁移到 EF Core，其余程序则继续使用 EF6。
