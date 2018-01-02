---
title: "命令行参考 - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 076e9251850ba10df323cd25922aa8b95b3a5491
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
---
<a name="entity-framework-core-tools"></a><span data-ttu-id="d2cb8-102">Entity Framework Core 工具</span><span class="sxs-lookup"><span data-stu-id="d2cb8-102">Entity Framework Core Tools</span></span>
===========================
<span data-ttu-id="d2cb8-103">Entity Framework Core 工具有助于 EF Core 应用的开发。</span><span class="sxs-lookup"><span data-stu-id="d2cb8-103">The Entity Framework Core Tools help you during the development of EF Core apps.</span></span> <span data-ttu-id="d2cb8-104">它们主要用于通过对数据库架构进行反向工程来建立 DbContext 和实体类型的基架，以及用于管理迁移。</span><span class="sxs-lookup"><span data-stu-id="d2cb8-104">They're primarily used to scaffold a DbContext and entity types by reverse engineering the schema of a database, and to manage Migrations.</span></span>

<span data-ttu-id="d2cb8-105">[EF Core 包管理器控制台 (PMC) 工具][1]在 Visual Studio 内提供卓越体验。</span><span class="sxs-lookup"><span data-stu-id="d2cb8-105">The [EF Core Package Manager Console (PMC) Tools][1] provide a superior experience inside Visual Studio.</span></span> <span data-ttu-id="d2cb8-106">使用 NuGet 的[包管理器控制台][2]运行这些工具。</span><span class="sxs-lookup"><span data-stu-id="d2cb8-106">Run them using NuGet's [Package Manager Console][2].</span></span> <span data-ttu-id="d2cb8-107">这些工具同时适用于 .NET Framework 和 .NET Core 项目。</span><span class="sxs-lookup"><span data-stu-id="d2cb8-107">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="d2cb8-108">[EF Core .NET 命令行工具][3]是 [.NET Core 命令行接口 (CLI) 工具][4]（这些工具是跨平台的，可在 Visual Studio 之外运行）的扩展。</span><span class="sxs-lookup"><span data-stu-id="d2cb8-108">The [EF Core .NET Command-line Tools][3] are an extension to the [.NET Core command-line interface (CLI) tools][4] that are cross-platform and can run outside of Visual Studio.</span></span> <span data-ttu-id="d2cb8-109">这些工具需要 .NET Core SDK 项目（具有 `Sdk="Microsoft.NET.Sdk"` 的项目或项目文件中的相似项目）。</span><span class="sxs-lookup"><span data-stu-id="d2cb8-109">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="d2cb8-110">这两种工具提供相同的功能。</span><span class="sxs-lookup"><span data-stu-id="d2cb8-110">Both tools expose the same functionality.</span></span> <span data-ttu-id="d2cb8-111">如果使用 Visual Studio 进行开发，我们建议使用 PMC 工具，因为这些工具提供更完整的体验。</span><span class="sxs-lookup"><span data-stu-id="d2cb8-111">If you're developing in Visual Studio, we recommend using the PMC Tools since they provide a more integrated experience.</span></span>

<a name="frameworks"></a><span data-ttu-id="d2cb8-112">框架</span><span class="sxs-lookup"><span data-stu-id="d2cb8-112">Frameworks</span></span>
----------
<span data-ttu-id="d2cb8-113">这些工具支持面向 .NET Framework 或 .NET Core 的项目。</span><span class="sxs-lookup"><span data-stu-id="d2cb8-113">The tools support projects targeting .NET Framework or .NET Core.</span></span>

<span data-ttu-id="d2cb8-114">如果项目面向另一框架（例如通用 Windows 或 Xamarin），我们建议创建一个单独的 .NET Standard 项目并交叉定向其中一个受支持框架。</span><span class="sxs-lookup"><span data-stu-id="d2cb8-114">If your project targets another framework (for example, Universal Windows or Xamarin), we recommend creating a separate .NET Standard project and cross-targeting one of the supported frameworks.</span></span>

<span data-ttu-id="d2cb8-115">例如，要交叉定向 .NET Core，请右键单击项目，然后选择“编辑 \*.csproj”。</span><span class="sxs-lookup"><span data-stu-id="d2cb8-115">To cross-target .NET Core, for example, right-click on the project and select **Edit \*.csproj**.</span></span> <span data-ttu-id="d2cb8-116">按照如下所示更新 `TargetFramework` 属性。</span><span class="sxs-lookup"><span data-stu-id="d2cb8-116">Update the `TargetFramework` property as follows.</span></span> <span data-ttu-id="d2cb8-117">（请注意，属性名称将变为复数形式。）</span><span class="sxs-lookup"><span data-stu-id="d2cb8-117">(Note, the property name becomes plural.)</span></span>

``` xml
<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>
```

<span data-ttu-id="d2cb8-118">使用 .NET Standard 类库时，如果启动项目面向 .NET Framework 或 .NET Core，则无需进行交叉定向。</span><span class="sxs-lookup"><span data-stu-id="d2cb8-118">If you're using a .NET Standard class library, you don't need to cross-target if your startup project targets .NET Framework or .NET Core.</span></span>

<a name="startup-and-target-projects"></a><span data-ttu-id="d2cb8-119">启动项目和目标项目</span><span class="sxs-lookup"><span data-stu-id="d2cb8-119">Startup and Target Projects</span></span>
---------------------------
<span data-ttu-id="d2cb8-120">每当调用命令时，始终会涉及两个项目：目标项目和启动项目。</span><span class="sxs-lookup"><span data-stu-id="d2cb8-120">Whenever you invoke a command, there are two projects involved: the target project and the startup project.</span></span>

<span data-ttu-id="d2cb8-121">目标项目是在其中添加任何文件（或在某些情况下删除文件）的项目。</span><span class="sxs-lookup"><span data-stu-id="d2cb8-121">The target project is where any files are added (or in some cases removed).</span></span>

<span data-ttu-id="d2cb8-122">启动项目是执行项目代码时由工具模拟的项目。</span><span class="sxs-lookup"><span data-stu-id="d2cb8-122">The startup project is the one emulated by the tools when executing your project's code.</span></span>

<span data-ttu-id="d2cb8-123">目标项目和启动项目可以相同。</span><span class="sxs-lookup"><span data-stu-id="d2cb8-123">Both the target project and the startup project can be the same.</span></span>


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
