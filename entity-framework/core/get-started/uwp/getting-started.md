---
title: UWP 入门 - 新数据库 - EF Core
author: rowanmiller
ms.date: 10/13/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: 456967a0dc981053919064a19cc9c98bf7309865
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022371"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="5e8dd-102">通过新数据库在通用 Windows 平台 (UWP) 上开始使用 EF Core</span><span class="sxs-lookup"><span data-stu-id="5e8dd-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

<span data-ttu-id="5e8dd-103">在本教程中，将构建一个通用 Windows 平台 (UWP) 应用程序，该应用程序使用 Entity Framework Core 对本地 SQLite 数据库执行基本数据访问。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-103">In this tutorial, you build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="5e8dd-104">[在 GitHub 上查看此文章的示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP)。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-104">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5e8dd-105">系统必备</span><span class="sxs-lookup"><span data-stu-id="5e8dd-105">Prerequisites</span></span>

* <span data-ttu-id="5e8dd-106">[Windows 10 Fall Creators Update（10.0；内部版本 16299）或更改版本](https://support.microsoft.com/help/4027667/windows-update-windows-10)。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-106">[Windows 10 Fall Creators Update (10.0; Build 16299) or later](https://support.microsoft.com/help/4027667/windows-update-windows-10).</span></span>

* <span data-ttu-id="5e8dd-107">具有通用 Windows 平台开发工作负载的 [Visual Studio 2017 15.7 版或更高版本](https://www.visualstudio.com/downloads/)。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-107">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with the **Universal Windows Platform Development** workload.</span></span>

* <span data-ttu-id="5e8dd-108">[.NET Core 2.1 SDK 或更高版本](https://www.microsoft.com/net/core)。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-108">[.NET Core 2.1 SDK or later](https://www.microsoft.com/net/core) or later.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5e8dd-109">本教程使用 Entity Framework Core [迁移](xref:core/managing-schemas/migrations/index)命令创建和更新数据库的架构。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-109">This tutorial uses Entity Framework Core [migrations](xref:core/managing-schemas/migrations/index) commands to create and update the schema of the database.</span></span>
> <span data-ttu-id="5e8dd-110">这些命令不会直接使用 UWP 项目。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-110">These commands don't work directly with UWP projects.</span></span>
> <span data-ttu-id="5e8dd-111">为此，应用程序的数据模型位于共享库项目中，并且单独的 .NET Core 控制台应用程序用于运行这些命令。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-111">For this reason, the application's data model is placed in a shared library project, and a separate .NET Core console application is used to run the commands.</span></span>

## <a name="create-a-library-project-to-hold-the-data-model"></a><span data-ttu-id="5e8dd-112">创建库项目以存放数据模型</span><span class="sxs-lookup"><span data-stu-id="5e8dd-112">Create a library project to hold the data model</span></span>

* <span data-ttu-id="5e8dd-113">打开 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5e8dd-113">Open Visual Studio</span></span>

* <span data-ttu-id="5e8dd-114">“文件”>“新建”>“项目”</span><span class="sxs-lookup"><span data-stu-id="5e8dd-114">**File > New > Project**</span></span>

* <span data-ttu-id="5e8dd-115">从左侧菜单中选择“已安装”>“Visual C#”>“.NET Standard”。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-115">From the left menu select **Installed > Visual C# > .NET Standard**.</span></span>

* <span data-ttu-id="5e8dd-116">选择“类库(.NET Standard)”模板。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-116">Select the **Class Library (.NET Standard)** template.</span></span>

* <span data-ttu-id="5e8dd-117">将该项目命名为 Blogging.Model。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-117">Name the project *Blogging.Model*.</span></span>

* <span data-ttu-id="5e8dd-118">将该解决方案命名为 Blogging。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-118">Name the solution *Blogging*.</span></span>

* <span data-ttu-id="5e8dd-119">单击 **“确定”**。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-119">Click **OK**.</span></span>

## <a name="install-entity-framework-core-runtime-in-the-data-model-project"></a><span data-ttu-id="5e8dd-120">在数据模型项目中安装 Entity Framework Core 运行时</span><span class="sxs-lookup"><span data-stu-id="5e8dd-120">Install Entity Framework Core runtime in the data model project</span></span>

<span data-ttu-id="5e8dd-121">要使用 EF Core，请为要作为目标对象的数据库提供程序安装程序包。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-121">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="5e8dd-122">本教程使用 SQLite。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-122">This tutorial uses SQLite.</span></span> <span data-ttu-id="5e8dd-123">有关可用提供程序的列表，请参阅[数据库提供程序](../../providers/index.md)。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-123">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="5e8dd-124">“工具”>“NuGet 包管理器”>“包管理器控制台”。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-124">**Tools > NuGet Package Manager > Package Manager Console**.</span></span>

* <span data-ttu-id="5e8dd-125">确保在包管理器控制台中选择库项目 Blogging.Model 作为默认项目。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-125">Make sure that the library project *Blogging.Model* is selected as the **Default Project** in the Package Manager Console.</span></span>

* <span data-ttu-id="5e8dd-126">运行 `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span><span class="sxs-lookup"><span data-stu-id="5e8dd-126">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

## <a name="create-the-data-model"></a><span data-ttu-id="5e8dd-127">创建数据模型</span><span class="sxs-lookup"><span data-stu-id="5e8dd-127">Create the data model</span></span>

<span data-ttu-id="5e8dd-128">现在是时候定义构成模型的 DbContext 和实体类了。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-128">Now it's time to define the *DbContext* and entity classes that make up the model.</span></span>

* <span data-ttu-id="5e8dd-129">删除 Class1.cs。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-129">Delete *Class1.cs*.</span></span>

* <span data-ttu-id="5e8dd-130">用以下代码创建 Model.cs：</span><span class="sxs-lookup"><span data-stu-id="5e8dd-130">Create *Model.cs* with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-console-project-to-run-migrations-commands"></a><span data-ttu-id="5e8dd-131">创建新的控制台项目以运行迁移命令</span><span class="sxs-lookup"><span data-stu-id="5e8dd-131">Create a new console project to run migrations commands</span></span>

* <span data-ttu-id="5e8dd-132">在解决方案资源管理器中，右键单击该解决方案，然后选择“添加”>“新项目”。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-132">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="5e8dd-133">从左菜单中选择“已安装”>“Visual C#”>“.NET Core”。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-133">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>

* <span data-ttu-id="5e8dd-134">选择“控制台应用程序(.NET Core)”项目模板。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-134">Select the **Console App (.NET Core)** project template.</span></span>

* <span data-ttu-id="5e8dd-135">将该项目命名为 Blogging.Migrations.Startup ，然后单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-135">Name the project *Blogging.Migrations.Startup*, and click **OK**.</span></span>

* <span data-ttu-id="5e8dd-136">将 Blogging.Migrations.Startup 项目中的项目引用添加到 Blogging.Model 项目。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-136">Add a project reference from the *Blogging.Migrations.Startup* project to the *Blogging.Model* project.</span></span>

## <a name="install-entity-framework-core-tools-in-the-migrations-startup-project"></a><span data-ttu-id="5e8dd-137">在迁移启动项目中安装 Entity Framework Core 工具</span><span class="sxs-lookup"><span data-stu-id="5e8dd-137">Install Entity Framework Core tools in the migrations startup project</span></span>

<span data-ttu-id="5e8dd-138">若要在包管理器控制台中启用 EF Core 迁移命令，请在控制台应用程序中安装 EF Core 工具包。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-138">To enable the EF Core migration commands in the Package Manager Console, install the EF Core tools package in the console application.</span></span>

* <span data-ttu-id="5e8dd-139">“工具”>“NuGet 包管理器”>“包管理器控制台”</span><span class="sxs-lookup"><span data-stu-id="5e8dd-139">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="5e8dd-140">运行 `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`</span><span class="sxs-lookup"><span data-stu-id="5e8dd-140">Run `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`</span></span>

## <a name="create-the-initial-migration"></a><span data-ttu-id="5e8dd-141">创建初始迁移</span><span class="sxs-lookup"><span data-stu-id="5e8dd-141">Create the initial migration</span></span>

 <span data-ttu-id="5e8dd-142">创建初始迁移，将控制台应用程序指定为启动项目。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-142">Create the initial migration, specifying the console application as the startup project.</span></span>

* <span data-ttu-id="5e8dd-143">运行 `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`</span><span class="sxs-lookup"><span data-stu-id="5e8dd-143">Run `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`</span></span>

<span data-ttu-id="5e8dd-144">此命令为迁移搭建基架，该迁移为数据模型创建一组初始数据库表。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-144">This command scaffolds a migration that creates the initial set of database tables for your data model.</span></span>

## <a name="create-the-uwp-project"></a><span data-ttu-id="5e8dd-145">创建 UWP 项目</span><span class="sxs-lookup"><span data-stu-id="5e8dd-145">Create the UWP project</span></span>

* <span data-ttu-id="5e8dd-146">在解决方案资源管理器中，右键单击该解决方案，然后选择“添加”>“新项目”。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-146">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="5e8dd-147">从左侧菜单中选择“已安装”>“Visual C#”>“Windows Universal”。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-147">From the left menu select **Installed > Visual C# > Windows Universal**.</span></span>

* <span data-ttu-id="5e8dd-148">选择“空白应用(通用 Windows)”项目模板。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-148">Select the **Blank App (Universal Windows)** project template.</span></span>

* <span data-ttu-id="5e8dd-149">将项目命名为 Blogging.UWP ，然后单击“确定”</span><span class="sxs-lookup"><span data-stu-id="5e8dd-149">Name the project *Blogging.UWP*, and click **OK**</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5e8dd-150">将目标和最低版本设置为至少“Windows 10 Fall Creators Update (10.0；内部版本 16299.0)”。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-150">Set the target and minimum versions to at least **Windows 10 Fall Creators Update (10.0; build 16299.0)**.</span></span>
> <span data-ttu-id="5e8dd-151">早期版本的 Windows 10 不支持 .NET Standard 2.0，但它是 Entity Framework Core 必需的。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-151">Previous  versions of Windows 10 do not support .NET Standard 2.0, which is required by Entity Framework Core.</span></span>

## <a name="add-code-to-create-the-database-on-application-startup"></a><span data-ttu-id="5e8dd-152">添加代码以在应用程序启动时创建数据库</span><span class="sxs-lookup"><span data-stu-id="5e8dd-152">Add code to create the database on application startup</span></span>

<span data-ttu-id="5e8dd-153">由于希望在运行该应用的设备上创建数据库，因此，将添加代码，以便在应用程序启动时将任何待定迁移应用到本地数据库。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-153">Since you want the database to be created on the device that the app runs on, add code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="5e8dd-154">应用第一次运行时，该操作将创建本地数据库。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-154">The first time that the app runs, this will take care of creating the local database.</span></span>

* <span data-ttu-id="5e8dd-155">将 Blogging.UWP 项目中的项目引用添加到 Blogging.Model 项目。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-155">Add a project reference from the *Blogging.UWP* project to the *Blogging.Model* project.</span></span>

* <span data-ttu-id="5e8dd-156">打开 App.xaml.cs。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-156">Open *App.xaml.cs*.</span></span>

* <span data-ttu-id="5e8dd-157">添加突出显示的代码，应用任何待定迁移。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-157">Add the highlighted code to apply any pending migrations.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> <span data-ttu-id="5e8dd-158">如果对模型进行了更改，则请使用 `Add-Migration` 命令为新迁移搭建基架，以便将相应更改应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-158">If you change your model, use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="5e8dd-159">当应用程序启动时，任何待定迁移都将应用到每个设备上的本地数据库。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-159">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="5e8dd-160">EF Core 使用数据库中的 `__EFMigrationsHistory` 表来跟踪哪些迁移已应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-160">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-data-model"></a><span data-ttu-id="5e8dd-161">使用数据模型</span><span class="sxs-lookup"><span data-stu-id="5e8dd-161">Use the data model</span></span>

<span data-ttu-id="5e8dd-162">现在可以使用 EF Core 执行数据访问。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-162">You can now use EF Core to perform data access.</span></span>

* <span data-ttu-id="5e8dd-163">打开 MainPage.xaml.</span><span class="sxs-lookup"><span data-stu-id="5e8dd-163">Open *MainPage.xaml*.</span></span>

* <span data-ttu-id="5e8dd-164">添加下面突出显示的网页加载处理程序和 UI 内容</span><span class="sxs-lookup"><span data-stu-id="5e8dd-164">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="5e8dd-165">现在，添加代码来连接 UI 和数据库</span><span class="sxs-lookup"><span data-stu-id="5e8dd-165">Now add code to wire up the UI with the database</span></span>

* <span data-ttu-id="5e8dd-166">打开 MainPage.xaml.cs.</span><span class="sxs-lookup"><span data-stu-id="5e8dd-166">Open *MainPage.xaml.cs*.</span></span>

* <span data-ttu-id="5e8dd-167">从以下列表中添加突出显示的代码：</span><span class="sxs-lookup"><span data-stu-id="5e8dd-167">Add the highlighted code from the following listing:</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

<span data-ttu-id="5e8dd-168">现在可以运行应用程序来查看其实际运行情况。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-168">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="5e8dd-169">在解决方案资源管理器中，右键单击Blogging.UWP 项目，然后选择“部署”。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-169">In **Solution Explorer**, right-click the *Blogging.UWP* project and then select **Deploy**.</span></span>

* <span data-ttu-id="5e8dd-170">将 Blogging.UWP 设置为启动项目。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-170">Set *Blogging.UWP* as the startup project.</span></span>

* <span data-ttu-id="5e8dd-171">“调试”>“开始执行(不调试)”</span><span class="sxs-lookup"><span data-stu-id="5e8dd-171">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="5e8dd-172">将生成并运行测试应用。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-172">The app builds and runs.</span></span>

* <span data-ttu-id="5e8dd-173">输入 URL，然后单击“添加”按钮</span><span class="sxs-lookup"><span data-stu-id="5e8dd-173">Enter a URL and click the **Add** button</span></span>

  ![图像](_static/create.png)

  ![图像](_static/list.png)

  <span data-ttu-id="5e8dd-176">哇哦！</span><span class="sxs-lookup"><span data-stu-id="5e8dd-176">Tada!</span></span> <span data-ttu-id="5e8dd-177">现在便有了一个运行 Entity Framework Core 的简单 UWP 应用。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-177">You now have a simple UWP app running Entity Framework Core.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5e8dd-178">后续步骤</span><span class="sxs-lookup"><span data-stu-id="5e8dd-178">Next steps</span></span>

<span data-ttu-id="5e8dd-179">有关结合使用 EF Core 和 UWP 时应了解的兼容性和性能信息，请参阅 [EF Core 支持的 .NET 实现](../../platforms/index.md#universal-windows-platform)。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-179">For compatibility and performance information that you should know when using EF Core with UWP, see [.NET implementations supported by EF Core](../../platforms/index.md#universal-windows-platform).</span></span>

<span data-ttu-id="5e8dd-180">查阅本文档中的其他文章，深入了解 Entity Framework Core 功能。</span><span class="sxs-lookup"><span data-stu-id="5e8dd-180">Check out other articles in this documentation to learn more about Entity Framework Core features.</span></span>
