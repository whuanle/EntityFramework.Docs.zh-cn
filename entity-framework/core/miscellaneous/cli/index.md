---
title: 命令行参考 - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
ms.openlocfilehash: d43b01fc61bb1c9b678e12e41c27d7efe9a59fa5
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490358"
---
<a name="entity-framework-core-tools"></a><span data-ttu-id="3e81d-102">Entity Framework Core 工具</span><span class="sxs-lookup"><span data-stu-id="3e81d-102">Entity Framework Core Tools</span></span>
===========================
<span data-ttu-id="3e81d-103">Entity Framework Core 工具有助于 EF Core 应用的开发。</span><span class="sxs-lookup"><span data-stu-id="3e81d-103">The Entity Framework Core Tools help you during the development of EF Core apps.</span></span> <span data-ttu-id="3e81d-104">它们主要用于通过对数据库架构进行反向工程来建立 DbContext 和实体类型的基架，以及用于管理迁移。</span><span class="sxs-lookup"><span data-stu-id="3e81d-104">They're primarily used to scaffold a DbContext and entity types by reverse engineering the schema of a database, and to manage Migrations.</span></span>

<span data-ttu-id="3e81d-105">[EF Core 包管理器控制台 (PMC) 工具][1]在 Visual Studio 内提供卓越体验。</span><span class="sxs-lookup"><span data-stu-id="3e81d-105">The [EF Core Package Manager Console (PMC) Tools][1] provide a superior experience inside Visual Studio.</span></span> <span data-ttu-id="3e81d-106">使用 NuGet 的[包管理器控制台][2]运行这些工具。</span><span class="sxs-lookup"><span data-stu-id="3e81d-106">Run them using NuGet's [Package Manager Console][2].</span></span> <span data-ttu-id="3e81d-107">这些工具同时适用于 .NET Framework 和 .NET Core 项目。</span><span class="sxs-lookup"><span data-stu-id="3e81d-107">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="3e81d-108">[EF Core .NET 命令行工具][3]是 [.NET Core 命令行接口 (CLI) 工具][4]（这些工具是跨平台的，可在 Visual Studio 之外运行）的扩展。</span><span class="sxs-lookup"><span data-stu-id="3e81d-108">The [EF Core .NET Command-line Tools][3] are an extension to the [.NET Core command-line interface (CLI) tools][4] that are cross-platform and can run outside of Visual Studio.</span></span> <span data-ttu-id="3e81d-109">这些工具需要 .NET Core SDK 项目（具有 `Sdk="Microsoft.NET.Sdk"` 的项目或项目文件中的相似项目）。</span><span class="sxs-lookup"><span data-stu-id="3e81d-109">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="3e81d-110">这两种工具提供相同的功能。</span><span class="sxs-lookup"><span data-stu-id="3e81d-110">Both tools expose the same functionality.</span></span> <span data-ttu-id="3e81d-111">如果使用 Visual Studio 进行开发，我们建议使用 PMC 工具，因为这些工具提供更完整的体验。</span><span class="sxs-lookup"><span data-stu-id="3e81d-111">If you're developing in Visual Studio, we recommend using the PMC Tools since they provide a more integrated experience.</span></span>

<a name="frameworks"></a><span data-ttu-id="3e81d-112">框架</span><span class="sxs-lookup"><span data-stu-id="3e81d-112">Frameworks</span></span>
----------
<span data-ttu-id="3e81d-113">这些工具支持面向 .NET Framework 或 .NET Core 的项目。</span><span class="sxs-lookup"><span data-stu-id="3e81d-113">The tools support projects targeting .NET Framework or .NET Core.</span></span>

<span data-ttu-id="3e81d-114">如果想要使用类库，如有可能，请考虑使用 .NET Core 或 .NET Framework 类库。</span><span class="sxs-lookup"><span data-stu-id="3e81d-114">If you want to use a class library, then consider using a .NET Core or .NET Framework class library if possible.</span></span> <span data-ttu-id="3e81d-115">这样，使用 .NET 工具的问题最少。</span><span class="sxs-lookup"><span data-stu-id="3e81d-115">This will result in the least issues with .NET tooling.</span></span> <span data-ttu-id="3e81d-116">而如果想要使用 .NET Standard 类库，则需要使用面向 .NET Framework 或 .NET Core 的启动项目，使该工具具有可在其中加载类库的具体目标平台。</span><span class="sxs-lookup"><span data-stu-id="3e81d-116">If instead you wish to use a .NET Standard class library, then you will need to use a startup project that targets .NET Framework or .NET Core so that the tooling has a conrete target platform into which it can load your class library.</span></span> <span data-ttu-id="3e81d-117">此启动项目可以是不包含实际代码的虚拟项目 - 需要它的唯一理由是为工具提供一个目标。</span><span class="sxs-lookup"><span data-stu-id="3e81d-117">This startup project can be a dummy project with no real code--it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="3e81d-118">如果项目面向其他框架（例如，通用 Windows 或 Xamarin），则需要创建一个单独的 .NET Standard 类库。</span><span class="sxs-lookup"><span data-stu-id="3e81d-118">If your project targets another framework (for example, Universal Windows or Xamarin), then you will need to create a separate .NET Standard class library.</span></span> <span data-ttu-id="3e81d-119">在这种情况下，请按照上述指南，也创建一个工具可使用的启动项目。</span><span class="sxs-lookup"><span data-stu-id="3e81d-119">In this case, follow the guidance above to also create a startup project that can be used by the tooling.</span></span>

<a name="startup-and-target-projects"></a><span data-ttu-id="3e81d-120">启动项目和目标项目</span><span class="sxs-lookup"><span data-stu-id="3e81d-120">Startup and Target Projects</span></span>
---------------------------
<span data-ttu-id="3e81d-121">每当调用命令时，始终会涉及两个项目：目标项目和启动项目。</span><span class="sxs-lookup"><span data-stu-id="3e81d-121">Whenever you invoke a command, there are two projects involved: the target project and the startup project.</span></span>

<span data-ttu-id="3e81d-122">目标项目是在其中添加任何文件（或在某些情况下删除文件）的项目。</span><span class="sxs-lookup"><span data-stu-id="3e81d-122">The target project is where any files are added (or in some cases removed).</span></span>

<span data-ttu-id="3e81d-123">启动项目是执行项目代码时由工具模拟的项目。</span><span class="sxs-lookup"><span data-stu-id="3e81d-123">The startup project is the one emulated by the tools when executing your project's code.</span></span>

<span data-ttu-id="3e81d-124">目标项目和启动项目可以相同。</span><span class="sxs-lookup"><span data-stu-id="3e81d-124">Both the target project and the startup project can be the same.</span></span>


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
