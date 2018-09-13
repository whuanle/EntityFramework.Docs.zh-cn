---
title: 针对空间类型-EF6 的提供程序支持
author: divega
ms.date: 10/23/2016
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: ffd22222f59a541d8135d3738d37a7e8f5dc5d7c
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489747"
---
# <a name="provider-support-for-spatial-types"></a><span data-ttu-id="47e96-102">针对空间类型的提供程序支持</span><span class="sxs-lookup"><span data-stu-id="47e96-102">Provider Support for Spatial Types</span></span>
<span data-ttu-id="47e96-103">实体框架支持使用通过 DbGeography 或 DbGeometry 类的空间数据。</span><span class="sxs-lookup"><span data-stu-id="47e96-103">Entity Framework supports working with spatial data through the DbGeography or DbGeometry classes.</span></span> <span data-ttu-id="47e96-104">这些类要依赖于实体框架提供程序提供特定于数据库的功能。</span><span class="sxs-lookup"><span data-stu-id="47e96-104">These classes rely on database-specific functionality offered by the Entity Framework provider.</span></span> <span data-ttu-id="47e96-105">并非所有提供程序支持空间数据，这样做的那些可能具有如的空间类型程序集安装的其他先决条件。</span><span class="sxs-lookup"><span data-stu-id="47e96-105">Not all providers support spatial data and those that do may have additional prerequisites such as the installation of spatial type assemblies.</span></span> <span data-ttu-id="47e96-106">下面提供了有关针对空间类型的提供程序支持的详细信息。</span><span class="sxs-lookup"><span data-stu-id="47e96-106">More information about provider support for spatial types is provided below.</span></span>  

<span data-ttu-id="47e96-107">可以在两个演练中，一个用于 Code First，另一个用于 Database First 或 Model First 找到如何在应用程序中使用的空间类型的其他信息：</span><span class="sxs-lookup"><span data-stu-id="47e96-107">Additional information on how to use spatial types in an application can be found in two walkthroughs, one for Code First, the other for Database First or Model First:</span></span>  

