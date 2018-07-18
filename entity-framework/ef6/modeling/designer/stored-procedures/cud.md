---
title: 设计器 CUD 存储过程-EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
caps.latest.revision: 3
ms.openlocfilehash: 6b6a1f843142713153fa86309ef55f9d6e804766
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/08/2018
ms.locfileid: "39119989"
---
# <a name="designer-cud-stored-procedures"></a><span data-ttu-id="cd7ba-102">设计器 CUD 存储过程</span><span class="sxs-lookup"><span data-stu-id="cd7ba-102">Designer CUD Stored Procedures</span></span>
<span data-ttu-id="cd7ba-103">此分步演练演示如何将映射创建\\插入、 更新和删除 (CUD) 操作的实体类型到存储过程使用实体框架设计器 （EF 设计器）。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-103">This step-by-step walkthrough show how to map the create\\insert, update, and delete (CUD) operations of an entity type to stored procedures using the Entity Framework Designer (EF Designer).</span></span>  <span data-ttu-id="cd7ba-104">默认情况下，实体框架会自动生成 SQL 语句的 CUD 操作，但还可以将存储的过程映射到这些操作。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-104">By default, the Entity Framework automatically generates the SQL statements for the CUD operations, but you can also map stored procedures to these operations.</span></span>  

<span data-ttu-id="cd7ba-105">请注意，Code First 不支持映射到存储的过程或函数。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-105">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="cd7ba-106">但是，可以通过使用 System.Data.Entity.DbSet.SqlQuery 方法中调用存储的过程或函数。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-106">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="cd7ba-107">例如：</span><span class="sxs-lookup"><span data-stu-id="cd7ba-107">For example:</span></span>
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a><span data-ttu-id="cd7ba-108">映射到 CUD 操作时的注意事项的存储过程</span><span class="sxs-lookup"><span data-stu-id="cd7ba-108">Considerations when Mapping the CUD Operations to Stored Procedures</span></span>

<span data-ttu-id="cd7ba-109">时 CUD 操作映射到存储过程，应该注意以下事项：</span><span class="sxs-lookup"><span data-stu-id="cd7ba-109">When mapping the CUD operations to stored procedures, the following considerations apply:</span></span> 

-   <span data-ttu-id="cd7ba-110">如果 CUD 操作的其中一个映射到存储过程，请将所有这些映射。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-110">If you are mapping one of the CUD operations to a stored procedure, map all of them.</span></span> <span data-ttu-id="cd7ba-111">如果未映射所有三个，映射的操作将失败，如果执行且**UpdateException**将引发。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-111">If you do not map all three, the unmapped operations will fail if executed and an **UpdateException** will be thrown.</span></span>
-   <span data-ttu-id="cd7ba-112">必须将存储过程的每个参数映射到实体属性。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-112">You must map every parameter of the stored procedure to entity properties.</span></span>
-   <span data-ttu-id="cd7ba-113">如果服务器生成的主键值为插入的行，必须将此值映射回实体的键属性。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-113">If the server generates the primary key value for the inserted row, you must map this value back to the entity's key property.</span></span> <span data-ttu-id="cd7ba-114">在示例中， **InsertPerson**存储的过程作为存储的过程的结果集的一部分返回新创建的主键。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-114">In the example that follows, the **InsertPerson** stored procedure returns the newly created primary key as part of the stored procedure's result set.</span></span> <span data-ttu-id="cd7ba-115">为主键映射到实体键 (**PersonID**) 使用**&lt;添加结果绑定&gt;** EF 设计器的功能。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-115">The primary key is mapped to the entity key (**PersonID**) using the **&lt;Add Result Bindings&gt;** feature of the EF Designer.</span></span>
-   <span data-ttu-id="cd7ba-116">存储的过程调用是使用概念模型中实体的映射的 1:1。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-116">The stored procedure calls are mapped 1:1 with the entities in the conceptual model.</span></span> <span data-ttu-id="cd7ba-117">例如，如果您在概念模型，然后映射中实现继承层次结构 CUD 存储过程**父**（基） 和**子**（派生） 的实体，保存**子**更改只会调用**子**的存储过程，它不会触发**父**的存储过程调用。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-117">For example, if you implement an inheritance hierarchy in your conceptual model and then map the CUD stored procedures for the **Parent** (base) and the **Child** (derived) entities, saving the **Child** changes will only call the **Child**’s stored procedures, it will not trigger the **Parent**’s stored procedures calls.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cd7ba-118">系统必备</span><span class="sxs-lookup"><span data-stu-id="cd7ba-118">Prerequisites</span></span>

