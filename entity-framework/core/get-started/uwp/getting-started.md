---
title: "UWP 入门 - 新数据库 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.topic: get-started-article
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
ms.technology: entity-framework-core
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: f743ff5392d1f30283a13d2e7fb8029be88387aa
ms.sourcegitcommit: 96324e58c02b97277395ed43173bf13ac80d2012
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2017
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="ba7ef-102">通过新数据库在通用 Windows 平台 (UWP) 上开始使用 EF Core</span><span class="sxs-lookup"><span data-stu-id="ba7ef-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

> [!NOTE]
> <span data-ttu-id="ba7ef-103">本教程使用 EF Core 2.0.1（与 ASP.NET Core 和 .NET Core SDK 2.0.3 一起发布）。</span><span class="sxs-lookup"><span data-stu-id="ba7ef-103">This tutorial uses EF Core 2.0.1 (released alongside ASP.NET Core and .NET Core SDK 2.0.3).</span></span> <span data-ttu-id="ba7ef-104">EF Core 2.0.0 缺少一些重要的 bug 修复，因而无法提供良好的 UWP 体验。</span><span class="sxs-lookup"><span data-stu-id="ba7ef-104">EF Core 2.0.0 lacks some crucial bug fixes required for a good UWP experience.</span></span>

<span data-ttu-id="ba7ef-105">在本演练中，你将构建一个通用 Windows 平台 (UWP) 应用程序，该程序使用 Entity Framework 对本地 SQLite 数据库执行基本数据访问。</span><span class="sxs-lookup"><span data-stu-id="ba7ef-105">In this walkthrough, you will build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ba7ef-106">考虑在针对 UWP 的 LINQ 查询中避免使用匿名类型。</span><span class="sxs-lookup"><span data-stu-id="ba7ef-106">**Consider avoiding anonymous types in LINQ queries on UWP**.</span></span> <span data-ttu-id="ba7ef-107">将 UWP 应用程序部署到应用商店要求使用 .NET Native 编译应用程序。</span><span class="sxs-lookup"><span data-stu-id="ba7ef-107">Deploying a UWP application to the app store requires your application to be compiled with .NET Native.</span></span> <span data-ttu-id="ba7ef-108">使用匿名类型的查询在 .NET Native 上性能较差。</span><span class="sxs-lookup"><span data-stu-id="ba7ef-108">Queries with anonymous types have worse performance on .NET Native.</span></span>

> [!TIP]
> <span data-ttu-id="ba7ef-109">可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite)。</span><span class="sxs-lookup"><span data-stu-id="ba7ef-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ba7ef-110">先决条件</span><span class="sxs-lookup"><span data-stu-id="ba7ef-110">Prerequisites</span></span>

<span data-ttu-id="ba7ef-111">完成本演练需要具有以下各项：</span><span class="sxs-lookup"><span data-stu-id="ba7ef-111">The following items are required to complete this walkthrough:</span></span>

* <span data-ttu-id="ba7ef-112">[Windows 10 Fall Creators Update](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0)</span><span class="sxs-lookup"><span data-stu-id="ba7ef-112">[Windows 10 Fall Creators Update](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0)</span></span>

* <span data-ttu-id="ba7ef-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="ba7ef-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>

