---
title: 选择 EF 设计器模型的 EF6 的 Entity Framework 运行时版本
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
caps.latest.revision: 3
ms.openlocfilehash: 75f7b4ed81528683801893c31de490ce15be6733
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/09/2018
ms.locfileid: "39120481"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a><span data-ttu-id="75202-102">选择实体框架运行时版本的 EF 设计器模型</span><span class="sxs-lookup"><span data-stu-id="75202-102">Selecting Entity Framework Runtime Version for EF Designer Models</span></span>
> [!NOTE]
> <span data-ttu-id="75202-103">**仅限 EF6 及更高版本** - 此页面中讨论的功能、API 等已引入实体框架 6。</span><span class="sxs-lookup"><span data-stu-id="75202-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="75202-104">如果使用的是早期版本，则部分或全部信息不适用。</span><span class="sxs-lookup"><span data-stu-id="75202-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="75202-105">从 EF6 开始下面的屏幕已添加到 EF 设计器，以便您可以选择你想要创建模型时为目标的运行时版本。</span><span class="sxs-lookup"><span data-stu-id="75202-105">Starting with EF6 the following screen was added to the EF Designer to allow you to select the version of the runtime you wish to target when creating a model.</span></span> <span data-ttu-id="75202-106">该屏幕将显示实体框架的最新版本尚未安装在项目中。</span><span class="sxs-lookup"><span data-stu-id="75202-106">The screen will appear when the latest version of Entity Framework is not already installed in the project.</span></span> <span data-ttu-id="75202-107">如果已安装最新版本，则只需将默认情况下使用。</span><span class="sxs-lookup"><span data-stu-id="75202-107">If the latest version is already installed it will just be used by default.</span></span>

![屏幕](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a><span data-ttu-id="75202-109">面向 EF6.x</span><span class="sxs-lookup"><span data-stu-id="75202-109">Targeting EF6.x</span></span>

<span data-ttu-id="75202-110">可以从选择版本屏幕将 EF6 运行时添加到你的项目选择 EF6。</span><span class="sxs-lookup"><span data-stu-id="75202-110">You can choose EF6 from the 'Choose Your Version' screen to add the EF6 runtime to your project.</span></span> <span data-ttu-id="75202-111">添加 EF6 后, 将停止会显示此屏幕当前项目中。</span><span class="sxs-lookup"><span data-stu-id="75202-111">Once you've added EF6, you’ll stop seeing this screen in the current project.</span></span>

<span data-ttu-id="75202-112">如果已有较旧版本的安装 （因为不能为目标运行时从同一个项目的多个版本） 的 EF，EF6 将被禁用。</span><span class="sxs-lookup"><span data-stu-id="75202-112">EF6 will be disabled if you already have an older version of EF installed (since you can't target multiple versions of the runtime from the same project).</span></span> <span data-ttu-id="75202-113">如果此处未启用 EF6 选项，请按照下列步骤以将项目升级到 EF6:</span><span class="sxs-lookup"><span data-stu-id="75202-113">If EF6 option is not enabled here, follow these steps to upgrade your project to EF6:</span></span>

1.  <span data-ttu-id="75202-114">右键单击解决方案资源管理器中的项目并选择**管理 NuGet 包...**</span><span class="sxs-lookup"><span data-stu-id="75202-114">Right-click on your project in Solution Explorer and select **Manage NuGet Packages...**</span></span>
2.  <span data-ttu-id="75202-115">选择**更新**</span><span class="sxs-lookup"><span data-stu-id="75202-115">Select **Updates**</span></span>
3.  <span data-ttu-id="75202-116">选择**EntityFramework** （请确保它将更新为所需的版本）</span><span class="sxs-lookup"><span data-stu-id="75202-116">Select **EntityFramework** (make sure it is going to update it to the version you want)</span></span>
4.  <span data-ttu-id="75202-117">单击**更新**</span><span class="sxs-lookup"><span data-stu-id="75202-117">Click **Update**</span></span>

 

## <a name="targeting-ef5x"></a><span data-ttu-id="75202-118">面向 EF5.x</span><span class="sxs-lookup"><span data-stu-id="75202-118">Targeting EF5.x</span></span>

<span data-ttu-id="75202-119">可以从选择版本屏幕将 EF5 运行时添加到你的项目选择 EF5。</span><span class="sxs-lookup"><span data-stu-id="75202-119">You can choose EF5 from the 'Choose Your Version' screen to add the EF5 runtime to your project.</span></span> <span data-ttu-id="75202-120">添加 EF5 后, 仍看到屏幕包含禁用的 EF6 选项。</span><span class="sxs-lookup"><span data-stu-id="75202-120">Once you've added EF5, you’ll still see the screen with the EF6 option disabled.</span></span>

<span data-ttu-id="75202-121">如果拥有 EF4.x 版本已安装的运行时，您将看到该版本的 EF 屏幕而不是 EF5 中列出。</span><span class="sxs-lookup"><span data-stu-id="75202-121">If you have an EF4.x version of the runtime already installed then you will see that version of EF listed in the screen rather than EF5.</span></span> <span data-ttu-id="75202-122">在这种情况下，你可以升级到 EF5 中使用以下步骤：</span><span class="sxs-lookup"><span data-stu-id="75202-122">In this situation you can upgrade to EF5 using the following steps:</span></span>

1.  <span data-ttu-id="75202-123">选择**工具-&gt;库程序包管理器-&gt;包管理器控制台**</span><span class="sxs-lookup"><span data-stu-id="75202-123">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="75202-124">运行**EntityFramework-5.0.0 版**</span><span class="sxs-lookup"><span data-stu-id="75202-124">Run **Install-Package EntityFramework -version 5.0.0**</span></span>

 

## <a name="targeting-ef4x"></a><span data-ttu-id="75202-125">面向 EF4.x</span><span class="sxs-lookup"><span data-stu-id="75202-125">Targeting EF4.x</span></span>

<span data-ttu-id="75202-126">你可以通过以下步骤向项目中安装 EF4.x 运行时：</span><span class="sxs-lookup"><span data-stu-id="75202-126">You can install the EF4.x runtime to your project using the following steps:</span></span>

1.  <span data-ttu-id="75202-127">选择**工具-&gt;库程序包管理器-&gt;包管理器控制台**</span><span class="sxs-lookup"><span data-stu-id="75202-127">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="75202-128">运行**EntityFramework-版本 4.3.0**</span><span class="sxs-lookup"><span data-stu-id="75202-128">Run **Install-Package EntityFramework -version 4.3.0**</span></span>
