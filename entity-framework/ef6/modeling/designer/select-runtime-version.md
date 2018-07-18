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
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a>选择实体框架运行时版本的 EF 设计器模型
> [!NOTE]
> **仅限 EF6 及更高版本** - 此页面中讨论的功能、API 等已引入实体框架 6。 如果使用的是早期版本，则部分或全部信息不适用。

从 EF6 开始下面的屏幕已添加到 EF 设计器，以便您可以选择你想要创建模型时为目标的运行时版本。 该屏幕将显示实体框架的最新版本尚未安装在项目中。 如果已安装最新版本，则只需将默认情况下使用。

![屏幕](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a>面向 EF6.x

可以从选择版本屏幕将 EF6 运行时添加到你的项目选择 EF6。 添加 EF6 后, 将停止会显示此屏幕当前项目中。

如果已有较旧版本的安装 （因为不能为目标运行时从同一个项目的多个版本） 的 EF，EF6 将被禁用。 如果此处未启用 EF6 选项，请按照下列步骤以将项目升级到 EF6:

1.  右键单击解决方案资源管理器中的项目并选择**管理 NuGet 包...**
2.  选择**更新**
3.  选择**EntityFramework** （请确保它将更新为所需的版本）
4.  单击**更新**

 

## <a name="targeting-ef5x"></a>面向 EF5.x

可以从选择版本屏幕将 EF5 运行时添加到你的项目选择 EF5。 添加 EF5 后, 仍看到屏幕包含禁用的 EF6 选项。

如果拥有 EF4.x 版本已安装的运行时，您将看到该版本的 EF 屏幕而不是 EF5 中列出。 在这种情况下，你可以升级到 EF5 中使用以下步骤：

1.  选择**工具-&gt;库程序包管理器-&gt;包管理器控制台**
2.  运行**EntityFramework-5.0.0 版**

 

## <a name="targeting-ef4x"></a>面向 EF4.x

你可以通过以下步骤向项目中安装 EF4.x 运行时：

1.  选择**工具-&gt;库程序包管理器-&gt;包管理器控制台**
2.  运行**EntityFramework-版本 4.3.0**
