---
title: ASP.NET Core入门 - 新数据库 - EF Core
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: c6a86dd943dc7fe6f600455fe6743ea01a062aab
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996059"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="c01ac-102">使用新数据库在 ASP.NET Core 上开始使用 EF Core</span><span class="sxs-lookup"><span data-stu-id="c01ac-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="c01ac-103">在本教程中，你将使用 Entity Framework Core 构建执行基本数据访问的 ASP.NET Core MVC 应用程序。</span><span class="sxs-lookup"><span data-stu-id="c01ac-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="c01ac-104">使用迁移功能从 EF Core 模型创建数据库。</span><span class="sxs-lookup"><span data-stu-id="c01ac-104">You use migrations to create the database from your EF Core model.</span></span>

<span data-ttu-id="c01ac-105">[在 GitHub 上查看此文章的示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb)。</span><span class="sxs-lookup"><span data-stu-id="c01ac-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c01ac-106">系统必备</span><span class="sxs-lookup"><span data-stu-id="c01ac-106">Prerequisites</span></span>

<span data-ttu-id="c01ac-107">安装以下软件：</span><span class="sxs-lookup"><span data-stu-id="c01ac-107">Install the following software:</span></span>

* <span data-ttu-id="c01ac-108">具有以下工作负载的 [Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/)：</span><span class="sxs-lookup"><span data-stu-id="c01ac-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="c01ac-109">“ASP.NET 和 Web 开发”（位于“Web 和云”下）</span><span class="sxs-lookup"><span data-stu-id="c01ac-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="c01ac-110">“.NET Core 跨平台开发”（位于“其他工具集”下）</span><span class="sxs-lookup"><span data-stu-id="c01ac-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="c01ac-111">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="c01ac-111">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="create-a-new-project-in-visual-studio-2017"></a><span data-ttu-id="c01ac-112">在 Visual Studio 2017 中创建新项目</span><span class="sxs-lookup"><span data-stu-id="c01ac-112">Create a new project in Visual Studio 2017</span></span>

* <span data-ttu-id="c01ac-113">打开 Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="c01ac-113">Open Visual Studio 2017</span></span>
* <span data-ttu-id="c01ac-114">“文件”>“新建”>“项目”</span><span class="sxs-lookup"><span data-stu-id="c01ac-114">**File > New > Project**</span></span>
* <span data-ttu-id="c01ac-115">从左菜单中选择“已安装”>“Visual C#”>“.NET Core”。</span><span class="sxs-lookup"><span data-stu-id="c01ac-115">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="c01ac-116">选择“ASP.NET Core Web 应用程序”。</span><span class="sxs-lookup"><span data-stu-id="c01ac-116">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="c01ac-117">输入“EFGetStarted.AspNetCore.NewDb”作为名称，然后单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="c01ac-117">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="c01ac-118">在“新建 ASP.NET Core Web 应用程序”对话框中：</span><span class="sxs-lookup"><span data-stu-id="c01ac-118">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="c01ac-119">确保在下拉列表中选择“.NET Core”和“ASP.NET Core 2.1”选项</span><span class="sxs-lookup"><span data-stu-id="c01ac-119">Ensure the options **.NET Core** and **ASP.NET Core 2.1** are selected in the drop down lists</span></span>
  * <span data-ttu-id="c01ac-120">选择“Web 应用程序(模型视图控制器)”项目模板</span><span class="sxs-lookup"><span data-stu-id="c01ac-120">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="c01ac-121">确保将“身份验证”设置为“无身份验证”</span><span class="sxs-lookup"><span data-stu-id="c01ac-121">Ensure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="c01ac-122">单击“确定” </span><span class="sxs-lookup"><span data-stu-id="c01ac-122">Click **OK**</span></span>

<span data-ttu-id="c01ac-123">警告：如果你使用“个人用户帐户”而不是“无身份验证”，则会将 Entity Framework Core 模型添加到 `Models\IdentityModel.cs` 中的项目中。</span><span class="sxs-lookup"><span data-stu-id="c01ac-123">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to your project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="c01ac-124">使用在本教程中学习的技巧，可以选择添加第二个模型，或者扩展此现有模型以包含实体类。</span><span class="sxs-lookup"><span data-stu-id="c01ac-124">Using the techniques you learn in this tutorial, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="c01ac-125">安装 Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c01ac-125">Install Entity Framework Core</span></span>

