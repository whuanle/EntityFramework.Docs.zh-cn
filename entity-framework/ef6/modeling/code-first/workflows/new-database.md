---
title: 到新的数据库的 EF6 代码优先
author: divega
ms.date: 2016-10-23
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
ms.openlocfilehash: bb44a3300bc8ffc9d7050c4784e7b76b29c61796
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994613"
---
# <a name="code-first-to-a-new-database"></a><span data-ttu-id="ceb22-102">代码优先的新数据库</span><span class="sxs-lookup"><span data-stu-id="ceb22-102">Code First to a New Database</span></span>
<span data-ttu-id="ceb22-103">此视频和分步演练会提供针对新数据库 Code First 开发的简介。</span><span class="sxs-lookup"><span data-stu-id="ceb22-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="ceb22-104">此方案包括目标不存在的数据库和 Code First 将创建，或 Code First 将添加新的空数据库表过。</span><span class="sxs-lookup"><span data-stu-id="ceb22-104">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables too.</span></span> <span data-ttu-id="ceb22-105">代码首先允许你定义使用 C 模型\#或 VB.Net 类。</span><span class="sxs-lookup"><span data-stu-id="ceb22-105">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="ceb22-106">（可选） 可以使用特性在类和属性，或使用 fluent API 执行其他配置。</span><span class="sxs-lookup"><span data-stu-id="ceb22-106">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="ceb22-107">观看视频</span><span class="sxs-lookup"><span data-stu-id="ceb22-107">Watch the video</span></span>
<span data-ttu-id="ceb22-108">此视频提供了针对新数据库 Code First 开发的简介。</span><span class="sxs-lookup"><span data-stu-id="ceb22-108">This video provides an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="ceb22-109">此方案包括目标不存在的数据库和 Code First 将创建，或 Code First 将添加新的空数据库表过。</span><span class="sxs-lookup"><span data-stu-id="ceb22-109">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables too.</span></span> <span data-ttu-id="ceb22-110">代码首先允许你定义使用 C# 或 VB.Net 类模型。</span><span class="sxs-lookup"><span data-stu-id="ceb22-110">Code First allows you to define your model using C# or VB.Net classes.</span></span> <span data-ttu-id="ceb22-111">（可选） 可以使用特性在类和属性，或使用 fluent API 执行其他配置。</span><span class="sxs-lookup"><span data-stu-id="ceb22-111">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

