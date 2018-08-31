---
title: 编写数据库提供程序的 EF Core
author: anmiller
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
uid: core/providers/writing-a-provider
ms.openlocfilehash: c7130b0d104cd26584d298da98eb3e7080ee7f3c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993661"
---
# <a name="writing-a-database-provider"></a><span data-ttu-id="a7949-102">编写数据库提供程序</span><span class="sxs-lookup"><span data-stu-id="a7949-102">Writing a Database Provider</span></span>

<span data-ttu-id="a7949-103">有关编写实体框架核心数据库提供程序的信息，请参阅[因此你想要编写 EF Core 提供程序](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/)通过[Arthur Vickers](https://github.com/ajcvickers)。</span><span class="sxs-lookup"><span data-stu-id="a7949-103">For information about writing an Entity Framework Core database provider, see [So you want to write an EF Core provider](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) by [Arthur Vickers](https://github.com/ajcvickers).</span></span>

> [!NOTE]
> <span data-ttu-id="a7949-104">自 EF Core 1.1 尚未更新这些文章和自那以后进行了重大更改[问题 681](https://github.com/aspnet/EntityFramework.Docs/issues/681)跟踪对此文档的更新。</span><span class="sxs-lookup"><span data-stu-id="a7949-104">These posts have not been updated since EF Core 1.1 and there have been significant changes since that time [Issue 681](https://github.com/aspnet/EntityFramework.Docs/issues/681) is tracking updates to this documentation.</span></span>

<span data-ttu-id="a7949-105">EF Core 代码库是开放源代码，并包含可用作参考的多个数据库提供程序。</span><span class="sxs-lookup"><span data-stu-id="a7949-105">The EF Core codebase is open source and contains several database providers that can be used as a reference.</span></span> <span data-ttu-id="a7949-106">您可以找到在源代码https://github.com/aspnet/EntityFrameworkCore。</span><span class="sxs-lookup"><span data-stu-id="a7949-106">You can find the source code at https://github.com/aspnet/EntityFrameworkCore.</span></span> <span data-ttu-id="a7949-107">它也可能非常有用，下面的代码对于常用第三方提供程序，如[Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL)， [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql)，并[SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact)。</span><span class="sxs-lookup"><span data-stu-id="a7949-107">It may also be helpful to look at the code for commonly used third-party providers, such as [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql), and [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span></span> <span data-ttu-id="a7949-108">具体而言，这些项目已安装程序将从扩展并在 NuGet 上运行我们发布的功能测试。</span><span class="sxs-lookup"><span data-stu-id="a7949-108">In particular, these projects are setup to extend from and run functional tests that we publish on NuGet.</span></span> <span data-ttu-id="a7949-109">强烈建议在这种类型的安装程序。</span><span class="sxs-lookup"><span data-stu-id="a7949-109">This kind of setup is strongly recommended.</span></span>

## <a name="keeping-up-to-date-with-provider-changes"></a><span data-ttu-id="a7949-110">随时获取最新提供程序更改</span><span class="sxs-lookup"><span data-stu-id="a7949-110">Keeping up-to-date with provider changes</span></span>

<span data-ttu-id="a7949-111">2.1 版本之后开始工作，我们创建了[的更改日志](provider-log.md)，可能需要提供程序代码中的相应更改。</span><span class="sxs-lookup"><span data-stu-id="a7949-111">Starting with work after the 2.1 release, we have created a [log of changes](provider-log.md) that may need corresponding changes in provider code.</span></span> <span data-ttu-id="a7949-112">目的是帮助更新现有的提供程序时要使用 EF Core 的新版本。</span><span class="sxs-lookup"><span data-stu-id="a7949-112">This is intended to help when updating an existing provider to work with a new version of EF Core.</span></span>

<span data-ttu-id="a7949-113">在 2.1 之前，我们使用[ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware)并[ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi)上我们的 GitHub 问题和相似的用途的拉取请求标签。</span><span class="sxs-lookup"><span data-stu-id="a7949-113">Prior to 2.1, we used the [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) and [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) labels on our GitHub issues and pull requests for a similar purpose.</span></span> <span data-ttu-id="a7949-114">我们将 continiue 能够在问题中使用这些标签来指示在给定的版本中的工作项可能还需要提供程序中完成的工作。</span><span class="sxs-lookup"><span data-stu-id="a7949-114">We will continiue to use these lables on issues to give an indication which work items in a given release may also require work to be done in providers.</span></span> <span data-ttu-id="a7949-115">一个`providers-beware`标签通常意味着工作项的实现可能会断开提供程序，而`providers-fyi`标签通常表示提供程序将不会断开，但代码可能需要进行更改，例如，若要启用新功能。</span><span class="sxs-lookup"><span data-stu-id="a7949-115">A `providers-beware` label typically means that the implementation of an work item may break providers, while a `providers-fyi` label typically means that providers will not be broken, but code may need to be changed anyway, for example, to enable new functionality.</span></span>

## <a name="suggested-naming-of-third-party-providers"></a><span data-ttu-id="a7949-116">建议的第三方提供程序命名</span><span class="sxs-lookup"><span data-stu-id="a7949-116">Suggested naming of third party providers</span></span>

<span data-ttu-id="a7949-117">我们建议使用以下命名适用于 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="a7949-117">We suggest using the following naming for NuGet packages.</span></span> <span data-ttu-id="a7949-118">这是与 EF Core 小组所传递的包名称一致。</span><span class="sxs-lookup"><span data-stu-id="a7949-118">This is consistent with the names of packages delivered by the EF Core team.</span></span>

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

<span data-ttu-id="a7949-119">例如：</span><span class="sxs-lookup"><span data-stu-id="a7949-119">For example:</span></span>
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
