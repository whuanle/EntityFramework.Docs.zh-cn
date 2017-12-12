---
title: "EF Core 和 EF6 - 哪个版本适合你"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 9a113e0965fa75a03510199fb75165f6e9be0bbd
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a>EF Core 和 EF6：哪个版本适合你

以下信息将帮助你在 Entity Framework Core 和 Entity Framework 6 之间作出选择。

## <a name="guidance-for-new-applications"></a>针对新应用程序的选择指南

由于 EF Core 是一款新产品，并且仍然缺乏一些重要的 O/RM 功能，因此，EF6 仍然是许多应用程序的最佳选择。

**以下是我们建议使用 EF Core 的应用程序类型：**

* 新的应用程序不需要在 EF Core 中尚未实现的功能。 查看[功能比较](features.md)以查看 EF Core 是否适合你的应用程序。

* 面向 .NET Core 的应用程序，如通用 Windows平台 (UWP) 和 ASP.NET Core 应用程序。 这些应用程序无法使用 EF6，因为它需要 .NET Framework（即 .NET Framework 4.5）。

## <a name="guidance-for-existing-ef6-applications"></a>针对现有 EF6 应用程序的选择指南

由于 EF Core 有一些根本性变化，我们不建议尝试将 EF6 应用程序迁移至 EF Core，除非你有令人信服的理由进行此项更改。 如果想迁移到 EF Core 以使用新功能，请确保在开始之前了解其限制。 查看[功能比较](features.md)以查看 EF Core 是否适合你的应用程序。

你应该将从 EF6 移至 EF Core 视作迁移而不是升级。 有关详细信息，请参阅[从 EF6 到 EF Core 的迁移](porting/index.md)。
