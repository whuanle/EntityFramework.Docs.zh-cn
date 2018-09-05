---
title: UWP 入门 - 新数据库 - EF Core
author: rowanmiller
ms.date: 08/08/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: c243ef2a1940af9bf4f4b32f17acfcce7f972862
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996905"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="e1232-102">通过新数据库在通用 Windows 平台 (UWP) 上开始使用 EF Core</span><span class="sxs-lookup"><span data-stu-id="e1232-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

<span data-ttu-id="e1232-103">在本教程中，将构建一个通用 Windows 平台 (UWP) 应用程序，该应用程序使用 Entity Framework Core 对本地 SQLite 数据库执行基本数据访问。</span><span class="sxs-lookup"><span data-stu-id="e1232-103">In this tutorial, you build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="e1232-104">[在 GitHub 上查看此文章的示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP)。</span><span class="sxs-lookup"><span data-stu-id="e1232-104">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e1232-105">系统必备</span><span class="sxs-lookup"><span data-stu-id="e1232-105">Prerequisites</span></span>

* <span data-ttu-id="e1232-106">[Windows 10 Fall Creators Update（10.0；内部版本 16299）或更改版本](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10)。</span><span class="sxs-lookup"><span data-stu-id="e1232-106">[Windows 10 Fall Creators Update (10.0; Build 16299) or later](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span></span>

* <span data-ttu-id="e1232-107">具有通用 Windows 平台开发工作负载的 [Visual Studio 2017 15.7 版或更高版本](https://www.visualstudio.com/downloads/)。</span><span class="sxs-lookup"><span data-stu-id="e1232-107">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with the **Universal Windows Platform Development** workload.</span></span>

* <span data-ttu-id="e1232-108">[.NET Core 2.1 SDK 或更高版本](https://www.microsoft.com/net/core)。</span><span class="sxs-lookup"><span data-stu-id="e1232-108">[.NET Core 2.1 SDK or later](https://www.microsoft.com/net/core) or later.</span></span>

## <a name="create-a-model-project"></a><span data-ttu-id="e1232-109">创建模型项目</span><span class="sxs-lookup"><span data-stu-id="e1232-109">Create a model project</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e1232-110">由于 .NET Core 工具与 UWP 项目交互的方式受到限制，因此该模型需要放在非 UWP 项目中才能在包管理器控制台 (PMC) 中运行迁移命令</span><span class="sxs-lookup"><span data-stu-id="e1232-110">Due to limitations in the way .NET Core tools interact with UWP projects the model needs to be placed in a non-UWP project to be able to run migrations commands in the **Package Manager Console** (PMC)</span></span>

* <span data-ttu-id="e1232-111">打开 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1232-111">Open Visual Studio</span></span>

* <span data-ttu-id="e1232-112">“文件”>“新建”>“项目”</span><span class="sxs-lookup"><span data-stu-id="e1232-112">**File > New > Project**</span></span>

* <span data-ttu-id="e1232-113">从左侧菜单中选择“已安装”>“Visual C#”>“.NET Standard”。</span><span class="sxs-lookup"><span data-stu-id="e1232-113">From the left menu select **Installed > Visual C# > .NET Standard**.</span></span>

* <span data-ttu-id="e1232-114">选择“类库(.NET Standard)”模板。</span><span class="sxs-lookup"><span data-stu-id="e1232-114">Select the **Class Library (.NET Standard)** template.</span></span>

* <span data-ttu-id="e1232-115">将该项目命名为 Blogging.Model。</span><span class="sxs-lookup"><span data-stu-id="e1232-115">Name the project *Blogging.Model*.</span></span>

* <span data-ttu-id="e1232-116">将该解决方案命名为 Blogging。</span><span class="sxs-lookup"><span data-stu-id="e1232-116">Name the solution *Blogging*.</span></span>

* <span data-ttu-id="e1232-117">单击 **“确定”**。</span><span class="sxs-lookup"><span data-stu-id="e1232-117">Click **OK**.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="e1232-118">安装 Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e1232-118">Install Entity Framework Core</span></span>

<span data-ttu-id="e1232-119">要使用 EF Core，请为要作为目标对象的数据库提供程序安装程序包。</span><span class="sxs-lookup"><span data-stu-id="e1232-119">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="e1232-120">本教程使用 SQLite。</span><span class="sxs-lookup"><span data-stu-id="e1232-120">This tutorial uses SQLite.</span></span> <span data-ttu-id="e1232-121">有关可用提供程序的列表，请参阅[数据库提供程序](../../providers/index.md)。</span><span class="sxs-lookup"><span data-stu-id="e1232-121">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="e1232-122">“工具”>“NuGet 包管理器”>“包管理器控制台”。</span><span class="sxs-lookup"><span data-stu-id="e1232-122">**Tools > NuGet Package Manager > Package Manager Console**.</span></span>

* <span data-ttu-id="e1232-123">运行 `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span><span class="sxs-lookup"><span data-stu-id="e1232-123">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

<span data-ttu-id="e1232-124">在本教程的后部分，将使用某些 Entity Framework Core 工具维护数据库。</span><span class="sxs-lookup"><span data-stu-id="e1232-124">Later in this tutorial you will be using some Entity Framework Core tools to maintain the database.</span></span> <span data-ttu-id="e1232-125">因此，请同时安装该工具包。</span><span class="sxs-lookup"><span data-stu-id="e1232-125">So install the tools package as well.</span></span>

* <span data-ttu-id="e1232-126">运行 `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="e1232-126">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="e1232-127">创建模型</span><span class="sxs-lookup"><span data-stu-id="e1232-127">Create the model</span></span>

<span data-ttu-id="e1232-128">现在是时候定义构成模型的上下文和实体类了。</span><span class="sxs-lookup"><span data-stu-id="e1232-128">Now it's time to define a context and entity classes that make up the model.</span></span>

* <span data-ttu-id="e1232-129">删除 Class1.cs。</span><span class="sxs-lookup"><span data-stu-id="e1232-129">Delete *Class1.cs*.</span></span>

* <span data-ttu-id="e1232-130">用以下代码创建 Model.cs：</span><span class="sxs-lookup"><span data-stu-id="e1232-130">Create *Model.cs* with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a><span data-ttu-id="e1232-131">创建新的 UWP 项目</span><span class="sxs-lookup"><span data-stu-id="e1232-131">Create a new UWP project</span></span>

* <span data-ttu-id="e1232-132">在解决方案资源管理器中，右键单击该解决方案，然后选择“添加”>“新项目”。</span><span class="sxs-lookup"><span data-stu-id="e1232-132">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="e1232-133">从左侧菜单中选择“已安装”>“Visual C#”>“Windows Universal”。</span><span class="sxs-lookup"><span data-stu-id="e1232-133">From the left menu select **Installed > Visual C# > Windows Universal**.</span></span>

* <span data-ttu-id="e1232-134">选择“空白应用(通用 Windows)”项目模板。</span><span class="sxs-lookup"><span data-stu-id="e1232-134">Select the **Blank App (Universal Windows)** project template.</span></span>

* <span data-ttu-id="e1232-135">将项目命名为 Blogging.UWP ，然后单击“确定”</span><span class="sxs-lookup"><span data-stu-id="e1232-135">Name the project *Blogging.UWP*, and click **OK**</span></span>

* <span data-ttu-id="e1232-136">将目标和最低版本设置为至少“Windows 10 Fall Creators Update (10.0；内部版本 16299.0)”。</span><span class="sxs-lookup"><span data-stu-id="e1232-136">Set the target and minimum versions to at least **Windows 10 Fall Creators Update (10.0; build 16299.0)**.</span></span>

## <a name="create-the-initial-migration"></a><span data-ttu-id="e1232-137">创建初始迁移</span><span class="sxs-lookup"><span data-stu-id="e1232-137">Create the initial migration</span></span>

<span data-ttu-id="e1232-138">现已有一个模型，可设置应用以在第一次运行时创建数据库。</span><span class="sxs-lookup"><span data-stu-id="e1232-138">Now that you have a model, set up the app to create a database the first time it runs.</span></span> <span data-ttu-id="e1232-139">在本部分中，将创建初始迁移。</span><span class="sxs-lookup"><span data-stu-id="e1232-139">In this section, you create the initial migration.</span></span> <span data-ttu-id="e1232-140">在接下来的部分中，将添加在应用启动时应用此迁移的代码。</span><span class="sxs-lookup"><span data-stu-id="e1232-140">In the following section, you add code that applies this migration when the app starts.</span></span>

<span data-ttu-id="e1232-141">迁移工具需要非 UWP 启动项目，因此需先创建该项目。</span><span class="sxs-lookup"><span data-stu-id="e1232-141">Migrations tools require a non-UWP startup project, so create that first.</span></span>

* <span data-ttu-id="e1232-142">在解决方案资源管理器中，右键单击该解决方案，然后选择“添加”>“新项目”。</span><span class="sxs-lookup"><span data-stu-id="e1232-142">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="e1232-143">从左菜单中选择“已安装”>“Visual C#”>“.NET Core”。</span><span class="sxs-lookup"><span data-stu-id="e1232-143">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>

* <span data-ttu-id="e1232-144">选择“控制台应用程序(.NET Core)”项目模板。</span><span class="sxs-lookup"><span data-stu-id="e1232-144">Select the **Console App (.NET Core)** project template.</span></span>

* <span data-ttu-id="e1232-145">将该项目命名为 Blogging.Migrations.Startup ，然后单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="e1232-145">Name the project *Blogging.Migrations.Startup*, and click **OK**.</span></span>

* <span data-ttu-id="e1232-146">将 Blogging.Migrations.Startup 项目中的项目引用添加到 Blogging.Model 项目。</span><span class="sxs-lookup"><span data-stu-id="e1232-146">Add a project reference from the *Blogging.Migrations.Startup* project to the *Blogging.Model* project.</span></span>

<span data-ttu-id="e1232-147">现在，可创建初始迁移。</span><span class="sxs-lookup"><span data-stu-id="e1232-147">Now you can create your initial migration.</span></span>

* <span data-ttu-id="e1232-148">“工具”>“NuGet 包管理器”>“包管理器控制台”</span><span class="sxs-lookup"><span data-stu-id="e1232-148">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="e1232-149">选择 Blogging.Model 项目作为“默认项目”。</span><span class="sxs-lookup"><span data-stu-id="e1232-149">Select the *Blogging.Model* project as the **Default project**.</span></span>

* <span data-ttu-id="e1232-150">在解决方案资源管理器中，将 Blogging.Migrations.Startup 项目设置为启动项目。</span><span class="sxs-lookup"><span data-stu-id="e1232-150">In **Solution Explorer**, set the *Blogging.Migrations.Startup* project as the startup project.</span></span>

* <span data-ttu-id="e1232-151">运行 `Add-Migration InitialCreate`。</span><span class="sxs-lookup"><span data-stu-id="e1232-151">Run `Add-Migration InitialCreate`.</span></span>

  <span data-ttu-id="e1232-152">此命令为迁移搭建基架，该迁移为模型创建一组初始表。</span><span class="sxs-lookup"><span data-stu-id="e1232-152">This command scaffolds a migration that creates the initial set of tables for your model.</span></span>

## <a name="create-the-database-on-app-startup"></a><span data-ttu-id="e1232-153">在应用启动时创建数据库</span><span class="sxs-lookup"><span data-stu-id="e1232-153">Create the database on app startup</span></span>

<span data-ttu-id="e1232-154">由于希望在运行该应用的设备上创建数据库，因此，将添加代码，以便在应用程序启动时将任何待定迁移应用到本地数据库。</span><span class="sxs-lookup"><span data-stu-id="e1232-154">Since you want the database to be created on the device that the app runs on, add code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="e1232-155">应用第一次运行时，该操作将创建本地数据库。</span><span class="sxs-lookup"><span data-stu-id="e1232-155">The first time that the app runs, this will take care of creating the local database.</span></span>

* <span data-ttu-id="e1232-156">将 Blogging.UWP 项目中的项目引用添加到 Blogging.Model 项目。</span><span class="sxs-lookup"><span data-stu-id="e1232-156">Add a project reference from the *Blogging.UWP* project to the *Blogging.Model* project.</span></span>

* <span data-ttu-id="e1232-157">打开 App.xaml.cs。</span><span class="sxs-lookup"><span data-stu-id="e1232-157">Open *App.xaml.cs*.</span></span>

* <span data-ttu-id="e1232-158">添加突出显示的代码，应用任何待定迁移。</span><span class="sxs-lookup"><span data-stu-id="e1232-158">Add the highlighted code to apply any pending migrations.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> <span data-ttu-id="e1232-159">如果对模型进行了更改，则请使用 `Add-Migration` 命令为新迁移搭建基架，以便将相应更改应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="e1232-159">If you change your model, use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="e1232-160">当应用程序启动时，任何待定迁移都将应用到每个设备上的本地数据库。</span><span class="sxs-lookup"><span data-stu-id="e1232-160">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="e1232-161">EF 使用数据库中的 `__EFMigrationsHistory` 表来跟踪哪些迁移已经应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="e1232-161">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="e1232-162">使用模型</span><span class="sxs-lookup"><span data-stu-id="e1232-162">Use the model</span></span>

<span data-ttu-id="e1232-163">现在可以使用模型执行数据访问。</span><span class="sxs-lookup"><span data-stu-id="e1232-163">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="e1232-164">打开 MainPage.xaml.</span><span class="sxs-lookup"><span data-stu-id="e1232-164">Open *MainPage.xaml*.</span></span>

* <span data-ttu-id="e1232-165">添加下面突出显示的网页加载处理程序和 UI 内容</span><span class="sxs-lookup"><span data-stu-id="e1232-165">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="e1232-166">现在，添加代码来连接 UI 和数据库</span><span class="sxs-lookup"><span data-stu-id="e1232-166">Now add code to wire up the UI with the database</span></span>

* <span data-ttu-id="e1232-167">打开 MainPage.xaml.cs.</span><span class="sxs-lookup"><span data-stu-id="e1232-167">Open *MainPage.xaml.cs*.</span></span>

* <span data-ttu-id="e1232-168">从以下列表中添加突出显示的代码：</span><span class="sxs-lookup"><span data-stu-id="e1232-168">Add the highlighted code from the following listing:</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

<span data-ttu-id="e1232-169">现在可以运行应用程序来查看其实际运行情况。</span><span class="sxs-lookup"><span data-stu-id="e1232-169">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="e1232-170">在解决方案资源管理器中，右键单击Blogging.UWP 项目，然后选择“部署”。</span><span class="sxs-lookup"><span data-stu-id="e1232-170">In **Solution Explorer**, right-click the *Blogging.UWP* project and then select **Deploy**.</span></span>

* <span data-ttu-id="e1232-171">将 Blogging.UWP 设置为启动项目。</span><span class="sxs-lookup"><span data-stu-id="e1232-171">Set *Blogging.UWP* as the startup project.</span></span>

* <span data-ttu-id="e1232-172">“调试”>“开始执行(不调试)”</span><span class="sxs-lookup"><span data-stu-id="e1232-172">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="e1232-173">将生成并运行测试应用。</span><span class="sxs-lookup"><span data-stu-id="e1232-173">The app builds and runs.</span></span>

* <span data-ttu-id="e1232-174">输入 URL，然后单击“添加”按钮</span><span class="sxs-lookup"><span data-stu-id="e1232-174">Enter a URL and click the **Add** button</span></span>

  ![图像](_static/create.png)

  ![图像](_static/list.png)

  <span data-ttu-id="e1232-177">哇哦！</span><span class="sxs-lookup"><span data-stu-id="e1232-177">Tada!</span></span> <span data-ttu-id="e1232-178">现在便有了一个运行 Entity Framework Core 的简单 UWP 应用。</span><span class="sxs-lookup"><span data-stu-id="e1232-178">You now have a simple UWP app running Entity Framework Core.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e1232-179">后续步骤</span><span class="sxs-lookup"><span data-stu-id="e1232-179">Next steps</span></span>

<span data-ttu-id="e1232-180">有关结合使用 EF Core 和 UWP 时应了解的兼容性和性能信息，请参阅 [EF Core 支持的 .NET 实现](../../platforms/index.md#universal-windows-platform)。</span><span class="sxs-lookup"><span data-stu-id="e1232-180">For compatibility and performance information that you should know when using EF Core with UWP, see [.NET implementations supported by EF Core](../../platforms/index.md#universal-windows-platform).</span></span>

<span data-ttu-id="e1232-181">查阅本文档中的其他文章，深入了解 Entity Framework Core 功能。</span><span class="sxs-lookup"><span data-stu-id="e1232-181">Check out other articles in this documentation to learn more about Entity Framework Core features.</span></span>
