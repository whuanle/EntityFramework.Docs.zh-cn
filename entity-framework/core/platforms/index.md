---
title: "支持的 .NET 实现 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 08/30/2017
ms.technology: entity-framework-core
uid: core/platforms/index
ms.openlocfilehash: 6b6ea9ca833810169d0d850f3bea8776b761c007
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="net-implementations-supported-by-ef-core"></a>EF Core 支持的 .NET 实现

我们希望 EF Core 可用于任何可编写 .NET 代码 的平台，我们仍在为实现这一目标而努力。 下表提供了我们希望支持 EF Core 的每个 .NET 实现的指南。

EF Core 2.0 面向 [.NET Standard 2.0](https://docs.microsoft.com/dotnet/standard/net-standard)，因此需要支持它的 .NET 实现。

| .NET 实现 | 状态 | 1.x 需要 | 2.x 需要
|-|-|-|-
| **.NET Core**（[ASP.NET Core](../get-started/aspnetcore/index.md)、[控制台](../get-started/netcore/index.md) 等） | **完全支持，推荐使用：**经过了自动测试，可成功用于许多应用程序。 | [.NET Core SDK 1.x](https://www.microsoft.com/net/core/) | [.NET Core SDK 2.x](https://www.microsoft.com/net/core/)
| .NET Framework（WinForms、WPF、ASP.NET、[控制台](../get-started/full-dotnet/index.md)等） | **完全支持，推荐使用**：经过了自动测试，可成功用于许多应用程序。 EF 6 也可用于此平台（请参阅[比较 EF Core 和 EF6](../../efcore-and-ef6/index.md) 以选择合适的技术）。 | .NET Framework 4.5.1 | .NET Framework 4.6.1
| **Mono 和 Xamarin** | **测试阶段，可能会出现问题**：EF Core 团队和客户已执行一些测试。 早期采用者报告了一些成功使用的情况，但是他们[已经遇到了一些问题](https://github.com/aspnet/entityframework/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin)，随着测试继续可能还会发现其他问题。 问题尤其在于 Xamarin.iOS 中存在一些限制，这些限制可能会阻止部分使用 EF Core 2.0 开发的应用程序正常运行。 | Mono 4.6 <br/> Xamarin.iOS 10 <br/> Xamarin.Mac 3 <br/> Xamarin.Android 7 | Mono 5.4 <br/> Xamarin.iOS 10.14 <br/> Xamarin.Mac 3.8 <br/> Xamarin.Android 7.5
| [通用 Windows 平台](../get-started/uwp/index.md) | **测试阶段，可能会出现问题**：EF Core 团队和客户已执行一些测试。 使用 .NET Native 工具链编译时已报告了一些[问题](https://github.com/aspnet/entityframework/issues?utf8=%E2%9C%93&q=is%3Aopen%20is%3Aissue%20label%3Aarea-uwp%20)，版本生成期间通常会使用该工具链，并且部署到 Windows 应用商店时也需要使用该工具链（如果不使用 .NET Native 或者仅希望进行试验，许多问题就不会影响到你）。 | [最新 .NET UWP 5 包](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/5.4.1) | [最新 .NET UWP 6 包](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) <sup>(1)</sup>

<sup>(1)</sup> 此版本的 .NET UWP 添加了对 .NET Standard 2.0 的支持，包含 .NET Native 2.0，修复了之前报告的大部分兼容性问题。但是测试表明此版本存在与 EF Core 2.0 相关的[少许余留问题](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A2.0.1+label%3Aarea-uwp)，我们计划在即将发布的修补版本中解决这些问题。

对于未按预期工作的任意组合，我们建议在 [EF Core 问题跟踪程序](https://github.com/aspnet/entityframeworkcore/issues/new)中创建新问题；对于 Xamarin 相关问题，请使用 [Xamarin 问题跟踪程序](https://bugzilla.xamarin.com/newbug)。
