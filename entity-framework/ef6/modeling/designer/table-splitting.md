---
title: 设计器的表拆分的 EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
ms.openlocfilehash: 87b6e1bd0374f77dfffab342c659cf4e16c8a337
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994498"
---
# <a name="designer-table-splitting"></a><span data-ttu-id="87e27-102">设计器的表拆分</span><span class="sxs-lookup"><span data-stu-id="87e27-102">Designer Table Splitting</span></span>
<span data-ttu-id="87e27-103">本演练演示如何通过修改模型与实体框架设计器 （EF 设计器） 将多个实体类型映射到单个表。</span><span class="sxs-lookup"><span data-stu-id="87e27-103">This walkthrough shows how to map multiple entity types to a single table by modifying a model with the Entity Framework Designer (EF Designer).</span></span>

<span data-ttu-id="87e27-104">使用延迟加载以加载您的对象时，可能想要使用表拆分的一个原因延迟加载的某些属性。</span><span class="sxs-lookup"><span data-stu-id="87e27-104">One reason you may want to use table splitting is delaying the loading of some properties when using lazy loading to load your objects.</span></span> <span data-ttu-id="87e27-105">您可以分隔可能会包含到另一个实体的数据量非常大并仅加载它时所需的属性。</span><span class="sxs-lookup"><span data-stu-id="87e27-105">You can separate the properties that might contain very large amount of data into a seperate entity and only load it when required.</span></span>

<span data-ttu-id="87e27-106">下图显示了使用 EF 设计器时使用的主窗口。</span><span class="sxs-lookup"><span data-stu-id="87e27-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EFDesigner](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="87e27-108">系统必备</span><span class="sxs-lookup"><span data-stu-id="87e27-108">Prerequisites</span></span>

<span data-ttu-id="87e27-109">若要完成此演练，您需要：</span><span class="sxs-lookup"><span data-stu-id="87e27-109">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="87e27-110">最新版本的 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="87e27-110">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="87e27-111">[School 示例数据库](~/ef6/resources/school-database.md)。</span><span class="sxs-lookup"><span data-stu-id="87e27-111">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="87e27-112">设置项目</span><span class="sxs-lookup"><span data-stu-id="87e27-112">Set up the Project</span></span>

<span data-ttu-id="87e27-113">本演练使用 Visual Studio 2012。</span><span class="sxs-lookup"><span data-stu-id="87e27-113">This walkthrough is using Visual Studio 2012.</span></span>

-   <span data-ttu-id="87e27-114">打开 Visual Studio 2012。</span><span class="sxs-lookup"><span data-stu-id="87e27-114">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="87e27-115">在 **“文件”** 菜单上，指向 **“新建”**，然后单击 **“项目”**。</span><span class="sxs-lookup"><span data-stu-id="87e27-115">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="87e27-116">在左窗格中，单击 Visual C\#，然后选择控制台应用程序模板。</span><span class="sxs-lookup"><span data-stu-id="87e27-116">In the left pane, click Visual C\#, and then select the Console Application template.</span></span>
-   <span data-ttu-id="87e27-117">输入**TableSplittingSample**作为名称的项目并单击**确定**。</span><span class="sxs-lookup"><span data-stu-id="87e27-117">Enter **TableSplittingSample** as the name of the project and click **OK**.</span></span>

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="87e27-118">创建基于 School 数据库的模型</span><span class="sxs-lookup"><span data-stu-id="87e27-118">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="87e27-119">右键单击解决方案资源管理器中的项目名称，指向**外**，然后单击**新项**。</span><span class="sxs-lookup"><span data-stu-id="87e27-119">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="87e27-120">选择**数据**从左侧的菜单，然后选择**ADO.NET 实体数据模型**模板窗格中。</span><span class="sxs-lookup"><span data-stu-id="87e27-120">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="87e27-121">输入**TableSplittingModel.edmx**以及该文件的名称，然后单击**添加**。</span><span class="sxs-lookup"><span data-stu-id="87e27-121">Enter **TableSplittingModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="87e27-122">在选择模型内容对话框中，选择**从数据库生成**，然后单击**下一步。**</span><span class="sxs-lookup"><span data-stu-id="87e27-122">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="87e27-123">单击新建连接。</span><span class="sxs-lookup"><span data-stu-id="87e27-123">Click New Connection.</span></span> <span data-ttu-id="87e27-124">在连接属性对话框中，输入服务器名称 (例如， **(localdb)\\mssqllocaldb**)，选择身份验证方法、 类型**学校**数据库名称，然后单击**确定**。</span><span class="sxs-lookup"><span data-stu-id="87e27-124">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="87e27-125">选择数据连接对话框中使用数据库连接设置更新。</span><span class="sxs-lookup"><span data-stu-id="87e27-125">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="87e27-126">在选择数据库对象对话框中，展开**表**节点并检查**人员**表。</span><span class="sxs-lookup"><span data-stu-id="87e27-126">In the Choose Your Database Objects dialog box, unfold the **Tables** node and check the **Person** table.</span></span> <span data-ttu-id="87e27-127">这将添加到指定的表**学校**模型。</span><span class="sxs-lookup"><span data-stu-id="87e27-127">This will add the specified table to the **School** model.</span></span>
-   <span data-ttu-id="87e27-128">单击 **“完成”**。</span><span class="sxs-lookup"><span data-stu-id="87e27-128">Click **Finish**.</span></span>