- [<span data-ttu-id="47e96-108">在代码中首先空间数据类型。</span><span class="sxs-lookup"><span data-stu-id="47e96-108">Spatial Data Types in Code First</span></span>](~/ef6/modeling/code-first/data-types/spatial.md)  
- [<span data-ttu-id="47e96-109">在 EF 设计器中的空间数据类型</span><span class="sxs-lookup"><span data-stu-id="47e96-109">Spatial Data Types in EF Designer</span></span>](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a><span data-ttu-id="47e96-110">支持空间类型的 EF 版本</span><span class="sxs-lookup"><span data-stu-id="47e96-110">EF releases that support spatial types</span></span>  

<span data-ttu-id="47e96-111">在 EF5 中引入了针对空间类型的支持。</span><span class="sxs-lookup"><span data-stu-id="47e96-111">Support for spatial types was introduced in EF5.</span></span> <span data-ttu-id="47e96-112">但是，在 EF5 空间类型才支持面向和.NET 4.5 上运行应用程序时。</span><span class="sxs-lookup"><span data-stu-id="47e96-112">However, in EF5 spatial types are only supported when the application targets and runs on .NET 4.5.</span></span>  

<span data-ttu-id="47e96-113">从 EF6 空间类型支持面向.NET 4 和.NET 4.5 的应用程序开始。</span><span class="sxs-lookup"><span data-stu-id="47e96-113">Starting with EF6 spatial types are supported for applications targeting both .NET 4 and .NET 4.5.</span></span>  

## <a name="ef-providers-that-support-spatial-types"></a><span data-ttu-id="47e96-114">支持空间类型的 EF 提供程序</span><span class="sxs-lookup"><span data-stu-id="47e96-114">EF providers that support spatial types</span></span>  

### <a name="ef5"></a><span data-ttu-id="47e96-115">EF5</span><span class="sxs-lookup"><span data-stu-id="47e96-115">EF5</span></span>  

<span data-ttu-id="47e96-116">EF5 我们已经注意到，支持空间类型与实体框架提供程序：</span><span class="sxs-lookup"><span data-stu-id="47e96-116">The Entity Framework providers for EF5 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="47e96-117">Microsoft SQL Server 提供程序</span><span class="sxs-lookup"><span data-stu-id="47e96-117">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="47e96-118">此提供程序附带的 EF5。</span><span class="sxs-lookup"><span data-stu-id="47e96-118">This provider is shipped as part of EF5.</span></span>  
    - <span data-ttu-id="47e96-119">此提供程序取决于安装可能需要一些其他低级别库 — 有关详细信息，请参阅下文。</span><span class="sxs-lookup"><span data-stu-id="47e96-119">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="47e96-120">适用于 Oracle 的 Devart dotConnect</span><span class="sxs-lookup"><span data-stu-id="47e96-120">Devart dotConnect for Oracle</span></span>](http://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="47e96-121">这是从 Devart 的第三方提供程序。</span><span class="sxs-lookup"><span data-stu-id="47e96-121">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="47e96-122">如果你知道 EF5 支持的提供程序的空间类型然后请联系我们，我们将很高兴地将其添加到此列表。</span><span class="sxs-lookup"><span data-stu-id="47e96-122">If you know of an EF5 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

### <a name="ef6"></a><span data-ttu-id="47e96-123">EF6</span><span class="sxs-lookup"><span data-stu-id="47e96-123">EF6</span></span>  

<span data-ttu-id="47e96-124">Ef6，我们已经注意到，支持空间类型与实体框架提供程序：</span><span class="sxs-lookup"><span data-stu-id="47e96-124">The Entity Framework providers for EF6 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="47e96-125">Microsoft SQL Server 提供程序</span><span class="sxs-lookup"><span data-stu-id="47e96-125">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="47e96-126">此提供程序附带的 EF6。</span><span class="sxs-lookup"><span data-stu-id="47e96-126">This provider is shipped as part of EF6.</span></span>  
    - <span data-ttu-id="47e96-127">此提供程序取决于安装可能需要一些其他低级别库 — 有关详细信息，请参阅下文。</span><span class="sxs-lookup"><span data-stu-id="47e96-127">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="47e96-128">适用于 Oracle 的 Devart dotConnect</span><span class="sxs-lookup"><span data-stu-id="47e96-128">Devart dotConnect for Oracle</span></span>](http://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="47e96-129">这是从 Devart 的第三方提供程序。</span><span class="sxs-lookup"><span data-stu-id="47e96-129">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="47e96-130">如果你知道的 EF6 支持的提供程序的空间类型然后请联系我们，我们将很高兴地将其添加到此列表。</span><span class="sxs-lookup"><span data-stu-id="47e96-130">If you know of an EF6 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a><span data-ttu-id="47e96-131">使用 Microsoft SQL Server 空间类型的先决条件</span><span class="sxs-lookup"><span data-stu-id="47e96-131">Prerequisites for spatial types with Microsoft SQL Server</span></span>  

<span data-ttu-id="47e96-132">SQL Server 空间支持取决于 SqlGeography 和 SqlGeometry 的低级别，特定于 SQL Server 的类型。</span><span class="sxs-lookup"><span data-stu-id="47e96-132">SQL Server spatial support depends on the low-level, SQL Server-specific types SqlGeography and SqlGeometry.</span></span> <span data-ttu-id="47e96-133">这些类型位于程序集 Microsoft.SqlServer.Types.dll，并作为 EF 的一部分或作为.NET Framework 的一部分不提供此程序集。</span><span class="sxs-lookup"><span data-stu-id="47e96-133">These types live in Microsoft.SqlServer.Types.dll assembly, and this assembly is not shipped as part of EF or as part of the .NET Framework.</span></span>  

<span data-ttu-id="47e96-134">安装 Visual Studio 时它通常还会安装 SQL Server 的版本，这将包括的 Microsoft.SqlServer.Types.dll 安装。</span><span class="sxs-lookup"><span data-stu-id="47e96-134">When Visual Studio is installed it will often also install a version of SQL Server, and this will include installation of the Microsoft.SqlServer.Types.dll.</span></span>  

<span data-ttu-id="47e96-135">如果想要使用的空间类型，在计算机上未安装 SQL Server 或从 SQL Server 安装中排除空间类型，您将需要手动安装它们。</span><span class="sxs-lookup"><span data-stu-id="47e96-135">If SQL Server is not installed on the machine where you want to use spatial types, or if spatial types were excluded from the SQL Server installation, then you will need to install them manually.</span></span> <span data-ttu-id="47e96-136">可以使用安装类型`SQLSysClrTypes.msi`，这是 Microsoft SQL Server 功能包的一部分。</span><span class="sxs-lookup"><span data-stu-id="47e96-136">The types can be installed using `SQLSysClrTypes.msi`, which is part of Microsoft SQL Server Feature Pack.</span></span> <span data-ttu-id="47e96-137">空间类型是 SQL Server 特定于版本，因此建议[搜索"SQL Server 功能包"](https://www.microsoft.com/en-us/search/result.aspx?q=sql+server+feature+pack)在 Microsoft 下载中心，然后选择并下载到您将使用的 SQL Server 的版本相对应的选项。</span><span class="sxs-lookup"><span data-stu-id="47e96-137">Spatial types are SQL Server version-specific, so we recommend [search for "SQL Server Feature Pack"](https://www.microsoft.com/en-us/search/result.aspx?q=sql+server+feature+pack) in the Microsoft Download Center, then select and download the option that corresponds to the version of SQL Server you will use.</span></span>
