---
title: 定义查询的 EF 设计器-EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: e52a297e-85aa-42f6-a922-ba960f8a4b22
caps.latest.revision: 3
ms.openlocfilehash: 593fb9925a7a0b59a69b8c8dc4846640627756aa
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/08/2018
ms.locfileid: "39120077"
---
# <a name="defining-query---ef-designer"></a><span data-ttu-id="62b38-102">定义查询的 EF 设计器</span><span class="sxs-lookup"><span data-stu-id="62b38-102">Defining Query - EF Designer</span></span>
<span data-ttu-id="62b38-103">本演练演示如何添加定义查询和相应的实体类型到使用 EF 设计器的模型。</span><span class="sxs-lookup"><span data-stu-id="62b38-103">This walkthrough demonstrates how to add a defining query and a corresponding entity type to a model using the EF Designer.</span></span> <span data-ttu-id="62b38-104">定义查询通常用于提供类似于数据库视图，由提供的功能，但该视图定义在模型中，不是数据库。</span><span class="sxs-lookup"><span data-stu-id="62b38-104">A defining query is commonly used to provide functionality similar to that provided by a database view, but the view is defined in the model, not the database.</span></span> <span data-ttu-id="62b38-105">定义查询，你可以执行 SQL 语句中指定**DefiningQuery** .edmx 文件的元素。</span><span class="sxs-lookup"><span data-stu-id="62b38-105">A defining query allows you to execute a SQL statement that is specified in the **DefiningQuery** element of an .edmx file.</span></span> <span data-ttu-id="62b38-106">有关详细信息，请参阅**DefiningQuery**中[SSDL 规范](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md)。</span><span class="sxs-lookup"><span data-stu-id="62b38-106">For more information, see **DefiningQuery** in the [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="62b38-107">当使用定义查询时，您还必须在模型中定义的实体类型。</span><span class="sxs-lookup"><span data-stu-id="62b38-107">When using defining queries, you also have to define an entity type in your model.</span></span> <span data-ttu-id="62b38-108">实体类型用于定义查询公开的数据。</span><span class="sxs-lookup"><span data-stu-id="62b38-108">The entity type is used to surface data exposed by the defining query.</span></span> <span data-ttu-id="62b38-109">请注意，显示此实体类型的数据是只读的。</span><span class="sxs-lookup"><span data-stu-id="62b38-109">Note that data surfaced through this entity type is read-only.</span></span>

<span data-ttu-id="62b38-110">无法将参数化查询作为定义查询执行。</span><span class="sxs-lookup"><span data-stu-id="62b38-110">Parameterized queries cannot be executed as defining queries.</span></span> <span data-ttu-id="62b38-111">但是，可以通过将显示数据的实体类型的插入、更新和删除函数映射到存储过程来更新数据。</span><span class="sxs-lookup"><span data-stu-id="62b38-111">However, the data can be updated by mapping the insert, update, and delete functions of the entity type that surfaces the data to stored procedures.</span></span> <span data-ttu-id="62b38-112">有关详细信息，请参阅[插入、 更新和删除的存储过程](~/ef6/modeling/designer/stored-procedures/cud.md)。</span><span class="sxs-lookup"><span data-stu-id="62b38-112">For more information, see [Insert, Update, and Delete with Stored Procedures](~/ef6/modeling/designer/stored-procedures/cud.md).</span></span>

<span data-ttu-id="62b38-113">本主题演示如何执行以下任务。</span><span class="sxs-lookup"><span data-stu-id="62b38-113">This topic shows how to perform the following tasks.</span></span>

-   <span data-ttu-id="62b38-114">添加定义查询</span><span class="sxs-lookup"><span data-stu-id="62b38-114">Add a Defining Query</span></span>
-   <span data-ttu-id="62b38-115">向模型添加实体类型</span><span class="sxs-lookup"><span data-stu-id="62b38-115">Add an Entity Type to the Model</span></span>
-   <span data-ttu-id="62b38-116">将定义查询映射到实体类型</span><span class="sxs-lookup"><span data-stu-id="62b38-116">Map the Defining Query to the Entity Type</span></span>

## <a name="prerequisites"></a><span data-ttu-id="62b38-117">系统必备</span><span class="sxs-lookup"><span data-stu-id="62b38-117">Prerequisites</span></span>

<span data-ttu-id="62b38-118">若要完成此演练，您需要：</span><span class="sxs-lookup"><span data-stu-id="62b38-118">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="62b38-119">最新版本的 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="62b38-119">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="62b38-120">[School 示例数据库](~/ef6/resources/school-database.md)。</span><span class="sxs-lookup"><span data-stu-id="62b38-120">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="62b38-121">设置项目</span><span class="sxs-lookup"><span data-stu-id="62b38-121">Set up the Project</span></span>

<span data-ttu-id="62b38-122">本演练使用 Visual Studio 2012 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="62b38-122">This walkthrough is using Visual Studio 2012 or newer.</span></span>

-   <span data-ttu-id="62b38-123">打开 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="62b38-123">Open Visual Studio.</span></span>
-   <span data-ttu-id="62b38-124">在 **“文件”** 菜单上，指向 **“新建”**，然后单击 **“项目”**。</span><span class="sxs-lookup"><span data-stu-id="62b38-124">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="62b38-125">在左窗格中，单击**Visual C\#**，然后选择**控制台应用程序**模板。</span><span class="sxs-lookup"><span data-stu-id="62b38-125">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="62b38-126">输入**DefiningQuerySample**作为名称的项目并单击**确定**。</span><span class="sxs-lookup"><span data-stu-id="62b38-126">Enter **DefiningQuerySample** as the name of the project and click **OK**.</span></span>

 

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="62b38-127">创建基于 School 数据库的模型</span><span class="sxs-lookup"><span data-stu-id="62b38-127">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="62b38-128">右键单击解决方案资源管理器中的项目名称，指向**外**，然后单击**新项**。</span><span class="sxs-lookup"><span data-stu-id="62b38-128">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="62b38-129">选择**数据**从左侧的菜单，然后选择**ADO.NET 实体数据模型**模板窗格中。</span><span class="sxs-lookup"><span data-stu-id="62b38-129">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="62b38-130">输入**DefiningQueryModel.edmx**以及该文件的名称，然后单击**添加**。</span><span class="sxs-lookup"><span data-stu-id="62b38-130">Enter **DefiningQueryModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="62b38-131">在选择模型内容对话框中，选择**从数据库生成**，然后单击**下一步**。</span><span class="sxs-lookup"><span data-stu-id="62b38-131">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="62b38-132">单击新建连接。</span><span class="sxs-lookup"><span data-stu-id="62b38-132">Click New Connection.</span></span> <span data-ttu-id="62b38-133">在连接属性对话框中，输入服务器名称 (例如， **(localdb)\\mssqllocaldb**)，选择身份验证方法、 类型**学校**数据库名称，然后单击**确定**。</span><span class="sxs-lookup"><span data-stu-id="62b38-133">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="62b38-134">选择数据连接对话框中使用数据库连接设置更新。</span><span class="sxs-lookup"><span data-stu-id="62b38-134">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="62b38-135">在选择数据库对象对话框中，检查**表**节点。</span><span class="sxs-lookup"><span data-stu-id="62b38-135">In the Choose Your Database Objects dialog box, check the **Tables** node.</span></span> <span data-ttu-id="62b38-136">这将添加到的所有表**学校**模型。</span><span class="sxs-lookup"><span data-stu-id="62b38-136">This will add all the tables to the **School** model.</span></span>
-   <span data-ttu-id="62b38-137">单击 **“完成”**。</span><span class="sxs-lookup"><span data-stu-id="62b38-137">Click **Finish**.</span></span>
-   <span data-ttu-id="62b38-138">在解决方案资源管理器中右键单击**DefiningQueryModel.edmx**文件，然后选择**打开方式...**.</span><span class="sxs-lookup"><span data-stu-id="62b38-138">In Solution Explorer, right-click the **DefiningQueryModel.edmx** file and select **Open With…**.</span></span>
-   <span data-ttu-id="62b38-139">选择**XML （文本） 编辑器**。</span><span class="sxs-lookup"><span data-stu-id="62b38-139">Select **XML (Text) Editor**.</span></span>

    ![XMLEditor](~/ef6/media/xmleditor.png)

-   <span data-ttu-id="62b38-141">单击**是**如果系统提示并显示以下消息：</span><span class="sxs-lookup"><span data-stu-id="62b38-141">Click **Yes** if prompted with the following message:</span></span>

    ![Warning2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a><span data-ttu-id="62b38-143">添加定义查询</span><span class="sxs-lookup"><span data-stu-id="62b38-143">Add a Defining Query</span></span>

<span data-ttu-id="62b38-144">在此步骤中，我们将使用 XML 编辑器来添加定义查询和的实体类型到.edmx 文件的 SSDL 部分。</span><span class="sxs-lookup"><span data-stu-id="62b38-144">In this step we will use the XML Editor to add a defining query and an entity type to the SSDL section of the .edmx file.</span></span> 

-   <span data-ttu-id="62b38-145">添加**EntitySet**向.edmx 文件 （通过 13 第 5 行） 的 SSDL 部分的元素。</span><span class="sxs-lookup"><span data-stu-id="62b38-145">Add an **EntitySet** element to the SSDL section of the .edmx file (line 5 thru 13).</span></span> <span data-ttu-id="62b38-146">指定下列各项：</span><span class="sxs-lookup"><span data-stu-id="62b38-146">Specify the following:</span></span>
    -   <span data-ttu-id="62b38-147">仅**名称**并**EntityType**的属性**EntitySet**指定元素。</span><span class="sxs-lookup"><span data-stu-id="62b38-147">Only the **Name** and **EntityType** attributes of the **EntitySet** element are specified.</span></span>
    -   <span data-ttu-id="62b38-148">中使用的实体类型的完全限定名称**EntityType**属性。</span><span class="sxs-lookup"><span data-stu-id="62b38-148">The fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="62b38-149">要执行的 SQL 语句中指定**DefiningQuery**元素。</span><span class="sxs-lookup"><span data-stu-id="62b38-149">The SQL statement to be executed is specified in the **DefiningQuery** element.</span></span>

``` xml
    <!-- SSDL content -->
    <edmx:StorageModels>
      <Schema Namespace="SchoolModel.Store" Alias="Self" Provider="System.Data.SqlClient" ProviderManifestToken="2008" xmlns:store="http://schemas.microsoft.com/ado/2007/12/edm/EntityStoreSchemaGenerator" xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
        <EntityContainer Name="SchoolModelStoreContainer">
           <EntitySet Name="GradeReport" EntityType="SchoolModel.Store.GradeReport">
              <DefiningQuery>
                SELECT CourseID, Grade, FirstName, LastName
                FROM StudentGrade
                JOIN
                (SELECT * FROM Person WHERE EnrollmentDate IS NOT NULL) AS p
                ON StudentID = p.PersonID
              </DefiningQuery>
          </EntitySet>
          <EntitySet Name="Course" EntityType="SchoolModel.Store.Course" store:Type="Tables" Schema="dbo" />
```

-   <span data-ttu-id="62b38-150">添加**EntityType**向.edmx 文件的 SSDL 部分的元素。</span><span class="sxs-lookup"><span data-stu-id="62b38-150">Add the **EntityType** element to the SSDL section of the .edmx.</span></span> <span data-ttu-id="62b38-151">如图所示以下文件。</span><span class="sxs-lookup"><span data-stu-id="62b38-151">file as shown below.</span></span> <span data-ttu-id="62b38-152">请注意以下事项：</span><span class="sxs-lookup"><span data-stu-id="62b38-152">Note the following:</span></span>
    -   <span data-ttu-id="62b38-153">值**名称**属性相对应的值**EntityType**属性中**EntitySet**元素更高版本，尽管的完全限定名称在使用实体类型**EntityType**属性。</span><span class="sxs-lookup"><span data-stu-id="62b38-153">The value of the **Name** attribute corresponds to the value of the **EntityType** attribute in the **EntitySet** element above, although the fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="62b38-154">属性名称对应于中的 SQL 语句返回的列名**DefiningQuery**元素 （上述）。</span><span class="sxs-lookup"><span data-stu-id="62b38-154">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element (above).</span></span>
    -   <span data-ttu-id="62b38-155">在此示例中，实体键由三个属性组成以确保唯一键值。</span><span class="sxs-lookup"><span data-stu-id="62b38-155">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

``` xml
    <EntityType Name="GradeReport">
      <Key>
        <PropertyRef Name="CourseID" />
        <PropertyRef Name="FirstName" />
        <PropertyRef Name="LastName" />
      </Key>
      <Property Name="CourseID"
                Type="int"
                Nullable="false" />
      <Property Name="Grade"
                Type="decimal"
                Precision="3"
                Scale="2" />
      <Property Name="FirstName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
      <Property Name="LastName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
    </EntityType>
```

>[!NOTE]
> <span data-ttu-id="62b38-156">如果您运行更高版本**模型更新向导**对话框中，对存储模型，包括定义查询，所做的任何更改将被覆盖。</span><span class="sxs-lookup"><span data-stu-id="62b38-156">If later you run the **Update Model Wizard** dialog, any changes made to the storage model, including defining queries, will be overwritten.</span></span>

 

## <a name="add-an-entity-type-to-the-model"></a><span data-ttu-id="62b38-157">向模型添加实体类型</span><span class="sxs-lookup"><span data-stu-id="62b38-157">Add an Entity Type to the Model</span></span>

<span data-ttu-id="62b38-158">在此步骤中我们将添加到在概念模型中使用 EF 设计器的实体类型。</span><span class="sxs-lookup"><span data-stu-id="62b38-158">In this step we will add the entity type to the conceptual model using the EF Designer.</span></span>  <span data-ttu-id="62b38-159">请注意以下事项：</span><span class="sxs-lookup"><span data-stu-id="62b38-159">Note the following:</span></span>

-   <span data-ttu-id="62b38-160">**名称**实体的值相对应的**EntityType**属性中**EntitySet**上述元素。</span><span class="sxs-lookup"><span data-stu-id="62b38-160">The **Name** of the entity corresponds to the value of the **EntityType** attribute in the **EntitySet** element above.</span></span>
-   <span data-ttu-id="62b38-161">属性名称对应于中的 SQL 语句返回的列名**DefiningQuery**上述元素。</span><span class="sxs-lookup"><span data-stu-id="62b38-161">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element above.</span></span>
-   <span data-ttu-id="62b38-162">在此示例中，实体键由三个属性组成以确保唯一键值。</span><span class="sxs-lookup"><span data-stu-id="62b38-162">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

<span data-ttu-id="62b38-163">在 EF 设计器中打开模型。</span><span class="sxs-lookup"><span data-stu-id="62b38-163">Open the model in the EF Designer.</span></span>

-   <span data-ttu-id="62b38-164">双击 DefiningQueryModel.edmx。</span><span class="sxs-lookup"><span data-stu-id="62b38-164">Double-click the DefiningQueryModel.edmx.</span></span>
-   <span data-ttu-id="62b38-165">说**是**于以下消息：</span><span class="sxs-lookup"><span data-stu-id="62b38-165">Say **Yes** to the following message:</span></span>

    ![Warning2](~/ef6/media/warning2.png)

 

<span data-ttu-id="62b38-167">提供用于编辑模型的设计图面，实体设计器随即出现。</span><span class="sxs-lookup"><span data-stu-id="62b38-167">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

-   <span data-ttu-id="62b38-168">右键单击设计器图面上，然后选择**新添**-&gt;**实体...**.</span><span class="sxs-lookup"><span data-stu-id="62b38-168">Right-click the designer surface and select **Add New**-&gt;**Entity…**.</span></span>
-   <span data-ttu-id="62b38-169">指定**GradeReport**的实体名称并**CourseID**有关**键属性**。</span><span class="sxs-lookup"><span data-stu-id="62b38-169">Specify **GradeReport** for the entity name and **CourseID** for the **Key Property**.</span></span>
-   <span data-ttu-id="62b38-170">右键单击**GradeReport**实体，然后选择**添加新**- &gt; **标量属性**。</span><span class="sxs-lookup"><span data-stu-id="62b38-170">Right-click the **GradeReport** entity and select **Add New**-&gt; **Scalar Property**.</span></span>
-   <span data-ttu-id="62b38-171">更改到的属性的默认名称**FirstName**。</span><span class="sxs-lookup"><span data-stu-id="62b38-171">Change the default name of the property to **FirstName**.</span></span>
-   <span data-ttu-id="62b38-172">添加另一个标量属性，并指定**LastName**的名称。</span><span class="sxs-lookup"><span data-stu-id="62b38-172">Add another scalar property and specify **LastName** for the name.</span></span>
-   <span data-ttu-id="62b38-173">添加另一个标量属性，并指定**级**的名称。</span><span class="sxs-lookup"><span data-stu-id="62b38-173">Add another scalar property and specify **Grade** for the name.</span></span>
-   <span data-ttu-id="62b38-174">在中**属性**窗口中，更改**级**的**类型**属性设置为**十进制**。</span><span class="sxs-lookup"><span data-stu-id="62b38-174">In the **Properties** window, change the **Grade**’s **Type** property to **Decimal**.</span></span>
-   <span data-ttu-id="62b38-175">选择**FirstName**并**LastName**属性。</span><span class="sxs-lookup"><span data-stu-id="62b38-175">Select the **FirstName** and **LastName** properties.</span></span>
-   <span data-ttu-id="62b38-176">在中**属性**窗口中，更改**EntityKey**属性值设置为**True**。</span><span class="sxs-lookup"><span data-stu-id="62b38-176">In the **Properties** window, change the **EntityKey** property value to **True**.</span></span>

<span data-ttu-id="62b38-177">因此，以下元素已添加到**CSDL** .edmx 文件的部分。</span><span class="sxs-lookup"><span data-stu-id="62b38-177">As a result, the following elements were added to the **CSDL** section of the .edmx file.</span></span>

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a><span data-ttu-id="62b38-178">将定义查询映射到实体类型</span><span class="sxs-lookup"><span data-stu-id="62b38-178">Map the Defining Query to the Entity Type</span></span>

<span data-ttu-id="62b38-179">在此步骤中，我们将使用映射详细信息窗口中要映射的概念和存储实体类型。</span><span class="sxs-lookup"><span data-stu-id="62b38-179">In this step, we will use the Mapping Details window to map the conceptual and storage entity types.</span></span>

-   <span data-ttu-id="62b38-180">右键单击**GradeReport**实体上设计图面并选择**表映射**。</span><span class="sxs-lookup"><span data-stu-id="62b38-180">Right-click the **GradeReport** entity on the design surface and select **Table Mapping**.</span></span>  
    <span data-ttu-id="62b38-181">**映射详细信息**显示窗口。</span><span class="sxs-lookup"><span data-stu-id="62b38-181">The **Mapping Details** window is displayed.</span></span>
-   <span data-ttu-id="62b38-182">选择**GradeReport**从**&lt;添加表或视图&gt;** 下拉列表中 (位于**表**s)。</span><span class="sxs-lookup"><span data-stu-id="62b38-182">Select **GradeReport** from the **&lt;Add a Table or View&gt;** dropdown list (located under **Table**s).</span></span>  
    <span data-ttu-id="62b38-183">默认的概念之间的映射和存储**GradeReport**实体类型出现。</span><span class="sxs-lookup"><span data-stu-id="62b38-183">Default mappings between the conceptual and storage **GradeReport** entity type appear.</span></span>  
    <span data-ttu-id="62b38-184">![MappingDetails3](~/ef6/media/mappingdetails.png)</span><span class="sxs-lookup"><span data-stu-id="62b38-184">![MappingDetails3](~/ef6/media/mappingdetails.png)</span></span>

<span data-ttu-id="62b38-185">因此， **EntitySetMapping**元素添加到.edmx 文件的映射部分。</span><span class="sxs-lookup"><span data-stu-id="62b38-185">As a result, the **EntitySetMapping** element is added to the mapping section of the .edmx file.</span></span> 

``` xml
    <EntitySetMapping Name="GradeReports">
      <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.GradeReport)">
        <MappingFragment StoreEntitySet="GradeReport">
          <ScalarProperty Name="LastName" ColumnName="LastName" />
          <ScalarProperty Name="FirstName" ColumnName="FirstName" />
          <ScalarProperty Name="Grade" ColumnName="Grade" />
          <ScalarProperty Name="CourseID" ColumnName="CourseID" />
        </MappingFragment>
      </EntityTypeMapping>
    </EntitySetMapping>
```

-   <span data-ttu-id="62b38-186">编译该应用程序。</span><span class="sxs-lookup"><span data-stu-id="62b38-186">Compile the application.</span></span>

 

## <a name="call-the-defining-query-in-your-code"></a><span data-ttu-id="62b38-187">在代码中调用定义的查询</span><span class="sxs-lookup"><span data-stu-id="62b38-187">Call the Defining Query in your Code</span></span>

<span data-ttu-id="62b38-188">现在可以通过使用执行定义查询**GradeReport**实体类型。</span><span class="sxs-lookup"><span data-stu-id="62b38-188">You can now execute the defining query by using the **GradeReport** entity type.</span></span> 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