<span data-ttu-id="87e27-129">提供用于编辑模型的设计图面，实体设计器随即出现。</span><span class="sxs-lookup"><span data-stu-id="87e27-129">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="87e27-130">在您选择的所有对象**选择数据库对象**对话框的添加到模型。</span><span class="sxs-lookup"><span data-stu-id="87e27-130">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>

## <a name="map-two-entities-to-a-single-table"></a><span data-ttu-id="87e27-131">将两个实体映射到单个表</span><span class="sxs-lookup"><span data-stu-id="87e27-131">Map Two Entities to a Single Table</span></span>

<span data-ttu-id="87e27-132">在本部分中，您将拆分**人员**为两个实体的实体并将其映射到单个表。</span><span class="sxs-lookup"><span data-stu-id="87e27-132">In this section you will split the **Person** entity into two entities and then map them to a single table.</span></span>

> [!NOTE]
> <span data-ttu-id="87e27-133">**人员**实体不包含任何属性可能包含大量的数据; 只是用作示例。</span><span class="sxs-lookup"><span data-stu-id="87e27-133">The **Person** entity does not contain any properties that may contain large amount of data; it is just used as an example.</span></span>

-   <span data-ttu-id="87e27-134">右键单击设计图面上的空白区域，指向**新添**，然后单击**实体**。</span><span class="sxs-lookup"><span data-stu-id="87e27-134">Right-click an empty area of the design surface, point to **Add New**, and click **Entity**.</span></span>
    <span data-ttu-id="87e27-135">**新实体**对话框随即出现。</span><span class="sxs-lookup"><span data-stu-id="87e27-135">The **New Entity** dialog box appears.</span></span>
-   <span data-ttu-id="87e27-136">类型**HireInfo**有关**实体名称**并**PersonID**有关**Key 属性**名称。</span><span class="sxs-lookup"><span data-stu-id="87e27-136">Type **HireInfo** for the **Entity name** and **PersonID** for the **Key Property** name.</span></span>
-   <span data-ttu-id="87e27-137">单击 **“确定”**。</span><span class="sxs-lookup"><span data-stu-id="87e27-137">Click **OK**.</span></span>
-   <span data-ttu-id="87e27-138">新的实体类型创建完毕，并且显示在设计图面上。</span><span class="sxs-lookup"><span data-stu-id="87e27-138">A new entity type is created and displayed on the design surface.</span></span>
-   <span data-ttu-id="87e27-139">选择**HireDate**的属性**人员**实体类型，然后按**Ctrl + X**密钥。</span><span class="sxs-lookup"><span data-stu-id="87e27-139">Select the **HireDate** property of the **Person** entity type and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="87e27-140">选择**HireInfo**实体，然后按**Ctrl + V**密钥。</span><span class="sxs-lookup"><span data-stu-id="87e27-140">Select the **HireInfo** entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="87e27-141">创建之间的关联**Person**并**HireInfo**。</span><span class="sxs-lookup"><span data-stu-id="87e27-141">Create an association between **Person** and **HireInfo**.</span></span> <span data-ttu-id="87e27-142">若要执行此操作，右键单击设计图面上的空白区域，指向**新添**，然后单击**关联**。</span><span class="sxs-lookup"><span data-stu-id="87e27-142">To do this, right-click an empty area of the design surface, point to **Add New**, and click **Association**.</span></span>
-   <span data-ttu-id="87e27-143">**添加关联**对话框随即出现。</span><span class="sxs-lookup"><span data-stu-id="87e27-143">The **Add Association** dialog box appears.</span></span> <span data-ttu-id="87e27-144">**PersonHireInfo**默认情况下提供名称。</span><span class="sxs-lookup"><span data-stu-id="87e27-144">The **PersonHireInfo** name is given by default.</span></span>
-   <span data-ttu-id="87e27-145">指定多重性**1(One)** 关系的两端。</span><span class="sxs-lookup"><span data-stu-id="87e27-145">Specify multiplicity **1(One)** on both ends of the relationship.</span></span>
-   <span data-ttu-id="87e27-146">按**确定**。</span><span class="sxs-lookup"><span data-stu-id="87e27-146">Press **OK**.</span></span>