<span data-ttu-id="ceb22-112">**主讲人**：[Rowan Miller](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="ceb22-112">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="ceb22-113">**视频**: [WMV](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span><span class="sxs-lookup"><span data-stu-id="ceb22-113">**Video**: [WMV](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span></span>

# <a name="pre-requisites"></a><span data-ttu-id="ceb22-114">系统必备组件</span><span class="sxs-lookup"><span data-stu-id="ceb22-114">Pre-Requisites</span></span>

<span data-ttu-id="ceb22-115">将需要安装最低版本为 Visual Studio 2010 或 Visual Studio 2012 安装来完成本演练。</span><span class="sxs-lookup"><span data-stu-id="ceb22-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="ceb22-116">如果使用 Visual Studio 2010，您还需要具有[NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)安装。</span><span class="sxs-lookup"><span data-stu-id="ceb22-116">If you are using Visual Studio 2010, you will also need to have [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="ceb22-117">1.创建应用程序</span><span class="sxs-lookup"><span data-stu-id="ceb22-117">1. Create the Application</span></span>

<span data-ttu-id="ceb22-118">为了简单起见，我们要构建使用 Code First 来执行数据访问的基本的控制台应用程序。</span><span class="sxs-lookup"><span data-stu-id="ceb22-118">To keep things simple we’re going to build a basic console application that uses Code First to perform data access.</span></span>

-   <span data-ttu-id="ceb22-119">打开 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ceb22-119">Open Visual Studio</span></span>
-   <span data-ttu-id="ceb22-120">**文件-&gt;新增-&gt;项目...**</span><span class="sxs-lookup"><span data-stu-id="ceb22-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="ceb22-121">选择**Windows**左侧的菜单和**控制台应用程序**</span><span class="sxs-lookup"><span data-stu-id="ceb22-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="ceb22-122">输入**CodeFirstNewDatabaseSample**作为名称</span><span class="sxs-lookup"><span data-stu-id="ceb22-122">Enter **CodeFirstNewDatabaseSample** as the name</span></span>
-   <span data-ttu-id="ceb22-123">选择“确定”</span><span class="sxs-lookup"><span data-stu-id="ceb22-123">Select **OK**</span></span>

## <a name="2-create-the-model"></a><span data-ttu-id="ceb22-124">2.创建模型</span><span class="sxs-lookup"><span data-stu-id="ceb22-124">2. Create the Model</span></span>

<span data-ttu-id="ceb22-125">让我们定义一个非常简单的模型使用的类。</span><span class="sxs-lookup"><span data-stu-id="ceb22-125">Let’s define a very simple model using classes.</span></span> <span data-ttu-id="ceb22-126">我们只将其定义中的 Program.cs 文件，但在实际应用程序会将出将类拆分为单独的文件和可能一个单独的项目中。</span><span class="sxs-lookup"><span data-stu-id="ceb22-126">We’re just defining them in the Program.cs file but in a real world application you would split your classes out into separate files and potentially a separate project.</span></span>

<span data-ttu-id="ceb22-127">在 Program.cs 中到 Program 类定义的下面添加以下两个类。</span><span class="sxs-lookup"><span data-stu-id="ceb22-127">Below the Program class definition in Program.cs add the following two classes.</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int BlogId { get; set; }
    public virtual Blog Blog { get; set; }
}
```

<span data-ttu-id="ceb22-128">您会注意到，我们正在实现两个导航属性 （Blog.Posts 和 Post.Blog） 虚拟。</span><span class="sxs-lookup"><span data-stu-id="ceb22-128">You’ll notice that we’re making the two navigation properties (Blog.Posts and Post.Blog) virtual.</span></span> <span data-ttu-id="ceb22-129">这使实体框架的延迟加载功能。</span><span class="sxs-lookup"><span data-stu-id="ceb22-129">This enables the Lazy Loading feature of Entity Framework.</span></span> <span data-ttu-id="ceb22-130">延迟加载意味着，这些属性的内容将自动从数据库加载时尝试对其进行访问。</span><span class="sxs-lookup"><span data-stu-id="ceb22-130">Lazy Loading means that the contents of these properties will be automatically loaded from the database when you try to access them.</span></span>

## <a name="3-create-a-context"></a><span data-ttu-id="ceb22-131">3.创建上下文</span><span class="sxs-lookup"><span data-stu-id="ceb22-131">3. Create a Context</span></span>

<span data-ttu-id="ceb22-132">现在就来定义派生的上下文，它表示与数据库中，使我们能够查询和保存数据的会话。</span><span class="sxs-lookup"><span data-stu-id="ceb22-132">Now it’s time to define a derived context, which represents a session with the database, allowing us to query and save data.</span></span> <span data-ttu-id="ceb22-133">我们定义的上下文，它派生自 System.Data.Entity.DbContext 并公开的类型化的 DbSet&lt;TEntity&gt;我们的模型中的一个类。</span><span class="sxs-lookup"><span data-stu-id="ceb22-133">We define a context that derives from System.Data.Entity.DbContext and exposes a typed DbSet&lt;TEntity&gt; for each class in our model.</span></span>

<span data-ttu-id="ceb22-134">我们现在已经开始使用实体框架中的类型，因此我们需要添加 EntityFramework NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="ceb22-134">We’re now starting to use types from the Entity Framework so we need to add the EntityFramework NuGet package.</span></span>

-   <span data-ttu-id="ceb22-135">**项目 –&gt;管理 NuGet 包...**</span><span class="sxs-lookup"><span data-stu-id="ceb22-135">**Project –&gt; Manage NuGet Packages…**</span></span>
    <span data-ttu-id="ceb22-136">注意： 如果你没有**管理 NuGet 包...**</span><span class="sxs-lookup"><span data-stu-id="ceb22-136">Note: If you don’t have the **Manage NuGet Packages…**</span></span> <span data-ttu-id="ceb22-137">应安装的选项[最新版本的 NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span><span class="sxs-lookup"><span data-stu-id="ceb22-137">option you should install the [latest version of NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span></span>
-   <span data-ttu-id="ceb22-138">选择**Online**选项卡</span><span class="sxs-lookup"><span data-stu-id="ceb22-138">Select the **Online** tab</span></span>
-   <span data-ttu-id="ceb22-139">选择**EntityFramework**包</span><span class="sxs-lookup"><span data-stu-id="ceb22-139">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="ceb22-140">单击**安装**</span><span class="sxs-lookup"><span data-stu-id="ceb22-140">Click **Install**</span></span>

<span data-ttu-id="ceb22-141">添加 using 语句 System.Data.Entity Program.cs 的顶部。</span><span class="sxs-lookup"><span data-stu-id="ceb22-141">Add a using statement for System.Data.Entity at the top of Program.cs.</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="ceb22-142">在 Program.cs 中的 Post 类下面添加以下派生的上下文。</span><span class="sxs-lookup"><span data-stu-id="ceb22-142">Below the Post class in Program.cs add the following derived context.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

<span data-ttu-id="ceb22-143">以下是 Program.cs 现在应如何包含的完整列表。</span><span class="sxs-lookup"><span data-stu-id="ceb22-143">Here is a complete listing of what Program.cs should now contain.</span></span>

``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Entity;

namespace CodeFirstNewDatabaseSample
{
    class Program
    {
        static void Main(string[] args)
        {
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Name { get; set; }

        public virtual List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }

    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
    }
}
```

<span data-ttu-id="ceb22-144">这是我们需要开始存储和检索数据的所有代码。</span><span class="sxs-lookup"><span data-stu-id="ceb22-144">That is all the code we need to start storing and retrieving data.</span></span> <span data-ttu-id="ceb22-145">很显然没有在后台发生的大量，我们来看看，但第一个稍后我们来看它的操作。</span><span class="sxs-lookup"><span data-stu-id="ceb22-145">Obviously there is quite a bit going on behind the scenes and we’ll take a look at that in a moment but first let’s see it in action.</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="ceb22-146">4.读取和写入数据</span><span class="sxs-lookup"><span data-stu-id="ceb22-146">4. Reading & Writing Data</span></span>

<span data-ttu-id="ceb22-147">在 Program.cs 中实现的 Main 方法，如下所示。</span><span class="sxs-lookup"><span data-stu-id="ceb22-147">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="ceb22-148">此代码将创建我们的上下文的新实例，并使用它来插入新博客。</span><span class="sxs-lookup"><span data-stu-id="ceb22-148">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="ceb22-149">然后它使用 LINQ 查询以从按标题按字母顺序排序的数据库中检索所有博客。</span><span class="sxs-lookup"><span data-stu-id="ceb22-149">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="ceb22-150">你现在可以运行该应用程序和它进行测试。</span><span class="sxs-lookup"><span data-stu-id="ceb22-150">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a><span data-ttu-id="ceb22-151">我的数据位于何处？</span><span class="sxs-lookup"><span data-stu-id="ceb22-151">Where’s My Data?</span></span>

<span data-ttu-id="ceb22-152">按照约定 DbContext 已为您创建数据库。</span><span class="sxs-lookup"><span data-stu-id="ceb22-152">By convention DbContext has created a database for you.</span></span>

-   <span data-ttu-id="ceb22-153">如果本地 SQL Express 实例不可用 （默认情况下使用 Visual Studio 2010 安装） 然后代码优先已创建了数据库在该实例上</span><span class="sxs-lookup"><span data-stu-id="ceb22-153">If a local SQL Express instance is available (installed by default with Visual Studio 2010) then Code First has created the database on that instance</span></span>
-   <span data-ttu-id="ceb22-154">如果 SQL Express 不可用，则 Code First 将尝试并使用[LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) （默认情况下使用 Visual Studio 2012 安装）</span><span class="sxs-lookup"><span data-stu-id="ceb22-154">If SQL Express isn’t available then Code First will try and use [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (installed by default with Visual Studio 2012)</span></span>
-   <span data-ttu-id="ceb22-155">派生上下文中，在本例中是完全限定名称命名数据库**CodeFirstNewDatabaseSample.BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="ceb22-155">The database is named after the fully qualified name of the derived context, in our case that is **CodeFirstNewDatabaseSample.BloggingContext**</span></span>

<span data-ttu-id="ceb22-156">这些是仅默认约定，通过多种方式，将使用 Code First 的数据库中提供了详细信息**DbContext 的模型和数据库连接的发现**主题。</span><span class="sxs-lookup"><span data-stu-id="ceb22-156">These are just the default conventions and there are various ways to change the database that Code First uses, more information is available in the **How DbContext Discovers the Model and Database Connection** topic.</span></span>
<span data-ttu-id="ceb22-157">可以连接到此数据库在 Visual Studio 中使用服务器资源管理器</span><span class="sxs-lookup"><span data-stu-id="ceb22-157">You can connect to this database using Server Explorer in Visual Studio</span></span>

-   <span data-ttu-id="ceb22-158">**视图-&gt;服务器资源管理器**</span><span class="sxs-lookup"><span data-stu-id="ceb22-158">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="ceb22-159">右键单击**数据连接**，然后选择**添加连接...**</span><span class="sxs-lookup"><span data-stu-id="ceb22-159">Right click on **Data Connections** and select **Add Connection…**</span></span>
-   <span data-ttu-id="ceb22-160">如果你尚未连接到数据库服务器资源管理器之前将需要选择 Microsoft SQL Server 作为数据源</span><span class="sxs-lookup"><span data-stu-id="ceb22-160">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![SelectDataSource](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="ceb22-162">连接到 LocalDB 或 SQL Express，具体取决于哪一个已安装</span><span class="sxs-lookup"><span data-stu-id="ceb22-162">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>

<span data-ttu-id="ceb22-163">我们现在可以检查 Code First 创建的架构。</span><span class="sxs-lookup"><span data-stu-id="ceb22-163">We can now inspect the schema that Code First created.</span></span>

![SchemaInitial](~/ef6/media/schemainitial.png)

<span data-ttu-id="ceb22-165">DbContext 计算出来以查看我们定义的 DbSet 属性包含在模型中哪些类。</span><span class="sxs-lookup"><span data-stu-id="ceb22-165">DbContext worked out what classes to include in the model by looking at the DbSet properties that we defined.</span></span> <span data-ttu-id="ceb22-166">然后使用默认组 Code First 约定来确定表和列的名称，确定数据类型，查找主键，等等。</span><span class="sxs-lookup"><span data-stu-id="ceb22-166">It then uses the default set of Code First conventions to determine table and column names, determine data types, find primary keys, etc.</span></span> <span data-ttu-id="ceb22-167">在本演练后面将介绍如何重写这些约定。</span><span class="sxs-lookup"><span data-stu-id="ceb22-167">Later in this walkthrough we’ll look at how you can override these conventions.</span></span>

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="ceb22-168">5.处理模型更改</span><span class="sxs-lookup"><span data-stu-id="ceb22-168">5. Dealing with Model Changes</span></span>

<span data-ttu-id="ceb22-169">现在就应该对我们的模型，进行一些更改，当我们进行这些更改，我们还需要更新数据库架构。</span><span class="sxs-lookup"><span data-stu-id="ceb22-169">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span> <span data-ttu-id="ceb22-170">若要执行此操作，我们将要使用简称称为 Code First 迁移或迁移的功能。</span><span class="sxs-lookup"><span data-stu-id="ceb22-170">To do this we are going to use a feature called Code First Migrations, or Migrations for short.</span></span>

<span data-ttu-id="ceb22-171">迁移使我们可以有序的集的步骤，描述了如何进行升级 （和降级） 我们数据库架构。</span><span class="sxs-lookup"><span data-stu-id="ceb22-171">Migrations allows us to have an ordered set of steps that describe how to upgrade (and downgrade) our database schema.</span></span> <span data-ttu-id="ceb22-172">每个步骤，称为迁移时，包含一些描述了要应用的更改的代码。</span><span class="sxs-lookup"><span data-stu-id="ceb22-172">Each of these steps, known as a migration, contains some code that describes the changes to be applied.</span></span> 

<span data-ttu-id="ceb22-173">第一步是为我们 bloggingcontext 启用 Code First 迁移。</span><span class="sxs-lookup"><span data-stu-id="ceb22-173">The first step is to enable Code First Migrations for our BloggingContext.</span></span>

-   <span data-ttu-id="ceb22-174">**工具-&gt;库程序包管理器-&gt;包管理器控制台**</span><span class="sxs-lookup"><span data-stu-id="ceb22-174">**Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
-   <span data-ttu-id="ceb22-175">在包管理器控制台中运行 Enable-Migrations 命令</span><span class="sxs-lookup"><span data-stu-id="ceb22-175">Run the **Enable-Migrations** command in Package Manager Console</span></span>
-   <span data-ttu-id="ceb22-176">一个新的 Migrations 文件夹已添加到我们包含两个项目的项目：</span><span class="sxs-lookup"><span data-stu-id="ceb22-176">A new Migrations folder has been added to our project that contains two items:</span></span>
    -   <span data-ttu-id="ceb22-177">**Configuration.cs** – 此文件包含迁移将用于迁移的设置 bloggingcontext。</span><span class="sxs-lookup"><span data-stu-id="ceb22-177">**Configuration.cs** – This file contains the settings that Migrations will use for migrating BloggingContext.</span></span> <span data-ttu-id="ceb22-178">我们无需更改任何内容对于本演练，但下面是你可以在其中指定种子数据，对于其他数据库，注册提供程序更改命名空间等中生成的迁移。</span><span class="sxs-lookup"><span data-stu-id="ceb22-178">We don’t need to change anything for this walkthrough, but here is where you can specify seed data, register providers for other databases, changes the namespace that migrations are generated in etc.</span></span>
    -   <span data-ttu-id="ceb22-179">**&lt;时间戳&gt;\_InitialCreate.cs** – 这是你第一次迁移，它表示已应用到数据库，以使其成为空数据库为包括博客和文章表的更改.</span><span class="sxs-lookup"><span data-stu-id="ceb22-179">**&lt;timestamp&gt;\_InitialCreate.cs** – This is your first migration, it represents the changes that have already been applied to the database to take it from being an empty database to one that includes the Blogs and Posts tables.</span></span> <span data-ttu-id="ceb22-180">尽管我们让代码优先会自动创建对我们而言，这些表，现在，我们已选择加入它们已转换为迁移的迁移。</span><span class="sxs-lookup"><span data-stu-id="ceb22-180">Although we let Code First automatically create these tables for us, now that we have opted in to Migrations they have been converted into a Migration.</span></span> <span data-ttu-id="ceb22-181">首先我们已应用此迁移的本地数据库中也有记录代码。</span><span class="sxs-lookup"><span data-stu-id="ceb22-181">Code First has also recorded in our local database that this Migration has already been applied.</span></span> <span data-ttu-id="ceb22-182">文件名上的时间戳用于排序目的。</span><span class="sxs-lookup"><span data-stu-id="ceb22-182">The timestamp on the filename is used for ordering purposes.</span></span>

    <span data-ttu-id="ceb22-183">现在让我们与我们的模型进行更改时，将 Url 属性添加到博客类：</span><span class="sxs-lookup"><span data-stu-id="ceb22-183">Now let’s make a change to our model, add a Url property to the Blog class:</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   <span data-ttu-id="ceb22-184">运行**Add-migration AddUrl**程序包管理器控制台命令。</span><span class="sxs-lookup"><span data-stu-id="ceb22-184">Run the **Add-Migration AddUrl** command in Package Manager Console.</span></span>
    <span data-ttu-id="ceb22-185">Add-migration 命令检查自上次迁移以来的更改和搭建基架以新的迁移与所发现的任何更改。</span><span class="sxs-lookup"><span data-stu-id="ceb22-185">The Add-Migration command checks for changes since your last migration and scaffolds a new migration with any changes that are found.</span></span> <span data-ttu-id="ceb22-186">我们可以为迁移指定的名称;在这种情况下，我们将调用迁移 AddUrl。</span><span class="sxs-lookup"><span data-stu-id="ceb22-186">We can give migrations a name; in this case we are calling the migration ‘AddUrl’.</span></span>
    <span data-ttu-id="ceb22-187">基架的代码说我们需要添加 Url 列时，可以保存字符串数据，为 dbo。Blogs 表。</span><span class="sxs-lookup"><span data-stu-id="ceb22-187">The scaffolded code is saying that we need to add a Url column, that can hold string data, to the dbo.Blogs table.</span></span> <span data-ttu-id="ceb22-188">必要时，我们无法编辑已搭建基架的代码，但在这种情况下不需要的。</span><span class="sxs-lookup"><span data-stu-id="ceb22-188">If needed, we could edit the scaffolded code but that’s not required in this case.</span></span>

``` csharp
namespace CodeFirstNewDatabaseSample.Migrations
{
    using System;
    using System.Data.Entity.Migrations;

    public partial class AddUrl : DbMigration
    {
        public override void Up()
        {
            AddColumn("dbo.Blogs", "Url", c => c.String());
        }

        public override void Down()
        {
            DropColumn("dbo.Blogs", "Url");
        }
    }
}
```

-   <span data-ttu-id="ceb22-189">运行**Update-database**程序包管理器控制台命令。</span><span class="sxs-lookup"><span data-stu-id="ceb22-189">Run the **Update-Database** command in Package Manager Console.</span></span> <span data-ttu-id="ceb22-190">此命令将任何待定迁移应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="ceb22-190">This command will apply any pending migrations to the database.</span></span> <span data-ttu-id="ceb22-191">因此迁移将只应用我们的新 AddUrl 迁移已应用我们 InitialCreate 迁移。</span><span class="sxs-lookup"><span data-stu-id="ceb22-191">Our InitialCreate migration has already been applied so migrations will just apply our new AddUrl migration.</span></span>
    <span data-ttu-id="ceb22-192">提示： 可以使用 **– Verbose**切换调用要查看正在执行对数据库的 SQL 数据库更新时。</span><span class="sxs-lookup"><span data-stu-id="ceb22-192">Tip: You can use the **–Verbose** switch when calling Update-Database to see the SQL that is being executed against the database.</span></span>

<span data-ttu-id="ceb22-193">新的 Url 列现在添加到数据库中的 Blogs 表：</span><span class="sxs-lookup"><span data-stu-id="ceb22-193">The new Url column is now added to the Blogs table in the database:</span></span>

![SchemaWithUrl](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a><span data-ttu-id="ceb22-195">6.数据注释</span><span class="sxs-lookup"><span data-stu-id="ceb22-195">6. Data Annotations</span></span>

<span data-ttu-id="ceb22-196">到目前为止我们只需让 EF 发现在模型中使用其默认约定，但存在将为我们的类不遵循的约定，并需要能够执行进一步的配置。</span><span class="sxs-lookup"><span data-stu-id="ceb22-196">So far we’ve just let EF discover the model using its default conventions, but there are going to be times when our classes don’t follow the conventions and we need to be able to perform further configuration.</span></span> <span data-ttu-id="ceb22-197">有两个选项;我们将介绍本部分中的数据注释，然后选择下一节中的 fluent API。</span><span class="sxs-lookup"><span data-stu-id="ceb22-197">There are two options for this; we’ll look at Data Annotations in this section and then the fluent API in the next section.</span></span>

-   <span data-ttu-id="ceb22-198">让我们将用户类添加到我们的模型</span><span class="sxs-lookup"><span data-stu-id="ceb22-198">Let’s add a User class to our model</span></span>

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="ceb22-199">我们还需要将一组添加到我们派生上下文</span><span class="sxs-lookup"><span data-stu-id="ceb22-199">We also need to add a set to our derived context</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   <span data-ttu-id="ceb22-200">如果我们尝试添加迁移，则会得到错误消息，指出"*EntityType 'User' 已定义任何键。为该 EntityType 定义键。"*</span><span class="sxs-lookup"><span data-stu-id="ceb22-200">If we tried to add a migration we’d get an error saying “*EntityType ‘User’ has no key defined. Define the key for this EntityType.”*</span></span> <span data-ttu-id="ceb22-201">因为 EF 有没有办法知道用户名应为用户的主键。</span><span class="sxs-lookup"><span data-stu-id="ceb22-201">because EF has no way of knowing that Username should be the primary key for User.</span></span>
-   <span data-ttu-id="ceb22-202">我们现在使用数据注释，因此我们需要添加一个 using 语句在 Program.cs 的顶部</span><span class="sxs-lookup"><span data-stu-id="ceb22-202">We’re going to use Data Annotations now so we need to add a using statement at the top of Program.cs</span></span>

```
using System.ComponentModel.DataAnnotations;
```

-   <span data-ttu-id="ceb22-203">现在添加批注以确定它是主键的 Username 属性</span><span class="sxs-lookup"><span data-stu-id="ceb22-203">Now annotate the Username property to identify that it is the primary key</span></span>

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="ceb22-204">使用**Add-migration AddUser**命令搭建基架，迁移，以应用这些更改到数据库</span><span class="sxs-lookup"><span data-stu-id="ceb22-204">Use the **Add-Migration AddUser** command to scaffold a migration to apply these changes to the database</span></span>
-   <span data-ttu-id="ceb22-205">运行**Update-database**命令，将新的迁移应用到数据库</span><span class="sxs-lookup"><span data-stu-id="ceb22-205">Run the **Update-Database** command to apply the new migration to the database</span></span>

<span data-ttu-id="ceb22-206">现在，新表已添加到数据库：</span><span class="sxs-lookup"><span data-stu-id="ceb22-206">The new table is now added to the database:</span></span>

![SchemaWithUsers](~/ef6/media/schemawithusers.png)

<span data-ttu-id="ceb22-208">是由 EF 支持注释的完整列表：</span><span class="sxs-lookup"><span data-stu-id="ceb22-208">The full list of annotations supported by EF is:</span></span>

-   [<span data-ttu-id="ceb22-209">KeyAttribute</span><span class="sxs-lookup"><span data-stu-id="ceb22-209">KeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [<span data-ttu-id="ceb22-210">StringLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="ceb22-210">StringLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [<span data-ttu-id="ceb22-211">MaxLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="ceb22-211">MaxLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [<span data-ttu-id="ceb22-212">ConcurrencyCheckAttribute</span><span class="sxs-lookup"><span data-stu-id="ceb22-212">ConcurrencyCheckAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [<span data-ttu-id="ceb22-213">有 RequiredAttribute</span><span class="sxs-lookup"><span data-stu-id="ceb22-213">RequiredAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [<span data-ttu-id="ceb22-214">TimestampAttribute</span><span class="sxs-lookup"><span data-stu-id="ceb22-214">TimestampAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [<span data-ttu-id="ceb22-215">ComplexTypeAttribute</span><span class="sxs-lookup"><span data-stu-id="ceb22-215">ComplexTypeAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [<span data-ttu-id="ceb22-216">ColumnAttribute</span><span class="sxs-lookup"><span data-stu-id="ceb22-216">ColumnAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [<span data-ttu-id="ceb22-217">TableAttribute</span><span class="sxs-lookup"><span data-stu-id="ceb22-217">TableAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [<span data-ttu-id="ceb22-218">InversePropertyAttribute</span><span class="sxs-lookup"><span data-stu-id="ceb22-218">InversePropertyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [<span data-ttu-id="ceb22-219">ForeignKeyAttribute</span><span class="sxs-lookup"><span data-stu-id="ceb22-219">ForeignKeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [<span data-ttu-id="ceb22-220">DatabaseGeneratedAttribute</span><span class="sxs-lookup"><span data-stu-id="ceb22-220">DatabaseGeneratedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [<span data-ttu-id="ceb22-221">NotMappedAttribute</span><span class="sxs-lookup"><span data-stu-id="ceb22-221">NotMappedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a><span data-ttu-id="ceb22-222">7.Fluent API</span><span class="sxs-lookup"><span data-stu-id="ceb22-222">7. Fluent API</span></span>

<span data-ttu-id="ceb22-223">在上一节中介绍了使用数据批注以补充或替代所检测到的约定。</span><span class="sxs-lookup"><span data-stu-id="ceb22-223">In the previous section we looked at using Data Annotations to supplement or override what was detected by convention.</span></span> <span data-ttu-id="ceb22-224">通过 Code First fluent API 配置模型的其他方法。</span><span class="sxs-lookup"><span data-stu-id="ceb22-224">The other way to configure the model is via the Code First fluent API.</span></span>

<span data-ttu-id="ceb22-225">模型配置最多可以使用简单的数据批注。</span><span class="sxs-lookup"><span data-stu-id="ceb22-225">Most model configuration can be done using simple data annotations.</span></span> <span data-ttu-id="ceb22-226">Fluent API 是指定数据批注可以做另外一些更高级的配置不可能的数据注释的一切内容的模型配置的更高级的方法。</span><span class="sxs-lookup"><span data-stu-id="ceb22-226">The fluent API is a more advanced way of specifying model configuration that covers everything that data annotations can do in addition to some more advanced configuration not possible with data annotations.</span></span> <span data-ttu-id="ceb22-227">数据注释和 fluent API 可以一起使用。</span><span class="sxs-lookup"><span data-stu-id="ceb22-227">Data annotations and the fluent API can be used together.</span></span>

<span data-ttu-id="ceb22-228">若要访问 fluent API 重写中 DbContext 的 OnModelCreating 方法。</span><span class="sxs-lookup"><span data-stu-id="ceb22-228">To access the fluent API you override the OnModelCreating method in DbContext.</span></span> <span data-ttu-id="ceb22-229">假设我们想要 User.DisplayName 存储中，若要显示此列重命名\_名称。</span><span class="sxs-lookup"><span data-stu-id="ceb22-229">Let’s say we wanted to rename the column that User.DisplayName is stored in to display\_name.</span></span>

-   <span data-ttu-id="ceb22-230">使用重写 OnModelCreating 方法上 bloggingcontext 下面的代码</span><span class="sxs-lookup"><span data-stu-id="ceb22-230">Override the OnModelCreating method on BloggingContext with the following code</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<User>()
            .Property(u => u.DisplayName)
            .HasColumnName("display_name");
    }
}
```

-   <span data-ttu-id="ceb22-231">使用**Add-migration ChangeDisplayName**命令搭建基架，迁移，以应用这些更改到数据库。</span><span class="sxs-lookup"><span data-stu-id="ceb22-231">Use the **Add-Migration ChangeDisplayName** command to scaffold a migration to apply these changes to the database.</span></span>
-   <span data-ttu-id="ceb22-232">运行**Update-database**命令，将新的迁移应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="ceb22-232">Run the **Update-Database** command to apply the new migration to the database.</span></span>

<span data-ttu-id="ceb22-233">DisplayName 列现在重命名以显示\_名称：</span><span class="sxs-lookup"><span data-stu-id="ceb22-233">The DisplayName column is now renamed to display\_name:</span></span>

![SchemaWithDisplayNameRenamed](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a><span data-ttu-id="ceb22-235">总结</span><span class="sxs-lookup"><span data-stu-id="ceb22-235">Summary</span></span>

<span data-ttu-id="ceb22-236">在本演练中介绍了使用新数据库 Code First 开发。</span><span class="sxs-lookup"><span data-stu-id="ceb22-236">In this walkthrough we looked at Code First development using a new database.</span></span> <span data-ttu-id="ceb22-237">我们定义使用类的模型，然后使用该模型来创建数据库和存储和检索数据。</span><span class="sxs-lookup"><span data-stu-id="ceb22-237">We defined a model using classes then used that model to create a database and store and retrieve data.</span></span> <span data-ttu-id="ceb22-238">创建数据库后我们将使用 Code First 迁移演变成我们的模型中更改架构。</span><span class="sxs-lookup"><span data-stu-id="ceb22-238">Once the database was created we used Code First Migrations to change the schema as our model evolved.</span></span> <span data-ttu-id="ceb22-239">我们还了解到如何将模型配置为使用数据注释和 Fluent API。</span><span class="sxs-lookup"><span data-stu-id="ceb22-239">We also saw how to configure a model using Data Annotations and the Fluent API.</span></span>
