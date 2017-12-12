---
title: "ASP.NET Core入门 - 新数据库 - EF Core"
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 7e7ecaff29e9830bf3bcf742e6a5d54e1ced24de
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2017
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="3b9dc-102">使用新数据库在 ASP.NET Core 上开始使用 EF Core</span><span class="sxs-lookup"><span data-stu-id="3b9dc-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="3b9dc-103">在本演练中，你将使用 Entity Framework Core 构建执行基本数据访问的 ASP.NET Core MVC 应用程序。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-103">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="3b9dc-104">你将使用迁移功能从 EF Core 模型创建数据库。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-104">You will use migrations to create the database from your EF Core model.</span></span> <span data-ttu-id="3b9dc-105">有关更多 Entity Framework Core 教程的信息，请参阅[其他资源](#additional-resources)。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-105">See [Additional Resources](#additional-resources) for more Entity Framework Core tutorials.</span></span>

<span data-ttu-id="3b9dc-106">本教程要求：</span><span class="sxs-lookup"><span data-stu-id="3b9dc-106">This tutorial requires:</span></span>
* <span data-ttu-id="3b9dc-107">具有以下工作负载的 [Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/)：</span><span class="sxs-lookup"><span data-stu-id="3b9dc-107">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="3b9dc-108">“ASP.NET 和 Web 开发”（位于“Web 和云”下）</span><span class="sxs-lookup"><span data-stu-id="3b9dc-108">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="3b9dc-109">“.NET Core 跨平台开发”（位于“其他工具集”下）</span><span class="sxs-lookup"><span data-stu-id="3b9dc-109">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="3b9dc-110">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="3b9dc-110">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>

> [!TIP]  
> <span data-ttu-id="3b9dc-111">可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb)。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-111">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb) on GitHub.</span></span>

## <a name="create-a-new-project-in-visual-studio-2017"></a><span data-ttu-id="3b9dc-112">在 Visual Studio 2017 中创建新项目</span><span class="sxs-lookup"><span data-stu-id="3b9dc-112">Create a new project in Visual Studio 2017</span></span>

* <span data-ttu-id="3b9dc-113">“文件”>“新建”>“项目”</span><span class="sxs-lookup"><span data-stu-id="3b9dc-113">**File > New > Project**</span></span>
* <span data-ttu-id="3b9dc-114">从左侧菜单中选择“已安装”>“模板”>“Visual C#”>“.NET Core”。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-114">From the left menu select **Installed > Templates > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="3b9dc-115">选择“ASP.NET Core Web 应用程序”。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-115">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="3b9dc-116">输入“EFGetStarted.AspNetCore.NewDb”作为名称，然后单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-116">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="3b9dc-117">在“新建 ASP.NET Core Web 应用程序”对话框中：</span><span class="sxs-lookup"><span data-stu-id="3b9dc-117">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="3b9dc-118">确保在下拉列表中选择“.NET Core”和“ASP.NET Core 2.0”选项</span><span class="sxs-lookup"><span data-stu-id="3b9dc-118">Ensure the options **.NET Core** and **ASP.NET Core 2.0** are selected in the drop down lists</span></span>
  * <span data-ttu-id="3b9dc-119">选择“Web 应用程序(模型视图控制器)”项目模板</span><span class="sxs-lookup"><span data-stu-id="3b9dc-119">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="3b9dc-120">确保将“身份验证”设置为“无身份验证”</span><span class="sxs-lookup"><span data-stu-id="3b9dc-120">Ensure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="3b9dc-121">单击“确定” </span><span class="sxs-lookup"><span data-stu-id="3b9dc-121">Click **OK**</span></span>

<span data-ttu-id="3b9dc-122">警告：如果你使用“个人用户帐户”而不是“无身份验证”，则会将 Entity Framework Core 模型添加到 `Models\IdentityModel.cs` 中的项目中。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-122">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to your project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="3b9dc-123">使用本演练中学习的技巧，你可以选择添加第二个模型，或者扩展此现有模型以包含实体类。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-123">Using the techniques you will learn in this walkthrough, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="3b9dc-124">安装 Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="3b9dc-124">Install Entity Framework Core</span></span>

