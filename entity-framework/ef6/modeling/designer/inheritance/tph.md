---
title: 设计器 TPH 继承-EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
caps.latest.revision: 3
ms.openlocfilehash: 0a017d3b97808cede3134119940b2e5839d0f282
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/08/2018
ms.locfileid: "39120078"
---
# <a name="designer-tph-inheritance"></a><span data-ttu-id="d98d0-102">设计器 TPH 继承</span><span class="sxs-lookup"><span data-stu-id="d98d0-102">Designer TPH Inheritance</span></span>
<span data-ttu-id="d98d0-103">此分步演练演示如何使用实体框架设计器 （EF 设计器） 在概念模型中实现每个层次结构一个表 (TPH) 继承。</span><span class="sxs-lookup"><span data-stu-id="d98d0-103">This step-by-step walkthrough shows how to implement table-per-hierarchy (TPH) inheritance in your conceptual model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="d98d0-104">TPH 继承使用数据库表维护继承层次结构中的实体类型的所有数据。</span><span class="sxs-lookup"><span data-stu-id="d98d0-104">TPH inheritance uses one database table to maintain data for all of the entity types in an inheritance hierarchy.</span></span>

<span data-ttu-id="d98d0-105">在本演练中我们会将 Person 表映射到三个实体类型： Person （基类型）、 （派生自人员） 的学生和教师 （派生自人员）。</span><span class="sxs-lookup"><span data-stu-id="d98d0-105">In this walkthrough we will map the Person table to three entity types: Person (the base type), Student (derives from Person), and Instructor (derives from Person).</span></span> <span data-ttu-id="d98d0-106">我们将从数据库 (Database First) 创建概念模型，然后更改模型来实现 TPH 继承使用 EF 设计器。</span><span class="sxs-lookup"><span data-stu-id="d98d0-106">We'll create a conceptual model from the database (Database First) and then alter the model to implement the TPH inheritance using the EF Designer.</span></span>

<span data-ttu-id="d98d0-107">可以将映射到使用模型优先 TPH 继承，但您必须编写您自己的数据库生成工作流即复杂。</span><span class="sxs-lookup"><span data-stu-id="d98d0-107">It is possible to map to a TPH inheritance using Model First but you would have to write your own database generation workflow which is complex.</span></span> <span data-ttu-id="d98d0-108">然后会将分配到此工作流**数据库生成工作流**EF 设计器中的属性。</span><span class="sxs-lookup"><span data-stu-id="d98d0-108">You would then assign this workflow to the **Database Generation Workflow** property in the EF Designer.</span></span> <span data-ttu-id="d98d0-109">更轻松的替代方法是使用 Code First。</span><span class="sxs-lookup"><span data-stu-id="d98d0-109">An easier alternative is to use Code First.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="d98d0-110">其他继承选项</span><span class="sxs-lookup"><span data-stu-id="d98d0-110">Other Inheritance Options</span></span>

<span data-ttu-id="d98d0-111">每种类型一个表 (TPT) 是继承的另一种类型在其数据库中的单独表映射到参与继承的实体。</span><span class="sxs-lookup"><span data-stu-id="d98d0-111">Table-per-Type (TPT) is another type of inheritance in which separate tables in the database are mapped to entities that participate in the inheritance.</span></span>  <span data-ttu-id="d98d0-112">有关如何将映射与 EF 设计器的每个类型一个表继承的信息，请参阅[EF 设计器 TPT 继承](~/ef6/modeling/designer/inheritance/tpt.md)。</span><span class="sxs-lookup"><span data-stu-id="d98d0-112">For information about how to map Table-per-Type inheritance with the EF Designer, see [EF Designer TPT Inheritance](~/ef6/modeling/designer/inheritance/tpt.md).</span></span>

<span data-ttu-id="d98d0-113">表每个具体类型继承 (TPC) 和混合的继承模型由实体框架运行时支持，但不是支持通过 EF 设计器。</span><span class="sxs-lookup"><span data-stu-id="d98d0-113">Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="d98d0-114">如果你想要使用 TPC 或混合的继承，有两个选项： 使用 Code First，或手动编辑 EDMX 文件。</span><span class="sxs-lookup"><span data-stu-id="d98d0-114">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="d98d0-115">如果您选择使用 EDMX 文件，映射详细信息窗口将放入"安全模式"，您将不能够使用设计器来更改映射。</span><span class="sxs-lookup"><span data-stu-id="d98d0-115">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d98d0-116">系统必备</span><span class="sxs-lookup"><span data-stu-id="d98d0-116">Prerequisites</span></span>

