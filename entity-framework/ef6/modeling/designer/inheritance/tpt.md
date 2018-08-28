---
title: 设计器 TPT 继承的 EF6
author: divega
ms.date: 2016-10-23
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
ms.openlocfilehash: 68980fa89446940b8b7f5f73c519d38e727a9039
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996343"
---
# <a name="designer-tpt-inheritance"></a><span data-ttu-id="1c145-102">设计器 TPT 继承</span><span class="sxs-lookup"><span data-stu-id="1c145-102">Designer TPT Inheritance</span></span>
<span data-ttu-id="1c145-103">此分步演练演示如何使用实体框架设计器 （EF 设计器） 在模型中实现每种类型一个表 (TPT) 继承。</span><span class="sxs-lookup"><span data-stu-id="1c145-103">This step-by-step walkthrough shows how to implement table-per-type (TPT) inheritance in your model using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="1c145-104">每种类型一个表继承使用数据库中单独的表为继承层次结构中的每种类型维护非继承属性和键属性的数据。</span><span class="sxs-lookup"><span data-stu-id="1c145-104">Table-per-type inheritance uses a separate table in the database to maintain data for non-inherited properties and key properties for each type in the inheritance hierarchy.</span></span>

<span data-ttu-id="1c145-105">在本演练中我们将映射**课程**（基类型）， **OnlineCourse** （派生自课程），并且**OnsiteCourse** (派生自**课程**)对表具有相同名称的实体。</span><span class="sxs-lookup"><span data-stu-id="1c145-105">In this walkthrough we will map the **Course** (base type), **OnlineCourse** (derives from Course), and **OnsiteCourse** (derives from **Course**) entities to tables with the same names.</span></span> <span data-ttu-id="1c145-106">我们将从数据库创建模型，然后更改要实现 TPT 继承的模型。</span><span class="sxs-lookup"><span data-stu-id="1c145-106">We'll create a model from the database and then alter the model to implement the TPT inheritance.</span></span>

<span data-ttu-id="1c145-107">可以还开始使用模型优先，然后从模型生成数据库。</span><span class="sxs-lookup"><span data-stu-id="1c145-107">You can also start with the Model First and then generate the database from the model.</span></span> <span data-ttu-id="1c145-108">在 EF 设计器默认情况下使用 TPT 策略并因此在模型中的任何继承将映射到单独表。</span><span class="sxs-lookup"><span data-stu-id="1c145-108">The EF Designer uses the TPT strategy by default and so any inheritance in the model will be mapped to separate tables.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="1c145-109">其他继承选项</span><span class="sxs-lookup"><span data-stu-id="1c145-109">Other Inheritance Options</span></span>

<span data-ttu-id="1c145-110">每个层次结构一张表 (TPH) 是继承的另一种类型的一个数据库中使用表来维护继承层次结构中的实体类型的所有数据。</span><span class="sxs-lookup"><span data-stu-id="1c145-110">Table-per-Hierarchy (TPH) is another type of inheritance in which one database table is used to maintain data for all of the entity types in an inheritance hierarchy.</span></span>  <span data-ttu-id="1c145-111">有关如何将映射与实体设计器的每个层次结构一个表继承的信息，请参阅[EF 设计器 TPH 继承](~/ef6/modeling/designer/inheritance/tph.md)。</span><span class="sxs-lookup"><span data-stu-id="1c145-111">For information about how to map Table-per-Hierarchy inheritance with the Entity Designer, see [EF Designer TPH Inheritance](~/ef6/modeling/designer/inheritance/tph.md).</span></span> 

<span data-ttu-id="1c145-112">请注意，每个具体表键入继承 (TPC) 和混合的继承模型由实体框架运行时支持，但不是受 EF 设计器。</span><span class="sxs-lookup"><span data-stu-id="1c145-112">Note that, the Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="1c145-113">如果你想要使用 TPC 或混合的继承，有两个选项： 使用 Code First，或手动编辑 EDMX 文件。</span><span class="sxs-lookup"><span data-stu-id="1c145-113">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="1c145-114">如果您选择使用 EDMX 文件，映射详细信息窗口将放入"安全模式"，您将不能够使用设计器来更改映射。</span><span class="sxs-lookup"><span data-stu-id="1c145-114">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1c145-115">系统必备</span><span class="sxs-lookup"><span data-stu-id="1c145-115">Prerequisites</span></span>