<span data-ttu-id="3b9dc-125">为要作为目标对象的 EF Core 数据库提供程序安装程序包。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-125">Install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="3b9dc-126">本演练使用 SQL Server。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-126">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="3b9dc-127">有关可用提供程序的列表，请参阅[数据库提供程序](../../providers/index.md)。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-127">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="3b9dc-128">“工具”>“NuGet 包管理器”>“包管理器控制台”</span><span class="sxs-lookup"><span data-stu-id="3b9dc-128">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="3b9dc-129">运行 `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="3b9dc-129">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="3b9dc-130">我们将使用一些 Entity Framework Core 工具从 EF Core 模型创建数据库。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-130">We will be using some Entity Framework Core Tools to create a database from your EF Core model.</span></span> <span data-ttu-id="3b9dc-131">因此，我们也会安装此工具包：</span><span class="sxs-lookup"><span data-stu-id="3b9dc-131">So we will install the tools package as well:</span></span>

* <span data-ttu-id="3b9dc-132">运行 `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="3b9dc-132">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="3b9dc-133">我们稍后将使用一些 ASP.NET Core 基架工具来创建控制器和视图。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-133">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="3b9dc-134">因此，我们也会安装此设计包：</span><span class="sxs-lookup"><span data-stu-id="3b9dc-134">So we will install this design package as well:</span></span>

* <span data-ttu-id="3b9dc-135">运行 `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span><span class="sxs-lookup"><span data-stu-id="3b9dc-135">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="3b9dc-136">创建模型</span><span class="sxs-lookup"><span data-stu-id="3b9dc-136">Create the model</span></span>

<span data-ttu-id="3b9dc-137">定义构成模型的上下文和实体类：</span><span class="sxs-lookup"><span data-stu-id="3b9dc-137">Define a context and entity classes that make up the model:</span></span>

* <span data-ttu-id="3b9dc-138">右键单击“Models”文件夹，然后选择“添加”>“类”。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-138">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="3b9dc-139">输入“Model.cs”作为名称，然后单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-139">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="3b9dc-140">将此文件的内容替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="3b9dc-140">Replace the contents of the file with the following code:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

<span data-ttu-id="3b9dc-141">注意：在真正的应用程序中，通常会将模型中的每个类放在单独的文件中。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-141">Note: In a real app you would typically put each class from your model in a separate file.</span></span> <span data-ttu-id="3b9dc-142">为简单起见，我们在本教程中将所有类放在一个文件中。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-142">For the sake of simplicity, we are putting all the classes in one file for this tutorial.</span></span>

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="3b9dc-143">通过依赖关系注入注册上下文</span><span class="sxs-lookup"><span data-stu-id="3b9dc-143">Register your context with dependency injection</span></span>

<span data-ttu-id="3b9dc-144">服务（例如 `BloggingContext`）在应用程序启动期间通过[依赖关系注入](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html)进行注册。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-144">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup.</span></span> <span data-ttu-id="3b9dc-145">然后，通过构造函数参数或属性为需要这些服务的组件（如 MVC 控制器）提供相应服务。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-145">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span>

<span data-ttu-id="3b9dc-146">为了使 MVC 控制器能够使用 `BloggingContext`，我们将把它注册为服务。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-146">In order for our MVC controllers to make use of `BloggingContext` we will register it as a service.</span></span>

* <span data-ttu-id="3b9dc-147">打开 Startup.cs</span><span class="sxs-lookup"><span data-stu-id="3b9dc-147">Open **Startup.cs**</span></span>
* <span data-ttu-id="3b9dc-148">添加以下 `using` 语句：</span><span class="sxs-lookup"><span data-stu-id="3b9dc-148">Add the following `using` statements:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

<span data-ttu-id="3b9dc-149">添加 `AddDbContext` 方法将其注册为服务：</span><span class="sxs-lookup"><span data-stu-id="3b9dc-149">Add the `AddDbContext` method to register it as a service:</span></span>

* <span data-ttu-id="3b9dc-150">将以下代码添加到 `ConfigureServices` 方法：</span><span class="sxs-lookup"><span data-stu-id="3b9dc-150">Add the following code to the `ConfigureServices` method:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

<span data-ttu-id="3b9dc-151">注意：真正的应用程序通常会将连接字符串放在配置文件中。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-151">Note: A real app would gennerally put the connection string in a configuration file.</span></span> <span data-ttu-id="3b9dc-152">为了简单起见，我们在代码中对它进行定义。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-152">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="3b9dc-153">有关详细信息，请参阅[连接字符串 ](../../miscellaneous/connection-strings.md)。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-153">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-your-database"></a><span data-ttu-id="3b9dc-154">创建数据库</span><span class="sxs-lookup"><span data-stu-id="3b9dc-154">Create your database</span></span>

