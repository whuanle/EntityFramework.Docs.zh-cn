---
title: Visual Studio 版本的 EF6
author: divega
ms.date: 2018-07-05
ms.assetid: 028FF890-4EDB-4F03-AE53-72F9C33EC92F
ms.openlocfilehash: aa5409506eeea599421608ddb2dd891df0413961
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997380"
---
# <a name="visual-studio-releases"></a><span data-ttu-id="80d3a-102">Visual Studio 版本</span><span class="sxs-lookup"><span data-stu-id="80d3a-102">Visual Studio Releases</span></span>

<span data-ttu-id="80d3a-103">我们建议始终使用最新版本的 Visual Studio，因为它包含用于.NET、 NuGet 和实体框架的最新的工具。</span><span class="sxs-lookup"><span data-stu-id="80d3a-103">We recommend to always use the latest version of Visual Studio because it contains the latest tools for .NET, NuGet, and Entity Framework.</span></span>
<span data-ttu-id="80d3a-104">事实上，各种示例和跨实体框架文档的演练假定您使用最新版本的 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="80d3a-104">In fact, the various samples and walkthroughs across the Entity Framework documentation assume that you are using a recent version of Visual Studio.</span></span>

<span data-ttu-id="80d3a-105">但是，若要使用较旧版本的 Visual Studio 使用不同版本的实体框架，只要您考虑到帐户一些差异，可以：</span><span class="sxs-lookup"><span data-stu-id="80d3a-105">It is possible however to use older versions of Visual Studio with different versions of Entity Framework as long as you take into account some differences:</span></span>

## <a name="visual-studio-2017-157-and-newer"></a><span data-ttu-id="80d3a-106">Visual Studio 2017 15.7年及更高版本</span><span class="sxs-lookup"><span data-stu-id="80d3a-106">Visual Studio 2017 15.7 and newer</span></span>

- <span data-ttu-id="80d3a-107">此版本的 Visual Studio 包括最新版本的实体框架工具和 EF 6.2 运行时，并且不需要额外的步骤。</span><span class="sxs-lookup"><span data-stu-id="80d3a-107">This version of Visual Studio includes the latest release of Entity Framework tools and the EF 6.2 runtime, and does not require additional setup steps.</span></span>
<span data-ttu-id="80d3a-108">请参阅[What's New](~/ef6/what-is-new/index.md)有关这些版本的详细信息。</span><span class="sxs-lookup"><span data-stu-id="80d3a-108">See [What's New](~/ef6/what-is-new/index.md) for more details on these releases.</span></span>
- <span data-ttu-id="80d3a-109">将实体框架添加到新项目使用 EF 工具将自动添加 EF 6.2 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="80d3a-109">Adding Entity Framework to new projects using the EF tools will automatically add the EF 6.2 NuGet package.</span></span>
<span data-ttu-id="80d3a-110">可以手动安装或联机升级到可用任何 EF NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="80d3a-110">You can manually install or upgrade to any EF NuGet package available online.</span></span>
- <span data-ttu-id="80d3a-111">默认情况下，适用于此版本的 Visual Studio 的 SQL Server 实例是一个 MSSQLLocalDB 命名 LocalDB 实例。</span><span class="sxs-lookup"><span data-stu-id="80d3a-111">By default, the SQL Server instance available with this version of Visual Studio is a LocalDB instance called MSSQLLocalDB.</span></span>
<span data-ttu-id="80d3a-112">连接字符串应使用的服务器部分是"(localdb)\\MSSQLLocalDB"。</span><span class="sxs-lookup"><span data-stu-id="80d3a-112">The server section of connection string you should use is "(localdb)\\MSSQLLocalDB".</span></span>
<span data-ttu-id="80d3a-113">请记住使用前缀为原义字符串`@`声或两个反斜杠"\\\\"在 C# 代码中指定的连接字符串时。</span><span class="sxs-lookup"><span data-stu-id="80d3a-113">Remember to use a verbatim string prefixed with `@` or double back-slashes "\\\\" when specifying a connection string in C# code.</span></span>  