<span data-ttu-id="d98d0-117">若要完成此演练，您需要：</span><span class="sxs-lookup"><span data-stu-id="d98d0-117">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="d98d0-118">最新版本的 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="d98d0-118">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="d98d0-119">[School 示例数据库](~/ef6/resources/school-database.md)。</span><span class="sxs-lookup"><span data-stu-id="d98d0-119">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="d98d0-120">设置项目</span><span class="sxs-lookup"><span data-stu-id="d98d0-120">Set up the Project</span></span>

-   <span data-ttu-id="d98d0-121">打开 Visual Studio 2012。</span><span class="sxs-lookup"><span data-stu-id="d98d0-121">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="d98d0-122">选择**文件-&gt;新增-&gt;项目**</span><span class="sxs-lookup"><span data-stu-id="d98d0-122">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="d98d0-123">在左窗格中，单击**Visual C\#**，然后选择**控制台**模板。</span><span class="sxs-lookup"><span data-stu-id="d98d0-123">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="d98d0-124">输入**TPHDBFirstSample**作为名称。</span><span class="sxs-lookup"><span data-stu-id="d98d0-124">Enter **TPHDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="d98d0-125">选择“确定”。</span><span class="sxs-lookup"><span data-stu-id="d98d0-125">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="d98d0-126">创建模型</span><span class="sxs-lookup"><span data-stu-id="d98d0-126">Create a Model</span></span>

-   <span data-ttu-id="d98d0-127">右键单击解决方案资源管理器中的项目名称并选择**添加-&gt;新项**。</span><span class="sxs-lookup"><span data-stu-id="d98d0-127">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="d98d0-128">选择**数据**从左侧的菜单，然后选择**ADO.NET 实体数据模型**模板窗格中。</span><span class="sxs-lookup"><span data-stu-id="d98d0-128">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="d98d0-129">输入**TPHModel.edmx**以及该文件的名称，然后单击**添加**。</span><span class="sxs-lookup"><span data-stu-id="d98d0-129">Enter **TPHModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="d98d0-130">在选择模型内容对话框中，选择**从数据库生成**，然后单击**下一步**。</span><span class="sxs-lookup"><span data-stu-id="d98d0-130">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="d98d0-131">单击**新的连接**。</span><span class="sxs-lookup"><span data-stu-id="d98d0-131">Click **New Connection**.</span></span>
    <span data-ttu-id="d98d0-132">在连接属性对话框中，输入服务器名称 (例如， **(localdb)\\mssqllocaldb**)，选择身份验证方法、 类型**学校**数据库名称，然后单击**确定**。</span><span class="sxs-lookup"><span data-stu-id="d98d0-132">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="d98d0-133">选择数据连接对话框中使用数据库连接设置更新。</span><span class="sxs-lookup"><span data-stu-id="d98d0-133">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="d98d0-134">在选择数据库对象对话框中，节点下的表，选择**人员**表。</span><span class="sxs-lookup"><span data-stu-id="d98d0-134">In the Choose Your Database Objects dialog box, under the Tables node, select the **Person** table.</span></span>
-   <span data-ttu-id="d98d0-135">单击 **“完成”**。</span><span class="sxs-lookup"><span data-stu-id="d98d0-135">Click **Finish**.</span></span>

<span data-ttu-id="d98d0-136">提供用于编辑模型的设计图面，实体设计器随即出现。</span><span class="sxs-lookup"><span data-stu-id="d98d0-136">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="d98d0-137">在选择数据库对象对话框中选择的所有对象都添加到模型中。</span><span class="sxs-lookup"><span data-stu-id="d98d0-137">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

<span data-ttu-id="d98d0-138">它是如何**人员**表查找数据库中。</span><span class="sxs-lookup"><span data-stu-id="d98d0-138">That is how the **Person** table looks in the database.</span></span>

