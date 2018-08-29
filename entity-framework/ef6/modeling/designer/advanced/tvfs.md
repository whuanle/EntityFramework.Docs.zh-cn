---
title: 表值函数 (Tvf)-EF6
author: divega
ms.date: 2016-10-23
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
ms.openlocfilehash: f4b8c1bd442bac67a06584bd23c040381374f303
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993242"
---
# <a name="table-valued-functions-tvfs"></a><span data-ttu-id="4e663-102">表值函数 (Tvf)</span><span class="sxs-lookup"><span data-stu-id="4e663-102">Table-Valued Functions (TVFs)</span></span>
> [!NOTE]
> <span data-ttu-id="4e663-103">**EF5 及更高版本仅**的功能，Api，Entity Framework 5 中引入了此页所述的等。</span><span class="sxs-lookup"><span data-stu-id="4e663-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="4e663-104">如果使用的是早期版本，则部分或全部信息不适用。</span><span class="sxs-lookup"><span data-stu-id="4e663-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="4e663-105">视频和分步演练演示如何将映射表值函数 (Tvf 使用实体框架设计器)。</span><span class="sxs-lookup"><span data-stu-id="4e663-105">The video and step-by-step walkthrough shows how to map table-valued functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="4e663-106">它还演示了如何从 LINQ 查询中调用 TVF。</span><span class="sxs-lookup"><span data-stu-id="4e663-106">It also demonstrates how to call a TVF from a LINQ query.</span></span>

<span data-ttu-id="4e663-107">Tvf 目前仅支持数据库优先工作流中。</span><span class="sxs-lookup"><span data-stu-id="4e663-107">TVFs are currently only supported in the Database First workflow.</span></span>

<span data-ttu-id="4e663-108">实体框架版本 5 中引入了 TVF 的支持。</span><span class="sxs-lookup"><span data-stu-id="4e663-108">TVF support was introduced in Entity Framework version 5.</span></span> <span data-ttu-id="4e663-109">请注意，若要使用新功能，如表值函数、 枚举和空间类型必须面向.NET Framework 4.5。</span><span class="sxs-lookup"><span data-stu-id="4e663-109">Note that to use the new features like table-valued functions, enums, and spatial types you must target .NET Framework 4.5.</span></span> <span data-ttu-id="4e663-110">默认情况下，visual Studio 2012 面向.NET 4.5。</span><span class="sxs-lookup"><span data-stu-id="4e663-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="4e663-111">Tvf 是非常类似于存储过程有一个重大差异： TVF 的结果是可组合。</span><span class="sxs-lookup"><span data-stu-id="4e663-111">TVFs are very similar to stored procedures with one key difference: the result of a TVF is composable.</span></span> <span data-ttu-id="4e663-112">这意味着可以在 LINQ 查询中使用 TVF 的结果，而不能存储过程的结果。</span><span class="sxs-lookup"><span data-stu-id="4e663-112">That means the results from a TVF can be used in a LINQ query while the results of a stored procedure cannot.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="4e663-113">观看视频</span><span class="sxs-lookup"><span data-stu-id="4e663-113">Watch the video</span></span>

