---
title: "从 EF6 移植到 EF 核心技术-移植了基于 EDMX 的模型"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: c999d2114c76ee3a7615ae897b42ee3376cff14e
ms.sourcegitcommit: 1880d5ce49d4c9cb891d0e8fb230770bb44799e5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2017
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="f774e-102">移植到 EF 核心了从 EF6 基于 EDMX 的模型</span><span class="sxs-lookup"><span data-stu-id="f774e-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="f774e-103">EF 核心不支持的模型的 EDMX 文件格式。</span><span class="sxs-lookup"><span data-stu-id="f774e-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="f774e-104">若要移植这些模型中，最佳选择是从你的应用程序数据库中生成新的基于代码的模型。</span><span class="sxs-lookup"><span data-stu-id="f774e-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="f774e-105">安装 EF 核心 NuGet 包</span><span class="sxs-lookup"><span data-stu-id="f774e-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="f774e-106">安装`Microsoft.EntityFrameworkCore.Tools`NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="f774e-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="f774e-107">重新生成模型</span><span class="sxs-lookup"><span data-stu-id="f774e-107">Regenerate the model</span></span>

<span data-ttu-id="f774e-108">反向工程功能现在可用于创建基于现有数据库的模型。</span><span class="sxs-lookup"><span data-stu-id="f774e-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="f774e-109">在 Package Manager Console 中运行以下命令 (工具 –> NuGet 包管理器 –> 程序包管理器控制台)。</span><span class="sxs-lookup"><span data-stu-id="f774e-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="f774e-110">请参阅[Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md)有关命令选项搭建基架的表等等的子集。</span><span class="sxs-lookup"><span data-stu-id="f774e-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="f774e-111">例如，下面是要从 SQL Server LocalDB 实例上的博客数据库模型搭建基架命令。</span><span class="sxs-lookup"><span data-stu-id="f774e-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="f774e-112">删除从 EF6 模型</span><span class="sxs-lookup"><span data-stu-id="f774e-112">Remove EF6 model</span></span>

<span data-ttu-id="f774e-113">现在，你将删除从 EF6 模型从你的应用程序。</span><span class="sxs-lookup"><span data-stu-id="f774e-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="f774e-114">最好将从 EF6 NuGet 包 (EntityFramework) 安装，因为 EF 核心和 ef6 更高版本可以是在同一应用程序并行使用。</span><span class="sxs-lookup"><span data-stu-id="f774e-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="f774e-115">但是，如果你不想要在你的应用程序的任何区域中使用 ef6 更高版本，然后卸载程序包将有助于为提供上的代码片段，需要注意的编译错误。</span><span class="sxs-lookup"><span data-stu-id="f774e-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="f774e-116">更新你的代码</span><span class="sxs-lookup"><span data-stu-id="f774e-116">Update your code</span></span>

<span data-ttu-id="f774e-117">此时，它是一个寻址编译错误和查看代码，以查看你会影响从 EF6 和 EF 核心之间的行为更改。</span><span class="sxs-lookup"><span data-stu-id="f774e-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="f774e-118">测试端口</span><span class="sxs-lookup"><span data-stu-id="f774e-118">Test the port</span></span>

<span data-ttu-id="f774e-119">只是因为你的应用程序将编译，并不意味着它成功移植到 EF 核心。</span><span class="sxs-lookup"><span data-stu-id="f774e-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="f774e-120">你将需要测试应用程序，以确保任何行为更改产生负面影响你的应用程序的所有区域。</span><span class="sxs-lookup"><span data-stu-id="f774e-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>

> [!TIP]
> <span data-ttu-id="f774e-121">请参阅[Getting Started with ASP.NET 核心使用现有的数据库上的 EF 核心](xref:core/get-started/aspnetcore/existing-db)有关现有数据库，使用方式的其他参考</span><span class="sxs-lookup"><span data-stu-id="f774e-121">See [Getting Started with EF Core on ASP.NET Core with an Existing Database](xref:core/get-started/aspnetcore/existing-db) for an additional reference on how to work with an existing database,</span></span> 