<span data-ttu-id="cd7ba-119">若要完成此演练，您需要：</span><span class="sxs-lookup"><span data-stu-id="cd7ba-119">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="cd7ba-120">最新版本的 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-120">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="cd7ba-121">[School 示例数据库](~/ef6/resources/school-database.md)。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-121">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="cd7ba-122">设置项目</span><span class="sxs-lookup"><span data-stu-id="cd7ba-122">Set up the Project</span></span>

-   <span data-ttu-id="cd7ba-123">打开 Visual Studio 2012。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-123">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="cd7ba-124">选择**文件-&gt;新增-&gt;项目**</span><span class="sxs-lookup"><span data-stu-id="cd7ba-124">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="cd7ba-125">在左窗格中，单击**Visual C\#**，然后选择**控制台**模板。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-125">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="cd7ba-126">输入**CUDSProcsSample**作为名称。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-126">Enter **CUDSProcsSample** as the name.</span></span>
-   <span data-ttu-id="cd7ba-127">选择“确定”。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-127">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="cd7ba-128">创建模型</span><span class="sxs-lookup"><span data-stu-id="cd7ba-128">Create a Model</span></span>

-   <span data-ttu-id="cd7ba-129">右键单击解决方案资源管理器中的项目名称并选择**添加-&gt;新项**。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-129">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="cd7ba-130">选择**数据**从左侧的菜单，然后选择**ADO.NET 实体数据模型**模板窗格中。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="cd7ba-131">输入**CUDSProcs.edmx**以及该文件的名称，然后单击**添加**。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-131">Enter **CUDSProcs.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="cd7ba-132">在选择模型内容对话框中，选择**从数据库生成**，然后单击**下一步**。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-132">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="cd7ba-133">单击**新的连接**。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-133">Click **New Connection**.</span></span> <span data-ttu-id="cd7ba-134">在连接属性对话框中，输入服务器名称 (例如， **(localdb)\\mssqllocaldb**)，选择身份验证方法、 类型**学校**数据库名称，然后单击**确定**。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-134">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="cd7ba-135">选择数据连接对话框中使用数据库连接设置更新。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-135">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="cd7ba-136">在选择数据库对象对话框框中下,**表**节点中，选择**人员**表。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-136">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person** table.</span></span>
-   <span data-ttu-id="cd7ba-137">此外，请选择下的以下存储的过程**存储过程和函数**节点： **DeletePerson**， **InsertPerson**，和**UpdatePerson**.</span><span class="sxs-lookup"><span data-stu-id="cd7ba-137">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **DeletePerson**, **InsertPerson**, and **UpdatePerson**.</span></span> 
-   <span data-ttu-id="cd7ba-138">从 Visual Studio 2012 EF 设计器支持大容量导入的存储过程。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-138">Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures.</span></span> <span data-ttu-id="cd7ba-139">**导入选定的存储的过程和函数的实体模型到**默认选中的。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-139">The **Import selected stored procedures and functions into the entity model** is checked by default.</span></span> <span data-ttu-id="cd7ba-140">由于在此示例中我们拥有存储过程的插入、 更新和删除实体类型，因此我们不希望将其导入，并将取消选中此复选框。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-140">Since in this example we have stored procedures that insert, update, and delete entity types, we do not want to import them and will uncheck this checkbox.</span></span> 

    ![ImportSProcs](~/ef6/media/importsprocs.jpg)

-   <span data-ttu-id="cd7ba-142">单击 **“完成”**。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-142">Click **Finish**.</span></span>
    <span data-ttu-id="cd7ba-143">EF 设计器中，为编辑您的模型提供了设计图面上，会显示。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-143">The EF Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-the-person-entity-to-stored-procedures"></a><span data-ttu-id="cd7ba-144">将 Person 实体映射到存储过程</span><span class="sxs-lookup"><span data-stu-id="cd7ba-144">Map the Person Entity to Stored Procedures</span></span>