* <span data-ttu-id="ba7ef-114">具有**通用 Windows 平台开发**工作负载的 [Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.4 版或更高版本。</span><span class="sxs-lookup"><span data-stu-id="ba7ef-114">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.4 or later with the **Universal Windows Platform Development** workload.</span></span>

## <a name="create-a-new-model-project"></a><span data-ttu-id="ba7ef-115">创建新模型项目</span><span class="sxs-lookup"><span data-stu-id="ba7ef-115">Create a new model project</span></span>

> [!WARNING]
> <span data-ttu-id="ba7ef-116">由于 .NET Core 工具与 UWP 项目交互的方式受到限制，因此该模型需要放在非 UWP 项目中才能在包管理器控制台中运行迁移命令</span><span class="sxs-lookup"><span data-stu-id="ba7ef-116">Due to limitations in the way .NET Core tools interact with UWP projects the model needs to be placed in a non-UWP project to be able to run migrations commands in the Package Manager Console</span></span>

* <span data-ttu-id="ba7ef-117">打开 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ba7ef-117">Open Visual Studio</span></span>

* <span data-ttu-id="ba7ef-118">“文件”>“新建”>“项目...”</span><span class="sxs-lookup"><span data-stu-id="ba7ef-118">File > New > Project...</span></span>

* <span data-ttu-id="ba7ef-119">从左侧菜单中选择“模板”>“Visual C#”</span><span class="sxs-lookup"><span data-stu-id="ba7ef-119">From the left menu select Templates > Visual C#</span></span>

* <span data-ttu-id="ba7ef-120">选择“类库(.NET Standard)”项目模板。</span><span class="sxs-lookup"><span data-stu-id="ba7ef-120">Select the **Class Library (.NET Standard)** project template</span></span>

* <span data-ttu-id="ba7ef-121">为项目提供名称，然后单击“确定”</span><span class="sxs-lookup"><span data-stu-id="ba7ef-121">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="ba7ef-122">安装 Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ba7ef-122">Install Entity Framework</span></span>

<span data-ttu-id="ba7ef-123">要使用 EF Core，请为要作为目标对象的数据库提供程序安装程序包。</span><span class="sxs-lookup"><span data-stu-id="ba7ef-123">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="ba7ef-124">本演练使用 SQLite。</span><span class="sxs-lookup"><span data-stu-id="ba7ef-124">This walkthrough uses SQLite.</span></span> <span data-ttu-id="ba7ef-125">有关可用提供程序的列表，请参阅[数据库提供程序](../../providers/index.md)。</span><span class="sxs-lookup"><span data-stu-id="ba7ef-125">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="ba7ef-126">“工具”>“NuGet 包管理器”>“包管理器控制台”</span><span class="sxs-lookup"><span data-stu-id="ba7ef-126">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="ba7ef-127">运行 `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span><span class="sxs-lookup"><span data-stu-id="ba7ef-127">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

<span data-ttu-id="ba7ef-128">在本演练的后面部分，我们还将一些 Entity Framework Tools 用于维护数据库。</span><span class="sxs-lookup"><span data-stu-id="ba7ef-128">Later in this walkthrough we will also be using some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="ba7ef-129">因此，我们也会安装此工具包。</span><span class="sxs-lookup"><span data-stu-id="ba7ef-129">So we will install the tools package as well.</span></span>

* <span data-ttu-id="ba7ef-130">运行 `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="ba7ef-130">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

* <span data-ttu-id="ba7ef-131">编辑 .csproj 文件，并将 `<TargetFramework>netstandard2.0</TargetFramework>` 替换为 `<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`</span><span class="sxs-lookup"><span data-stu-id="ba7ef-131">Edit the .csproj file and replace `<TargetFramework>netstandard2.0</TargetFramework>` with `<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`</span></span>

## <a name="create-your-model"></a><span data-ttu-id="ba7ef-132">创建你自己的模型</span><span class="sxs-lookup"><span data-stu-id="ba7ef-132">Create your model</span></span>

<span data-ttu-id="ba7ef-133">现在是时候定义构成模型的上下文和实体类了。</span><span class="sxs-lookup"><span data-stu-id="ba7ef-133">Now it's time to define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="ba7ef-134">“项目”>“添加类...”</span><span class="sxs-lookup"><span data-stu-id="ba7ef-134">Project > Add Class...</span></span>

* <span data-ttu-id="ba7ef-135">输入“Model.cs”作为名称，然后单击“确定”</span><span class="sxs-lookup"><span data-stu-id="ba7ef-135">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="ba7ef-136">将此文件的内容替换为以下代码</span><span class="sxs-lookup"><span data-stu-id="ba7ef-136">Replace the contents of the file with the following code</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a><span data-ttu-id="ba7ef-137">创建新的 UWP 项目</span><span class="sxs-lookup"><span data-stu-id="ba7ef-137">Create a new UWP project</span></span>

* <span data-ttu-id="ba7ef-138">打开 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ba7ef-138">Open Visual Studio</span></span>

* <span data-ttu-id="ba7ef-139">“文件”>“新建”>“项目...”</span><span class="sxs-lookup"><span data-stu-id="ba7ef-139">File > New > Project...</span></span>

* <span data-ttu-id="ba7ef-140">从左侧菜单中选择“模板”>“Visual C#”>“Windows 通用”</span><span class="sxs-lookup"><span data-stu-id="ba7ef-140">From the left menu select Templates > Visual C# > Windows Universal</span></span>

* <span data-ttu-id="ba7ef-141">选择“空白应用(通用 Windows)”项目模板</span><span class="sxs-lookup"><span data-stu-id="ba7ef-141">Select the **Blank App (Universal Windows)** project template</span></span>

* <span data-ttu-id="ba7ef-142">为项目提供名称，然后单击“确定”</span><span class="sxs-lookup"><span data-stu-id="ba7ef-142">Give the project a name and click **OK**</span></span>

* <span data-ttu-id="ba7ef-143">将目标版本和最低版本至少设置为 `Windows 10 Fall Creators Update (10.0; build 16299.0)`</span><span class="sxs-lookup"><span data-stu-id="ba7ef-143">Set the target and minimum versions to at least `Windows 10 Fall Creators Update (10.0; build 16299.0)`</span></span>

## <a name="create-your-database"></a><span data-ttu-id="ba7ef-144">创建数据库</span><span class="sxs-lookup"><span data-stu-id="ba7ef-144">Create your database</span></span>

<span data-ttu-id="ba7ef-145">现在你已经有了模型，可以使用迁移为自己创建数据库。</span><span class="sxs-lookup"><span data-stu-id="ba7ef-145">Now that you have a model, you can use migrations to create a database for you.</span></span>

* <span data-ttu-id="ba7ef-146">“工具”–>“NuGet 包管理器”–>“包管理器控制台”</span><span class="sxs-lookup"><span data-stu-id="ba7ef-146">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="ba7ef-147">选择作为默认项目的模型项目并将其设置为启动项目</span><span class="sxs-lookup"><span data-stu-id="ba7ef-147">Select the model project as the Default project and set it as the startup project</span></span>

* <span data-ttu-id="ba7ef-148">运行 `Add-Migration MyFirstMigration` 来为迁移搭建基架，从而为模型创建一组初始表。</span><span class="sxs-lookup"><span data-stu-id="ba7ef-148">Run `Add-Migration MyFirstMigration` to scaffold a migration to create the initial set of tables for your model.</span></span>

<span data-ttu-id="ba7ef-149">我们希望在运行该应用程序的设备上创建数据库，因此，我们将添加一些代码，以便在应用程序启动时将任何待定迁移应用到本地数据库。</span><span class="sxs-lookup"><span data-stu-id="ba7ef-149">Since we want the database to be created on the device that the app runs on, we will add some code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="ba7ef-150">应用程序第一次运行时，该操作将为我们创建本地数据库。</span><span class="sxs-lookup"><span data-stu-id="ba7ef-150">The first time that the app runs, this will take care of creating the local database for us.</span></span>

* <span data-ttu-id="ba7ef-151">在“解决方案资源管理器”中，右键单击“App.xaml”，然后选择“查看代码”。</span><span class="sxs-lookup"><span data-stu-id="ba7ef-151">Right-click on **App.xaml** in **Solution Explorer** and select **View Code**</span></span>

* <span data-ttu-id="ba7ef-152">将突出显示的 using 添加到文件的开头</span><span class="sxs-lookup"><span data-stu-id="ba7ef-152">Add the highlighted using to the start of the file</span></span>

* <span data-ttu-id="ba7ef-153">添加突出显示的代码，以应用任何待定迁移</span><span class="sxs-lookup"><span data-stu-id="ba7ef-153">Add the highlighted code to apply any pending migrations</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/App.xaml.cs?highlight=1,25-28)]

