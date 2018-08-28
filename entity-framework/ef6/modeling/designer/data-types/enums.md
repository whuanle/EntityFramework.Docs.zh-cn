---
title: 枚举支持的 EF 设计器-EF6
author: divega
ms.date: 2016-10-23
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
ms.openlocfilehash: d4c5528c4dc13ab7189421feebf84c2cb2f4b2bb
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995632"
---
# <a name="enum-support---ef-designer"></a><span data-ttu-id="11154-102">枚举支持的 EF 设计器</span><span class="sxs-lookup"><span data-stu-id="11154-102">Enum Support - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="11154-103">**EF5 及更高版本仅**的功能，Api，Entity Framework 5 中引入了此页所述的等。</span><span class="sxs-lookup"><span data-stu-id="11154-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="11154-104">如果使用的是早期版本，则部分或全部信息不适用。</span><span class="sxs-lookup"><span data-stu-id="11154-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="11154-105">此视频和分步演练演示如何使用实体框架设计器的枚举类型。</span><span class="sxs-lookup"><span data-stu-id="11154-105">This video and step-by-step walkthrough shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="11154-106">它还演示了如何在 LINQ 查询中使用的枚举。</span><span class="sxs-lookup"><span data-stu-id="11154-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="11154-107">本演练将使用第一个模型来创建新的数据库，但在 EF 设计器还可用于[Database First](~/ef6/modeling/designer/workflows/database-first.md)工作流，以便将映射到现有数据库。</span><span class="sxs-lookup"><span data-stu-id="11154-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="11154-108">Entity Framework 5 中引入了枚举支持。</span><span class="sxs-lookup"><span data-stu-id="11154-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="11154-109">若要使用新功能，如枚举、 空间数据类型和表值函数，必须面向.NET Framework 4.5。</span><span class="sxs-lookup"><span data-stu-id="11154-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="11154-110">默认情况下，visual Studio 2012 面向.NET 4.5。</span><span class="sxs-lookup"><span data-stu-id="11154-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="11154-111">在实体框架中，枚举可以具有以下基础类型：**字节**， **Int16**， **Int32**， **Int64** ，或**SByte**。</span><span class="sxs-lookup"><span data-stu-id="11154-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="11154-112">观看视频</span><span class="sxs-lookup"><span data-stu-id="11154-112">Watch the Video</span></span>
<span data-ttu-id="11154-113">此视频介绍如何使用实体框架设计器的枚举类型。</span><span class="sxs-lookup"><span data-stu-id="11154-113">This video shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="11154-114">它还演示了如何在 LINQ 查询中使用的枚举。</span><span class="sxs-lookup"><span data-stu-id="11154-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="11154-115">**主讲人**： 以下 Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="11154-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="11154-116">**视频**: [WMV](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span><span class="sxs-lookup"><span data-stu-id="11154-116">**Video**: [WMV](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="11154-117">系统必备组件</span><span class="sxs-lookup"><span data-stu-id="11154-117">Pre-Requisites</span></span>

<span data-ttu-id="11154-118">需要具有 Visual Studio 2012、 Ultimate、 Premium、 Professional 或 Web Express 版安装来完成本演练。</span><span class="sxs-lookup"><span data-stu-id="11154-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="11154-119">设置项目</span><span class="sxs-lookup"><span data-stu-id="11154-119">Set up the Project</span></span>

1.  <span data-ttu-id="11154-120">打开 Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="11154-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="11154-121">上**文件**菜单，依次指向**新建**，然后单击**项目**</span><span class="sxs-lookup"><span data-stu-id="11154-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="11154-122">在左窗格中，单击**Visual C\#**，然后选择**控制台**模板</span><span class="sxs-lookup"><span data-stu-id="11154-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="11154-123">输入**EnumEFDesigner**作为名称的项目并单击**确定**</span><span class="sxs-lookup"><span data-stu-id="11154-123">Enter **EnumEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="11154-124">创建新的模型使用 EF 设计器</span><span class="sxs-lookup"><span data-stu-id="11154-124">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="11154-125">右键单击解决方案资源管理器中的项目名称，指向**外**，然后单击**新项**</span><span class="sxs-lookup"><span data-stu-id="11154-125">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="11154-126">选择**数据**从左侧的菜单，然后选择**ADO.NET 实体数据模型**中模板窗格</span><span class="sxs-lookup"><span data-stu-id="11154-126">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="11154-127">输入**EnumTestModel.edmx**以及该文件的名称，然后单击**添加**</span><span class="sxs-lookup"><span data-stu-id="11154-127">Enter **EnumTestModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="11154-128">在实体数据模型向导页上，选择**空模型**中选择模型内容对话框</span><span class="sxs-lookup"><span data-stu-id="11154-128">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="11154-129">单击**完成**</span><span class="sxs-lookup"><span data-stu-id="11154-129">Click **Finish**</span></span>

<span data-ttu-id="11154-130">提供用于编辑模型的设计图面，实体设计器随即出现。</span><span class="sxs-lookup"><span data-stu-id="11154-130">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="11154-131">该向导执行下列操作：</span><span class="sxs-lookup"><span data-stu-id="11154-131">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="11154-132">生成定义概念模型、 存储模型以及两者之间的映射的 EnumTestModel.edmx 文件。</span><span class="sxs-lookup"><span data-stu-id="11154-132">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="11154-133">将.edmx 文件的元数据项目处理属性设置为嵌入在输出程序集，这样生成的元数据文件获取嵌入到程序集。</span><span class="sxs-lookup"><span data-stu-id="11154-133">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="11154-134">添加对以下程序集的引用： EntityFramework、 System.ComponentModel.DataAnnotations 和 system.data.entity 的引用。</span><span class="sxs-lookup"><span data-stu-id="11154-134">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="11154-135">创建 EnumTestModel.tt 和 EnumTestModel.Context.tt 文件并将它们添加.edmx 文件。</span><span class="sxs-lookup"><span data-stu-id="11154-135">Creates EnumTestModel.tt and EnumTestModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="11154-136">这些 T4 模板文件生成定义的 DbContext 派生类型和映射到.edmx 模型中实体的 POCO 类型的代码。</span><span class="sxs-lookup"><span data-stu-id="11154-136">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model.</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="11154-137">添加新的实体类型</span><span class="sxs-lookup"><span data-stu-id="11154-137">Add a New Entity Type</span></span>

1.  <span data-ttu-id="11154-138">右键单击空白区域的设计图面上，选择**添加-&gt;实体**，出现新实体对话框</span><span class="sxs-lookup"><span data-stu-id="11154-138">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="11154-139">指定**部门**的类型名称，并指定**DepartmentID**对于键属性名称，将类型保留为**Int32**</span><span class="sxs-lookup"><span data-stu-id="11154-139">Specify **Department** for the type name and specify **DepartmentID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="11154-140">单击“确定” </span><span class="sxs-lookup"><span data-stu-id="11154-140">Click **OK**</span></span>
4.  <span data-ttu-id="11154-141">右键单击该实体，然后选择**添加新的&gt;标量属性**</span><span class="sxs-lookup"><span data-stu-id="11154-141">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="11154-142">重命名的新属性**名称**</span><span class="sxs-lookup"><span data-stu-id="11154-142">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="11154-143">更改到的新属性的类型**Int32** （默认情况下，新属性为字符串类型） 若要更改的类型，打开属性窗口并更改类型属性等于**Int32**</span><span class="sxs-lookup"><span data-stu-id="11154-143">Change the type of the new property to **Int32** (by default, the new property is of String type) To change the type, open the Properties window and change the Type property to **Int32**</span></span>
7.  <span data-ttu-id="11154-144">添加另一个标量属性和其重命名为**预算**，将类型更改为**十进制**</span><span class="sxs-lookup"><span data-stu-id="11154-144">Add another scalar property and rename it to **Budget**, change the type to **Decimal**</span></span>

## <a name="add-an-enum-type"></a><span data-ttu-id="11154-145">添加枚举类型</span><span class="sxs-lookup"><span data-stu-id="11154-145">Add an Enum Type</span></span>

1.  <span data-ttu-id="11154-146">在实体框架设计器中，右键单击名称属性中，选择**将转换为枚举**</span><span class="sxs-lookup"><span data-stu-id="11154-146">In the Entity Framework Designer, right-click the Name property, select **Convert to enum**</span></span>

    ![ConvertToEnum](~/ef6/media/converttoenum.png)

2.  <span data-ttu-id="11154-148">在**添加枚举**对话框框中，键入**DepartmentNames**对于枚举类型名称，将基础类型更改为**Int32**，然后将以下成员添加到该类型： 适用于英语，数学和经济性</span><span class="sxs-lookup"><span data-stu-id="11154-148">In the **Add Enum** dialog box type **DepartmentNames** for the Enum Type Name, change the Underlying Type to **Int32**, and then add the following members to the type: English, Math, and Economics</span></span>

    ![AddEnumType](~/ef6/media/addenumtype.png)

3.  <span data-ttu-id="11154-150">按**确定**</span><span class="sxs-lookup"><span data-stu-id="11154-150">Press **OK**</span></span>
4.  <span data-ttu-id="11154-151">保存模型，并生成项目</span><span class="sxs-lookup"><span data-stu-id="11154-151">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="11154-152">在生成时，可能会在错误列表中出现有关未映射的实体和关联的警告。</span><span class="sxs-lookup"><span data-stu-id="11154-152">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="11154-153">你可以忽略这些警告，因为我们选择从模型生成数据库后，这些错误将会消失。</span><span class="sxs-lookup"><span data-stu-id="11154-153">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

<span data-ttu-id="11154-154">如果您看一下属性窗口，您会注意到 Name 属性的类型已更改为**DepartmentNames**和新添加的枚举类型已添加到类型的列表。</span><span class="sxs-lookup"><span data-stu-id="11154-154">If you look at the Properties window, you will notice that the type of the Name property was changed to **DepartmentNames** and the newly added enum type was added to the list of types.</span></span>

<span data-ttu-id="11154-155">如果切换到模型浏览器窗口时，将看到该类型也添加到枚举类型的节点。</span><span class="sxs-lookup"><span data-stu-id="11154-155">If you switch to the Model Browser window, you will see that the type was also added to the Enum Types node.</span></span>

![ModelBrowser](~/ef6/media/modelbrowser.png)

>[!NOTE]
> <span data-ttu-id="11154-157">您还可以添加新的枚举类型在此窗口中通过单击鼠标右键并选择**添加枚举类型**。</span><span class="sxs-lookup"><span data-stu-id="11154-157">You can also add new enum types from this window by clicking the right mouse button and selecting **Add Enum Type**.</span></span> <span data-ttu-id="11154-158">创建类型后则会在类型的列表中，并能够与属性相关联</span><span class="sxs-lookup"><span data-stu-id="11154-158">Once the type is created it will appear in the list of types and you would be able to associate with a property</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="11154-159">从模型生成数据库</span><span class="sxs-lookup"><span data-stu-id="11154-159">Generate Database from Model</span></span>

<span data-ttu-id="11154-160">现在我们可以生成基于模型的数据库。</span><span class="sxs-lookup"><span data-stu-id="11154-160">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="11154-161">右键单击实体设计器图面上，然后选择上的空白空间**根据模型生成数据库**</span><span class="sxs-lookup"><span data-stu-id="11154-161">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="11154-162">生成数据库向导的在数据连接对话框为的选择显示单击**新的连接**按钮指定 **(localdb)\\mssqllocaldb**服务器名称和**EnumTest**数据库并单击**确定**</span><span class="sxs-lookup"><span data-stu-id="11154-162">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **EnumTest** for the database and click **OK**</span></span>
3.  <span data-ttu-id="11154-163">将弹出一个对话框，询问你是否想要创建新的数据库中，单击**是**。</span><span class="sxs-lookup"><span data-stu-id="11154-163">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="11154-164">单击**下一步**并创建数据库向导生成数据定义语言 (DDL) 创建数据库生成的 DDL 将显示在摘要和设置对话框框下，DDL 不包含的定义映射到枚举类型的表</span><span class="sxs-lookup"><span data-stu-id="11154-164">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="11154-165">单击**完成**单击完成不会执行 DDL 脚本。</span><span class="sxs-lookup"><span data-stu-id="11154-165">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="11154-166">创建数据库向导将执行以下： 打开**EnumTest.edmx.sql** T-SQL 编辑器生成存储架构和映射部分的 EDMX 的文件中添加连接字符串信息到 App.config 文件</span><span class="sxs-lookup"><span data-stu-id="11154-166">The Create Database Wizard does the following: Opens the **EnumTest.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="11154-167">单击鼠标右键按钮在 T-SQL 编辑器中，然后选择**Execute**连接到服务器对话框出现，请输入在步骤 2 中的连接信息并单击**连接**</span><span class="sxs-lookup"><span data-stu-id="11154-167">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="11154-168">若要查看生成的架构，右键单击 SQL Server 对象资源管理器中的数据库名称，然后选择**刷新**</span><span class="sxs-lookup"><span data-stu-id="11154-168">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="11154-169">保留和检索数据</span><span class="sxs-lookup"><span data-stu-id="11154-169">Persist and Retrieve Data</span></span>

<span data-ttu-id="11154-170">打开 Program.cs 文件的 Main 方法定义的位置。</span><span class="sxs-lookup"><span data-stu-id="11154-170">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="11154-171">以下代码添加到 Main 函数。</span><span class="sxs-lookup"><span data-stu-id="11154-171">Add the following code into the Main function.</span></span> <span data-ttu-id="11154-172">该代码将新的部门对象添加到上下文。</span><span class="sxs-lookup"><span data-stu-id="11154-172">The code adds a new Department object to the context.</span></span> <span data-ttu-id="11154-173">然后，它将保存数据。</span><span class="sxs-lookup"><span data-stu-id="11154-173">It then saves the data.</span></span> <span data-ttu-id="11154-174">代码也会执行 LINQ 查询返回其中的名称是 DepartmentNames.English 的部门。</span><span class="sxs-lookup"><span data-stu-id="11154-174">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

``` csharp
using (var context = new EnumTestModelContainer())
{
    context.Departments.Add(new Department{ Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} and Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

<span data-ttu-id="11154-175">编译并运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="11154-175">Compile and run the application.</span></span> <span data-ttu-id="11154-176">该程序生成以下输出：</span><span class="sxs-lookup"><span data-stu-id="11154-176">The program produces the following output:</span></span>

```
DepartmentID: 1 Name: English
```

<span data-ttu-id="11154-177">若要查看数据库中的数据，右键单击 SQL Server 对象资源管理器中的数据库名称，然后选择**刷新**。</span><span class="sxs-lookup"><span data-stu-id="11154-177">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="11154-178">然后，单击鼠标右键按钮表并选择**查看数据**。</span><span class="sxs-lookup"><span data-stu-id="11154-178">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="11154-179">总结</span><span class="sxs-lookup"><span data-stu-id="11154-179">Summary</span></span>

<span data-ttu-id="11154-180">在本演练中我们介绍了如何将使用实体框架设计器的枚举类型映射以及如何在代码中使用的枚举。</span><span class="sxs-lookup"><span data-stu-id="11154-180">In this walkthrough we looked at how to map enum types using the Entity Framework Designer and how to use enums in code.</span></span> 