<span data-ttu-id="3b9dc-155">有了模型后，你可以使用[迁移](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)创建数据库。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-155">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="3b9dc-156">打开 PMC：</span><span class="sxs-lookup"><span data-stu-id="3b9dc-156">Open the PMC:</span></span>

  <span data-ttu-id="3b9dc-157">“工具”–>“NuGet 包管理器”–>“包管理器控制台”</span><span class="sxs-lookup"><span data-stu-id="3b9dc-157">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="3b9dc-158">运行 `Add-Migration InitialCreate` 来为迁移搭建基架，从而为模型创建一组初始表。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-158">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for your model.</span></span> <span data-ttu-id="3b9dc-159">如果收到错误，指出 `The term 'add-migration' is not recognized as the name of a cmdlet`，请关闭并重新打开 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-159">If you receive an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>
* <span data-ttu-id="3b9dc-160">运行 `Update-Database` 以将新迁移应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-160">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="3b9dc-161">在应用迁移之前，此命令可创建数据库。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-161">This command creates the database before applying migrations.</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="3b9dc-162">创建控制器</span><span class="sxs-lookup"><span data-stu-id="3b9dc-162">Create a controller</span></span>

<span data-ttu-id="3b9dc-163">在项目中启用基架：</span><span class="sxs-lookup"><span data-stu-id="3b9dc-163">Enable scaffolding in the project:</span></span>

* <span data-ttu-id="3b9dc-164">在“解决方案资源管理器”中，右键单击“控制器”文件夹，然后选择“添加”>“控制器”。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-164">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="3b9dc-165">选择“最小依赖项”，然后单击“添加”。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-165">Select **Minimal Dependencies** and click **Add**.</span></span>
* <span data-ttu-id="3b9dc-166">你可以忽略或删除 *ScaffoldingReadMe.txt* 文件。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-166">You can ignore or delete the *ScaffoldingReadMe.txt* file.</span></span>

<span data-ttu-id="3b9dc-167">现在基架已启用，我们可以为 `Blog` 实体搭建一个控制器。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-167">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="3b9dc-168">在“解决方案资源管理器”中，右键单击“控制器”文件夹，然后选择“添加”>“控制器”。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-168">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="3b9dc-169">选择“视图使用 Entity Framework 的 MVC 控制器”，然后单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-169">Select **MVC Controller with views, using Entity Framework** and click **Ok**.</span></span>
* <span data-ttu-id="3b9dc-170">将“模型类”设置为“Blog”，将“数据上下文类”设置为“BloggingContext”。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-170">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="3b9dc-171">单击 **“添加”**。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-171">Click **Add**.</span></span>


## <a name="run-the-application"></a><span data-ttu-id="3b9dc-172">运行此应用程序</span><span class="sxs-lookup"><span data-stu-id="3b9dc-172">Run the application</span></span>

<span data-ttu-id="3b9dc-173">按 F5 以运行和测试应用程序。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-173">Press F5 to run and test the app.</span></span>

* <span data-ttu-id="3b9dc-174">导航到 `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="3b9dc-174">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="3b9dc-175">使用创建链接创建一些博客条目。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-175">Use the create link to create some blog entries.</span></span> <span data-ttu-id="3b9dc-176">测试“详细信息”和“删除”链接。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-176">Test the details and delete links.</span></span>

![图像](_static/create.png)

![图像](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="3b9dc-179">其他资源</span><span class="sxs-lookup"><span data-stu-id="3b9dc-179">Additional Resources</span></span>

* <span data-ttu-id="3b9dc-180">[EF - 通过 SQLite 新建数据库](xref:core/get-started/netcore/new-db-sqlite) - 跨平台的控制台 EF 教程。</span><span class="sxs-lookup"><span data-stu-id="3b9dc-180">[EF - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="3b9dc-181">Mac 或 Linux 上的 ASP.NET Core MVC 简介</span><span class="sxs-lookup"><span data-stu-id="3b9dc-181">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="3b9dc-182">具有 Visual Studio 的 ASP.NET Core MVC 简介</span><span class="sxs-lookup"><span data-stu-id="3b9dc-182">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="3b9dc-183">借助 Visual Studio 使用 ASP.NET Core 和 Entity Framework Core 入门</span><span class="sxs-lookup"><span data-stu-id="3b9dc-183">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
