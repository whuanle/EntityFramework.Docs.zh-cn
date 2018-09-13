---
title: 代码优先的现有数据库-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
ms.openlocfilehash: f05420beb3dff2d632151fcbf48986b0d9cd18ff
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490605"
---
# <a name="code-first-to-an-existing-database"></a><span data-ttu-id="87f0a-102">代码优先的现有数据库</span><span class="sxs-lookup"><span data-stu-id="87f0a-102">Code First to an Existing Database</span></span>
<span data-ttu-id="87f0a-103">此视频和分步演练会提供面向现有数据库的代码优先开发的简介。</span><span class="sxs-lookup"><span data-stu-id="87f0a-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting an existing database.</span></span> <span data-ttu-id="87f0a-104">代码首先允许你定义使用 C 模型\#或 VB.Net 类。</span><span class="sxs-lookup"><span data-stu-id="87f0a-104">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="87f0a-105">可使用特性在类和属性，或使用 fluent API 执行可选的其他配置。</span><span class="sxs-lookup"><span data-stu-id="87f0a-105">Optionally additional configuration can be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="87f0a-106">观看视频</span><span class="sxs-lookup"><span data-stu-id="87f0a-106">Watch the video</span></span>
<span data-ttu-id="87f0a-107">此视频[第 9 频道上现已推出](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-)。</span><span class="sxs-lookup"><span data-stu-id="87f0a-107">This video is [now available on Channel 9](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="87f0a-108">系统必备组件</span><span class="sxs-lookup"><span data-stu-id="87f0a-108">Pre-Requisites</span></span>

<span data-ttu-id="87f0a-109">需要能够**Visual Studio 2012**或**Visual Studio 2013**安装来完成本演练。</span><span class="sxs-lookup"><span data-stu-id="87f0a-109">You will need to have **Visual Studio 2012** or **Visual Studio 2013** installed to complete this walkthrough.</span></span>

<span data-ttu-id="87f0a-110">您还需要版本**6.1** （或更高版本） 的**Entity Framework Tools for Visual Studio**安装。</span><span class="sxs-lookup"><span data-stu-id="87f0a-110">You will also need version **6.1** (or later) of the **Entity Framework Tools for Visual Studio** installed.</span></span> <span data-ttu-id="87f0a-111">请参阅[获取实体框架](~/ef6/fundamentals/install.md)安装实体框架工具的最新版本的信息。</span><span class="sxs-lookup"><span data-stu-id="87f0a-111">See [Get Entity Framework](~/ef6/fundamentals/install.md) for information on installing the latest version of the Entity Framework Tools.</span></span>

## <a name="1-create-an-existing-database"></a><span data-ttu-id="87f0a-112">1.创建现有数据库</span><span class="sxs-lookup"><span data-stu-id="87f0a-112">1. Create an Existing Database</span></span>

<span data-ttu-id="87f0a-113">通常当你面向的现有数据库，则将已创建，但我们需要在本演练中创建的数据库访问。</span><span class="sxs-lookup"><span data-stu-id="87f0a-113">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="87f0a-114">让我们继续并生成的数据库。</span><span class="sxs-lookup"><span data-stu-id="87f0a-114">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="87f0a-115">打开 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="87f0a-115">Open Visual Studio</span></span>
-   <span data-ttu-id="87f0a-116">**视图-&gt;服务器资源管理器**</span><span class="sxs-lookup"><span data-stu-id="87f0a-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="87f0a-117">右键单击**数据连接-&gt;添加连接...**</span><span class="sxs-lookup"><span data-stu-id="87f0a-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="87f0a-118">如果尚未连接到将数据库从**服务器资源管理器**将需要选择之前**Microsoft SQL Server**作为数据源</span><span class="sxs-lookup"><span data-stu-id="87f0a-118">If you haven’t connected to a database from **Server Explorer** before you’ll need to select **Microsoft SQL Server** as the data source</span></span>

    ![选择数据源](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="87f0a-120">连接到 LocalDB 实例，并输入**博客**作为数据库名称</span><span class="sxs-lookup"><span data-stu-id="87f0a-120">Connect to your LocalDB instance, and enter **Blogging** as the database name</span></span>

    ![LocalDB 连接](~/ef6/media/localdbconnection.png)

-   <span data-ttu-id="87f0a-122">选择**确定**并将你想要创建新数据库，请选择要求你**是**</span><span class="sxs-lookup"><span data-stu-id="87f0a-122">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![创建数据库对话框](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="87f0a-124">新数据库现在将出现在服务器资源管理器，右键单击它并选择**新查询**</span><span class="sxs-lookup"><span data-stu-id="87f0a-124">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="87f0a-125">将下面的 SQL 复制到新的查询，然后右键单击查询并选择**Execute**</span><span class="sxs-lookup"><span data-stu-id="87f0a-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
CREATE TABLE [dbo].[Blogs] (
    [BlogId] INT IDENTITY (1, 1) NOT NULL,
    [Name] NVARCHAR (200) NULL,
    [Url]  NVARCHAR (200) NULL,
    CONSTRAINT [PK_dbo.Blogs] PRIMARY KEY CLUSTERED ([BlogId] ASC)
);

CREATE TABLE [dbo].[Posts] (
    [PostId] INT IDENTITY (1, 1) NOT NULL,
    [Title] NVARCHAR (200) NULL,
    [Content] NTEXT NULL,
    [BlogId] INT NOT NULL,
    CONSTRAINT [PK_dbo.Posts] PRIMARY KEY CLUSTERED ([PostId] ASC),
    CONSTRAINT [FK_dbo.Posts_dbo.Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [dbo].[Blogs] ([BlogId]) ON DELETE CASCADE
);

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('The Visual Studio Blog', 'http://blogs.msdn.com/visualstudio/')

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('.NET Framework Blog', 'http://blogs.msdn.com/dotnet/')
```

## <a name="2-create-the-application"></a><span data-ttu-id="87f0a-126">2.创建应用程序</span><span class="sxs-lookup"><span data-stu-id="87f0a-126">2. Create the Application</span></span>

<span data-ttu-id="87f0a-127">为了简单起见，我们要构建使用 Code First 来执行数据访问的基本的控制台应用程序：</span><span class="sxs-lookup"><span data-stu-id="87f0a-127">To keep things simple we’re going to build a basic console application that uses Code First to perform data access:</span></span>

-   <span data-ttu-id="87f0a-128">打开 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="87f0a-128">Open Visual Studio</span></span>
-   <span data-ttu-id="87f0a-129">**文件-&gt;新增-&gt;项目...**</span><span class="sxs-lookup"><span data-stu-id="87f0a-129">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="87f0a-130">选择**Windows**左侧的菜单和**控制台应用程序**</span><span class="sxs-lookup"><span data-stu-id="87f0a-130">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="87f0a-131">输入**CodeFirstExistingDatabaseSample**作为名称</span><span class="sxs-lookup"><span data-stu-id="87f0a-131">Enter **CodeFirstExistingDatabaseSample** as the name</span></span>
-   <span data-ttu-id="87f0a-132">选择“确定”</span><span class="sxs-lookup"><span data-stu-id="87f0a-132">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="87f0a-133">3.反向工程模型</span><span class="sxs-lookup"><span data-stu-id="87f0a-133">3. Reverse Engineer Model</span></span>

<span data-ttu-id="87f0a-134">我们将充分利用适用于 Visual Studio 的实体框架工具来帮助我们获得一些初始代码来映射到数据库。</span><span class="sxs-lookup"><span data-stu-id="87f0a-134">We’re going to make use of the Entity Framework Tools for Visual Studio to help us generate some initial code to map to the database.</span></span> <span data-ttu-id="87f0a-135">这些工具只生成代码，您也可以键入手动您的喜好而定。</span><span class="sxs-lookup"><span data-stu-id="87f0a-135">These tools are just generating code that you could also type by hand if you prefer.</span></span>

-   <span data-ttu-id="87f0a-136">**项目-&gt;添加新项...**</span><span class="sxs-lookup"><span data-stu-id="87f0a-136">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="87f0a-137">选择**数据**左侧的菜单，然后**ADO.NET 实体数据模型**</span><span class="sxs-lookup"><span data-stu-id="87f0a-137">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="87f0a-138">输入**bloggingcontext**作为名称，然后单击**确定**</span><span class="sxs-lookup"><span data-stu-id="87f0a-138">Enter **BloggingContext** as the name and click **OK**</span></span>
-   <span data-ttu-id="87f0a-139">这将启动**实体数据模型向导**</span><span class="sxs-lookup"><span data-stu-id="87f0a-139">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="87f0a-140">选择**从数据库 Code First**单击**下一步**</span><span class="sxs-lookup"><span data-stu-id="87f0a-140">Select **Code First from Database** and click **Next**</span></span>

    ![向导一个 CFE](~/ef6/media/wizardonecfe.png)

-   <span data-ttu-id="87f0a-142">选择第一个部分中创建的数据库的连接，然后单击**下一步**</span><span class="sxs-lookup"><span data-stu-id="87f0a-142">Select the connection to the database you created in the first section and click **Next**</span></span>

    ![向导两个 CFE](~/ef6/media/wizardtwocfe.png)

-   <span data-ttu-id="87f0a-144">单击复选框旁边**表**导入的所有表并单击**完成**</span><span class="sxs-lookup"><span data-stu-id="87f0a-144">Click the checkbox next to **Tables** to import all tables and click **Finish**</span></span>

    ![向导三个 CFE](~/ef6/media/wizardthreecfe.png)

<span data-ttu-id="87f0a-146">反向工程过程完成的项目数后将已添加到项目中，让我们看看添加的内容。</span><span class="sxs-lookup"><span data-stu-id="87f0a-146">Once the reverse engineer process completes a number of items will have been added to the project, let's take a look at what's been added.</span></span>

### <a name="configuration-file"></a><span data-ttu-id="87f0a-147">配置文件</span><span class="sxs-lookup"><span data-stu-id="87f0a-147">Configuration file</span></span>

<span data-ttu-id="87f0a-148">App.config 文件已添加到项目中，此文件包含到现有数据库的连接字符串。</span><span class="sxs-lookup"><span data-stu-id="87f0a-148">An App.config file has been added to the project, this file contains the connection string to the existing database.</span></span>

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

<span data-ttu-id="87f0a-149">*也可以看到使用配置文件中的某些其他设置，这些是默认 EF 设置，以告诉 Code First 创建数据库的位置。由于我们要在我们的应用程序中映射到现有数据库将忽略这些设置。*</span><span class="sxs-lookup"><span data-stu-id="87f0a-149">*You’ll notice some other settings in the configuration file too, these are default EF settings that tell Code First where to create databases. Since we are mapping to an existing database these setting will be ignored in our application.*</span></span>

### <a name="derived-context"></a><span data-ttu-id="87f0a-150">派生的上下文</span><span class="sxs-lookup"><span data-stu-id="87f0a-150">Derived Context</span></span>

<span data-ttu-id="87f0a-151">一个**bloggingcontext**类已添加到项目。</span><span class="sxs-lookup"><span data-stu-id="87f0a-151">A **BloggingContext** class has been added to the project.</span></span> <span data-ttu-id="87f0a-152">上下文表示与数据库中，使我们能够查询和保存数据的会话。</span><span class="sxs-lookup"><span data-stu-id="87f0a-152">The context represents a session with the database, allowing us to query and save data.</span></span>
<span data-ttu-id="87f0a-153">上下文公开**DbSet&lt;TEntity&gt;** 我们的模型中每个类型。</span><span class="sxs-lookup"><span data-stu-id="87f0a-153">The context exposes a **DbSet&lt;TEntity&gt;** for each type in our model.</span></span> <span data-ttu-id="87f0a-154">您还会发现默认构造函数调用基构造函数使用**名称 =** 语法。</span><span class="sxs-lookup"><span data-stu-id="87f0a-154">You’ll also notice that the default constructor calls a base constructor using the **name=** syntax.</span></span> <span data-ttu-id="87f0a-155">这将告知 Code First，应从配置文件加载要使用此上下文的连接字符串。</span><span class="sxs-lookup"><span data-stu-id="87f0a-155">This tells Code First that the connection string to use for this context should be loaded from the configuration file.</span></span>

``` csharp
public partial class BloggingContext : DbContext
    {
        public BloggingContext()
            : base("name=BloggingContext")
        {
        }

        public virtual DbSet<Blog> Blogs { get; set; }
        public virtual DbSet<Post> Posts { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
        }
    }
```

<span data-ttu-id="87f0a-156">*应始终使用\*\*名称 =*\* 语法时配置文件中使用的连接字符串。这可确保，如果连接字符串不存在则实体框架将引发而不是通过约定创建新数据库。\*</span><span class="sxs-lookup"><span data-stu-id="87f0a-156">*You should always use the **name=** syntax when you are using a connection string in the config file. This ensures that if the connection string is not present then Entity Framework will throw rather than creating a new database by convention.*</span></span>

### <a name="model-classes"></a><span data-ttu-id="87f0a-157">模型类</span><span class="sxs-lookup"><span data-stu-id="87f0a-157">Model classes</span></span>

<span data-ttu-id="87f0a-158">最后，**博客**并**Post**类也已添加到项目。</span><span class="sxs-lookup"><span data-stu-id="87f0a-158">Finally, a **Blog** and **Post** class have also been added to the project.</span></span> <span data-ttu-id="87f0a-159">这些是构成了我们的模型的域类。</span><span class="sxs-lookup"><span data-stu-id="87f0a-159">These are the domain classes that make up our model.</span></span> <span data-ttu-id="87f0a-160">你将看到数据注释应用于的类来指定配置其中的代码优先约定将与现有数据库的结构不一致。</span><span class="sxs-lookup"><span data-stu-id="87f0a-160">You'll see Data Annotations applied to the classes to specify configuration where the Code First conventions would not align with the structure of the existing database.</span></span> <span data-ttu-id="87f0a-161">例如，你将看到**StringLength**上的批注**Blog.Name**并**Blog.Url**因为它们具有的最大长度**200**中数据库 (Code First 的默认设置是使用数据库提供程序-支持的最大长度**nvarchar （max)** SQL Server 中)。</span><span class="sxs-lookup"><span data-stu-id="87f0a-161">For example, you'll see the **StringLength** annotation on **Blog.Name** and **Blog.Url** since they have a maximum length of **200** in the database (the Code First default is to use the maximun length supported by the database provider - **nvarchar(max)** in SQL Server).</span></span>

``` csharp
public partial class Blog
{
    public Blog()
    {
        Posts = new HashSet<Post>();
    }

    public int BlogId { get; set; }

    [StringLength(200)]
    public string Name { get; set; }

    [StringLength(200)]
    public string Url { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}
```

## <a name="4-reading--writing-data"></a><span data-ttu-id="87f0a-162">4.读取和写入数据</span><span class="sxs-lookup"><span data-stu-id="87f0a-162">4. Reading & Writing Data</span></span>

<span data-ttu-id="87f0a-163">现在，我们有一个模型就可以使用它来访问某些数据。</span><span class="sxs-lookup"><span data-stu-id="87f0a-163">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="87f0a-164">实现**Main**中的方法**Program.cs** ，如下所示。</span><span class="sxs-lookup"><span data-stu-id="87f0a-164">Implement the **Main** method in **Program.cs** as shown below.</span></span> <span data-ttu-id="87f0a-165">此代码创建我们的上下文的新实例，并使用它来插入一个新**博客**。</span><span class="sxs-lookup"><span data-stu-id="87f0a-165">This code creates a new instance of our context and then uses it to insert a new **Blog**.</span></span> <span data-ttu-id="87f0a-166">然后，使用 LINQ 查询将检索所有**博客**从数据库按字母顺序排序**标题**。</span><span class="sxs-lookup"><span data-stu-id="87f0a-166">Then it uses a LINQ query to retrieve all **Blogs** from the database ordered alphabetically by **Title**.</span></span>

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

<span data-ttu-id="87f0a-167">你现在可以运行该应用程序和它进行测试。</span><span class="sxs-lookup"><span data-stu-id="87f0a-167">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a><span data-ttu-id="87f0a-168">如果我的数据库更改？</span><span class="sxs-lookup"><span data-stu-id="87f0a-168">What if My Database Changes?</span></span>

<span data-ttu-id="87f0a-169">Code First 数据库向导旨在生成的类，则可以调整和修改一起始点组。</span><span class="sxs-lookup"><span data-stu-id="87f0a-169">The Code First to Database wizard is designed to generate a starting point set of classes that you can then tweak and modify.</span></span> <span data-ttu-id="87f0a-170">如果数据库架构的更改可以手动编辑类也可以执行另一个反向工程，以覆盖类。</span><span class="sxs-lookup"><span data-stu-id="87f0a-170">If your database schema changes you can either manually edit the classes or perform another reverse engineer to overwrite the classes.</span></span>

## <a name="using-code-first-migrations-to-an-existing-database"></a><span data-ttu-id="87f0a-171">使用 Code First 迁移对现有数据库</span><span class="sxs-lookup"><span data-stu-id="87f0a-171">Using Code First Migrations to an Existing Database</span></span>

<span data-ttu-id="87f0a-172">如果你想要使用现有的数据库使用 Code First 迁移，请参阅[Code First 迁移对现有数据库](~/ef6/modeling/code-first/migrations/existing-database.md)。</span><span class="sxs-lookup"><span data-stu-id="87f0a-172">If you want to use Code First Migrations with an existing database, see [Code First Migrations to an existing database](~/ef6/modeling/code-first/migrations/existing-database.md).</span></span>

## <a name="summary"></a><span data-ttu-id="87f0a-173">总结</span><span class="sxs-lookup"><span data-stu-id="87f0a-173">Summary</span></span>

<span data-ttu-id="87f0a-174">在本演练中介绍了使用现有数据库的 Code First 开发。</span><span class="sxs-lookup"><span data-stu-id="87f0a-174">In this walkthrough we looked at Code First development using an existing database.</span></span> <span data-ttu-id="87f0a-175">我们使用 Entity Framework Tools for Visual Studio 进行反向工程，一组类，它映射到数据库并且无法用于存储和检索数据。</span><span class="sxs-lookup"><span data-stu-id="87f0a-175">We used the Entity Framework Tools for Visual Studio to reverse engineer a set of classes that mapped to the database and could be used to store and retrieve data.</span></span>