<span data-ttu-id="4e663-114">**主讲人**： 以下 Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="4e663-114">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="4e663-115">[WMV](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span><span class="sxs-lookup"><span data-stu-id="4e663-115">[WMV](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="4e663-116">系统必备组件</span><span class="sxs-lookup"><span data-stu-id="4e663-116">Pre-Requisites</span></span>

<span data-ttu-id="4e663-117">若要完成本演练，需要：</span><span class="sxs-lookup"><span data-stu-id="4e663-117">To complete this walkthrough, you need to:</span></span>

- <span data-ttu-id="4e663-118">安装[School 数据库](~/ef6/resources/school-database.md)。</span><span class="sxs-lookup"><span data-stu-id="4e663-118">Install the [School database](~/ef6/resources/school-database.md).</span></span>

- <span data-ttu-id="4e663-119">具有最新版本的 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4e663-119">Have a recent version of Visual Studio</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="4e663-120">设置项目</span><span class="sxs-lookup"><span data-stu-id="4e663-120">Set up the Project</span></span>

1.  <span data-ttu-id="4e663-121">打开 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4e663-121">Open Visual Studio</span></span>
2.  <span data-ttu-id="4e663-122">上**文件**菜单，依次指向**新建**，然后单击**项目**</span><span class="sxs-lookup"><span data-stu-id="4e663-122">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="4e663-123">在左窗格中，单击**Visual C\#**，然后选择**控制台**模板</span><span class="sxs-lookup"><span data-stu-id="4e663-123">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="4e663-124">输入**TVF**作为名称的项目并单击**确定**</span><span class="sxs-lookup"><span data-stu-id="4e663-124">Enter **TVF** as the name of the project and click **OK**</span></span>

## <a name="add-a-tvf-to-the-database"></a><span data-ttu-id="4e663-125">向数据库添加 TVF</span><span class="sxs-lookup"><span data-stu-id="4e663-125">Add a TVF to the Database</span></span>

-   <span data-ttu-id="4e663-126">选择**视图-&gt; SQL Server 对象资源管理器**</span><span class="sxs-lookup"><span data-stu-id="4e663-126">Select **View -&gt; SQL Server Object Explorer**</span></span>
-   <span data-ttu-id="4e663-127">如果 LocalDB 中的服务器列表不是： 右键单击**SQL Server** ，然后选择**添加 SQL Server**使用默认**Windows 身份验证**连接到 LocalDB 服务器</span><span class="sxs-lookup"><span data-stu-id="4e663-127">If LocalDB is not in the list of servers: Right-click on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB server</span></span>
-   <span data-ttu-id="4e663-128">展开 LocalDB 节点</span><span class="sxs-lookup"><span data-stu-id="4e663-128">Expand the LocalDB node</span></span>
-   <span data-ttu-id="4e663-129">在数据库节点下右键单击 School 数据库节点，然后选择**新建查询...**</span><span class="sxs-lookup"><span data-stu-id="4e663-129">Under the Databases node, right-click the School database node and select **New Query…**</span></span>
-   <span data-ttu-id="4e663-130">在 T-SQL 编辑器中，粘贴以下 TVF 定义</span><span class="sxs-lookup"><span data-stu-id="4e663-130">In T-SQL Editor, paste the following TVF definition</span></span>

``` SQL
CREATE FUNCTION [dbo].[GetStudentGradesForCourse]

(@CourseID INT)

RETURNS TABLE

RETURN
    SELECT [EnrollmentID],
           [CourseID],
           [StudentID],
           [Grade]
    FROM   [dbo].[StudentGrade]
    WHERE  CourseID = @CourseID
```

-   <span data-ttu-id="4e663-131">单击鼠标右键在 T-SQL 编辑器，然后选择**Execute**</span><span class="sxs-lookup"><span data-stu-id="4e663-131">Click the right mouse button on the T-SQL editor and select **Execute**</span></span>
-   <span data-ttu-id="4e663-132">GetStudentGradesForCourse 函数添加到 School 数据库</span><span class="sxs-lookup"><span data-stu-id="4e663-132">The GetStudentGradesForCourse function is added to the School database</span></span>

 

## <a name="create-a-model"></a><span data-ttu-id="4e663-133">创建模型</span><span class="sxs-lookup"><span data-stu-id="4e663-133">Create a Model</span></span>

1.  <span data-ttu-id="4e663-134">右键单击解决方案资源管理器中的项目名称，指向**外**，然后单击**新项**</span><span class="sxs-lookup"><span data-stu-id="4e663-134">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="4e663-135">选择**数据**从左侧的菜单，然后选择**ADO.NET 实体数据模型**中**模板**窗格</span><span class="sxs-lookup"><span data-stu-id="4e663-135">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the **Templates** pane</span></span>
3.  <span data-ttu-id="4e663-136">输入**TVFModel.edmx**以及该文件的名称，然后单击**添加**</span><span class="sxs-lookup"><span data-stu-id="4e663-136">Enter **TVFModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="4e663-137">在选择模型内容对话框中，选择**从数据库生成**，然后单击**下一步**</span><span class="sxs-lookup"><span data-stu-id="4e663-137">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**</span></span>
5.  <span data-ttu-id="4e663-138">单击**新的连接**Enter **(localdb)\\mssqllocaldb**在服务器名称文本框中输入**学校**数据库命名为单击**确定**</span><span class="sxs-lookup"><span data-stu-id="4e663-138">Click **New Connection** Enter **(localdb)\\mssqllocaldb** in the Server name text box Enter **School** for the database name Click **OK**</span></span>
6.  <span data-ttu-id="4e663-139">在选择数据库对象对话框框中下,**表**节点中，选择**人员**， **StudentGrade**，以及**课程**表</span><span class="sxs-lookup"><span data-stu-id="4e663-139">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person**, **StudentGrade**, and **Course** tables</span></span>
7.  <span data-ttu-id="4e663-140">选择**GetStudentGradesForCourse**函数位于**存储过程和函数**节点请注意，开始 Visual Studio 2012 中，实体设计器允许你批量导入到存储的过程和函数</span><span class="sxs-lookup"><span data-stu-id="4e663-140">Select the **GetStudentGradesForCourse** function located under the **Stored Procedures and Functions** node Note, that starting with Visual Studio 2012, the Entity Designer allows you to batch import your Stored Procedures and Functions</span></span>
8.  <span data-ttu-id="4e663-141">单击**完成**</span><span class="sxs-lookup"><span data-stu-id="4e663-141">Click **Finish**</span></span>
9.  <span data-ttu-id="4e663-142">提供用于编辑模型的设计图面，实体设计器随即出现。</span><span class="sxs-lookup"><span data-stu-id="4e663-142">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="4e663-143">在您选择的所有对象**选择数据库对象**对话框的添加到模型。</span><span class="sxs-lookup"><span data-stu-id="4e663-143">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>
10. <span data-ttu-id="4e663-144">默认情况下，每个导入存储的过程或函数的结果形状将自动变为实体模型中的新复杂类型。</span><span class="sxs-lookup"><span data-stu-id="4e663-144">By default, the result shape of each imported stored procedure or function will automatically become a new complex type in your entity model.</span></span> <span data-ttu-id="4e663-145">但我们想要将 GetStudentGradesForCourse 函数的结果映射到 StudentGrade 实体： 右键单击设计图面，然后选择**模型浏览器**在模型浏览器中，选择**函数导入**，然后双击**GetStudentGradesForCourse**函数中编辑函数导入对话框中，选择**实体**，然后选择**StudentGrade**</span><span class="sxs-lookup"><span data-stu-id="4e663-145">But we want to map the results of the GetStudentGradesForCourse function to the StudentGrade entity: Right-click the design surface and select **Model Browser** In Model Browser, select **Function Imports**, and then double-click the **GetStudentGradesForCourse** function In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="4e663-146">保留和检索数据</span><span class="sxs-lookup"><span data-stu-id="4e663-146">Persist and Retrieve Data</span></span>

<span data-ttu-id="4e663-147">打开其中定义 Main 方法的文件。</span><span class="sxs-lookup"><span data-stu-id="4e663-147">Open the file where the Main method is defined.</span></span> <span data-ttu-id="4e663-148">以下代码添加到 Main 函数。</span><span class="sxs-lookup"><span data-stu-id="4e663-148">Add the following code into the Main function.</span></span>

<span data-ttu-id="4e663-149">下面的代码演示如何构建使用表值函数的查询。</span><span class="sxs-lookup"><span data-stu-id="4e663-149">The following code demonstrates how to build a query that uses a Table-valued Function.</span></span> <span data-ttu-id="4e663-150">该查询将结果投影到匿名类型包含的相关的课程标题和相关的学生的等级大于或等于 3.5。</span><span class="sxs-lookup"><span data-stu-id="4e663-150">The query projects the results into an anonymous type that contains the related Course title and related students with a grade greater or equal to 3.5.</span></span>

``` csharp
using (var context = new SchoolEntities())
{
    var CourseID = 4022;
    var Grade = 3.5M;

    // Return all the best students in the Microeconomics class.
    var students = from s in context.GetStudentGradesForCourse(CourseID)
                            where s.Grade >= Grade
                            select new
                            {
                                s.Person,
                                s.Course.Title
                            };

    foreach (var result in students)
    {
        Console.WriteLine(
            "Couse: {0}, Student: {1} {2}",
            result.Title,  
            result.Person.FirstName,  
            result.Person.LastName);
    }
}
```

<span data-ttu-id="4e663-151">编译并运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="4e663-151">Compile and run the application.</span></span> <span data-ttu-id="4e663-152">该程序生成以下输出：</span><span class="sxs-lookup"><span data-stu-id="4e663-152">The program produces the following output:</span></span>

```
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a><span data-ttu-id="4e663-153">总结</span><span class="sxs-lookup"><span data-stu-id="4e663-153">Summary</span></span>

<span data-ttu-id="4e663-154">在本演练中介绍了如何将映射表值函数 (Tvf 使用实体框架设计器)。</span><span class="sxs-lookup"><span data-stu-id="4e663-154">In this walkthrough we looked at how to map Table-valued Functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="4e663-155">它还演示了如何从 LINQ 查询中调用 TVF。</span><span class="sxs-lookup"><span data-stu-id="4e663-155">It also demonstrated how to call a TVF from a LINQ query.</span></span>