> [!TIP]  
> <span data-ttu-id="ba7ef-154">如果将来对模型进行更改，则可以使用 `Add-Migration` 命令为新迁移搭建基架，以便将相应更改应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="ba7ef-154">If you make future changes to your model, you can use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="ba7ef-155">当应用程序启动时，任何待定迁移都将应用到每个设备上的本地数据库。</span><span class="sxs-lookup"><span data-stu-id="ba7ef-155">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="ba7ef-156">EF 使用数据库中的 `__EFMigrationsHistory` 表来跟踪哪些迁移已经应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="ba7ef-156">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="ba7ef-157">使用模型</span><span class="sxs-lookup"><span data-stu-id="ba7ef-157">Use your model</span></span>

<span data-ttu-id="ba7ef-158">你现在可以使用模型执行数据访问。</span><span class="sxs-lookup"><span data-stu-id="ba7ef-158">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="ba7ef-159">打开 MainPage.xaml</span><span class="sxs-lookup"><span data-stu-id="ba7ef-159">Open *MainPage.xaml*</span></span>

* <span data-ttu-id="ba7ef-160">添加下面突出显示的网页加载处理程序和 UI 内容</span><span class="sxs-lookup"><span data-stu-id="ba7ef-160">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="ba7ef-161">现在我们将添加代码来连接 UI 和数据库</span><span class="sxs-lookup"><span data-stu-id="ba7ef-161">Now we'll add code to wire up the UI with the database</span></span>

