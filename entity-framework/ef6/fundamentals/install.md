---
title: 获取实体框架的 EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
ms.openlocfilehash: 601f8d123d5494be6a658da1c4ad3743ed50385c
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250876"
---
# <a name="get-entity-framework"></a><span data-ttu-id="b7d60-102">获取实体框架</span><span class="sxs-lookup"><span data-stu-id="b7d60-102">Get Entity Framework</span></span>
<span data-ttu-id="b7d60-103">实体框架的 Visual Studio 和 EF 运行时由组成的 EF 工具。</span><span class="sxs-lookup"><span data-stu-id="b7d60-103">Entity Framework is made up of the EF Tools for Visual Studio and the EF Runtime.</span></span>

## <a name="ef-tools-for-visual-studio"></a><span data-ttu-id="b7d60-104">用于 Visual Studio 的 EF 工具</span><span class="sxs-lookup"><span data-stu-id="b7d60-104">EF Tools for Visual Studio</span></span>

<span data-ttu-id="b7d60-105">适用于 Visual Studio 的实体框架工具包括在 EF 设计器和 EF 模型向导和所需的数据库首次和第一个工作流的模型。</span><span class="sxs-lookup"><span data-stu-id="b7d60-105">The Entity Framework Tools for Visual Studio include the EF Designer and the EF Model Wizard and are required for the database first and model first workflows.</span></span> <span data-ttu-id="b7d60-106">在所有最新版本的 Visual Studio 包含 EF 工具。</span><span class="sxs-lookup"><span data-stu-id="b7d60-106">EF Tools are included in all recent versions of Visual Studio.</span></span> <span data-ttu-id="b7d60-107">如果执行自定义安装的 Visual Studio 将需要确保该项目通过选择包含它的工作负荷或通过选择作为单个组件，则选择"Entity Framework 6 工具"。</span><span class="sxs-lookup"><span data-stu-id="b7d60-107">If you perform a custom install of Visual Studio you will need to ensure that the item "Entity Framework 6 Tools" is selected by either choosing a workload that includes it or by selecting it as an individual component.</span></span>

<span data-ttu-id="b7d60-108">对于某些以前的 Visual Studio 版本，已更新的 EF 工具是可通过下载获得。</span><span class="sxs-lookup"><span data-stu-id="b7d60-108">For some past versions of Visual Studio, updated EF Tools are available as a download.</span></span> <span data-ttu-id="b7d60-109">请参阅[Visual Studio 版本](~/ef6/what-is-new/visual-studio.md)有关如何获取你的 Visual Studio 版本的 EF 工具可用的最新版本的指南。</span><span class="sxs-lookup"><span data-stu-id="b7d60-109">See [Visual Studio Versions](~/ef6/what-is-new/visual-studio.md) for guidance on how to get the latest version of EF Tools available for your version of Visual Studio.</span></span>

## <a name="ef-runtime"></a><span data-ttu-id="b7d60-110">EF 运行时</span><span class="sxs-lookup"><span data-stu-id="b7d60-110">EF Runtime</span></span>

