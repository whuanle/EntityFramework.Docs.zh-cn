---
title: EF Core 和 EF6 - 哪个版本适合你
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: f0a632902384a65ea3cddf752ad262c7a2e89e2e
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2018
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a>EF Core 和 EF6：哪个版本适合你

以下信息将帮助你在 Entity Framework Core 和 Entity Framework 6 之间作出选择。

## <a name="guidance-for-new-applications"></a>针对新应用程序的选择指南

如果想要利用 EF Core 的所有功能且应用程序不需要任何 EF Core 中尚未实现的功能，请考虑对新应用程序使用 EF Core。

EF6 需要 .NET Framework 4.0（或更高版本）且仅在 Windows 上受支持（即它不在 .NET Core 上运行，在其他操作系统中不受支持），但是，只要这些约束是可接受的，且应用程序不需要 EF6 不适用的 EF Core 的新功能，这对新应用程序仍是一个可行的选择。

查看[功能比较](features.md)以查看 EF Core 是否适合你的应用程序。

## <a name="guidance-for-existing-ef6-applications"></a>针对现有 EF6 应用程序的选择指南

由于 EF Core 有一些根本性变化，我们不建议尝试将 EF6 应用程序迁移至 EF Core，除非你有令人信服的理由进行此项更改。 如果想迁移到 EF Core 以使用新功能，请确保在开始之前了解其限制。 查看[功能比较](features.md)以查看 EF Core 是否适合你的应用程序。

你应该将从 EF6 移至 EF Core 视作迁移而不是升级。 有关详细信息，请参阅[从 EF6 到 EF Core 的迁移](porting/index.md)。
