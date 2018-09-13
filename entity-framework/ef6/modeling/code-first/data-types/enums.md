---
title: 枚举支持的代码优先的 EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 986991c2dd9470867aaf5424ecb176d7db172426
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489500"
---
# <a name="enum-support---code-first"></a><span data-ttu-id="9bf39-102">枚举支持的代码优先</span><span class="sxs-lookup"><span data-stu-id="9bf39-102">Enum Support - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="9bf39-103">**EF5 及更高版本仅**的功能，Api，Entity Framework 5 中引入了此页所述的等。</span><span class="sxs-lookup"><span data-stu-id="9bf39-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="9bf39-104">如果使用的是早期版本，则部分或全部信息不适用。</span><span class="sxs-lookup"><span data-stu-id="9bf39-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="9bf39-105">此视频和分步演练演示如何使用 Entity Framework Code First 枚举类型。</span><span class="sxs-lookup"><span data-stu-id="9bf39-105">This video and step-by-step walkthrough shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="9bf39-106">它还演示了如何在 LINQ 查询中使用的枚举。</span><span class="sxs-lookup"><span data-stu-id="9bf39-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="9bf39-107">本演练将使用 Code First 来创建新的数据库，但也可以使用[Code First 将映射到现有数据库](~/ef6/modeling/code-first/workflows/existing-database.md)。</span><span class="sxs-lookup"><span data-stu-id="9bf39-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to map to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="9bf39-108">Entity Framework 5 中引入了枚举支持。</span><span class="sxs-lookup"><span data-stu-id="9bf39-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="9bf39-109">若要使用新功能，如枚举、 空间数据类型和表值函数，必须面向.NET Framework 4.5。</span><span class="sxs-lookup"><span data-stu-id="9bf39-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="9bf39-110">默认情况下，visual Studio 2012 面向.NET 4.5。</span><span class="sxs-lookup"><span data-stu-id="9bf39-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="9bf39-111">在实体框架中，枚举可以具有以下基础类型：**字节**， **Int16**， **Int32**， **Int64** ，或**SByte**。</span><span class="sxs-lookup"><span data-stu-id="9bf39-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="9bf39-112">观看视频</span><span class="sxs-lookup"><span data-stu-id="9bf39-112">Watch the video</span></span>
<span data-ttu-id="9bf39-113">此视频介绍如何将使用与 Entity Framework Code First 枚举类型。</span><span class="sxs-lookup"><span data-stu-id="9bf39-113">This video shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="9bf39-114">它还演示了如何在 LINQ 查询中使用的枚举。</span><span class="sxs-lookup"><span data-stu-id="9bf39-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="9bf39-115">**主讲人**： 以下 Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="9bf39-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="9bf39-116">**视频**: [WMV](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="9bf39-116">**Video**: [WMV](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="9bf39-117">系统必备组件</span><span class="sxs-lookup"><span data-stu-id="9bf39-117">Pre-Requisites</span></span>

<span data-ttu-id="9bf39-118">需要具有 Visual Studio 2012、 Ultimate、 Premium、 Professional 或 Web Express 版安装来完成本演练。</span><span class="sxs-lookup"><span data-stu-id="9bf39-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

 

## <a name="set-up-the-project"></a><span data-ttu-id="9bf39-119">设置项目</span><span class="sxs-lookup"><span data-stu-id="9bf39-119">Set up the Project</span></span>

1.  <span data-ttu-id="9bf39-120">打开 Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="9bf39-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="9bf39-121">上**文件**菜单，依次指向**新建**，然后单击**项目**</span><span class="sxs-lookup"><span data-stu-id="9bf39-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="9bf39-122">在左窗格中，单击**Visual C\#**，然后选择**控制台**模板</span><span class="sxs-lookup"><span data-stu-id="9bf39-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="9bf39-123">输入**EnumCodeFirst**作为名称的项目并单击**确定**</span><span class="sxs-lookup"><span data-stu-id="9bf39-123">Enter **EnumCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="9bf39-124">定义使用 Code First 的新模型</span><span class="sxs-lookup"><span data-stu-id="9bf39-124">Define a New Model using Code First</span></span>

<span data-ttu-id="9bf39-125">使用 Code First 开发时通常首先编写.NET Framework 类定义概念 （域） 模型。</span><span class="sxs-lookup"><span data-stu-id="9bf39-125">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="9bf39-126">下面的代码定义部门类。</span><span class="sxs-lookup"><span data-stu-id="9bf39-126">The code below defines the Department class.</span></span>

<span data-ttu-id="9bf39-127">该代码还定义 DepartmentNames 枚举。</span><span class="sxs-lookup"><span data-stu-id="9bf39-127">The code also defines the DepartmentNames enumeration.</span></span> <span data-ttu-id="9bf39-128">默认情况下，枚举属于**int**类型。</span><span class="sxs-lookup"><span data-stu-id="9bf39-128">By default, the enumeration is of **int** type.</span></span> <span data-ttu-id="9bf39-129">部门类上的名称属性是 DepartmentNames 类型。</span><span class="sxs-lookup"><span data-stu-id="9bf39-129">The Name property on the Department class is of the DepartmentNames type.</span></span>

<span data-ttu-id="9bf39-130">打开 Program.cs 文件并粘贴以下类定义。</span><span class="sxs-lookup"><span data-stu-id="9bf39-130">Open the Program.cs file and paste the following class definitions.</span></span>

``` csharp
public enum DepartmentNames
{
    English,
    Math,
    Economics
}     

public partial class Department
{
    public int DepartmentID { get; set; }
    public DepartmentNames Name { get; set; }
    public decimal Budget { get; set; }
}
```
 

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="9bf39-131">定义 DbContext 派生类型</span><span class="sxs-lookup"><span data-stu-id="9bf39-131">Define the DbContext Derived Type</span></span>

<span data-ttu-id="9bf39-132">除了定义实体，还需要定义一个类，派生自 DbContext，并公开 DbSet&lt;TEntity&gt;属性。</span><span class="sxs-lookup"><span data-stu-id="9bf39-132">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="9bf39-133">DbSet&lt;TEntity&gt;属性使知道你想要在模型中包含哪些的类型的上下文。</span><span class="sxs-lookup"><span data-stu-id="9bf39-133">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="9bf39-134">在运行时，其中包括填充数据库中的数据的对象期间将 DbContext 派生类型的实例管理的实体对象，到数据库更改跟踪，并保留数据。</span><span class="sxs-lookup"><span data-stu-id="9bf39-134">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="9bf39-135">EntityFramework 程序集中定义的 DbContext 和 DbSet 类型。</span><span class="sxs-lookup"><span data-stu-id="9bf39-135">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="9bf39-136">我们将使用 EntityFramework NuGet 包添加到此 DLL 的引用。</span><span class="sxs-lookup"><span data-stu-id="9bf39-136">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="9bf39-137">在解决方案资源管理器，右键单击项目名称。</span><span class="sxs-lookup"><span data-stu-id="9bf39-137">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="9bf39-138">选择**管理 NuGet 包...**</span><span class="sxs-lookup"><span data-stu-id="9bf39-138">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="9bf39-139">在管理 NuGet 包对话框中，选择**联机**选项卡，选择**EntityFramework**包。</span><span class="sxs-lookup"><span data-stu-id="9bf39-139">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="9bf39-140">单击**安装**</span><span class="sxs-lookup"><span data-stu-id="9bf39-140">Click **Install**</span></span>

<span data-ttu-id="9bf39-141">请注意，除了 EntityFramework 程序集，System.ComponentModel.DataAnnotations 和 system.data.entity 的引用程序集的引用将添加。</span><span class="sxs-lookup"><span data-stu-id="9bf39-141">Note, that in addition to the EntityFramework  assembly, references to System.ComponentModel.DataAnnotations and System.Data.Entity assemblies are added as well.</span></span>

<span data-ttu-id="9bf39-142">在 Program.cs 文件的顶部，添加以下 using 语句：</span><span class="sxs-lookup"><span data-stu-id="9bf39-142">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="9bf39-143">在 Program.cs 中添加上下文定义。</span><span class="sxs-lookup"><span data-stu-id="9bf39-143">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="9bf39-144">保留和检索数据</span><span class="sxs-lookup"><span data-stu-id="9bf39-144">Persist and Retrieve Data</span></span>

<span data-ttu-id="9bf39-145">打开 Program.cs 文件的 Main 方法定义的位置。</span><span class="sxs-lookup"><span data-stu-id="9bf39-145">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="9bf39-146">以下代码添加到 Main 函数。</span><span class="sxs-lookup"><span data-stu-id="9bf39-146">Add the following code into the Main function.</span></span> <span data-ttu-id="9bf39-147">该代码将新的部门对象添加到上下文。</span><span class="sxs-lookup"><span data-stu-id="9bf39-147">The code adds a new Department object to the context.</span></span> <span data-ttu-id="9bf39-148">然后，它将保存数据。</span><span class="sxs-lookup"><span data-stu-id="9bf39-148">It then saves the data.</span></span> <span data-ttu-id="9bf39-149">代码也会执行 LINQ 查询返回其中的名称是 DepartmentNames.English 的部门。</span><span class="sxs-lookup"><span data-stu-id="9bf39-149">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

``` csharp
using (var context = new EnumTestContext())
{
    context.Departments.Add(new Department { Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

<span data-ttu-id="9bf39-150">编译并运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="9bf39-150">Compile and run the application.</span></span> <span data-ttu-id="9bf39-151">该程序生成以下输出：</span><span class="sxs-lookup"><span data-stu-id="9bf39-151">The program produces the following output:</span></span>

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a><span data-ttu-id="9bf39-152">查看生成的数据库</span><span class="sxs-lookup"><span data-stu-id="9bf39-152">View the Generated Database</span></span>

<span data-ttu-id="9bf39-153">当您运行应用程序第一次时，Entity Framework 创建数据库。</span><span class="sxs-lookup"><span data-stu-id="9bf39-153">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="9bf39-154">由于我们具有安装 Visual Studio 2012，则数据库将创建 LocalDB 实例上。</span><span class="sxs-lookup"><span data-stu-id="9bf39-154">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="9bf39-155">默认情况下，实体框架的数据库的名称派生上下文的完全限定名称 (对于本示例中为**EnumCodeFirst.EnumTestContext**)。</span><span class="sxs-lookup"><span data-stu-id="9bf39-155">By default, the Entity Framework names the database after the fully qualified name of the derived context (for this example that is **EnumCodeFirst.EnumTestContext**).</span></span> <span data-ttu-id="9bf39-156">将使用现有数据库的后续时间。</span><span class="sxs-lookup"><span data-stu-id="9bf39-156">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="9bf39-157">请注意，是否创建数据库后，可以对模型进行任何更改，则应使用 Code First 迁移更新数据库架构。</span><span class="sxs-lookup"><span data-stu-id="9bf39-157">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="9bf39-158">请参阅[到新数据库 Code First](~/ef6/modeling/code-first/workflows/new-database.md)有关使用迁移的示例。</span><span class="sxs-lookup"><span data-stu-id="9bf39-158">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="9bf39-159">若要查看的数据库和数据，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="9bf39-159">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="9bf39-160">在 Visual Studio 2012 主菜单中，选择**视图** - &gt; **SQL Server 对象资源管理器**。</span><span class="sxs-lookup"><span data-stu-id="9bf39-160">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="9bf39-161">如果 LocalDB 不在的服务器列表中，单击鼠标右键**SQL Server** ，然后选择**添加 SQL Server**使用默认**Windows 身份验证**连接到LocalDB 实例</span><span class="sxs-lookup"><span data-stu-id="9bf39-161">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB instance</span></span>
3.  <span data-ttu-id="9bf39-162">展开 LocalDB 节点</span><span class="sxs-lookup"><span data-stu-id="9bf39-162">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="9bf39-163">Unfold**数据库**文件夹，以查看新数据库并浏览到**部门**表注意第一个代码不会创建一个表映射到枚举类型</span><span class="sxs-lookup"><span data-stu-id="9bf39-163">Unfold the **Databases** folder to see the new database and browse to the **Department** table Note, that Code First does not create a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="9bf39-164">若要查看数据，右键单击表并选择**查看数据**</span><span class="sxs-lookup"><span data-stu-id="9bf39-164">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="9bf39-165">总结</span><span class="sxs-lookup"><span data-stu-id="9bf39-165">Summary</span></span>

<span data-ttu-id="9bf39-166">在本演练中介绍了如何将使用与 Entity Framework Code First 枚举类型。</span><span class="sxs-lookup"><span data-stu-id="9bf39-166">In this walkthrough we looked at how to use enum types with Entity Framework Code First.</span></span> 