<span data-ttu-id="c01ac-126">要安装 EF Core，请为要作为目标对象的 EF Core 数据库提供程序安装程序包。</span><span class="sxs-lookup"><span data-stu-id="c01ac-126">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="c01ac-127">有关可用提供程序的列表，请参阅[数据库提供程序](../../providers/index.md)。</span><span class="sxs-lookup"><span data-stu-id="c01ac-127">For a list of available providers see [Database Providers](../../providers/index.md).</span></span> 

<span data-ttu-id="c01ac-128">对于本教程，无需安装提供程序包，因为本教程使用 SQL Server。</span><span class="sxs-lookup"><span data-stu-id="c01ac-128">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="c01ac-129">SQL Server 提供程序包包含在 [Microsoft.AspnetCore.App 元包](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1)中。</span><span class="sxs-lookup"><span data-stu-id="c01ac-129">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

## <a name="create-the-model"></a><span data-ttu-id="c01ac-130">创建模型</span><span class="sxs-lookup"><span data-stu-id="c01ac-130">Create the model</span></span>

<span data-ttu-id="c01ac-131">定义构成模型的上下文类和实体类：</span><span class="sxs-lookup"><span data-stu-id="c01ac-131">Define a context class and entity classes that make up the model:</span></span>

* <span data-ttu-id="c01ac-132">右键单击“Models”文件夹，然后选择“添加”>“类”。</span><span class="sxs-lookup"><span data-stu-id="c01ac-132">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="c01ac-133">输入“Model.cs”作为名称，然后单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="c01ac-133">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="c01ac-134">将此文件的内容替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="c01ac-134">Replace the contents of the file with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

<span data-ttu-id="c01ac-135">在真正的应用程序中，通常会将模型中的每个类放在单独的文件中。</span><span class="sxs-lookup"><span data-stu-id="c01ac-135">In a real app you would typically put each class from your model in a separate file.</span></span> <span data-ttu-id="c01ac-136">为简单起见，本教程将所有类放在一个文件中。</span><span class="sxs-lookup"><span data-stu-id="c01ac-136">For the sake of simplicity, this tutorial puts all the classes in one file.</span></span>

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="c01ac-137">通过依赖关系注入注册上下文</span><span class="sxs-lookup"><span data-stu-id="c01ac-137">Register your context with dependency injection</span></span>

<span data-ttu-id="c01ac-138">服务（例如 `BloggingContext`）在应用程序启动期间通过[依赖关系注入](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html)进行注册。</span><span class="sxs-lookup"><span data-stu-id="c01ac-138">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup.</span></span> <span data-ttu-id="c01ac-139">然后，通过构造函数参数或属性为需要这些服务的组件（如 MVC 控制器）提供相应服务。</span><span class="sxs-lookup"><span data-stu-id="c01ac-139">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span>

<span data-ttu-id="c01ac-140">若要使 `BloggingContext` 对 MVC 控制器可用，请将其注册为服务。</span><span class="sxs-lookup"><span data-stu-id="c01ac-140">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

* <span data-ttu-id="c01ac-141">打开 Startup.cs</span><span class="sxs-lookup"><span data-stu-id="c01ac-141">Open **Startup.cs**</span></span>
* <span data-ttu-id="c01ac-142">添加以下 `using` 语句：</span><span class="sxs-lookup"><span data-stu-id="c01ac-142">Add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

<span data-ttu-id="c01ac-143">调用 `AddDbContext` 方法，将上下文注册为服务。</span><span class="sxs-lookup"><span data-stu-id="c01ac-143">Call the `AddDbContext` method to register the context as a service.</span></span>

* <span data-ttu-id="c01ac-144">将以下突出显示的代码添加到 `ConfigureServices` 方法：</span><span class="sxs-lookup"><span data-stu-id="c01ac-144">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=13-14)]

<span data-ttu-id="c01ac-145">注意：真正的应用程序通常会将连接字符串放在配置文件或环境变量中。</span><span class="sxs-lookup"><span data-stu-id="c01ac-145">Note: A real app would generally put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="c01ac-146">为简单起见，本教程在代码中定义它。</span><span class="sxs-lookup"><span data-stu-id="c01ac-146">For the sake of simplicity, this tutorial defines it in code.</span></span> <span data-ttu-id="c01ac-147">有关详细信息，请参阅[连接字符串 ](../../miscellaneous/connection-strings.md)。</span><span class="sxs-lookup"><span data-stu-id="c01ac-147">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="c01ac-148">创建数据库</span><span class="sxs-lookup"><span data-stu-id="c01ac-148">Create the database</span></span>

