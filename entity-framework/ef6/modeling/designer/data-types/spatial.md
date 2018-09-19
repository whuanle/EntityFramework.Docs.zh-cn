---
title: 空间的 EF 设计器-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
ms.openlocfilehash: 67cc6c007a4477b650e7c4875de8fac36a9ba2be
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283753"
---
# <a name="spatial---ef-designer"></a><span data-ttu-id="25d80-102">空间的 EF 设计器</span><span class="sxs-lookup"><span data-stu-id="25d80-102">Spatial - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="25d80-103">**EF5 及更高版本仅**的功能，Api，Entity Framework 5 中引入了此页所述的等。</span><span class="sxs-lookup"><span data-stu-id="25d80-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="25d80-104">如果使用的是早期版本，则部分或全部信息不适用。</span><span class="sxs-lookup"><span data-stu-id="25d80-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="25d80-105">视频和分步演练演示如何使用实体框架设计器的空间类型映射。</span><span class="sxs-lookup"><span data-stu-id="25d80-105">The video and step-by-step walkthrough shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="25d80-106">它还演示了如何使用 LINQ 查询来查找两个位置之间的距离。</span><span class="sxs-lookup"><span data-stu-id="25d80-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="25d80-107">本演练将使用第一个模型来创建新的数据库，但在 EF 设计器还可用于[Database First](~/ef6/modeling/designer/workflows/database-first.md)工作流，以便将映射到现有数据库。</span><span class="sxs-lookup"><span data-stu-id="25d80-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="25d80-108">在 Entity Framework 5 中引入的空间类型支持。</span><span class="sxs-lookup"><span data-stu-id="25d80-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="25d80-109">请注意，若要使用的空间类型、 枚举和表值函数等新功能，您必须面向.NET Framework 4.5。</span><span class="sxs-lookup"><span data-stu-id="25d80-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="25d80-110">默认情况下，visual Studio 2012 面向.NET 4.5。</span><span class="sxs-lookup"><span data-stu-id="25d80-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="25d80-111">若要使用的空间数据类型还必须使用具有空间支持实体框架提供程序。</span><span class="sxs-lookup"><span data-stu-id="25d80-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="25d80-112">请参阅[针对空间类型的提供程序支持](~/ef6/fundamentals/providers/spatial-support.md)有关详细信息。</span><span class="sxs-lookup"><span data-stu-id="25d80-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="25d80-113">有两种主要的空间数据类型： geography 和 geometry。</span><span class="sxs-lookup"><span data-stu-id="25d80-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="25d80-114">Geography 数据类型存储之类的椭球体数据 （例如 GPS 纬度和经度坐标表示）。</span><span class="sxs-lookup"><span data-stu-id="25d80-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="25d80-115">Geometry 数据类型表示欧几里得 （平面） 坐标系。</span><span class="sxs-lookup"><span data-stu-id="25d80-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="25d80-116">观看视频</span><span class="sxs-lookup"><span data-stu-id="25d80-116">Watch the video</span></span>
<span data-ttu-id="25d80-117">此视频介绍如何将映射与实体框架设计器的空间类型。</span><span class="sxs-lookup"><span data-stu-id="25d80-117">This video shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="25d80-118">它还演示了如何使用 LINQ 查询来查找两个位置之间的距离。</span><span class="sxs-lookup"><span data-stu-id="25d80-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="25d80-119">**主讲人**： 以下 Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="25d80-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="25d80-120">**视频**: [WMV](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span><span class="sxs-lookup"><span data-stu-id="25d80-120">**Video**: [WMV](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="25d80-121">系统必备组件</span><span class="sxs-lookup"><span data-stu-id="25d80-121">Pre-Requisites</span></span>

<span data-ttu-id="25d80-122">需要具有 Visual Studio 2012、 Ultimate、 Premium、 Professional 或 Web Express 版安装来完成本演练。</span><span class="sxs-lookup"><span data-stu-id="25d80-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="25d80-123">设置项目</span><span class="sxs-lookup"><span data-stu-id="25d80-123">Set up the Project</span></span>

1.  <span data-ttu-id="25d80-124">打开 Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="25d80-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="25d80-125">上**文件**菜单，依次指向**新建**，然后单击**项目**</span><span class="sxs-lookup"><span data-stu-id="25d80-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="25d80-126">在左窗格中，单击**Visual C\#**，然后选择**控制台**模板</span><span class="sxs-lookup"><span data-stu-id="25d80-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="25d80-127">输入**SpatialEFDesigner**作为名称的项目并单击**确定**</span><span class="sxs-lookup"><span data-stu-id="25d80-127">Enter **SpatialEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="25d80-128">创建新的模型使用 EF 设计器</span><span class="sxs-lookup"><span data-stu-id="25d80-128">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="25d80-129">右键单击解决方案资源管理器中的项目名称，指向**外**，然后单击**新项**</span><span class="sxs-lookup"><span data-stu-id="25d80-129">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="25d80-130">选择**数据**从左侧的菜单，然后选择**ADO.NET 实体数据模型**中模板窗格</span><span class="sxs-lookup"><span data-stu-id="25d80-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="25d80-131">输入**UniversityModel.edmx**以及该文件的名称，然后单击**添加**</span><span class="sxs-lookup"><span data-stu-id="25d80-131">Enter **UniversityModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="25d80-132">在实体数据模型向导页上，选择**空模型**中选择模型内容对话框</span><span class="sxs-lookup"><span data-stu-id="25d80-132">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="25d80-133">单击**完成**</span><span class="sxs-lookup"><span data-stu-id="25d80-133">Click **Finish**</span></span>

<span data-ttu-id="25d80-134">提供用于编辑模型的设计图面，实体设计器随即出现。</span><span class="sxs-lookup"><span data-stu-id="25d80-134">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="25d80-135">该向导执行下列操作：</span><span class="sxs-lookup"><span data-stu-id="25d80-135">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="25d80-136">生成定义概念模型、 存储模型以及两者之间的映射的 EnumTestModel.edmx 文件。</span><span class="sxs-lookup"><span data-stu-id="25d80-136">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="25d80-137">将.edmx 文件的元数据项目处理属性设置为嵌入在输出程序集，这样生成的元数据文件获取嵌入到程序集。</span><span class="sxs-lookup"><span data-stu-id="25d80-137">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="25d80-138">添加对以下程序集的引用： EntityFramework、 System.ComponentModel.DataAnnotations 和 system.data.entity 的引用。</span><span class="sxs-lookup"><span data-stu-id="25d80-138">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="25d80-139">创建 UniversityModel.tt 和 UniversityModel.Context.tt 文件并将它们添加.edmx 文件。</span><span class="sxs-lookup"><span data-stu-id="25d80-139">Creates UniversityModel.tt and UniversityModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="25d80-140">这些 T4 模板文件生成定义的 DbContext 派生类型和映射到.edmx 模型中实体的 POCO 类型的代码</span><span class="sxs-lookup"><span data-stu-id="25d80-140">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="25d80-141">添加新的实体类型</span><span class="sxs-lookup"><span data-stu-id="25d80-141">Add a New Entity Type</span></span>

1.  <span data-ttu-id="25d80-142">右键单击空白区域的设计图面上，选择**添加-&gt;实体**，出现新实体对话框</span><span class="sxs-lookup"><span data-stu-id="25d80-142">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="25d80-143">指定**大学**的类型名称，并指定**UniversityID**对于键属性名称，将类型保留为**Int32**</span><span class="sxs-lookup"><span data-stu-id="25d80-143">Specify **University** for the type name and specify **UniversityID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="25d80-144">单击“确定” </span><span class="sxs-lookup"><span data-stu-id="25d80-144">Click **OK**</span></span>
4.  <span data-ttu-id="25d80-145">右键单击该实体，然后选择**添加新的&gt;标量属性**</span><span class="sxs-lookup"><span data-stu-id="25d80-145">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="25d80-146">重命名的新属性**名称**</span><span class="sxs-lookup"><span data-stu-id="25d80-146">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="25d80-147">添加另一个标量属性和其重命名为**位置**打开属性窗口并更改到新属性的类型**Geography**</span><span class="sxs-lookup"><span data-stu-id="25d80-147">Add another scalar property and rename it to **Location** Open the Properties window and change the type of the new property to **Geography**</span></span>
7.  <span data-ttu-id="25d80-148">保存模型，并生成项目</span><span class="sxs-lookup"><span data-stu-id="25d80-148">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="25d80-149">在生成时，可能会在错误列表中出现有关未映射的实体和关联的警告。</span><span class="sxs-lookup"><span data-stu-id="25d80-149">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="25d80-150">你可以忽略这些警告，因为我们选择从模型生成数据库后，这些错误将会消失。</span><span class="sxs-lookup"><span data-stu-id="25d80-150">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="25d80-151">从模型生成数据库</span><span class="sxs-lookup"><span data-stu-id="25d80-151">Generate Database from Model</span></span>

<span data-ttu-id="25d80-152">现在我们可以生成基于模型的数据库。</span><span class="sxs-lookup"><span data-stu-id="25d80-152">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="25d80-153">右键单击实体设计器图面上，然后选择上的空白空间**根据模型生成数据库**</span><span class="sxs-lookup"><span data-stu-id="25d80-153">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="25d80-154">生成数据库向导的在数据连接对话框为的选择显示单击**新的连接**按钮指定 **(localdb)\\mssqllocaldb**服务器名称和**大学**数据库并单击**确定**</span><span class="sxs-lookup"><span data-stu-id="25d80-154">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **University** for the database and click **OK**</span></span>
3.  <span data-ttu-id="25d80-155">将弹出一个对话框，询问你是否想要创建新的数据库中，单击**是**。</span><span class="sxs-lookup"><span data-stu-id="25d80-155">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="25d80-156">单击**下一步**并创建数据库向导生成数据定义语言 (DDL) 创建数据库生成的 DDL 将显示在摘要和设置对话框框下，DDL 不包含的定义映射到枚举类型的表</span><span class="sxs-lookup"><span data-stu-id="25d80-156">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="25d80-157">单击**完成**单击完成不会执行 DDL 脚本。</span><span class="sxs-lookup"><span data-stu-id="25d80-157">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="25d80-158">创建数据库向导将执行以下： 打开**UniversityModel.edmx.sql** T-SQL 编辑器生成存储架构和映射部分的 EDMX 的文件中添加连接字符串信息到 App.config 文件</span><span class="sxs-lookup"><span data-stu-id="25d80-158">The Create Database Wizard does the following: Opens the **UniversityModel.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="25d80-159">单击鼠标右键按钮在 T-SQL 编辑器中，然后选择**Execute**连接到服务器对话框出现，请输入在步骤 2 中的连接信息并单击**连接**</span><span class="sxs-lookup"><span data-stu-id="25d80-159">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="25d80-160">若要查看生成的架构，右键单击 SQL Server 对象资源管理器中的数据库名称，然后选择**刷新**</span><span class="sxs-lookup"><span data-stu-id="25d80-160">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="25d80-161">保留和检索数据</span><span class="sxs-lookup"><span data-stu-id="25d80-161">Persist and Retrieve Data</span></span>

<span data-ttu-id="25d80-162">打开 Program.cs 文件的 Main 方法定义的位置。</span><span class="sxs-lookup"><span data-stu-id="25d80-162">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="25d80-163">以下代码添加到 Main 函数。</span><span class="sxs-lookup"><span data-stu-id="25d80-163">Add the following code into the Main function.</span></span>

<span data-ttu-id="25d80-164">该代码将两个新的大学对象添加到上下文。</span><span class="sxs-lookup"><span data-stu-id="25d80-164">The code adds two new University objects to the context.</span></span> <span data-ttu-id="25d80-165">使用 DbGeography.FromText 方法初始化空间属性。</span><span class="sxs-lookup"><span data-stu-id="25d80-165">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="25d80-166">表示为 WellKnownText 传递给该方法地理点。</span><span class="sxs-lookup"><span data-stu-id="25d80-166">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="25d80-167">然后，此代码将保存数据。</span><span class="sxs-lookup"><span data-stu-id="25d80-167">The code then saves the data.</span></span> <span data-ttu-id="25d80-168">然后，LINQ 查询返回大学对象其位置是最接近指定的位置，在构建并执行。</span><span class="sxs-lookup"><span data-stu-id="25d80-168">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

``` csharp
using (var context = new UniversityModelContainer())
{
    context.Universities.Add(new University()
    {
        Name = "Graphic Design Institute",
        Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
    });

    context.Universities.Add(new University()
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

<span data-ttu-id="25d80-169">编译并运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="25d80-169">Compile and run the application.</span></span> <span data-ttu-id="25d80-170">该程序生成以下输出：</span><span class="sxs-lookup"><span data-stu-id="25d80-170">The program produces the following output:</span></span>

```
The closest University to you is: School of Fine Art.
```

<span data-ttu-id="25d80-171">若要查看数据库中的数据，右键单击 SQL Server 对象资源管理器中的数据库名称，然后选择**刷新**。</span><span class="sxs-lookup"><span data-stu-id="25d80-171">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="25d80-172">然后，单击鼠标右键按钮表并选择**查看数据**。</span><span class="sxs-lookup"><span data-stu-id="25d80-172">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="25d80-173">总结</span><span class="sxs-lookup"><span data-stu-id="25d80-173">Summary</span></span>

<span data-ttu-id="25d80-174">在本演练中我们介绍了如何将使用实体框架设计器的空间类型映射以及如何在代码中使用的空间类型。</span><span class="sxs-lookup"><span data-stu-id="25d80-174">In this walkthrough we looked at how to map spatial types using the Entity Framework Designer and how to use spatial types in code.</span></span> 
