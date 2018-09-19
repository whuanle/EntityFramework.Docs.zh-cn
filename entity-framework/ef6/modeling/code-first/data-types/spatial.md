---
title: 空间的 EF6 Code First –
author: divega
ms.date: 10/23/2016
ms.assetid: d617aed1-15f2-48a9-b187-186991c666e3
ms.openlocfilehash: b8f2485a7002dcb4079f4045432f3224123fdb2f
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283597"
---
# <a name="spatial---code-first"></a><span data-ttu-id="cba3d-102">空间的代码优先</span><span class="sxs-lookup"><span data-stu-id="cba3d-102">Spatial - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="cba3d-103">**EF5 及更高版本仅**的功能，Api，Entity Framework 5 中引入了此页所述的等。</span><span class="sxs-lookup"><span data-stu-id="cba3d-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="cba3d-104">如果使用的是早期版本，则部分或全部信息不适用。</span><span class="sxs-lookup"><span data-stu-id="cba3d-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="cba3d-105">视频和分步演练演示如何使用 Entity Framework Code First 的空间类型映射。</span><span class="sxs-lookup"><span data-stu-id="cba3d-105">The video and step-by-step walkthrough shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="cba3d-106">它还演示了如何使用 LINQ 查询来查找两个位置之间的距离。</span><span class="sxs-lookup"><span data-stu-id="cba3d-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="cba3d-107">本演练将使用 Code First 来创建新的数据库，但也可以使用[Code First 到现有数据库](~/ef6/modeling/code-first/workflows/existing-database.md)。</span><span class="sxs-lookup"><span data-stu-id="cba3d-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="cba3d-108">在 Entity Framework 5 中引入的空间类型支持。</span><span class="sxs-lookup"><span data-stu-id="cba3d-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="cba3d-109">请注意，若要使用的空间类型、 枚举和表值函数等新功能，您必须面向.NET Framework 4.5。</span><span class="sxs-lookup"><span data-stu-id="cba3d-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="cba3d-110">默认情况下，visual Studio 2012 面向.NET 4.5。</span><span class="sxs-lookup"><span data-stu-id="cba3d-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="cba3d-111">若要使用的空间数据类型还必须使用具有空间支持实体框架提供程序。</span><span class="sxs-lookup"><span data-stu-id="cba3d-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="cba3d-112">请参阅[针对空间类型的提供程序支持](~/ef6/fundamentals/providers/spatial-support.md)有关详细信息。</span><span class="sxs-lookup"><span data-stu-id="cba3d-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="cba3d-113">有两种主要的空间数据类型： geography 和 geometry。</span><span class="sxs-lookup"><span data-stu-id="cba3d-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="cba3d-114">Geography 数据类型存储之类的椭球体数据 （例如 GPS 纬度和经度坐标表示）。</span><span class="sxs-lookup"><span data-stu-id="cba3d-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="cba3d-115">Geometry 数据类型表示欧几里得 （平面） 坐标系。</span><span class="sxs-lookup"><span data-stu-id="cba3d-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="cba3d-116">观看视频</span><span class="sxs-lookup"><span data-stu-id="cba3d-116">Watch the video</span></span>
<span data-ttu-id="cba3d-117">此视频介绍如何将映射与 Entity Framework Code First 的空间类型。</span><span class="sxs-lookup"><span data-stu-id="cba3d-117">This video shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="cba3d-118">它还演示了如何使用 LINQ 查询来查找两个位置之间的距离。</span><span class="sxs-lookup"><span data-stu-id="cba3d-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="cba3d-119">**主讲人**： 以下 Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="cba3d-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="cba3d-120">**视频**: [WMV](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="cba3d-120">**Video**: [WMV](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="cba3d-121">系统必备组件</span><span class="sxs-lookup"><span data-stu-id="cba3d-121">Pre-Requisites</span></span>

<span data-ttu-id="cba3d-122">需要具有 Visual Studio 2012、 Ultimate、 Premium、 Professional 或 Web Express 版安装来完成本演练。</span><span class="sxs-lookup"><span data-stu-id="cba3d-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="cba3d-123">设置项目</span><span class="sxs-lookup"><span data-stu-id="cba3d-123">Set up the Project</span></span>

1.  <span data-ttu-id="cba3d-124">打开 Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="cba3d-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="cba3d-125">上**文件**菜单，依次指向**新建**，然后单击**项目**</span><span class="sxs-lookup"><span data-stu-id="cba3d-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="cba3d-126">在左窗格中，单击**Visual C\#**，然后选择**控制台**模板</span><span class="sxs-lookup"><span data-stu-id="cba3d-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="cba3d-127">输入**SpatialCodeFirst**作为名称的项目并单击**确定**</span><span class="sxs-lookup"><span data-stu-id="cba3d-127">Enter **SpatialCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="cba3d-128">定义使用 Code First 的新模型</span><span class="sxs-lookup"><span data-stu-id="cba3d-128">Define a New Model using Code First</span></span>

<span data-ttu-id="cba3d-129">使用 Code First 开发时通常首先编写.NET Framework 类定义概念 （域） 模型。</span><span class="sxs-lookup"><span data-stu-id="cba3d-129">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="cba3d-130">下面的代码定义大学类。</span><span class="sxs-lookup"><span data-stu-id="cba3d-130">The code below defines the University class.</span></span>

<span data-ttu-id="cba3d-131">该大学有 DbGeography 类型的位置属性。</span><span class="sxs-lookup"><span data-stu-id="cba3d-131">The University has the Location property of the DbGeography type.</span></span> <span data-ttu-id="cba3d-132">若要使用 DbGeography 类型时，必须添加对 system.data.entity 的引用程序集的引用，并还将添加 using 语句 System.Data.Spatial。</span><span class="sxs-lookup"><span data-stu-id="cba3d-132">To use the DbGeography type, you must add a reference to the System.Data.Entity assembly and also add the System.Data.Spatial using statement.</span></span>

<span data-ttu-id="cba3d-133">打开 Program.cs 文件并粘贴以下语句使用的文件的顶部：</span><span class="sxs-lookup"><span data-stu-id="cba3d-133">Open the Program.cs file and paste the following using statements at the top of the file:</span></span>

``` csharp
using System.Data.Spatial;
```

<span data-ttu-id="cba3d-134">将以下大学类定义添加到 Program.cs 文件。</span><span class="sxs-lookup"><span data-stu-id="cba3d-134">Add the following University class definition to the Program.cs file.</span></span>

``` csharp
public class University  
{
    public int UniversityID { get; set; }
    public string Name { get; set; }
    public DbGeography Location { get; set; }
}
```

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="cba3d-135">定义 DbContext 派生类型</span><span class="sxs-lookup"><span data-stu-id="cba3d-135">Define the DbContext Derived Type</span></span>

<span data-ttu-id="cba3d-136">除了定义实体，还需要定义一个类，派生自 DbContext，并公开 DbSet&lt;TEntity&gt;属性。</span><span class="sxs-lookup"><span data-stu-id="cba3d-136">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="cba3d-137">DbSet&lt;TEntity&gt;属性使知道你想要在模型中包含哪些的类型的上下文。</span><span class="sxs-lookup"><span data-stu-id="cba3d-137">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="cba3d-138">在运行时，其中包括填充数据库中的数据的对象期间将 DbContext 派生类型的实例管理的实体对象，到数据库更改跟踪，并保留数据。</span><span class="sxs-lookup"><span data-stu-id="cba3d-138">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="cba3d-139">EntityFramework 程序集中定义的 DbContext 和 DbSet 类型。</span><span class="sxs-lookup"><span data-stu-id="cba3d-139">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="cba3d-140">我们将使用 EntityFramework NuGet 包添加到此 DLL 的引用。</span><span class="sxs-lookup"><span data-stu-id="cba3d-140">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="cba3d-141">在解决方案资源管理器，右键单击项目名称。</span><span class="sxs-lookup"><span data-stu-id="cba3d-141">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="cba3d-142">选择**管理 NuGet 包...**</span><span class="sxs-lookup"><span data-stu-id="cba3d-142">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="cba3d-143">在管理 NuGet 包对话框中，选择**联机**选项卡，选择**EntityFramework**包。</span><span class="sxs-lookup"><span data-stu-id="cba3d-143">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="cba3d-144">单击**安装**</span><span class="sxs-lookup"><span data-stu-id="cba3d-144">Click **Install**</span></span>

<span data-ttu-id="cba3d-145">请注意，EntityFramework 程序集，除了 System.ComponentModel.DataAnnotations 程序集的引用也添加。</span><span class="sxs-lookup"><span data-stu-id="cba3d-145">Note, that in addition to the EntityFramework  assembly, a reference to the System.ComponentModel.DataAnnotations assembly is also added.</span></span>

<span data-ttu-id="cba3d-146">在 Program.cs 文件的顶部，添加以下 using 语句：</span><span class="sxs-lookup"><span data-stu-id="cba3d-146">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="cba3d-147">在 Program.cs 中添加上下文定义。</span><span class="sxs-lookup"><span data-stu-id="cba3d-147">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class UniversityContext : DbContext
{
    public DbSet<University> Universities { get; set; }
}
```

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="cba3d-148">保留和检索数据</span><span class="sxs-lookup"><span data-stu-id="cba3d-148">Persist and Retrieve Data</span></span>

<span data-ttu-id="cba3d-149">打开 Program.cs 文件的 Main 方法定义的位置。</span><span class="sxs-lookup"><span data-stu-id="cba3d-149">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="cba3d-150">以下代码添加到 Main 函数。</span><span class="sxs-lookup"><span data-stu-id="cba3d-150">Add the following code into the Main function.</span></span>

<span data-ttu-id="cba3d-151">该代码将两个新的大学对象添加到上下文。</span><span class="sxs-lookup"><span data-stu-id="cba3d-151">The code adds two new University objects to the context.</span></span> <span data-ttu-id="cba3d-152">使用 DbGeography.FromText 方法初始化空间属性。</span><span class="sxs-lookup"><span data-stu-id="cba3d-152">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="cba3d-153">表示为 WellKnownText 传递给该方法地理点。</span><span class="sxs-lookup"><span data-stu-id="cba3d-153">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="cba3d-154">然后，此代码将保存数据。</span><span class="sxs-lookup"><span data-stu-id="cba3d-154">The code then saves the data.</span></span> <span data-ttu-id="cba3d-155">然后，LINQ 查询返回大学对象其位置是最接近指定的位置，在构建并执行。</span><span class="sxs-lookup"><span data-stu-id="cba3d-155">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

``` csharp
using (var context = new UniversityContext ())
{
    context.Universities.Add(new University()
        {
            Name = "Graphic Design Institute",
            Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
        });

    context. Universities.Add(new University()
        {
            Name = "School of Fine Art",
            Location = DbGeography.FromText("POINT(-122.335197 47.646711)"),
        });

    context.SaveChanges();

    var myLocation = DbGeography.FromText("POINT(-122.296623 47.640405)");

    var university = (from u in context.Universities
                        orderby u.Location.Distance(myLocation)
                        select u).FirstOrDefault();

    Console.WriteLine(
        "The closest University to you is: {0}.",
        university.Name);
}
```

<span data-ttu-id="cba3d-156">编译并运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="cba3d-156">Compile and run the application.</span></span> <span data-ttu-id="cba3d-157">该程序生成以下输出：</span><span class="sxs-lookup"><span data-stu-id="cba3d-157">The program produces the following output:</span></span>

```
The closest University to you is: School of Fine Art.
```

## <a name="view-the-generated-database"></a><span data-ttu-id="cba3d-158">查看生成的数据库</span><span class="sxs-lookup"><span data-stu-id="cba3d-158">View the Generated Database</span></span>

<span data-ttu-id="cba3d-159">当您运行应用程序第一次时，Entity Framework 创建数据库。</span><span class="sxs-lookup"><span data-stu-id="cba3d-159">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="cba3d-160">由于我们具有安装 Visual Studio 2012，则数据库将创建 LocalDB 实例上。</span><span class="sxs-lookup"><span data-stu-id="cba3d-160">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="cba3d-161">默认情况下，实体框架的数据库的名称派生上下文的完全限定名称 (在本示例中为**SpatialCodeFirst.UniversityContext**)。</span><span class="sxs-lookup"><span data-stu-id="cba3d-161">By default, the Entity Framework names the database after the fully qualified name of the derived context (in this example that is **SpatialCodeFirst.UniversityContext**).</span></span> <span data-ttu-id="cba3d-162">将使用现有数据库的后续时间。</span><span class="sxs-lookup"><span data-stu-id="cba3d-162">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="cba3d-163">请注意，是否创建数据库后，可以对模型进行任何更改，则应使用 Code First 迁移更新数据库架构。</span><span class="sxs-lookup"><span data-stu-id="cba3d-163">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="cba3d-164">请参阅[到新数据库 Code First](~/ef6/modeling/code-first/workflows/new-database.md)有关使用迁移的示例。</span><span class="sxs-lookup"><span data-stu-id="cba3d-164">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="cba3d-165">若要查看的数据库和数据，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="cba3d-165">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="cba3d-166">在 Visual Studio 2012 主菜单中，选择**视图** - &gt; **SQL Server 对象资源管理器**。</span><span class="sxs-lookup"><span data-stu-id="cba3d-166">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="cba3d-167">如果 LocalDB 不在的服务器列表中，单击鼠标右键**SQL Server** ，然后选择**添加 SQL Server**使用默认**Windows 身份验证**连接到LocalDB 实例</span><span class="sxs-lookup"><span data-stu-id="cba3d-167">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the the LocalDB instance</span></span>
3.  <span data-ttu-id="cba3d-168">展开 LocalDB 节点</span><span class="sxs-lookup"><span data-stu-id="cba3d-168">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="cba3d-169">Unfold**数据库**文件夹，以查看新数据库并浏览到**大学**表</span><span class="sxs-lookup"><span data-stu-id="cba3d-169">Unfold the **Databases** folder to see the new database and browse to the **Universities** table</span></span>
5.  <span data-ttu-id="cba3d-170">若要查看数据，右键单击表并选择**查看数据**</span><span class="sxs-lookup"><span data-stu-id="cba3d-170">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="cba3d-171">总结</span><span class="sxs-lookup"><span data-stu-id="cba3d-171">Summary</span></span>

<span data-ttu-id="cba3d-172">在本演练中介绍了如何将使用与 Entity Framework Code First 的空间类型。</span><span class="sxs-lookup"><span data-stu-id="cba3d-172">In this walkthrough we looked at how to use spatial types with Entity Framework Code First.</span></span> 
