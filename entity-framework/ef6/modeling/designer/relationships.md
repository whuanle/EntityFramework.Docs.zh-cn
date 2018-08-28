---
title: 关系的 EF 设计器的 EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 402fe960-754b-470f-976b-e5de3e9986b5
ms.openlocfilehash: 72efe76956c930a787449e6cce453ab0317adc7c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994643"
---
# <a name="relationships---ef-designer"></a><span data-ttu-id="bccda-102">关系的 EF 设计器</span><span class="sxs-lookup"><span data-stu-id="bccda-102">Relationships - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="bccda-103">此页提供了有关设置关系在模型中使用 EF 设计器的信息。</span><span class="sxs-lookup"><span data-stu-id="bccda-103">This page provides information about setting up relationships in your model using the EF Designer.</span></span> <span data-ttu-id="bccda-104">有关在 EF 和如何访问和处理数据使用关系的关系的常规信息，请参阅[关系和导航属性](~/ef6/fundamentals/relationships.md)。</span><span class="sxs-lookup"><span data-stu-id="bccda-104">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).</span></span>

<span data-ttu-id="bccda-105">关联定义一个模型中的实体类型之间的关系。</span><span class="sxs-lookup"><span data-stu-id="bccda-105">Associations define relationships between entity types in a model.</span></span> <span data-ttu-id="bccda-106">本主题演示如何将映射与实体框架设计器 （EF 设计器） 之间的关联。</span><span class="sxs-lookup"><span data-stu-id="bccda-106">This topic shows how to map associations with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="bccda-107">下图显示了使用 EF 设计器时使用的主窗口。</span><span class="sxs-lookup"><span data-stu-id="bccda-107">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EFDesigner](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="bccda-109">生成概念模型时，可能会在错误列表中出现有关未映射的实体和关联的警告。</span><span class="sxs-lookup"><span data-stu-id="bccda-109">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="bccda-110">可以忽略这些警告，因为您选择从模型生成数据库后，这些错误将会消失。</span><span class="sxs-lookup"><span data-stu-id="bccda-110">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="associations-overview"></a><span data-ttu-id="bccda-111">关联概述</span><span class="sxs-lookup"><span data-stu-id="bccda-111">Associations Overview</span></span>

<span data-ttu-id="bccda-112">在设计时使用 EF 设计器模型，.edmx 文件表示您的模型。</span><span class="sxs-lookup"><span data-stu-id="bccda-112">When you design your model using the EF Designer, an .edmx file represents your model.</span></span> <span data-ttu-id="bccda-113">在.edmx 文件中，**关联**元素定义两个实体类型之间的关系。</span><span class="sxs-lookup"><span data-stu-id="bccda-113">In the .edmx file, an **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="bccda-114">关联必须指定关系中涉及的实体类型和关系的每一端可能的实体类型数量（也称为重数）。</span><span class="sxs-lookup"><span data-stu-id="bccda-114">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="bccda-115">关联端的多重性可以具有的一 (1)、 零个或一个值 (0..1) 或许多 (\*)。</span><span class="sxs-lookup"><span data-stu-id="bccda-115">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="bccda-116">此信息指定在两个子**最终**元素。</span><span class="sxs-lookup"><span data-stu-id="bccda-116">This information is specified in two child **End** elements.</span></span>

<span data-ttu-id="bccda-117">在运行时，关联的一端的实体类型实例可以通过访问导航属性或外键 （如果您选择公开您的实体中的外键）。</span><span class="sxs-lookup"><span data-stu-id="bccda-117">At run time, entity type instances at one end of an association can be accessed through navigation properties or foreign keys (if you choose to expose foreign keys in your entities).</span></span> <span data-ttu-id="bccda-118">通过公开的外键，实体之间的关系进行管理**ReferentialConstraint**元素 (的子元素**关联**元素)。</span><span class="sxs-lookup"><span data-stu-id="bccda-118">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element (a child element of the **Association** element).</span></span> <span data-ttu-id="bccda-119">建议始终在你的实体中公开外的键关系。</span><span class="sxs-lookup"><span data-stu-id="bccda-119">It is recommended that you always expose foreign keys for relationships in your entities.</span></span>

> [!NOTE]
> <span data-ttu-id="bccda-120">在多对多 (\*:\*) 不能向实体添加外键。</span><span class="sxs-lookup"><span data-stu-id="bccda-120">In many-to-many (\*:\*) you cannot add foreign keys to the entities.</span></span> <span data-ttu-id="bccda-121">在中\*:\*关系中，关联信息通过独立对象进行管理。</span><span class="sxs-lookup"><span data-stu-id="bccda-121">In a \*:\* relationship, the association information is managed with an independent object.</span></span>

<span data-ttu-id="bccda-122">有关 CSDL 元素 (**ReferentialConstraint**，**关联**等) 请参阅[CSDL 规范](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md)。</span><span class="sxs-lookup"><span data-stu-id="bccda-122">For information about CSDL elements (**ReferentialConstraint**, **Association**, etc.) see the [CSDL specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span>

## <a name="create-and-delete-associations"></a><span data-ttu-id="bccda-123">创建和删除关联</span><span class="sxs-lookup"><span data-stu-id="bccda-123">Create and Delete Associations</span></span>

<span data-ttu-id="bccda-124">创建与 EF 设计器更新的关联模型的.edmx 文件的内容。</span><span class="sxs-lookup"><span data-stu-id="bccda-124">Creating an association with the EF Designer updates the model content of the .edmx file.</span></span> <span data-ttu-id="bccda-125">之后创建的关联，必须创建 （在本主题后面讨论） 的关联的映射。</span><span class="sxs-lookup"><span data-stu-id="bccda-125">After creating an association, you must create the mappings for the association (discussed later in this topic).</span></span>

> [!NOTE]
> <span data-ttu-id="bccda-126">本部分假设你已添加你想要创建到模型之间的关联的实体。</span><span class="sxs-lookup"><span data-stu-id="bccda-126">This section assumes that you already added the entities you wish to create an association between to your model.</span></span>

### <a name="to-create-an-association"></a><span data-ttu-id="bccda-127">创建关联</span><span class="sxs-lookup"><span data-stu-id="bccda-127">To create an association</span></span>

1.  <span data-ttu-id="bccda-128">右键单击设计图面上的空白区域，指向**新添**，然后选择**关联...**.</span><span class="sxs-lookup"><span data-stu-id="bccda-128">Right-click an empty area of the design surface, point to **Add New**, and select **Association…**.</span></span>
2.  <span data-ttu-id="bccda-129">中的关联的设置中填充**添加关联**对话框。</span><span class="sxs-lookup"><span data-stu-id="bccda-129">Fill in the settings for the association in the **Add Association** dialog.</span></span>

    ![AddAssociation](~/ef6/media/addassociation.png)

    > [!NOTE]
    > <span data-ttu-id="bccda-131">您可以选择不添加导航属性或外键属性向位于关联各端实体通过清除 * * 导航属性 * * 和 * * 添加到外的键属性&lt;实体类型名称&gt;实体 * *复选框。</span><span class="sxs-lookup"><span data-stu-id="bccda-131">You can choose to not add navigation properties or foreign key properties to the entities at the ends of the association by clearing the **Navigation Property **and **Add foreign key properties to the &lt;entity type name&gt; Entity **checkboxes.</span></span> <span data-ttu-id="bccda-132">如果只添加一个导航属性，则将只能在一个方向遍历关联。</span><span class="sxs-lookup"><span data-stu-id="bccda-132">If you add only one navigation property, the association will be traversable in only one direction.</span></span> <span data-ttu-id="bccda-133">如果不添加导航属性，则必须选择添加外键属性才能访问位于关联各端的实体。</span><span class="sxs-lookup"><span data-stu-id="bccda-133">If you add no navigation properties, you must choose to add foreign key properties in order to access entities at the ends of the association.</span></span>
    
3.  <span data-ttu-id="bccda-134">单击 **“确定”**。</span><span class="sxs-lookup"><span data-stu-id="bccda-134">Click **OK**.</span></span>

### <a name="to-delete-an-association"></a><span data-ttu-id="bccda-135">删除关联</span><span class="sxs-lookup"><span data-stu-id="bccda-135">To delete an association</span></span>

<span data-ttu-id="bccda-136">若要删除关联执行下列任一操作：</span><span class="sxs-lookup"><span data-stu-id="bccda-136">To delete an association do one of the following:</span></span>

-   <span data-ttu-id="bccda-137">右键单击在 EF 设计器图面上，然后选择关联**删除**。</span><span class="sxs-lookup"><span data-stu-id="bccda-137">Right-click the association on the EF Designer surface and select **Delete**.</span></span>

- <span data-ttu-id="bccda-138">OR-</span><span class="sxs-lookup"><span data-stu-id="bccda-138">OR -</span></span>

-   <span data-ttu-id="bccda-139">选择一个或多个关联并按 Delete 键。</span><span class="sxs-lookup"><span data-stu-id="bccda-139">Select one or more associations and press the DELETE key.</span></span>

## <a name="include-foreign-key-properties-in-your-entities-referential-constraints"></a><span data-ttu-id="bccda-140">实体 （引用约束） 中包括外键属性</span><span class="sxs-lookup"><span data-stu-id="bccda-140">Include Foreign Key Properties in Your Entities (Referential Constraints)</span></span>

<span data-ttu-id="bccda-141">建议始终在你的实体中公开外的键关系。</span><span class="sxs-lookup"><span data-stu-id="bccda-141">It is recommended that you always expose foreign keys for relationships in your entities.</span></span> <span data-ttu-id="bccda-142">实体框架使用引用约束来标识一个属性可作为一种关系的外键。</span><span class="sxs-lookup"><span data-stu-id="bccda-142">Entity Framework uses a referential constraint to identify that a property acts as the foreign key for a relationship.</span></span>

<span data-ttu-id="bccda-143">如果您选中***添加到外的键属性&lt;实体类型名称&gt;实体***复选框为你创建关系时，添加此引用约束。</span><span class="sxs-lookup"><span data-stu-id="bccda-143">If you checked the ***Add foreign key properties to the &lt;entity type name&gt; Entity*** checkbox when creating a relationship, this referential constraint was added for you.</span></span>

<span data-ttu-id="bccda-144">当使用 EF 设计器添加或编辑引用约束时，EF 设计器添加或修改**ReferentialConstraint** .edmx 文件的 CSDL 内容中的元素。</span><span class="sxs-lookup"><span data-stu-id="bccda-144">When you use the EF Designer to add or edit a referential constraint, the EF Designer adds or modifies a **ReferentialConstraint** element in the CSDL content of the .edmx file.</span></span>

-   <span data-ttu-id="bccda-145">双击要编辑的关联。</span><span class="sxs-lookup"><span data-stu-id="bccda-145">Double-click the association that you want to edit.</span></span>
    <span data-ttu-id="bccda-146">**引用约束**对话框随即出现。</span><span class="sxs-lookup"><span data-stu-id="bccda-146">The **Referential Constraint** dialog box appears.</span></span>
-   <span data-ttu-id="bccda-147">从**主体**下拉列表中，选择引用约束中的主体实体。</span><span class="sxs-lookup"><span data-stu-id="bccda-147">From the **Principal** drop-down list, select the principal entity in the referential constraint.</span></span>
    <span data-ttu-id="bccda-148">实体的键属性添加到**主体键**对话框列表中的。</span><span class="sxs-lookup"><span data-stu-id="bccda-148">The entity's key properties are added to the **Principal Key** list in the dialog box.</span></span>
-   <span data-ttu-id="bccda-149">从**依赖**下拉列表中，选择依赖实体的引用约束中。</span><span class="sxs-lookup"><span data-stu-id="bccda-149">From the **Dependent** drop-down list, select the dependent entity in the referential constraint.</span></span>
-   <span data-ttu-id="bccda-150">对于具有依赖键每个主体键，从下拉列表中选择对应的依赖键**依赖键**列。</span><span class="sxs-lookup"><span data-stu-id="bccda-150">For each principal key that has a dependent key, select a corresponding dependent key from the drop-down lists in the **Dependent Key** column.</span></span>

    ![RefConstraint](~/ef6/media/refconstraint.png)

-   <span data-ttu-id="bccda-152">单击 **“确定”**。</span><span class="sxs-lookup"><span data-stu-id="bccda-152">Click **OK**.</span></span>

## <a name="create-and-edit-association-mappings"></a><span data-ttu-id="bccda-153">创建和编辑关联映射</span><span class="sxs-lookup"><span data-stu-id="bccda-153">Create and Edit Association Mappings</span></span>

<span data-ttu-id="bccda-154">您可以指定如何关联映射到数据库中**映射详细信息**的 EF 设计器窗口。</span><span class="sxs-lookup"><span data-stu-id="bccda-154">You can specify how an association maps to the database in the **Mapping Details** window of the EF Designer.</span></span>

> [!NOTE]
> <span data-ttu-id="bccda-155">你只能映射不具有指定的引用约束的关联的详细信息。</span><span class="sxs-lookup"><span data-stu-id="bccda-155">You can only map details for the associations that do not have a referential constraint specified.</span></span> <span data-ttu-id="bccda-156">如果指定引用约束的实体中包含外键属性，并可用于控制哪一列的外键映射到实体映射详细信息。</span><span class="sxs-lookup"><span data-stu-id="bccda-156">If a referential constraint is specified then a foreign key property is included in the entity and you can use the Mapping Details for the entity to control which column the foreign key maps to.</span></span>

### <a name="create-an-association-mapping"></a><span data-ttu-id="bccda-157">创建关联映射</span><span class="sxs-lookup"><span data-stu-id="bccda-157">Create an association mapping</span></span>

-   <span data-ttu-id="bccda-158">右键单击设计图面并选择中的关联**表映射**。</span><span class="sxs-lookup"><span data-stu-id="bccda-158">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="bccda-159">这将显示在关联映射**映射的详细信息**窗口。</span><span class="sxs-lookup"><span data-stu-id="bccda-159">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="bccda-160">单击**添加一个表或视图**。</span><span class="sxs-lookup"><span data-stu-id="bccda-160">Click **Add a Table or View**.</span></span>
    <span data-ttu-id="bccda-161">此时将显示一个下拉列表，其中包含存储模型中的所有表。</span><span class="sxs-lookup"><span data-stu-id="bccda-161">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="bccda-162">选择关联要映射到的表。</span><span class="sxs-lookup"><span data-stu-id="bccda-162">Select the table to which the association will map.</span></span>
    <span data-ttu-id="bccda-163">**映射详细信息**窗口将显示在每个关联和键属性的实体类型的两端**最终**。</span><span class="sxs-lookup"><span data-stu-id="bccda-163">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each **End**.</span></span>
-   <span data-ttu-id="bccda-164">每个键属性，请单击**列**字段，然后选择该属性将映射到其中的列。</span><span class="sxs-lookup"><span data-stu-id="bccda-164">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

    ![MappingDetails4](~/ef6/media/mappingdetails4.png)

### <a name="edit-an-association-mapping"></a><span data-ttu-id="bccda-166">编辑关联映射</span><span class="sxs-lookup"><span data-stu-id="bccda-166">Edit an association mapping</span></span>

-   <span data-ttu-id="bccda-167">右键单击设计图面并选择中的关联**表映射**。</span><span class="sxs-lookup"><span data-stu-id="bccda-167">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="bccda-168">这将显示在关联映射**映射的详细信息**窗口。</span><span class="sxs-lookup"><span data-stu-id="bccda-168">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="bccda-169">单击**映射到&lt;表名&gt;**。</span><span class="sxs-lookup"><span data-stu-id="bccda-169">Click **Maps to &lt;Table Name&gt;**.</span></span>
    <span data-ttu-id="bccda-170">此时将显示一个下拉列表，其中包含存储模型中的所有表。</span><span class="sxs-lookup"><span data-stu-id="bccda-170">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="bccda-171">选择关联要映射到的表。</span><span class="sxs-lookup"><span data-stu-id="bccda-171">Select the table to which the association will map.</span></span>
    <span data-ttu-id="bccda-172">**映射详细信息**窗口将显示在每一端的关联和键属性的实体类型的两端。</span><span class="sxs-lookup"><span data-stu-id="bccda-172">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each End.</span></span>
-   <span data-ttu-id="bccda-173">每个键属性，请单击**列**字段，然后选择该属性将映射到其中的列。</span><span class="sxs-lookup"><span data-stu-id="bccda-173">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

## <a name="edit-and-delete-navigation-properties"></a><span data-ttu-id="bccda-174">编辑和删除导航属性</span><span class="sxs-lookup"><span data-stu-id="bccda-174">Edit and Delete Navigation Properties</span></span>

<span data-ttu-id="bccda-175">导航属性是快捷方式属性，用于定位位于关联各端的实体模型中。</span><span class="sxs-lookup"><span data-stu-id="bccda-175">Navigation properties are shortcut properties that are used to locate the entities at the ends of an association in a model.</span></span> <span data-ttu-id="bccda-176">在创建两个实体类型之间的关联时可以创建导航属性。</span><span class="sxs-lookup"><span data-stu-id="bccda-176">Navigation properties can be created when you create an association between two entity types.</span></span>

#### <a name="to-edit-navigation-properties"></a><span data-ttu-id="bccda-177">若要编辑导航属性</span><span class="sxs-lookup"><span data-stu-id="bccda-177">To edit navigation properties</span></span>

-   <span data-ttu-id="bccda-178">选择 EF 设计器图面上的导航属性。</span><span class="sxs-lookup"><span data-stu-id="bccda-178">Select a navigation property on the EF Designer surface.</span></span>
    <span data-ttu-id="bccda-179">有关导航属性信息显示在 Visual Studio**属性**窗口。</span><span class="sxs-lookup"><span data-stu-id="bccda-179">Information about the navigation property is displayed in the Visual Studio **Properties** window.</span></span>
-   <span data-ttu-id="bccda-180">更改中的属性设置**属性**窗口。</span><span class="sxs-lookup"><span data-stu-id="bccda-180">Change the property settings in the **Properties** window.</span></span>

#### <a name="to-delete-navigation-properties"></a><span data-ttu-id="bccda-181">若要删除导航属性</span><span class="sxs-lookup"><span data-stu-id="bccda-181">To delete navigation properties</span></span>

-   <span data-ttu-id="bccda-182">如果概念模型中的实体类型上没有公开外键，则删除导航属性可能导致仅在一个方向遍历相应的关联，或者根本不遍历关联。</span><span class="sxs-lookup"><span data-stu-id="bccda-182">If foreign keys are not exposed on entity types in the conceptual model, deleting a navigation property may make the corresponding association traversable in only one direction or not traversable at all.</span></span>
-   <span data-ttu-id="bccda-183">右键单击 EF 设计器图面上，然后选择上的一个导航属性**删除**。</span><span class="sxs-lookup"><span data-stu-id="bccda-183">Right-click a navigation property on the EF Designer surface and select **Delete**.</span></span>