<span data-ttu-id="c01ac-149">有了模型后，你可以使用[迁移](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)创建数据库。</span><span class="sxs-lookup"><span data-stu-id="c01ac-149">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="c01ac-150">“工具”>“NuGet 包管理器”>“包管理器控制台”</span><span class="sxs-lookup"><span data-stu-id="c01ac-150">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="c01ac-151">运行 `Add-Migration InitialCreate` 来为迁移搭建基架，从而为模型创建一组初始表。</span><span class="sxs-lookup"><span data-stu-id="c01ac-151">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for your model.</span></span> <span data-ttu-id="c01ac-152">如果收到错误，指出 `The term 'add-migration' is not recognized as the name of a cmdlet`，请关闭并重新打开 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="c01ac-152">If you receive an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>
* <span data-ttu-id="c01ac-153">运行 `Update-Database` 以将新迁移应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="c01ac-153">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="c01ac-154">在应用迁移之前，此命令可创建数据库。</span><span class="sxs-lookup"><span data-stu-id="c01ac-154">This command creates the database before applying migrations.</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="c01ac-155">创建控制器</span><span class="sxs-lookup"><span data-stu-id="c01ac-155">Create a controller</span></span>

<span data-ttu-id="c01ac-156">生成 `Blog` 实体控制器和视图的基架。</span><span class="sxs-lookup"><span data-stu-id="c01ac-156">Scaffold a controller and views for the `Blog` entity.</span></span>

* <span data-ttu-id="c01ac-157">在“解决方案资源管理器”中，右键单击“控制器”文件夹，然后选择“添加”>“控制器”。</span><span class="sxs-lookup"><span data-stu-id="c01ac-157">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="c01ac-158">选择“视图使用 Entity Framework 的 MVC 控制器”，然后单击“添加”。</span><span class="sxs-lookup"><span data-stu-id="c01ac-158">Select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
* <span data-ttu-id="c01ac-159">将“模型类”设置为“Blog”，将“数据上下文类”设置为“BloggingContext”。</span><span class="sxs-lookup"><span data-stu-id="c01ac-159">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="c01ac-160">单击 **添加**。</span><span class="sxs-lookup"><span data-stu-id="c01ac-160">Click **Add**.</span></span>


## <a name="run-the-application"></a><span data-ttu-id="c01ac-161">运行此应用程序</span><span class="sxs-lookup"><span data-stu-id="c01ac-161">Run the application</span></span>

<span data-ttu-id="c01ac-162">按 F5 以运行和测试应用程序。</span><span class="sxs-lookup"><span data-stu-id="c01ac-162">Press F5 to run and test the app.</span></span>

* <span data-ttu-id="c01ac-163">导航到 `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="c01ac-163">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="c01ac-164">使用创建链接创建一些博客条目。</span><span class="sxs-lookup"><span data-stu-id="c01ac-164">Use the create link to create some blog entries.</span></span> <span data-ttu-id="c01ac-165">测试“详细信息”和“删除”链接。</span><span class="sxs-lookup"><span data-stu-id="c01ac-165">Test the details and delete links.</span></span>

![图像](_static/create.png)

![图像](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="c01ac-168">其他资源</span><span class="sxs-lookup"><span data-stu-id="c01ac-168">Additional Resources</span></span>

* <span data-ttu-id="c01ac-169">[EF - 通过 SQLite 新建数据库](xref:core/get-started/netcore/new-db-sqlite) - 跨平台的控制台 EF 教程。</span><span class="sxs-lookup"><span data-stu-id="c01ac-169">[EF - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="c01ac-170">Mac 或 Linux 上的 ASP.NET Core MVC 简介</span><span class="sxs-lookup"><span data-stu-id="c01ac-170">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="c01ac-171">具有 Visual Studio 的 ASP.NET Core MVC 简介</span><span class="sxs-lookup"><span data-stu-id="c01ac-171">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="c01ac-172">借助 Visual Studio 使用 ASP.NET Core 和 Entity Framework Core 入门</span><span class="sxs-lookup"><span data-stu-id="c01ac-172">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