## <a name="visual-studio-2015-to-visual-studio-2017-156"></a><span data-ttu-id="80d3a-114">Visual Studio 2015，转至 Visual Studio 2017 15.6 版</span><span class="sxs-lookup"><span data-stu-id="80d3a-114">Visual Studio 2015 to Visual Studio 2017 15.6</span></span>

- <span data-ttu-id="80d3a-115">这些版本的 Visual Studio 包括实体框架工具和运行时 6.1.3。</span><span class="sxs-lookup"><span data-stu-id="80d3a-115">These versions of Visual Studio include Entity Framework tools and runtime 6.1.3.</span></span>
<span data-ttu-id="80d3a-116">请参阅[过去释放](~/ef6/what-is-new/past-releases.md#ef-613)有关这些版本的详细信息。</span><span class="sxs-lookup"><span data-stu-id="80d3a-116">See [Past Releases](~/ef6/what-is-new/past-releases.md#ef-613) for more details on these releases.</span></span>
- <span data-ttu-id="80d3a-117">将实体框架添加到新项目使用 EF 工具将自动添加 EF 6.1.3 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="80d3a-117">Adding Entity Framework to new projects using the EF tools will automatically add the EF 6.1.3 NuGet package.</span></span>
<span data-ttu-id="80d3a-118">可以手动安装或联机升级到可用任何 EF NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="80d3a-118">You can manually install or upgrade to any EF NuGet package available online.</span></span>
- <span data-ttu-id="80d3a-119">默认情况下，适用于此版本的 Visual Studio 的 SQL Server 实例是一个 MSSQLLocalDB 命名 LocalDB 实例。</span><span class="sxs-lookup"><span data-stu-id="80d3a-119">By default, the SQL Server instance available with this version of Visual Studio is a LocalDB instance called MSSQLLocalDB.</span></span>
<span data-ttu-id="80d3a-120">连接字符串应使用的服务器部分是"(localdb)\\MSSQLLocalDB"。</span><span class="sxs-lookup"><span data-stu-id="80d3a-120">The server section of connection string you should use is "(localdb)\\MSSQLLocalDB".</span></span>
<span data-ttu-id="80d3a-121">请记住使用前缀为原义字符串`@`声或两个反斜杠"\\\\"在 C# 代码中指定的连接字符串时。</span><span class="sxs-lookup"><span data-stu-id="80d3a-121">Remember to use a verbatim string prefixed with `@` or double back-slashes "\\\\" when specifying a connection string in C# code.</span></span>  


## <a name="visual-studio-2013"></a><span data-ttu-id="80d3a-122">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="80d3a-122">Visual Studio 2013</span></span>
- <span data-ttu-id="80d3a-123">此版本的 Visual Studio 包括和旧版本的实体框架工具和运行时。</span><span class="sxs-lookup"><span data-stu-id="80d3a-123">This version of Visual Studio includes and older version of Entity Framework tools and runtime.</span></span>
<span data-ttu-id="80d3a-124">我们建议您升级到 Entity Framework Tools 6.1.3，使用[安装程序](https://www.microsoft.com/en-us/download/details.aspx?id=40762)Microsoft Download Center 中可用。</span><span class="sxs-lookup"><span data-stu-id="80d3a-124">It is recommended that you upgrade to Entity Framework Tools 6.1.3, using [the installer](https://www.microsoft.com/en-us/download/details.aspx?id=40762) available in the Microsoft Download Center.</span></span>
<span data-ttu-id="80d3a-125">请参阅[过去释放](~/ef6/what-is-new/past-releases.md#ef-613)有关这些版本的详细信息。</span><span class="sxs-lookup"><span data-stu-id="80d3a-125">See [Past Releases](~/ef6/what-is-new/past-releases.md#ef-613) for more details on these releases.</span></span>
- <span data-ttu-id="80d3a-126">将实体框架添加到新项目使用升级后的 EF 工具将自动添加 EF 6.1.3 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="80d3a-126">Adding Entity Framework to new projects using the upgraded EF tools will automatically add the EF 6.1.3 NuGet package.</span></span>
<span data-ttu-id="80d3a-127">可以手动安装或联机升级到可用任何 EF NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="80d3a-127">You can manually install or upgrade to any EF NuGet package available online.</span></span>
- <span data-ttu-id="80d3a-128">默认情况下，适用于此版本的 Visual Studio 的 SQL Server 实例是一个 MSSQLLocalDB 命名 LocalDB 实例。</span><span class="sxs-lookup"><span data-stu-id="80d3a-128">By default, the SQL Server instance available with this version of Visual Studio is a LocalDB instance called MSSQLLocalDB.</span></span>
<span data-ttu-id="80d3a-129">连接字符串应使用的服务器部分是"(localdb)\\MSSQLLocalDB"。</span><span class="sxs-lookup"><span data-stu-id="80d3a-129">The server section of connection string you should use is "(localdb)\\MSSQLLocalDB".</span></span>
<span data-ttu-id="80d3a-130">请记住使用前缀为原义字符串`@`声或两个反斜杠"\\\\"在 C# 代码中指定的连接字符串时。</span><span class="sxs-lookup"><span data-stu-id="80d3a-130">Remember to use a verbatim string prefixed with `@` or double back-slashes "\\\\" when specifying a connection string in C# code.</span></span>  

## <a name="visual-studio-2012"></a><span data-ttu-id="80d3a-131">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="80d3a-131">Visual Studio 2012</span></span>

- <span data-ttu-id="80d3a-132">此版本的 Visual Studio 包括和旧版本的实体框架工具和运行时。</span><span class="sxs-lookup"><span data-stu-id="80d3a-132">This version of Visual Studio includes and older version of Entity Framework tools and runtime.</span></span>
<span data-ttu-id="80d3a-133">我们建议您升级到 Entity Framework Tools 6.1.3，使用[安装程序](https://www.microsoft.com/en-us/download/details.aspx?id=40762)Microsoft Download Center 中可用。</span><span class="sxs-lookup"><span data-stu-id="80d3a-133">It is recommended that you upgrade to Entity Framework Tools 6.1.3, using [the installer](https://www.microsoft.com/en-us/download/details.aspx?id=40762) available in the Microsoft Download Center.</span></span>
<span data-ttu-id="80d3a-134">请参阅[过去释放](~/ef6/what-is-new/past-releases.md#ef-613)有关这些版本的详细信息。</span><span class="sxs-lookup"><span data-stu-id="80d3a-134">See [Past Releases](~/ef6/what-is-new/past-releases.md#ef-613) for more details on these releases.</span></span>
- <span data-ttu-id="80d3a-135">将实体框架添加到新项目使用升级后的 EF 工具将自动添加 EF 6.1.3 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="80d3a-135">Adding Entity Framework to new projects using the upgraded EF tools will automatically add the EF 6.1.3 NuGet package.</span></span>
<span data-ttu-id="80d3a-136">可以手动安装或联机升级到可用任何 EF NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="80d3a-136">You can manually install or upgrade to any EF NuGet package available online.</span></span>
- <span data-ttu-id="80d3a-137">默认情况下，可使用此版本的 Visual Studio 的 SQL Server 实例是名为 v11.0 的 LocalDB 实例。</span><span class="sxs-lookup"><span data-stu-id="80d3a-137">By default, the SQL Server instance available with this version of Visual Studio is a LocalDB instance called v11.0.</span></span>
<span data-ttu-id="80d3a-138">连接字符串应使用的服务器部分是"(localdb)\\v11.0"。</span><span class="sxs-lookup"><span data-stu-id="80d3a-138">The server section of connection string you should use is "(localdb)\\v11.0".</span></span>
<span data-ttu-id="80d3a-139">请记住使用前缀为原义字符串`@`声或两个反斜杠"\\\\"在 C# 代码中指定的连接字符串时。</span><span class="sxs-lookup"><span data-stu-id="80d3a-139">Remember to use a verbatim string prefixed with `@` or double back-slashes "\\\\" when specifying a connection string in C# code.</span></span>  

## <a name="visual-studio-2010"></a><span data-ttu-id="80d3a-140">Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="80d3a-140">Visual Studio 2010</span></span>

- <span data-ttu-id="80d3a-141">与此版本的 Visual Studio 提供的实体框架工具的版本与 Entity Framework 6 运行时不兼容，并且不能升级。</span><span class="sxs-lookup"><span data-stu-id="80d3a-141">The version of Entity Framework Tools available with this version of Visual Studio is not compatible with the Entity Framework 6 runtime and cannot be upgraded.</span></span>
- <span data-ttu-id="80d3a-142">默认情况下，实体框架工具将向你的项目添加 Entity Framework 4.0。</span><span class="sxs-lookup"><span data-stu-id="80d3a-142">By default, the Entity Framework tools will add Entity Framework 4.0 to your projects.</span></span>
<span data-ttu-id="80d3a-143">若要创建使用任何较新版本的 EF 应用程序，首先需要安装[NuGet 包管理器扩展](https://marketplace.visualstudio.com/items?itemName=NuGetTeam.NuGetPackageManager)。</span><span class="sxs-lookup"><span data-stu-id="80d3a-143">In order to create applications using any newer versions of EF, you will first need to install the [NuGet Package Manager extension](https://marketplace.visualstudio.com/items?itemName=NuGetTeam.NuGetPackageManager).</span></span>
- <span data-ttu-id="80d3a-144">默认情况下，EF 工具的版本中的所有代码生成都基于 EntityObject 和 Entity Framework 4。</span><span class="sxs-lookup"><span data-stu-id="80d3a-144">By default, all code generation in the version of EF tools is based on EntityObject and Entity Framework 4.</span></span>
<span data-ttu-id="80d3a-145">我们推荐你切换代码生成通过安装适用于的 DbContext 代码生成模板基于 DbContext 和 Entity Framework 5 [C#](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforC)或[Visual Basic](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforVBNET)。</span><span class="sxs-lookup"><span data-stu-id="80d3a-145">We recommend that you switch the code generation to be based on DbContext and Entity Framework 5, by installing the DbContext code generation templates for [C#](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforC) or [Visual Basic](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforVBNET).</span></span>
- <span data-ttu-id="80d3a-146">一旦您已安装的 NuGet 包管理器扩展，您可以手动安装或联机升级到可用任何 EF NuGet 包并使用 EF6 Code First，它不需要使用设计器。</span><span class="sxs-lookup"><span data-stu-id="80d3a-146">Once you have installed the NuGet Package Manager extensions, you can manually install or upgrade to any EF NuGet package available online and use EF6 with Code First, which does not require a designer.</span></span>
- <span data-ttu-id="80d3a-147">默认情况下，可使用此版本的 Visual Studio 的 SQL Server 实例是名为 SQLEXPRESS 的 SQL Server Express。</span><span class="sxs-lookup"><span data-stu-id="80d3a-147">By default, the SQL Server instance available with this version of Visual Studio is SQL Server Express named SQLEXPRESS.</span></span>
<span data-ttu-id="80d3a-148">连接字符串应使用的服务器部分是"。\\SQLEXPRESS"。</span><span class="sxs-lookup"><span data-stu-id="80d3a-148">The server section of connection string you should use is ".\\SQLEXPRESS".</span></span>
<span data-ttu-id="80d3a-149">请记住使用前缀为原义字符串`@`声或两个反斜杠"\\\\"在 C# 代码中指定的连接字符串时。</span><span class="sxs-lookup"><span data-stu-id="80d3a-149">Remember to use a verbatim string prefixed with `@` or double back-slashes "\\\\" when specifying a connection string in C# code.</span></span>