* <span data-ttu-id="ba7ef-162">在“解决方案资源管理器”中，右键单击“MainPage.xaml”，然后选择“查看代码”。</span><span class="sxs-lookup"><span data-stu-id="ba7ef-162">Right-click **MainPage.xaml** in **Solution Explorer** and select **View Code**</span></span>

* <span data-ttu-id="ba7ef-163">从以下列表中添加突出显示的代码</span><span class="sxs-lookup"><span data-stu-id="ba7ef-163">Add the highlighted code from the following listing</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml.cs?highlight=30-48)]

<span data-ttu-id="ba7ef-164">现在可以运行应用程序来查看其实际运行情况。</span><span class="sxs-lookup"><span data-stu-id="ba7ef-164">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="ba7ef-165">“调试”>“开始执行(不调试)”</span><span class="sxs-lookup"><span data-stu-id="ba7ef-165">Debug > Start Without Debugging</span></span>

* <span data-ttu-id="ba7ef-166">应用程序将生成并启动</span><span class="sxs-lookup"><span data-stu-id="ba7ef-166">The application will build and launch</span></span>

* <span data-ttu-id="ba7ef-167">输入 URL，然后单击“添加”按钮</span><span class="sxs-lookup"><span data-stu-id="ba7ef-167">Enter a URL and click the **Add** button</span></span>

![图像](_static/create.png)

![图像](_static/list.png)

## <a name="next-steps"></a><span data-ttu-id="ba7ef-170">后续步骤</span><span class="sxs-lookup"><span data-stu-id="ba7ef-170">Next steps</span></span>

> [!TIP]
> <span data-ttu-id="ba7ef-171">通过实现实体类型中的 [`INotifyPropertyChanged`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx)、[`INotifyPropertyChanging`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx)、[`INotifyCollectionChanged`](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) 并使用 `ChangeTrackingStrategy.ChangingAndChangedNotifications` 可改进 `SaveChanges()` 性能。</span><span class="sxs-lookup"><span data-stu-id="ba7ef-171">`SaveChanges()` performance can be improved by implementing [`INotifyPropertyChanged`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [`INotifyPropertyChanging`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx), [`INotifyCollectionChanged`](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) in your entity types and using `ChangeTrackingStrategy.ChangingAndChangedNotifications`.</span></span>

<span data-ttu-id="ba7ef-172">哇哦！</span><span class="sxs-lookup"><span data-stu-id="ba7ef-172">Tada!</span></span> <span data-ttu-id="ba7ef-173">你现在已经有了一个运行 Entity Framework 的简单 UWP 应用了。</span><span class="sxs-lookup"><span data-stu-id="ba7ef-173">You now have a simple UWP app running Entity Framework.</span></span>

<span data-ttu-id="ba7ef-174">查阅本文档中的其他文章，详细了解关于 Entity Framework 功能的信息。</span><span class="sxs-lookup"><span data-stu-id="ba7ef-174">Check out other articles in this documentation to learn more about Entity Framework's features.</span></span>
