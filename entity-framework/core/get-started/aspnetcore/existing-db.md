---
title: ASP.NET Core 入门 - 现有数据库 - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: e28149346ccd7531449ea696505588317471e6dd
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949148"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="113e8-102">通过现有数据库在 ASP.NET Core 上开始使用 EF Core</span><span class="sxs-lookup"><span data-stu-id="113e8-102">Getting Started with EF Core on ASP.NET Core with an Existing Database</span></span>

<span data-ttu-id="113e8-103">在本演练中，你将使用 Entity Framework 构建执行基本数据访问的 ASP.NET Core MVC 应用程序。</span><span class="sxs-lookup"><span data-stu-id="113e8-103">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework.</span></span> <span data-ttu-id="113e8-104">你将使用反向工程基于现有数据库创建 Entity Framework 模型。</span><span class="sxs-lookup"><span data-stu-id="113e8-104">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="113e8-105">可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb)。</span><span class="sxs-lookup"><span data-stu-id="113e8-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="113e8-106">系统必备</span><span class="sxs-lookup"><span data-stu-id="113e8-106">Prerequisites</span></span>

<span data-ttu-id="113e8-107">完成本演练需要以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="113e8-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* <span data-ttu-id="113e8-108">具有以下工作负载的 [Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/)：</span><span class="sxs-lookup"><span data-stu-id="113e8-108">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="113e8-109">“ASP.NET 和 Web 开发”（位于“Web 和云”下）</span><span class="sxs-lookup"><span data-stu-id="113e8-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="113e8-110">“.NET Core 跨平台开发”（位于“其他工具集”下）</span><span class="sxs-lookup"><span data-stu-id="113e8-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="113e8-111">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="113e8-111">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>
* [<span data-ttu-id="113e8-112">博客数据库</span><span class="sxs-lookup"><span data-stu-id="113e8-112">Blogging database</span></span>](#blogging-database)

### <a name="blogging-database"></a><span data-ttu-id="113e8-113">博客数据库</span><span class="sxs-lookup"><span data-stu-id="113e8-113">Blogging database</span></span>

<span data-ttu-id="113e8-114">本教程使用 LocalDb 实例上的博客数据库作为现有数据库。</span><span class="sxs-lookup"><span data-stu-id="113e8-114">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="113e8-115">如果你已在其他教程中创建了博客数据库，则可以跳过这些步骤。</span><span class="sxs-lookup"><span data-stu-id="113e8-115">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="113e8-116">打开 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="113e8-116">Open Visual Studio</span></span>
* <span data-ttu-id="113e8-117">“工具”->“连接到数据库...”</span><span class="sxs-lookup"><span data-stu-id="113e8-117">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="113e8-118">选择“Microsoft SQL Server”，然后单击“继续”</span><span class="sxs-lookup"><span data-stu-id="113e8-118">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="113e8-119">输入“(localdb)\mssqllocaldb”作为服务器名称</span><span class="sxs-lookup"><span data-stu-id="113e8-119">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="113e8-120">输入“master”作为数据库名称，然后单击“确定”</span><span class="sxs-lookup"><span data-stu-id="113e8-120">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="113e8-121">Master 数据库现在显示在“服务器资源管理器”的“数据连接”中</span><span class="sxs-lookup"><span data-stu-id="113e8-121">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="113e8-122">右键单击“服务器资源管理器”中的数据库，然后选择“新建查询”</span><span class="sxs-lookup"><span data-stu-id="113e8-122">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="113e8-123">将下面列出的脚本复制到查询编辑器中</span><span class="sxs-lookup"><span data-stu-id="113e8-123">Copy the script, listed below, into the query editor</span></span>
* <span data-ttu-id="113e8-124">右键单击查询编辑器，然后选择“执行”</span><span class="sxs-lookup"><span data-stu-id="113e8-124">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="113e8-125">创建新项目</span><span class="sxs-lookup"><span data-stu-id="113e8-125">Create a new project</span></span>

* <span data-ttu-id="113e8-126">打开 Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="113e8-126">Open Visual Studio 2017</span></span>
* <span data-ttu-id="113e8-127">“文件”>“新建”>“项目...”</span><span class="sxs-lookup"><span data-stu-id="113e8-127">**File -> New -> Project...**</span></span>
* <span data-ttu-id="113e8-128">从左侧菜单中选择“已安装”->“模板”->“Visual C#”->“Web”</span><span class="sxs-lookup"><span data-stu-id="113e8-128">From the left menu select **Installed -> Templates -> Visual C# -> Web**</span></span>
* <span data-ttu-id="113e8-129">选择“ASP.NET Core Web 应用程序(.NET Core)”项目模板</span><span class="sxs-lookup"><span data-stu-id="113e8-129">Select the **ASP.NET Core Web Application (.NET Core)** project template</span></span>
* <span data-ttu-id="113e8-130">输入 **EFGetStarted.AspNetCore.ExistingDb**作为名称，然后单击“确定”</span><span class="sxs-lookup"><span data-stu-id="113e8-130">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name and click **OK**</span></span>
* <span data-ttu-id="113e8-131">等待“新建 ASP.NET Core Web 应用程序”对话框显示出来</span><span class="sxs-lookup"><span data-stu-id="113e8-131">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="113e8-132">在“ASP.NET Core 模板 2.0”下，选择“Web 应用程序(模型视图控制器)”</span><span class="sxs-lookup"><span data-stu-id="113e8-132">Under **ASP.NET Core Templates 2.0** select the **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="113e8-133">确保将“身份验证”设置为“无身份验证”</span><span class="sxs-lookup"><span data-stu-id="113e8-133">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="113e8-134">单击“确定” </span><span class="sxs-lookup"><span data-stu-id="113e8-134">Click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="113e8-135">安装 Entity Framework</span><span class="sxs-lookup"><span data-stu-id="113e8-135">Install Entity Framework</span></span>

<span data-ttu-id="113e8-136">要使用 EF Core，请为要作为目标对象的数据库提供程序安装程序包。</span><span class="sxs-lookup"><span data-stu-id="113e8-136">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="113e8-137">本演练使用 SQL Server。</span><span class="sxs-lookup"><span data-stu-id="113e8-137">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="113e8-138">有关可用提供程序的列表，请参阅[数据库提供程序](../../providers/index.md)。</span><span class="sxs-lookup"><span data-stu-id="113e8-138">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="113e8-139">“工具”>“NuGet 包管理器”>“包管理器控制台”</span><span class="sxs-lookup"><span data-stu-id="113e8-139">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="113e8-140">运行 `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="113e8-140">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="113e8-141">我们将使用一些 Entity Framework Tools 从数据库创建模型。</span><span class="sxs-lookup"><span data-stu-id="113e8-141">We will be using some Entity Framework Tools to create a model from the database.</span></span> <span data-ttu-id="113e8-142">因此，我们也会安装此工具包：</span><span class="sxs-lookup"><span data-stu-id="113e8-142">So we will install the tools package as well:</span></span>

* <span data-ttu-id="113e8-143">运行 `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="113e8-143">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="113e8-144">我们稍后将使用一些 ASP.NET Core 基架工具来创建控制器和视图。</span><span class="sxs-lookup"><span data-stu-id="113e8-144">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="113e8-145">因此，我们也会安装此设计包：</span><span class="sxs-lookup"><span data-stu-id="113e8-145">So we will install this design package as well:</span></span>

* <span data-ttu-id="113e8-146">运行 `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span><span class="sxs-lookup"><span data-stu-id="113e8-146">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="113e8-147">对模型实施反向工程</span><span class="sxs-lookup"><span data-stu-id="113e8-147">Reverse engineer your model</span></span>

<span data-ttu-id="113e8-148">现在是时候基于现有数据库创建 EF 模型了。</span><span class="sxs-lookup"><span data-stu-id="113e8-148">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="113e8-149">“工具”–>“NuGet 包管理器”–>“包管理器控制台”</span><span class="sxs-lookup"><span data-stu-id="113e8-149">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="113e8-150">运行以下命令以从现有数据库创建模型：</span><span class="sxs-lookup"><span data-stu-id="113e8-150">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="113e8-151">如果收到错误，指出 `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`，请关闭并重新打开 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="113e8-151">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="113e8-152">可以通过将 `-Tables` 参数添加到上述命令来指定要为哪些表生成实体。</span><span class="sxs-lookup"><span data-stu-id="113e8-152">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="113e8-153">例如 `-Tables Blog,Post`。</span><span class="sxs-lookup"><span data-stu-id="113e8-153">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="113e8-154">反向工程过程基于现有数据库的架构创建实体类 (`Blog.cs` & `Post.cs`) 和派生上下文 (`BloggingContext.cs`)。</span><span class="sxs-lookup"><span data-stu-id="113e8-154">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="113e8-155">实体类是简单的 C# 对象，代表要查询和保存的数据。</span><span class="sxs-lookup"><span data-stu-id="113e8-155">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 <span data-ttu-id="113e8-156">上下文表示与数据库的会话，并允许查询和保存实体类的实例。</span><span class="sxs-lookup"><span data-stu-id="113e8-156">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

<!-- Static code listing, rather than a linked file, because the walkthrough modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
    public virtual DbSet<Blog> Blog { get; set; }
    public virtual DbSet<Post> Post { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        if (!optionsBuilder.IsConfigured)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>(entity =>
        {
            entity.Property(e => e.Url).IsRequired();
        });

        modelBuilder.Entity<Post>(entity =>
        {
            entity.HasOne(d => d.Blog)
                .WithMany(p => p.Post)
                .HasForeignKey(d => d.BlogId);
        });
    }
}
```

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="113e8-157">通过依赖关系注入注册上下文</span><span class="sxs-lookup"><span data-stu-id="113e8-157">Register your context with dependency injection</span></span>

<span data-ttu-id="113e8-158">依赖关系注入的概念是 ASP.NET Core 的核心。</span><span class="sxs-lookup"><span data-stu-id="113e8-158">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="113e8-159">服务（例如 `BloggingContext`）在应用程序启动期间通过依赖关系注入进行注册。</span><span class="sxs-lookup"><span data-stu-id="113e8-159">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="113e8-160">然后，通过构造函数参数或属性为需要这些服务的组件（如 MVC 控制器）提供相应服务。</span><span class="sxs-lookup"><span data-stu-id="113e8-160">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="113e8-161">有关依赖关系注入的详细信息，请参阅 ASP.NET 网站上的文章[依赖关系注入](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html)。</span><span class="sxs-lookup"><span data-stu-id="113e8-161">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="remove-inline-context-configuration"></a><span data-ttu-id="113e8-162">删除内联上下文配置</span><span class="sxs-lookup"><span data-stu-id="113e8-162">Remove inline context configuration</span></span>

<span data-ttu-id="113e8-163">在 ASP.NET Core 中，通常在 Startup.cs 中进行配置。</span><span class="sxs-lookup"><span data-stu-id="113e8-163">In ASP.NET Core, configuration is generally performed in **Startup.cs**.</span></span> <span data-ttu-id="113e8-164">为符合这种模式，我们将把数据库提供程序的配置移至 Startup.cs。</span><span class="sxs-lookup"><span data-stu-id="113e8-164">To conform to this pattern, we will move configuration of the database provider to **Startup.cs**.</span></span>

* <span data-ttu-id="113e8-165">打开 `Models\BloggingContext.cs`</span><span class="sxs-lookup"><span data-stu-id="113e8-165">Open `Models\BloggingContext.cs`</span></span>
* <span data-ttu-id="113e8-166">删除 `OnConfiguring(...)` 方法</span><span class="sxs-lookup"><span data-stu-id="113e8-166">Delete the `OnConfiguring(...)` method</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* <span data-ttu-id="113e8-167">添加以下构造函数，这将允许通过依赖关系注入将配置传递到上下文中</span><span class="sxs-lookup"><span data-stu-id="113e8-167">Add the following constructor, which will allow configuration to be passed into the context by dependency injection</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="113e8-168">在 Startup.cs 中注册并配置上下文</span><span class="sxs-lookup"><span data-stu-id="113e8-168">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="113e8-169">为了使 MVC 控制器能够使用 `BloggingContext`，我们将把它注册为一项服务。</span><span class="sxs-lookup"><span data-stu-id="113e8-169">In order for our MVC controllers to make use of `BloggingContext` we are going to register it as a service.</span></span>

* <span data-ttu-id="113e8-170">打开 Startup.cs</span><span class="sxs-lookup"><span data-stu-id="113e8-170">Open **Startup.cs**</span></span>
* <span data-ttu-id="113e8-171">在文件开头添加以下 `using` 语句</span><span class="sxs-lookup"><span data-stu-id="113e8-171">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="113e8-172">现在我们可以使用 `AddDbContext(...)` 方法将其注册为服务。</span><span class="sxs-lookup"><span data-stu-id="113e8-172">Now we can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="113e8-173">找到 `ConfigureServices(...)` 方法</span><span class="sxs-lookup"><span data-stu-id="113e8-173">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="113e8-174">添加以下代码以将上下文注册为服务</span><span class="sxs-lookup"><span data-stu-id="113e8-174">Add the following code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> <span data-ttu-id="113e8-175">在实际的应用程序中，通常会将连接字符串置于配置文件中。</span><span class="sxs-lookup"><span data-stu-id="113e8-175">In a real application you would typically put the connection string in a configuration file.</span></span> <span data-ttu-id="113e8-176">为了简单起见，我们在代码中对它进行定义。</span><span class="sxs-lookup"><span data-stu-id="113e8-176">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="113e8-177">有关详细信息，请参阅[连接字符串](../../miscellaneous/connection-strings.md)。</span><span class="sxs-lookup"><span data-stu-id="113e8-177">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="113e8-178">创建控制器</span><span class="sxs-lookup"><span data-stu-id="113e8-178">Create a controller</span></span>

<span data-ttu-id="113e8-179">接下来，我们将在项目中启用基架。</span><span class="sxs-lookup"><span data-stu-id="113e8-179">Next, we'll enable scaffolding in our project.</span></span>

* <span data-ttu-id="113e8-180">在“解决方案资源管理器”中，右键单击“控制器”文件夹，然后选择“添加”->“控制器...”</span><span class="sxs-lookup"><span data-stu-id="113e8-180">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="113e8-181">选择“全部依赖项”，然单击“添加”</span><span class="sxs-lookup"><span data-stu-id="113e8-181">Select **Full Dependencies** and click **Add**</span></span>
* <span data-ttu-id="113e8-182">你可以忽略随后打开的 `ScaffoldingReadMe.txt` 文件中的说明</span><span class="sxs-lookup"><span data-stu-id="113e8-182">You can ignore the instructions in the `ScaffoldingReadMe.txt` file that opens</span></span>

<span data-ttu-id="113e8-183">现在基架已启用，我们可以为 `Blog` 实体搭建一个控制器。</span><span class="sxs-lookup"><span data-stu-id="113e8-183">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="113e8-184">在“解决方案资源管理器”中，右键单击“控制器”文件夹，然后选择“添加”->“控制器...”</span><span class="sxs-lookup"><span data-stu-id="113e8-184">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="113e8-185">选择“视图使用 Entity Framework 的 MVC 控制器”，然后单击“确定”</span><span class="sxs-lookup"><span data-stu-id="113e8-185">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="113e8-186">将“模型类”设置为“Blog”，将“数据上下文类”设置为“BloggingContext”</span><span class="sxs-lookup"><span data-stu-id="113e8-186">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="113e8-187">单击“添加”</span><span class="sxs-lookup"><span data-stu-id="113e8-187">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="113e8-188">运行此应用程序</span><span class="sxs-lookup"><span data-stu-id="113e8-188">Run the application</span></span>

<span data-ttu-id="113e8-189">现在可以运行应用程序来查看其实际运行情况。</span><span class="sxs-lookup"><span data-stu-id="113e8-189">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="113e8-190">“调试”->“开始执行(不调试)”</span><span class="sxs-lookup"><span data-stu-id="113e8-190">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="113e8-191">应用程序将生成并在 web 浏览器中打开</span><span class="sxs-lookup"><span data-stu-id="113e8-191">The application will build and open in a web browser</span></span>
* <span data-ttu-id="113e8-192">导航到 `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="113e8-192">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="113e8-193">单击“新建”</span><span class="sxs-lookup"><span data-stu-id="113e8-193">Click **Create New**</span></span>
* <span data-ttu-id="113e8-194">输入新博客的 Url，然后单击“创建”</span><span class="sxs-lookup"><span data-stu-id="113e8-194">Enter a **Url** for the new blog and click **Create**</span></span>

![图像](_static/create.png)

![图像](_static/index-existing-db.png)