-   <span data-ttu-id="cd7ba-145">右键单击**Person**实体类型并选择**存储过程映射**。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-145">Right-click the **Person** entity type and select **Stored Procedure Mapping**.</span></span>
-   <span data-ttu-id="cd7ba-146">存储的过程映射将显示在**映射详细信息**窗口。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-146">The stored procedure mappings appear in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="cd7ba-147">单击**&lt;选择插入函数&gt;**。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-147">Click **&lt;Select Insert Function&gt;**.</span></span>
    <span data-ttu-id="cd7ba-148">该字段变成一个下拉列表，该列表中包含存储模型中可以映射到概念模型中的实体类型的存储过程。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-148">The field becomes a drop-down list of the stored procedures in the storage model that can be mapped to entity types in the conceptual model.</span></span>
    <span data-ttu-id="cd7ba-149">选择**InsertPerson**从下拉列表。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-149">Select **InsertPerson** from the drop-down list.</span></span>
-   <span data-ttu-id="cd7ba-150">此时将显示存储过程参数和实体属性之间的默认映射。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-150">Default mappings between stored procedure parameters and entity properties appear.</span></span> <span data-ttu-id="cd7ba-151">请注意，箭头指示映射方向：向存储过程参数提供属性值。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-151">Note that arrows indicate the mapping direction: Property values are supplied to stored procedure parameters.</span></span>
-   <span data-ttu-id="cd7ba-152">单击**&lt;添加结果绑定&gt;**。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-152">Click **&lt;Add Result Binding&gt;**.</span></span>
-   <span data-ttu-id="cd7ba-153">类型**NewPersonID**，返回的参数的名称**InsertPerson**存储过程。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-153">Type **NewPersonID**, the name of the parameter returned by the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="cd7ba-154">请确保不键入前导或尾部空格。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-154">Make sure not to type leading or trailing spaces.</span></span>
-   <span data-ttu-id="cd7ba-155">按 **Enter**。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-155">Press **Enter**.</span></span>
-   <span data-ttu-id="cd7ba-156">默认情况下**NewPersonID**映射到实体键**PersonID**。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-156">By default, **NewPersonID** is mapped to the entity key **PersonID**.</span></span> <span data-ttu-id="cd7ba-157">请注意，箭头指示映射的方向：向属性提供结果列的值。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-157">Note that an arrow indicates the direction of the mapping: The value of the result column is supplied to the property.</span></span>

    ![MappingDetails](~/ef6/media/mappingdetails.png)

-   <span data-ttu-id="cd7ba-159">单击**&lt;选择更新函数&gt;** ，然后选择**UpdatePerson**从产生的下拉列表。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-159">Click **&lt;Select Update Function&gt;** and select **UpdatePerson** from the resulting drop-down list.</span></span>
-   <span data-ttu-id="cd7ba-160">此时将显示存储过程参数和实体属性之间的默认映射。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-160">Default mappings between stored procedure parameters and entity properties appear.</span></span>
-   <span data-ttu-id="cd7ba-161">单击**&lt;选择删除函数&gt;** ，然后选择**DeletePerson**从产生的下拉列表。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-161">Click **&lt;Select Delete Function&gt;** and select **DeletePerson** from the resulting drop-down list.</span></span>
-   <span data-ttu-id="cd7ba-162">此时将显示存储过程参数和实体属性之间的默认映射。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-162">Default mappings between stored procedure parameters and entity properties appear.</span></span>

<span data-ttu-id="cd7ba-163">插入、 更新和删除操作**人员**实体类型现在映射到存储过程。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-163">The insert, update, and delete operations of the **Person** entity type are now mapped to stored procedures.</span></span>

<span data-ttu-id="cd7ba-164">如果你想要启用并发检查更新或删除的实体的存储过程时，，使用以下选项之一：</span><span class="sxs-lookup"><span data-stu-id="cd7ba-164">If you want to enable concurrency checking when updating or deleting an entity with stored procedures, use one of the following options:</span></span>