<span data-ttu-id="1c145-116">若要完成此演练，您需要：</span><span class="sxs-lookup"><span data-stu-id="1c145-116">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="1c145-117">最新版本的 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="1c145-117">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="1c145-118">[School 示例数据库](~/ef6/resources/school-database.md)。</span><span class="sxs-lookup"><span data-stu-id="1c145-118">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="1c145-119">设置项目</span><span class="sxs-lookup"><span data-stu-id="1c145-119">Set up the Project</span></span>

-   <span data-ttu-id="1c145-120">打开 Visual Studio 2012。</span><span class="sxs-lookup"><span data-stu-id="1c145-120">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="1c145-121">选择**文件-&gt;新增-&gt;项目**</span><span class="sxs-lookup"><span data-stu-id="1c145-121">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="1c145-122">在左窗格中，单击**Visual C\#**，然后选择**控制台**模板。</span><span class="sxs-lookup"><span data-stu-id="1c145-122">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="1c145-123">输入**TPTDBFirstSample**作为名称。</span><span class="sxs-lookup"><span data-stu-id="1c145-123">Enter **TPTDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="1c145-124">选择“确定”。</span><span class="sxs-lookup"><span data-stu-id="1c145-124">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="1c145-125">创建模型</span><span class="sxs-lookup"><span data-stu-id="1c145-125">Create a Model</span></span>

-   <span data-ttu-id="1c145-126">右键单击解决方案资源管理器中的项目并选择**添加-&gt;新项**。</span><span class="sxs-lookup"><span data-stu-id="1c145-126">Right-click the project in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="1c145-127">选择**数据**从左侧的菜单，然后选择**ADO.NET 实体数据模型**模板窗格中。</span><span class="sxs-lookup"><span data-stu-id="1c145-127">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="1c145-128">输入**TPTModel.edmx**以及该文件的名称，然后单击**添加**。</span><span class="sxs-lookup"><span data-stu-id="1c145-128">Enter **TPTModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="1c145-129">在选择模型内容对话框框中，选择 * * 生成从数据库 * *，然后依次**下一步**。</span><span class="sxs-lookup"><span data-stu-id="1c145-129">In the Choose Model Contents dialog box, select** Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="1c145-130">单击**新的连接**。</span><span class="sxs-lookup"><span data-stu-id="1c145-130">Click **New Connection**.</span></span>
    <span data-ttu-id="1c145-131">在连接属性对话框中，输入服务器名称 (例如， **(localdb)\\mssqllocaldb**)，选择身份验证方法、 类型**学校**数据库名称，然后单击**确定**。</span><span class="sxs-lookup"><span data-stu-id="1c145-131">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="1c145-132">选择数据连接对话框中使用数据库连接设置更新。</span><span class="sxs-lookup"><span data-stu-id="1c145-132">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="1c145-133">在选择数据库对象对话框中，节点下的表，选择**部门**，**课程、 OnlineCourse 和 OnsiteCourse**表。</span><span class="sxs-lookup"><span data-stu-id="1c145-133">In the Choose Your Database Objects dialog box, under the Tables node, select the **Department**, **Course, OnlineCourse, and OnsiteCourse** tables.</span></span>
-   <span data-ttu-id="1c145-134">单击 **“完成”**。</span><span class="sxs-lookup"><span data-stu-id="1c145-134">Click **Finish**.</span></span>

<span data-ttu-id="1c145-135">提供用于编辑模型的设计图面，实体设计器随即出现。</span><span class="sxs-lookup"><span data-stu-id="1c145-135">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="1c145-136">在选择数据库对象对话框中选择的所有对象都添加到模型中。</span><span class="sxs-lookup"><span data-stu-id="1c145-136">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

## <a name="implement-table-per-type-inheritance"></a><span data-ttu-id="1c145-137">实现每种类型一个表继承</span><span class="sxs-lookup"><span data-stu-id="1c145-137">Implement Table-per-Type Inheritance</span></span>

