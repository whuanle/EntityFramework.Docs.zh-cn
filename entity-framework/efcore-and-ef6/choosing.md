---
title: EF Core 和 EF6 - 哪个版本适合你
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 17c81e0d6c384c06e45f0cf4f338d4ba402788e1
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949135"
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a><span data-ttu-id="9e450-102">EF Core 和 EF6：哪个版本适合你</span><span class="sxs-lookup"><span data-stu-id="9e450-102">EF Core and EF6: Which One Is Right for You</span></span>

<span data-ttu-id="9e450-103">以下信息将帮助你在 Entity Framework Core 和 Entity Framework 6 之间作出选择。</span><span class="sxs-lookup"><span data-stu-id="9e450-103">The following information will help you choose between Entity Framework Core and Entity Framework 6.</span></span>

## <a name="guidance-for-new-applications"></a><span data-ttu-id="9e450-104">针对新应用程序的选择指南</span><span class="sxs-lookup"><span data-stu-id="9e450-104">Guidance for new applications</span></span>

<span data-ttu-id="9e450-105">如果想要利用 EF Core 的所有功能且应用程序不需要任何 EF Core 中尚未实现的功能，请考虑对新应用程序使用 EF Core。</span><span class="sxs-lookup"><span data-stu-id="9e450-105">Consider using EF Core for new applications if you want to take advantage of the all the capabilities of EF Core and your application does not require any features that are not yet implemented in EF Core.</span></span>

<span data-ttu-id="9e450-106">EF6 需要 .NET Framework 4.0 或更高版本，且仅在 Windows 上受支持（即 EF6 当前不在 .NET Core 上运行且在其他操作系统中不受支持），但只要这些约束是可接受的且应用程序无需使用 EF6 不适用的 EF Core 的新功能，新的应用程序仍可选择使用它。</span><span class="sxs-lookup"><span data-stu-id="9e450-106">EF6 requires .NET Framework 4.0 (or a later version) and is only supported on Windows (that is, EF6 does not currently run on .NET Core and is not supported in other operating systems), but it is still a viable choice for new applications as long those constraints are acceptable and the application does not require new features in EF Core that are not available to EF6.</span></span>

<span data-ttu-id="9e450-107">查看[功能比较](features.md)以查看 EF Core 是否适合你的应用程序。</span><span class="sxs-lookup"><span data-stu-id="9e450-107">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

## <a name="guidance-for-existing-ef6-applications"></a><span data-ttu-id="9e450-108">针对现有 EF6 应用程序的选择指南</span><span class="sxs-lookup"><span data-stu-id="9e450-108">Guidance for existing EF6 applications</span></span>

<span data-ttu-id="9e450-109">由于 EF Core 有一些根本性变化，我们不建议尝试将 EF6 应用程序迁移至 EF Core，除非你有令人信服的理由进行此项更改。</span><span class="sxs-lookup"><span data-stu-id="9e450-109">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span> <span data-ttu-id="9e450-110">如果想迁移到 EF Core 以使用新功能，请确保在开始之前了解其限制。</span><span class="sxs-lookup"><span data-stu-id="9e450-110">If you want to move to EF Core to make use of new features, then make sure you are aware of its limitations before you start.</span></span> <span data-ttu-id="9e450-111">查看[功能比较](features.md)以查看 EF Core 是否适合你的应用程序。</span><span class="sxs-lookup"><span data-stu-id="9e450-111">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

<span data-ttu-id="9e450-112">你应该将从 EF6 移至 EF Core 视作迁移而不是升级。</span><span class="sxs-lookup"><span data-stu-id="9e450-112">**You should view the move from EF6 to EF Core as a port rather than an upgrade.**</span></span> <span data-ttu-id="9e450-113">有关详细信息，请参阅[从 EF6 到 EF Core 的迁移](porting/index.md)。</span><span class="sxs-lookup"><span data-stu-id="9e450-113">See [Porting from EF6 to EF Core](porting/index.md) for more information.</span></span>
