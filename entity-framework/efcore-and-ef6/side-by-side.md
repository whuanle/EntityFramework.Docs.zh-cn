---
title: EF6 和 EF Core - 在同一应用程序中使用它们
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: ead251c5454473738c2f2bfdac6557aa3e1c5591
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949068"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a><span data-ttu-id="fb94f-102">在同一个应用程序中使用 EF Core 和 EF6</span><span class="sxs-lookup"><span data-stu-id="fb94f-102">Using EF Core and EF6 in the Same Application</span></span>

<span data-ttu-id="fb94f-103">通过安装两个 NuGet 包可在同一个 .NET Framework 应用程序或库中使用 EF Core 和 EF6。</span><span class="sxs-lookup"><span data-stu-id="fb94f-103">It is possible to use EF Core and EF6 in the same .NET Framework application or library by installing both NuGet packages.</span></span>

<span data-ttu-id="fb94f-104">某些类型在 EF Core 和 EF6 中具有相同的名称，并且仅命名空间有所不同，这可能会使在同一代码文件中同时使用 EF Core 和 EF6 变得复杂。</span><span class="sxs-lookup"><span data-stu-id="fb94f-104">Some types have the same names in EF Core and EF6 and differ only by namespace, which may complicate using both EF Core and EF6 in the same code file.</span></span> <span data-ttu-id="fb94f-105">可通过命名空间别名指令轻松消除多义性。</span><span class="sxs-lookup"><span data-stu-id="fb94f-105">The ambiguity can be easily removed using namespace alias directives.</span></span> <span data-ttu-id="fb94f-106">例如:</span><span class="sxs-lookup"><span data-stu-id="fb94f-106">For example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

<span data-ttu-id="fb94f-107">如果要迁移具有多个 EF 模型的现有应用程序，则可以将其中一些选择性地迁移到 EF Core，其余程序则继续使用 EF6。</span><span class="sxs-lookup"><span data-stu-id="fb94f-107">If you are porting an existing application that has multiple EF models, you can choose to selectively port some of them to EF Core, and continue using EF6 for the others.</span></span>