<span data-ttu-id="b7d60-111">实体框架的最新版本是可用作[EntityFramework NuGet 包](http://nuget.org/packages/EntityFramework/)。</span><span class="sxs-lookup"><span data-stu-id="b7d60-111">The latest version of Entity Framework is available as the [EntityFramework NuGet package](http://nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="b7d60-112">如果您不熟悉使用 NuGet 程序包管理器中，我们建议您阅读[NuGet 概述](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow)。</span><span class="sxs-lookup"><span data-stu-id="b7d60-112">If you are not familiar with the NuGet Package Manager, we encourage you to read the [NuGet Overview](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span></span>

### <a name="installing-the-ef-nuget-package"></a><span data-ttu-id="b7d60-113">安装 EF NuGet 包</span><span class="sxs-lookup"><span data-stu-id="b7d60-113">Installing the EF NuGet Package</span></span>

<span data-ttu-id="b7d60-114">可以通过右键单击安装 EntityFramework 包**引用**项目文件夹并选择**管理 NuGet 包...**</span><span class="sxs-lookup"><span data-stu-id="b7d60-114">You can install the EntityFramework package by right-clicking on the **References** folder of your project and selecting **Manage NuGet Packages…**</span></span>

![管理 NuGet 包](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a><span data-ttu-id="b7d60-116">从包管理器控制台安装</span><span class="sxs-lookup"><span data-stu-id="b7d60-116">Installing from Package Manager Console</span></span>

<span data-ttu-id="b7d60-117">或者，可以通过在运行以下命令安装 EntityFramework[程序包管理器控制台](http://docs.nuget.org/docs/start-here/using-the-package-manager-console)。</span><span class="sxs-lookup"><span data-stu-id="b7d60-117">Alternatively, you can install EntityFramework by running the following command in the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a><span data-ttu-id="b7d60-118">安装特定版本的 EF</span><span class="sxs-lookup"><span data-stu-id="b7d60-118">Installing a specific version of EF</span></span>

<span data-ttu-id="b7d60-119">从 EF 4.1 及更高版本，已作为发布新版本的 EF 运行时[EntityFramework NuGet 包](https://www.nuget.org/packages/EntityFramework/)。</span><span class="sxs-lookup"><span data-stu-id="b7d60-119">From EF 4.1 onwards, new versions of the EF runtime have been released as the [EntityFramework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="b7d60-120">任何这些版本可以向基于.NET Framework 的项目添加，通过在 Visual Studio 中运行以下命令[程序包管理器控制台](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):</span><span class="sxs-lookup"><span data-stu-id="b7d60-120">Any of those versions can be added to a .NET Framework-based project by running the following command in Visual Studio's [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):</span></span>

``` powershell
Install-Package EntityFramework -Version <number>
```

<span data-ttu-id="b7d60-121">请注意，`<number>`表示的特定版本的 EF 安装。</span><span class="sxs-lookup"><span data-stu-id="b7d60-121">Note that `<number>` represents the specific version of EF to install.</span></span> <span data-ttu-id="b7d60-122">例如，6.2.0 是 EF 6.2 的版本号。</span><span class="sxs-lookup"><span data-stu-id="b7d60-122">For example, 6.2.0 is the version of number for EF 6.2.</span></span>   

<span data-ttu-id="b7d60-123">EF 4.1 之前的运行时是.NET Framework 的一部分，并且不能单独安装。</span><span class="sxs-lookup"><span data-stu-id="b7d60-123">EF runtimes before 4.1 were part of .NET Framework and cannot be installed separately.</span></span>

### <a name="installing-the-latest-preview"></a><span data-ttu-id="b7d60-124">安装最新预览版</span><span class="sxs-lookup"><span data-stu-id="b7d60-124">Installing the Latest Preview</span></span>

<span data-ttu-id="b7d60-125">上述方法将为您提供最新版本完全支持 Entity Framework 的版本。</span><span class="sxs-lookup"><span data-stu-id="b7d60-125">The above methods will give you the latest fully supported release of Entity Framework.</span></span> <span data-ttu-id="b7d60-126">通常是实体框架可用，我们很高兴您尝试并向我们提供有关反馈的预发布版本。</span><span class="sxs-lookup"><span data-stu-id="b7d60-126">There are often prerelease versions of Entity Framework available that we would love you to try out and give us feedback on.</span></span>

<span data-ttu-id="b7d60-127">若要安装的 EntityFramework 可以选择最新预览版**包括预发行版**管理 NuGet 包窗口中。</span><span class="sxs-lookup"><span data-stu-id="b7d60-127">To install the latest preview of EntityFramework you can select **Include Prerelease** in the Manage NuGet Packages window.</span></span> <span data-ttu-id="b7d60-128">如果不有任何预发布版本将自动获取最新的实体框架的完全受支持的版本。</span><span class="sxs-lookup"><span data-stu-id="b7d60-128">If no prerelease versions are available you will automatically get the latest fully supported version of Entity Framework.</span></span>

![包括预发行版](~/ef6/media/includeprerelease.png)

<span data-ttu-id="b7d60-130">或者，可以运行以下命令[程序包管理器控制台](http://docs.nuget.org/docs/start-here/using-the-package-manager-console)。</span><span class="sxs-lookup"><span data-stu-id="b7d60-130">Alternatively, you can run the following command in the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework -Pre
```
