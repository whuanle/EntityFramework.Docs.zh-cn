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
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a><span data-ttu-id="84f69-102">EF Core 和 EF6：哪个版本适合你</span><span class="sxs-lookup"><span data-stu-id="84f69-102">EF Core and EF6: Which One Is Right for You</span></span>

<span data-ttu-id="84f69-103">以下信息将帮助你在 Entity Framework Core 和 Entity Framework 6 之间作出选择。</span><span class="sxs-lookup"><span data-stu-id="84f69-103">The following information will help you choose between Entity Framework Core and Entity Framework 6.</span></span>

## <a name="guidance-for-new-applications"></a><span data-ttu-id="84f69-104">针对新应用程序的选择指南</span><span class="sxs-lookup"><span data-stu-id="84f69-104">Guidance for new applications</span></span>

<span data-ttu-id="84f69-105">由于 EF Core 是一款新产品，并且仍然缺乏一些重要的 O/RM 功能，因此，EF6 仍然是许多应用程序的最佳选择。</span><span class="sxs-lookup"><span data-stu-id="84f69-105">Because EF Core is a new product, and still lacks some critical O/RM features, EF6 will still be the most suitable choice for many applications.</span></span>

<span data-ttu-id="84f69-106">**以下是我们建议使用 EF Core 的应用程序类型：**</span><span class="sxs-lookup"><span data-stu-id="84f69-106">**These are the types of applications we would recommend using EF Core for:**</span></span>

* <span data-ttu-id="84f69-107">新的应用程序不需要在 EF Core 中尚未实现的功能。</span><span class="sxs-lookup"><span data-stu-id="84f69-107">New applications that do not require features that are not yet implemented in EF Core.</span></span> <span data-ttu-id="84f69-108">查看[功能比较](features.md)以查看 EF Core 是否适合你的应用程序。</span><span class="sxs-lookup"><span data-stu-id="84f69-108">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

* <span data-ttu-id="84f69-109">面向 .NET Core 的应用程序，如通用 Windows平台 (UWP) 和 ASP.NET Core 应用程序。</span><span class="sxs-lookup"><span data-stu-id="84f69-109">Applications that target .NET Core, such as Universal Windows Platform (UWP) and ASP.NET Core applications.</span></span> <span data-ttu-id="84f69-110">这些应用程序无法使用 EF6，因为它需要 .NET Framework（即 .NET Framework 4.5）。</span><span class="sxs-lookup"><span data-stu-id="84f69-110">These applications can not use EF6 as it requires the .NET Framework (i.e. .NET Framework 4.5).</span></span>

## <a name="guidance-for-existing-ef6-applications"></a><span data-ttu-id="84f69-111">针对现有 EF6 应用程序的选择指南</span><span class="sxs-lookup"><span data-stu-id="84f69-111">Guidance for existing EF6 applications</span></span>

<span data-ttu-id="84f69-112">由于 EF Core 有一些根本性变化，我们不建议尝试将 EF6 应用程序迁移至 EF Core，除非你有令人信服的理由进行此项更改。</span><span class="sxs-lookup"><span data-stu-id="84f69-112">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span> <span data-ttu-id="84f69-113">如果想迁移到 EF Core 以使用新功能，请确保在开始之前了解其限制。</span><span class="sxs-lookup"><span data-stu-id="84f69-113">If you want to move to EF Core to make use of new features, then make sure you are aware of its limitations before you start.</span></span> <span data-ttu-id="84f69-114">查看[功能比较](features.md)以查看 EF Core 是否适合你的应用程序。</span><span class="sxs-lookup"><span data-stu-id="84f69-114">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

<span data-ttu-id="84f69-115">你应该将从 EF6 移至 EF Core 视作迁移而不是升级。</span><span class="sxs-lookup"><span data-stu-id="84f69-115">**You should view the move from EF6 to EF Core as a port rather than an upgrade.**</span></span> <span data-ttu-id="84f69-116">有关详细信息，请参阅[从 EF6 到 EF Core 的迁移](porting/index.md)。</span><span class="sxs-lookup"><span data-stu-id="84f69-116">See [Porting from EF6 to EF Core](porting/index.md) for more information.</span></span>