-   <span data-ttu-id="cd7ba-165">使用**输出**参数来返回数受影响的行从存储的过程并检查**受影响的行数参数**参数名称旁边的复选框。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-165">Use an **OUTPUT** parameter to return the number of affected rows from the stored procedure and check the **Rows Affected Parameter** checkbox next to the parameter name.</span></span> <span data-ttu-id="cd7ba-166">如果返回的值为零，当调用该操作时， [ **OptimisticConcurrencyException** ](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx)将引发。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-166">If the value returned is zero when the operation is called, an  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) will be thrown.</span></span>
-   <span data-ttu-id="cd7ba-167">检查**使用原始值**你想要使用的并发检查的属性旁边的复选框。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-167">Check the **Use Original Value** checkbox next to a property that you want to use for concurrency checking.</span></span> <span data-ttu-id="cd7ba-168">当试图更新时，写入数据发回数据库时将使用最初从数据库中读取的属性的值。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-168">When an update is attempted, the value of the property that was originally read from the database will be used when writing data back to the database.</span></span> <span data-ttu-id="cd7ba-169">如果值与在数据库中，值不匹配**OptimisticConcurrencyException**将引发。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-169">If the value does not match the value in the database, an **OptimisticConcurrencyException** will be thrown.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="cd7ba-170">使用模型</span><span class="sxs-lookup"><span data-stu-id="cd7ba-170">Use the Model</span></span>

<span data-ttu-id="cd7ba-171">打开**Program.cs**文件，其中**Main**方法的定义。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-171">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="cd7ba-172">以下代码添加到 Main 函数。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-172">Add the following code into the Main function.</span></span>

<span data-ttu-id="cd7ba-173">代码创建一个新**人员**对象，然后更新对象，并最终删除对象。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-173">The code creates a new **Person** object, then updates the object, and finally deletes the object.</span></span>         

``` csharp
    using (var context = new SchoolEntities())
    {
        var newInstructor = new Person
        {
            FirstName = "Robyn",
            LastName = "Martin",
            HireDate = DateTime.Now,
            Discriminator = "Instructor"
        }

        // Add the new object to the context.
        context.People.Add(newInstructor);

        Console.WriteLine("Added {0} {1} to the context.",
            newInstructor.FirstName, newInstructor.LastName);

        Console.WriteLine("Before SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // SaveChanges will call the InsertPerson sproc.  
        // The PersonID property will be assigned the value
        // returned by the sproc.
        context.SaveChanges();

        Console.WriteLine("After SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // Modify the object and call SaveChanges.
        // This time, the UpdatePerson will be called.
        newInstructor.FirstName = "Rachel";
        context.SaveChanges();

        // Remove the object from the context and call SaveChanges.
        // The DeletePerson sproc will be called.
        context.People.Remove(newInstructor);
        context.SaveChanges();

        Person deletedInstructor = context.People.
            Where(p => p.PersonID == newInstructor.PersonID).
            FirstOrDefault();

        if (deletedInstructor == null)
            Console.WriteLine("A person with PersonID {0} was deleted.",
                newInstructor.PersonID);
    }
```

-   <span data-ttu-id="cd7ba-174">编译并运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-174">Compile and run the application.</span></span> <span data-ttu-id="cd7ba-175">该程序生成以下输出 \*</span><span class="sxs-lookup"><span data-stu-id="cd7ba-175">The program produces the following output \*</span></span>
    >[!NOTE]
> <span data-ttu-id="cd7ba-176">PersonID 是自动生成服务器，因此很可能会看到不同数 \*</span><span class="sxs-lookup"><span data-stu-id="cd7ba-176">PersonID is auto-generated by the server, so you will most likely see a different number\*</span></span>

```
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

<span data-ttu-id="cd7ba-177">如果您正在使用 Visual Studio Ultimate 版本，可以使用调试器使用 Intellitrace，若要查看执行的 SQL 语句。</span><span class="sxs-lookup"><span data-stu-id="cd7ba-177">If you are working with the Ultimate version of Visual Studio, you can use Intellitrace with the debugger to see the SQL statements that get executed.</span></span>

![Intellitrace](~/ef6/media/intellitrace.png)
