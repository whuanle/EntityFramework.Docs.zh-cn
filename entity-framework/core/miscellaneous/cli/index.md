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
<a name="entity-framework-core-tools"></a>Entity Framework Core 工具
===========================
Entity Framework Core 工具有助于 EF Core 应用的开发。 它们主要用于通过对数据库架构进行反向工程来建立 DbContext 和实体类型的基架，以及用于管理迁移。

[EF Core 包管理器控制台 (PMC) 工具][1]在 Visual Studio 内提供卓越体验。 使用 NuGet 的[包管理器控制台][2]运行这些工具。 这些工具同时适用于 .NET Framework 和 .NET Core 项目。

[EF Core .NET 命令行工具][3]是 [.NET Core 命令行接口 (CLI) 工具][4]（这些工具是跨平台的，可在 Visual Studio 之外运行）的扩展。 这些工具需要 .NET Core SDK 项目（具有 `Sdk="Microsoft.NET.Sdk"` 的项目或项目文件中的相似项目）。

这两种工具提供相同的功能。 如果使用 Visual Studio 进行开发，我们建议使用 PMC 工具，因为这些工具提供更完整的体验。

<a name="frameworks"></a>框架
----------
这些工具支持面向 .NET Framework 或 .NET Core 的项目。

如果想要使用类库，如有可能，请考虑使用 .NET Core 或 .NET Framework 类库。 这样，使用 .NET 工具的问题最少。 而如果想要使用 .NET Standard 类库，则需要使用面向 .NET Framework 或 .NET Core 的启动项目，使该工具具有可在其中加载类库的具体目标平台。 此启动项目可以是不包含实际代码的虚拟项目 - 需要它的唯一理由是为工具提供一个目标。

如果项目面向其他框架（例如，通用 Windows 或 Xamarin），则需要创建一个单独的 .NET Standard 类库。 在这种情况下，请按照上述指南，也创建一个工具可使用的启动项目。

<a name="startup-and-target-projects"></a>启动项目和目标项目
---------------------------
每当调用命令时，始终会涉及两个项目：目标项目和启动项目。

目标项目是在其中添加任何文件（或在某些情况下删除文件）的项目。

启动项目是执行项目代码时由工具模拟的项目。

目标项目和启动项目可以相同。


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
