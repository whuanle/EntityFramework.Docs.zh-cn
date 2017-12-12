---
title: "ASP.NET Core 入门 - 现有数据库 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: afd99d68d2ba25ce58a21dc48d2c7ce27f208807
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2017
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a>通过现有数据库在 ASP.NET Core 上开始使用 EF Core

> [!IMPORTANT]  
> [.NET Core SDK](https://www.microsoft.com/net/download/core) 不再支持 `project.json` 或 Visual Studio 2015。 我们鼓励每位 .NET Core 开发人员都[从 project.json 迁移到 csproj](https://docs.microsoft.com/dotnet/articles/core/migration/) 和 [Visual Studio 2017](https://www.visualstudio.com/downloads/)。

在本演练中，你将使用 Entity Framework 构建执行基本数据访问的 ASP.NET Core MVC 应用程序。 你将使用反向工程基于现有数据库创建 Entity Framework 模型。

> [!TIP]  
> 可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb)。

## <a name="prerequisites"></a>先决条件

完成本演练需要以下先决条件：

* 具有以下工作负载的 [Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/)：
  * “ASP.NET 和 Web 开发”（位于“Web 和云”下）
  * “.NET Core 跨平台开发”（位于“其他工具集”下）
* [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).
* [博客数据库](#blogging-database)

### <a name="blogging-database"></a>博客数据库

本教程使用 LocalDb 实例上的博客数据库作为现有数据库。

> [!TIP]  
> 如果你已在其他教程中创建了博客数据库，则可以跳过这些步骤。

* 打开 Visual Studio
* “工具”->“连接到数据库...”
* 选择“Microsoft SQL Server”，然后单击“继续”
* 输入“(localdb)\mssqllocaldb”作为服务器名称
* 输入“master”作为数据库名称，然后单击“确定”
* Master 数据库现在显示在“服务器资源管理器”的“数据连接”中
* 右键单击“服务器资源管理器”中的数据库，然后选择“新建查询”
* 将下面列出的脚本复制到查询编辑器中
* 右键单击查询编辑器，然后选择“执行”

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>创建新项目

* 打开 Visual Studio 2017
* “文件”>“新建”>“项目...”
* 从左侧菜单中选择“已安装”->“模板”->“Visual C#”->“Web”
* 选择“ASP.NET Core Web 应用程序(.NET Core)”项目模板
* 输入 **EFGetStarted.AspNetCore.ExistingDb**作为名称，然后单击“确定”
* 等待“新建 ASP.NET Core Web 应用程序”对话框显示出来
* 在“ASP.NET Core 模板 2.0”下，选择“Web 应用程序(模型视图控制器)”
* 确保将“身份验证”设置为“无身份验证”
* 单击“确定” 

## <a name="install-entity-framework"></a>安装 Entity Framework

要使用 EF Core，请为要作为目标对象的数据库提供程序安装程序包。 本演练使用 SQL Server。 有关可用提供程序的列表，请参阅[数据库提供程序](../../providers/index.md)。

* “工具”>“NuGet 包管理器”>“包管理器控制台”

* 运行 `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

我们将使用一些 Entity Framework Tools 从数据库创建模型。 因此，我们也会安装此工具包：

* 运行 `Install-Package Microsoft.EntityFrameworkCore.Tools`

我们稍后将使用一些 ASP.NET Core 基架工具来创建控制器和视图。 因此，我们也会安装此设计包：

* 运行 `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`

## <a name="reverse-engineer-your-model"></a>对模型实施反向工程

现在是时候基于现有数据库创建 EF 模型了。

* “工具”–>“NuGet 包管理器”–>“包管理器控制台”
* 运行以下命令以从现有数据库创建模型：

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

如果收到错误，指出 `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`，请关闭并重新打开 Visual Studio。

> [!TIP]  
> 可以通过将 `-Tables` 参数添加到上述命令来指定要为哪些表生成实体。 例如 `-Tables Blog,Post`。

反向工程过程基于现有数据库的架构创建实体类 (`Blog.cs` & `Post.cs`) 和派生上下文 (`BloggingContext.cs`)。

 实体类是简单的 C# 对象，代表要查询和保存的数据。

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 上下文表示与数据库的会话，并允许查询和保存实体类的实例。

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

## <a name="register-your-context-with-dependency-injection"></a>通过依赖关系注入注册上下文

依赖关系注入的概念是 ASP.NET Core 的核心。 服务（例如 `BloggingContext`）在应用程序启动期间通过依赖关系注入进行注册。 然后，通过构造函数参数或属性为需要这些服务的组件（如 MVC 控制器）提供相应服务。 有关依赖关系注入的详细信息，请参阅 ASP.NET 网站上的文章[依赖关系注入](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html)。

### <a name="remove-inline-context-configuration"></a>删除内联上下文配置

在 ASP.NET Core 中，通常在 Startup.cs 中进行配置。 为符合这种模式，我们将把数据库提供程序的配置移至 Startup.cs。

* 打开 `Models\BloggingContext.cs`
* 删除 `OnConfiguring(...)` 方法

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* 添加以下构造函数，这将允许通过依赖关系注入将配置传递到上下文中

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a>在 Startup.cs 中注册并配置上下文

为了使 MVC 控制器能够使用 `BloggingContext`，我们将把它注册为一项服务。

* 打开 Startup.cs
* 在文件开头添加以下 `using` 语句

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

现在我们可以使用 `AddDbContext(...)` 方法将其注册为服务。
* 找到 `ConfigureServices(...)` 方法
* 添加以下代码以将上下文注册为服务

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> 在实际的应用程序中，通常会将连接字符串置于配置文件中。 为了简单起见，我们在代码中对它进行定义。 有关详细信息，请参阅[连接字符串](../../miscellaneous/connection-strings.md)。

## <a name="create-a-controller"></a>创建控制器

接下来，我们将在项目中启用基架。

* 在“解决方案资源管理器”中，右键单击“控制器”文件夹，然后选择“添加”->“控制器...”
* 选择“全部依赖项”，然单击“添加”
* 你可以忽略随后打开的 `ScaffoldingReadMe.txt` 文件中的说明

现在基架已启用，我们可以为 `Blog` 实体搭建一个控制器。

* 在“解决方案资源管理器”中，右键单击“控制器”文件夹，然后选择“添加”->“控制器...”
* 选择“视图使用 Entity Framework 的 MVC 控制器”，然后单击“确定”
* 将“模型类”设置为“Blog”，将“数据上下文类”设置为“BloggingContext”
* 单击“添加”

## <a name="run-the-application"></a>运行此应用程序

现在可以运行应用程序来查看其实际运行情况。

* “调试”->“开始执行(不调试)”
* 应用程序将生成并在 web 浏览器中打开
* 导航到 `/Blogs`
* 单击“新建”
* 输入新博客的 Url，然后单击“创建”

![图像](_static/create.png)

![图像](_static/index-existing-db.png)