![PersonTable](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a><span data-ttu-id="d98d0-140">实现每个层次结构一个表继承</span><span class="sxs-lookup"><span data-stu-id="d98d0-140">Implement Table-per-Hierarchy Inheritance</span></span>

<span data-ttu-id="d98d0-141">**Person**表中有**鉴别器**列，该列可以具有两个值之一:"学生"和"讲师"。</span><span class="sxs-lookup"><span data-stu-id="d98d0-141">The **Person** table has the **Discriminator** column, which can have one of two values: “Student” and “Instructor”.</span></span> <span data-ttu-id="d98d0-142">根据值**Person**表将映射到**学生**实体或**讲师**实体。</span><span class="sxs-lookup"><span data-stu-id="d98d0-142">Depending on the value the **Person** table will be mapped to the **Student** entity or the **Instructor** entity.</span></span> <span data-ttu-id="d98d0-143">**Person**表还包含两个列**HireDate**和**EnrollmentDate**，它必须是**可以为 null**因为用户不能为学生和同时在一名讲师，（至少不在本演练）。</span><span class="sxs-lookup"><span data-stu-id="d98d0-143">The **Person** table also has two columns, **HireDate** and **EnrollmentDate**, which must be **nullable** because a person cannot be a student and an instructor at the same time (at least not in this walkthrough).</span></span>

### <a name="add-new-entities"></a><span data-ttu-id="d98d0-144">添加新实体</span><span class="sxs-lookup"><span data-stu-id="d98d0-144">Add new Entities</span></span>

-   <span data-ttu-id="d98d0-145">添加新实体。</span><span class="sxs-lookup"><span data-stu-id="d98d0-145">Add a new entity.</span></span>
    <span data-ttu-id="d98d0-146">若要执行此操作，右键单击实体框架设计器设计图面的空白区域，然后选择**Add-&gt;实体**。</span><span class="sxs-lookup"><span data-stu-id="d98d0-146">To do this, right-click on an empty space of the design surface of the Entity Framework Designer, and select **Add-&gt;Entity**.</span></span>
-   <span data-ttu-id="d98d0-147">类型**讲师**有关**实体名称**，然后选择**人员**从下拉列表中为**基类型**。</span><span class="sxs-lookup"><span data-stu-id="d98d0-147">Type **Instructor** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>
-   <span data-ttu-id="d98d0-148">单击 **“确定”**。</span><span class="sxs-lookup"><span data-stu-id="d98d0-148">Click **OK**.</span></span>
-   <span data-ttu-id="d98d0-149">添加另一个新实体。</span><span class="sxs-lookup"><span data-stu-id="d98d0-149">Add another new entity.</span></span> <span data-ttu-id="d98d0-150">类型**学生**有关**实体名称**，然后选择**人员**从下拉列表中为**基类型**。</span><span class="sxs-lookup"><span data-stu-id="d98d0-150">Type **Student** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>

<span data-ttu-id="d98d0-151">两个新的实体类型已添加到设计图面。</span><span class="sxs-lookup"><span data-stu-id="d98d0-151">Two new entity types were added to the design surface.</span></span> <span data-ttu-id="d98d0-152">从新的实体类型有一个箭头**Person**实体类型; 这指示**人员**是新的实体类型的基类型。</span><span class="sxs-lookup"><span data-stu-id="d98d0-152">An arrow points from the new entity types to the **Person** entity type; this indicates that **Person** is the base type for the new entity types.</span></span>

-   <span data-ttu-id="d98d0-153">右键单击**HireDate**的属性**人员**实体。</span><span class="sxs-lookup"><span data-stu-id="d98d0-153">Right-click the **HireDate** property of the **Person** entity.</span></span> <span data-ttu-id="d98d0-154">选择**剪切**（或使用 Ctrl-X 键）。</span><span class="sxs-lookup"><span data-stu-id="d98d0-154">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="d98d0-155">右键单击**讲师**实体，然后选择**粘贴**（或使用 Ctrl + V 键）。</span><span class="sxs-lookup"><span data-stu-id="d98d0-155">Right-click the **Instructor** entity and select **Paste** (or use the Ctrl-V key).</span></span>
-   <span data-ttu-id="d98d0-156">右键单击**HireDate**属性，然后选择**属性**。</span><span class="sxs-lookup"><span data-stu-id="d98d0-156">Right-click the **HireDate** property and select **Properties**.</span></span>
-   <span data-ttu-id="d98d0-157">在中**属性**窗口中，将**Nullable**属性设置为**false**。</span><span class="sxs-lookup"><span data-stu-id="d98d0-157">In the **Properties** window, set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="d98d0-158">右键单击**EnrollmentDate**的属性**人员**实体。</span><span class="sxs-lookup"><span data-stu-id="d98d0-158">Right-click the **EnrollmentDate** property of the **Person** entity.</span></span> <span data-ttu-id="d98d0-159">选择**剪切**（或使用 Ctrl-X 键）。</span><span class="sxs-lookup"><span data-stu-id="d98d0-159">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="d98d0-160">右键单击**学生**实体，然后选择**粘贴 （或使用 Ctrl + V 键）。**</span><span class="sxs-lookup"><span data-stu-id="d98d0-160">Right-click the **Student** entity and select **Paste(or use the Ctrl-V key).**</span></span>
-   <span data-ttu-id="d98d0-161">选择**EnrollmentDate**属性并设置**Nullable**属性设置为**false**。</span><span class="sxs-lookup"><span data-stu-id="d98d0-161">Select the **EnrollmentDate** property and set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="d98d0-162">选择**人员**实体类型。</span><span class="sxs-lookup"><span data-stu-id="d98d0-162">Select the **Person** entity type.</span></span> <span data-ttu-id="d98d0-163">在中**属性**窗口中，将其**抽象**属性设置为**true**。</span><span class="sxs-lookup"><span data-stu-id="d98d0-163">In the **Properties** window, set its **Abstract** property to **true**.</span></span>
-   <span data-ttu-id="d98d0-164">删除**鉴别器**属性从**人员**。</span><span class="sxs-lookup"><span data-stu-id="d98d0-164">Delete the **Discriminator** property from **Person**.</span></span> <span data-ttu-id="d98d0-165">应删除的原因是下一节中所述。</span><span class="sxs-lookup"><span data-stu-id="d98d0-165">The reason it should be deleted is explained in the following section.</span></span>

### <a name="map-the-entities"></a><span data-ttu-id="d98d0-166">将实体映射</span><span class="sxs-lookup"><span data-stu-id="d98d0-166">Map the entities</span></span>

-   <span data-ttu-id="d98d0-167">右键单击**讲师**，然后选择**表映射。**</span><span class="sxs-lookup"><span data-stu-id="d98d0-167">Right-click the **Instructor** and select **Table Mapping.**</span></span>
    <span data-ttu-id="d98d0-168">Instructor 实体映射详细信息窗口中选择。</span><span class="sxs-lookup"><span data-stu-id="d98d0-168">The Instructor entity is selected in the Mapping Details window.</span></span>
-   <span data-ttu-id="d98d0-169">单击**&lt;添加表或视图&gt;** 中**映射详细信息**窗口。</span><span class="sxs-lookup"><span data-stu-id="d98d0-169">Click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
    <span data-ttu-id="d98d0-170">**&lt;添加表或视图&gt;** 字段将变成一个下拉列表的表或视图到所选的实体可以映射。</span><span class="sxs-lookup"><span data-stu-id="d98d0-170">The **&lt;Add a Table or View&gt;** field becomes a drop-down list of tables or views to which the selected entity can be mapped.</span></span>
-   <span data-ttu-id="d98d0-171">选择**人员**从下拉列表。</span><span class="sxs-lookup"><span data-stu-id="d98d0-171">Select **Person** from the drop-down list.</span></span>
-   <span data-ttu-id="d98d0-172">**映射详细信息**窗口将会更新默认的列映射和添加条件的选项。</span><span class="sxs-lookup"><span data-stu-id="d98d0-172">The **Mapping Details** window is updated with default column mappings and an option for adding a condition.</span></span>
-   <span data-ttu-id="d98d0-173">单击**&lt;添加条件&gt;**。</span><span class="sxs-lookup"><span data-stu-id="d98d0-173">Click on **&lt;Add a Condition&gt;**.</span></span>
    <span data-ttu-id="d98d0-174">**&lt;添加条件&gt;** 字段变为一个下拉列表可以为其设置条件的列。</span><span class="sxs-lookup"><span data-stu-id="d98d0-174">The **&lt;Add a Condition&gt;** field becomes a drop-down list of columns for which conditions can be set.</span></span>
-   <span data-ttu-id="d98d0-175">选择**鉴别器**从下拉列表。</span><span class="sxs-lookup"><span data-stu-id="d98d0-175">Select **Discriminator** from the drop-down list.</span></span>
-   <span data-ttu-id="d98d0-176">在中**运算符**的列**映射详细信息**窗口中，选择 = 从下拉列表。</span><span class="sxs-lookup"><span data-stu-id="d98d0-176">In the **Operator** column of the **Mapping Details** window, select = from the drop-down list.</span></span>
-   <span data-ttu-id="d98d0-177">在中**值/属性**列中，键入**讲师**。</span><span class="sxs-lookup"><span data-stu-id="d98d0-177">In the **Value/Property** column, type **Instructor**.</span></span> <span data-ttu-id="d98d0-178">最终结果应如下所示：</span><span class="sxs-lookup"><span data-stu-id="d98d0-178">The end result should look like this:</span></span>

    ![MappingDetails2](~/ef6/media/mappingdetails2.png)

-   <span data-ttu-id="d98d0-180">重复这些步骤**学生**实体类型，但请等于条件**学生**值。</span><span class="sxs-lookup"><span data-stu-id="d98d0-180">Repeat these steps for the **Student** entity type, but make the condition equal to **Student** value.</span></span>  
    <span data-ttu-id="d98d0-181">*我们想要删除的原因**鉴别器**属性，是因为您不能超过一次将映射的表列。将为条件映射时，使用此列，因此不能为属性映射还使用它。它可用于这种，如果条件使用的唯一方式**Is Null**或**Is Not Null**比较。*</span><span class="sxs-lookup"><span data-stu-id="d98d0-181">*The reason we wanted to remove the **Discriminator** property, is because you cannot map a table column more than once. This column will be used for conditional mapping, so it cannot be used for property mapping as well. The only way it can be used for both, if a condition uses an **Is Null** or **Is Not Null** comparison.*</span></span>

<span data-ttu-id="d98d0-182">“每个层次结构一个表”继承实现完毕。</span><span class="sxs-lookup"><span data-stu-id="d98d0-182">Table-per-hierarchy inheritance is now implemented.</span></span>

![FinalTPH](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a><span data-ttu-id="d98d0-184">使用模型</span><span class="sxs-lookup"><span data-stu-id="d98d0-184">Use the Model</span></span>

<span data-ttu-id="d98d0-185">打开**Program.cs**文件，其中**Main**方法的定义。</span><span class="sxs-lookup"><span data-stu-id="d98d0-185">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="d98d0-186">粘贴下面的代码插入**Main**函数。</span><span class="sxs-lookup"><span data-stu-id="d98d0-186">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="d98d0-187">代码执行三个查询。</span><span class="sxs-lookup"><span data-stu-id="d98d0-187">The code executes three queries.</span></span> <span data-ttu-id="d98d0-188">第一个查询返回所有**人员**对象。</span><span class="sxs-lookup"><span data-stu-id="d98d0-188">The first query brings back all **Person** objects.</span></span> <span data-ttu-id="d98d0-189">第二个查询使用**OfType**方法返回**讲师**对象。</span><span class="sxs-lookup"><span data-stu-id="d98d0-189">The second query uses the **OfType** method to return **Instructor** objects.</span></span> <span data-ttu-id="d98d0-190">第三个查询使用**OfType**方法返回**学生**对象。</span><span class="sxs-lookup"><span data-stu-id="d98d0-190">The third query uses the **OfType** method to return **Student** objects.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        Console.WriteLine("All people:");
        foreach (var person in context.People)
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Instructors only: ");
        foreach (var person in context.People.OfType<Instructor>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Students only: ");
        foreach (var person in context.People.OfType<Student>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }
    }
```