-   <span data-ttu-id="1c145-138">在设计图面上，右键单击**OnlineCourse**实体类型并选择**属性**。</span><span class="sxs-lookup"><span data-stu-id="1c145-138">On the design surface, right-click the **OnlineCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="1c145-139">在中**属性**窗口中，将 Base Type 属性设置为**课程**。</span><span class="sxs-lookup"><span data-stu-id="1c145-139">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="1c145-140">右键单击**OnsiteCourse**实体类型并选择**属性**。</span><span class="sxs-lookup"><span data-stu-id="1c145-140">Right-click the **OnsiteCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="1c145-141">在中**属性**窗口中，将 Base Type 属性设置为**课程**。</span><span class="sxs-lookup"><span data-stu-id="1c145-141">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="1c145-142">右键单击之间的关联 （行） **OnlineCourse**并**课程**实体类型。</span><span class="sxs-lookup"><span data-stu-id="1c145-142">Right-click the association (the line) between the **OnlineCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="1c145-143">选择**从模型中删除**。</span><span class="sxs-lookup"><span data-stu-id="1c145-143">Select **Delete from Model**.</span></span>
-   <span data-ttu-id="1c145-144">右键单击之间的关联**OnsiteCourse**并**课程**实体类型。</span><span class="sxs-lookup"><span data-stu-id="1c145-144">Right-click the association between the **OnsiteCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="1c145-145">选择**从模型中删除**。</span><span class="sxs-lookup"><span data-stu-id="1c145-145">Select **Delete from Model**.</span></span>

<span data-ttu-id="1c145-146">我们现在将删除**CourseID**属性从**OnlineCourse**并**OnsiteCourse**这些类继承，因此**CourseID**从**课程**基类型。</span><span class="sxs-lookup"><span data-stu-id="1c145-146">We will now delete the **CourseID** property from **OnlineCourse** and **OnsiteCourse** because these classes inherit **CourseID** from the **Course** base type.</span></span>

-   <span data-ttu-id="1c145-147">右键单击**CourseID**的属性**OnlineCourse**实体类型，并选择**从模型中删除**。</span><span class="sxs-lookup"><span data-stu-id="1c145-147">Right-click the **CourseID** property of the **OnlineCourse** entity type, and then select **Delete from Model**.</span></span>
-   <span data-ttu-id="1c145-148">右键单击**CourseID**的属性**OnsiteCourse**实体类型，并选择**从模型中删除**</span><span class="sxs-lookup"><span data-stu-id="1c145-148">Right-click the **CourseID** property of the **OnsiteCourse** entity type, and then select **Delete from Model**</span></span>
-   <span data-ttu-id="1c145-149">至此已实现每种类型一个表继承。</span><span class="sxs-lookup"><span data-stu-id="1c145-149">Table-per-type inheritance is now implemented.</span></span>

![TPT](~/ef6/media/tpt.png)

## <a name="use-the-model"></a><span data-ttu-id="1c145-151">使用模型</span><span class="sxs-lookup"><span data-stu-id="1c145-151">Use the Model</span></span>

<span data-ttu-id="1c145-152">打开**Program.cs**文件，其中**Main**方法的定义。</span><span class="sxs-lookup"><span data-stu-id="1c145-152">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="1c145-153">粘贴下面的代码插入**Main**函数。</span><span class="sxs-lookup"><span data-stu-id="1c145-153">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="1c145-154">代码执行三个查询。</span><span class="sxs-lookup"><span data-stu-id="1c145-154">The code executes three queries.</span></span> <span data-ttu-id="1c145-155">第一个查询返回所有**课程**与指定系建立联系。</span><span class="sxs-lookup"><span data-stu-id="1c145-155">The first query brings back all **Courses** related to the specified department.</span></span> <span data-ttu-id="1c145-156">第二个查询使用**OfType**方法返回**OnlineCourses**与指定系建立联系。</span><span class="sxs-lookup"><span data-stu-id="1c145-156">The second query uses the **OfType** method to return **OnlineCourses** related to the specified department.</span></span> <span data-ttu-id="1c145-157">第三个查询将返回**OnsiteCourses**。</span><span class="sxs-lookup"><span data-stu-id="1c145-157">The third query returns **OnsiteCourses**.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        foreach (var department in context.Departments)
        {
            Console.WriteLine("The {0} department has the following courses:",
                               department.Name);

            Console.WriteLine("   All courses");
            foreach (var course in department.Courses )
            {
                Console.WriteLine("     {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnlineCourse>())
            {
                Console.WriteLine("   Online - {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnsiteCourse>())
            {
                Console.WriteLine("   Onsite - {0}", course.Title);
            }
        }
    }
```