<span data-ttu-id="87e27-147">下一步需要**映射详细信息**窗口。</span><span class="sxs-lookup"><span data-stu-id="87e27-147">The next step requires the **Mapping Details** window.</span></span> <span data-ttu-id="87e27-148">如果您无法查看此窗口中，右键单击设计图面并选择**映射详细信息**。</span><span class="sxs-lookup"><span data-stu-id="87e27-148">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="87e27-149">选择**HireInfo**实体类型，然后单击**&lt;添加表或视图&gt;** 中**映射详细信息**窗口。</span><span class="sxs-lookup"><span data-stu-id="87e27-149">Select the **HireInfo** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="87e27-150">选择**Person**从**&lt;添加表或视图&gt;** 字段下拉列表。</span><span class="sxs-lookup"><span data-stu-id="87e27-150">Select **Person** from the **&lt;Add a Table or View&gt;** field drop-down list.</span></span> <span data-ttu-id="87e27-151">列表包含的表或视图到所选的实体可以映射。</span><span class="sxs-lookup"><span data-stu-id="87e27-151">The list contains tables or views to which the selected entity can be mapped.</span></span>
    <span data-ttu-id="87e27-152">默认情况下，应映射相应的属性。</span><span class="sxs-lookup"><span data-stu-id="87e27-152">The appropriate properties should be mapped by default.</span></span>

    ![映射](~/ef6/media/mapping.png)

-   <span data-ttu-id="87e27-154">选择**PersonHireInfo**设计图面上的关联。</span><span class="sxs-lookup"><span data-stu-id="87e27-154">Select the **PersonHireInfo** association on the design surface.</span></span>
-   <span data-ttu-id="87e27-155">右键单击设计图面，然后选择关联**属性**。</span><span class="sxs-lookup"><span data-stu-id="87e27-155">Right-click the association on the design surface and select **Properties**.</span></span>
-   <span data-ttu-id="87e27-156">在中**属性**窗口中，选择**引用约束**属性，然后单击省略号按钮。</span><span class="sxs-lookup"><span data-stu-id="87e27-156">In the **Properties** window, select the **Referential Constraints** property and click the ellipses button.</span></span>
-   <span data-ttu-id="87e27-157">选择**Person**从**主体**下拉列表。</span><span class="sxs-lookup"><span data-stu-id="87e27-157">Select **Person** from the **Principal** drop-down list.</span></span>
-   <span data-ttu-id="87e27-158">按**确定**。</span><span class="sxs-lookup"><span data-stu-id="87e27-158">Press **OK**.</span></span>

 

## <a name="use-the-model"></a><span data-ttu-id="87e27-159">使用模型</span><span class="sxs-lookup"><span data-stu-id="87e27-159">Use the Model</span></span>

-   <span data-ttu-id="87e27-160">以下代码粘贴到 Main 方法中。</span><span class="sxs-lookup"><span data-stu-id="87e27-160">Paste the following code in the Main method.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        Person person = new Person()
        {
            FirstName = "Kimberly",
            LastName = "Morgan",
            Discriminator = "Instructor",
        };

        person.HireInfo = new HireInfo()
        {
            HireDate = DateTime.Now
        };

        // Add the new person to the context.
        context.People.Add(person);

        // Insert a row into the Person table.  
        context.SaveChanges();

        // Execute a query against the Person table.
        // The query returns columns that map to the Person entity.
        var existingPerson = context.People.FirstOrDefault();

        // Execute a query against the Person table.
        // The query returns columns that map to the Instructor entity.
        var hireInfo = existingPerson.HireInfo;

        Console.WriteLine("{0} was hired on {1}",
            existingPerson.LastName, hireInfo.HireDate);
    }
```
-   <span data-ttu-id="87e27-161">编译并运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="87e27-161">Compile and run the application.</span></span>

<span data-ttu-id="87e27-162">以下 T-SQL 语句执行针对**学校**由于运行此应用程序的数据库。</span><span class="sxs-lookup"><span data-stu-id="87e27-162">The following T-SQL statements were executed against the **School** database as a result of running this application.</span></span> 

-   <span data-ttu-id="87e27-163">以下**插入**因执行上下文执行。中的 savechanges （） 和合并数据**Person**并**HireInfo**实体</span><span class="sxs-lookup"><span data-stu-id="87e27-163">The following **INSERT** was executed as a result of executing context.SaveChanges() and combines data from the **Person** and **HireInfo** entities</span></span>

    ![Insert](~/ef6/media/insert.png)

-   <span data-ttu-id="87e27-165">以下**选择**因执行上下文执行。People.FirstOrDefault() 并选择只是列映射到**人员**</span><span class="sxs-lookup"><span data-stu-id="87e27-165">The following **SELECT** was executed as a result of executing context.People.FirstOrDefault() and selects just the columns mapped to **Person**</span></span>

    ![Select1](~/ef6/media/select1.png)

-   <span data-ttu-id="87e27-167">以下**选择**访问导航属性 existingPerson.Instructor 已执行与选择映射到列**HireInfo**</span><span class="sxs-lookup"><span data-stu-id="87e27-167">The following **SELECT** was executed as a result of accessing the navigation property existingPerson.Instructor and selects just the columns mapped to **HireInfo**</span></span>

    ![Select2](~/ef6/media/select2.png)
