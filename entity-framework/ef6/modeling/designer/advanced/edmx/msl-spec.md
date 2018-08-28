---
title: MSL 规范的 EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
ms.openlocfilehash: 77dc7072c70b104188cd23974f32308960daebb6
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996026"
---
# <a name="msl-specification"></a><span data-ttu-id="dc361-102">MSL 规范</span><span class="sxs-lookup"><span data-stu-id="dc361-102">MSL Specification</span></span>
<span data-ttu-id="dc361-103">映射规范语言 (MSL) 是一种基于 XML 的语言描述概念模型和实体框架应用程序的存储模型之间的映射。</span><span class="sxs-lookup"><span data-stu-id="dc361-103">Mapping specification language (MSL) is an XML-based language that describes the mapping between the conceptual model and storage model of an Entity Framework application.</span></span>

<span data-ttu-id="dc361-104">在实体框架应用程序，映射元数据从.msl 文件 （以 MSL 编写） 加载在生成时。</span><span class="sxs-lookup"><span data-stu-id="dc361-104">In an Entity Framework application, mapping metadata is loaded from an .msl file (written in MSL) at build time.</span></span> <span data-ttu-id="dc361-105">实体框架使用在运行时映射元数据将针对特定于应用商店的命令对概念模型的查询。</span><span class="sxs-lookup"><span data-stu-id="dc361-105">Entity Framework uses mapping metadata at runtime to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="dc361-106">在设计时中，实体框架设计器 （EF 设计器） 将.edmx 文件中存储映射信息。</span><span class="sxs-lookup"><span data-stu-id="dc361-106">The Entity Framework Designer (EF Designer) stores mapping information in an .edmx file at design time.</span></span> <span data-ttu-id="dc361-107">在生成时，实体设计器使用的信息在.edmx 文件创建在运行时所需的实体框架的.msl 文件</span><span class="sxs-lookup"><span data-stu-id="dc361-107">At build time, the Entity Designer uses information in an .edmx file to create the .msl file that is needed by Entity Framework at runtime</span></span>

<span data-ttu-id="dc361-108">MSL 中引用的所有概念模型类型或存储模型类型的名称必须由其各自的命名空间名称限定。</span><span class="sxs-lookup"><span data-stu-id="dc361-108">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="dc361-109">有关概念模型命名空间名称的信息，请参阅[CSDL 规范](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md)。</span><span class="sxs-lookup"><span data-stu-id="dc361-109">For information about the conceptual model namespace name, see [CSDL Specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span> <span data-ttu-id="dc361-110">有关存储模型命名空间名称的信息，请参阅[SSDL 规范](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md)。</span><span class="sxs-lookup"><span data-stu-id="dc361-110">For information about the storage model namespace name, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="dc361-111">MSL 的版本按 XML 命名空间进行区分。</span><span class="sxs-lookup"><span data-stu-id="dc361-111">Versions of MSL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="dc361-112">MSL 版本</span><span class="sxs-lookup"><span data-stu-id="dc361-112">MSL Version</span></span> | <span data-ttu-id="dc361-113">XML Namespace</span><span class="sxs-lookup"><span data-stu-id="dc361-113">XML Namespace</span></span>                                        |
|:------------|:-----------------------------------------------------|
| <span data-ttu-id="dc361-114">MSL v1</span><span class="sxs-lookup"><span data-stu-id="dc361-114">MSL v1</span></span>      | <span data-ttu-id="dc361-115">urn： 架构-microsoft-com:windows:storage:mapping:CS</span><span class="sxs-lookup"><span data-stu-id="dc361-115">urn:schemas-microsoft-com:windows:storage:mapping:CS</span></span> |
| <span data-ttu-id="dc361-116">MSL v2</span><span class="sxs-lookup"><span data-stu-id="dc361-116">MSL v2</span></span>      | http://schemas.microsoft.com/ado/2008/09/mapping/cs  |
| <span data-ttu-id="dc361-117">MSL v3</span><span class="sxs-lookup"><span data-stu-id="dc361-117">MSL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a><span data-ttu-id="dc361-118">Alias 元素 (MSL)</span><span class="sxs-lookup"><span data-stu-id="dc361-118">Alias Element (MSL)</span></span>

<span data-ttu-id="dc361-119">**别名**以映射规范语言 （msl） 表示的元素是用于定义概念模型和存储模型命名空间的别名映射元素的子元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-119">The **Alias** element in mapping specification language (MSL) is a child of the Mapping element that is used to define aliases for conceptual model and storage model namespaces.</span></span> <span data-ttu-id="dc361-120">MSL 中引用的所有概念模型类型或存储模型类型的名称必须由其各自的命名空间名称限定。</span><span class="sxs-lookup"><span data-stu-id="dc361-120">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="dc361-121">有关概念模型命名空间名称的信息，请参阅架构元素 (CSDL)。</span><span class="sxs-lookup"><span data-stu-id="dc361-121">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="dc361-122">有关存储模型命名空间名称的信息，请参阅架构元素 (SSDL)。</span><span class="sxs-lookup"><span data-stu-id="dc361-122">For information about the storage model namespace name, see Schema Element (SSDL).</span></span>

<span data-ttu-id="dc361-123">**别名**元素不能具有子元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-123">The **Alias** element cannot have child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="dc361-124">适用的特性</span><span class="sxs-lookup"><span data-stu-id="dc361-124">Applicable Attributes</span></span>

<span data-ttu-id="dc361-125">下表描述了可应用于的特性**别名**元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-125">The table below describes the attributes that can be applied to the **Alias** element.</span></span>

| <span data-ttu-id="dc361-126">特性名</span><span class="sxs-lookup"><span data-stu-id="dc361-126">Attribute Name</span></span> | <span data-ttu-id="dc361-127">是否必需</span><span class="sxs-lookup"><span data-stu-id="dc361-127">Is Required</span></span> | <span data-ttu-id="dc361-128">“值”</span><span class="sxs-lookup"><span data-stu-id="dc361-128">Value</span></span>                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| <span data-ttu-id="dc361-129">**Key**</span><span class="sxs-lookup"><span data-stu-id="dc361-129">**Key**</span></span>        | <span data-ttu-id="dc361-130">是</span><span class="sxs-lookup"><span data-stu-id="dc361-130">Yes</span></span>         | <span data-ttu-id="dc361-131">指定的命名空间的别名**值**属性。</span><span class="sxs-lookup"><span data-stu-id="dc361-131">The alias for the namespace that is specified by the **Value** attribute.</span></span> |
| <span data-ttu-id="dc361-132">**值**</span><span class="sxs-lookup"><span data-stu-id="dc361-132">**Value**</span></span>      | <span data-ttu-id="dc361-133">是</span><span class="sxs-lookup"><span data-stu-id="dc361-133">Yes</span></span>         | <span data-ttu-id="dc361-134">为其命名空间的值**密钥**元素是一个别名。</span><span class="sxs-lookup"><span data-stu-id="dc361-134">The namespace for which the value of the **Key** element is an alias.</span></span>     |

### <a name="example"></a><span data-ttu-id="dc361-135">示例</span><span class="sxs-lookup"><span data-stu-id="dc361-135">Example</span></span>

<span data-ttu-id="dc361-136">下面的示例演示**别名**元素，用于定义别名， `c`，概念模型中定义的类型。</span><span class="sxs-lookup"><span data-stu-id="dc361-136">The following example shows an **Alias** element that defines an alias, `c`, for types that are defined in the conceptual model.</span></span>

``` xml
 <Mapping Space="C-S"
          xmlns="http://schemas.microsoft.com/ado/2009/11/mapping/cs">
   <Alias Key="c" Value="SchoolModel"/>
   <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                           CdmEntityContainer="SchoolModelEntities">
     <EntitySetMapping Name="Courses">
       <EntityTypeMapping TypeName="c.Course">
         <MappingFragment StoreEntitySet="Course">
           <ScalarProperty Name="CourseID" ColumnName="CourseID" />
           <ScalarProperty Name="Title" ColumnName="Title" />
           <ScalarProperty Name="Credits" ColumnName="Credits" />
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
     <EntitySetMapping Name="Departments">
       <EntityTypeMapping TypeName="c.Department">
         <MappingFragment StoreEntitySet="Department">
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
           <ScalarProperty Name="Name" ColumnName="Name" />
           <ScalarProperty Name="Budget" ColumnName="Budget" />
           <ScalarProperty Name="StartDate" ColumnName="StartDate" />
           <ScalarProperty Name="Administrator" ColumnName="Administrator" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
   </EntityContainerMapping>
 </Mapping>
```

## <a name="associationend-element-msl"></a><span data-ttu-id="dc361-137">AssociationEnd 元素 (MSL)</span><span class="sxs-lookup"><span data-stu-id="dc361-137">AssociationEnd Element (MSL)</span></span>

<span data-ttu-id="dc361-138">**AssociationEnd**概念模型中的实体类型的修改函数映射到基础数据库中的存储过程时使用映射规范语言 (MSL) 中的元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-138">The **AssociationEnd** element in mapping specification language (MSL) is used when the modification functions of an entity type in the conceptual model are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="dc361-139">如果存储的过程采用一个参数值保存在关联属性，修改**AssociationEnd**元素映射到的参数的属性值。</span><span class="sxs-lookup"><span data-stu-id="dc361-139">If a modification stored procedure takes a parameter whose value is held in an association property, the **AssociationEnd** element maps the property value to the parameter.</span></span> <span data-ttu-id="dc361-140">有关更多信息，请参见下面的示例。</span><span class="sxs-lookup"><span data-stu-id="dc361-140">For more information, see the example below.</span></span>

<span data-ttu-id="dc361-141">有关实体类型的修改函数映射到存储过程的详细信息，请参阅 ModificationFunctionMapping 元素 (MSL) 和演练： 将一个实体映射到存储过程。</span><span class="sxs-lookup"><span data-stu-id="dc361-141">For more information about mapping modification functions of entity types to stored procedures, see ModificationFunctionMapping Element (MSL) and Walkthrough: Mapping an Entity to Stored Procedures.</span></span>

<span data-ttu-id="dc361-142">**AssociationEnd**元素可以具有以下子元素：</span><span class="sxs-lookup"><span data-stu-id="dc361-142">The **AssociationEnd** element can have the following child elements:</span></span>

-   <span data-ttu-id="dc361-143">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="dc361-143">ScalarProperty</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="dc361-144">适用的特性</span><span class="sxs-lookup"><span data-stu-id="dc361-144">Applicable Attributes</span></span>

<span data-ttu-id="dc361-145">下表描述了适用于的特性**AssociationEnd**元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-145">The following table describes the attributes that are applicable to the **AssociationEnd** element.</span></span>

| <span data-ttu-id="dc361-146">特性名</span><span class="sxs-lookup"><span data-stu-id="dc361-146">Attribute Name</span></span>     | <span data-ttu-id="dc361-147">是否必需</span><span class="sxs-lookup"><span data-stu-id="dc361-147">Is Required</span></span> | <span data-ttu-id="dc361-148">“值”</span><span class="sxs-lookup"><span data-stu-id="dc361-148">Value</span></span>                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="dc361-149">**AssociationSet**</span><span class="sxs-lookup"><span data-stu-id="dc361-149">**AssociationSet**</span></span> | <span data-ttu-id="dc361-150">是</span><span class="sxs-lookup"><span data-stu-id="dc361-150">Yes</span></span>         | <span data-ttu-id="dc361-151">要映射的关联的名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-151">The name of the association that is being mapped.</span></span>                                                                                                                                 |
| <span data-ttu-id="dc361-152">**From**</span><span class="sxs-lookup"><span data-stu-id="dc361-152">**From**</span></span>           | <span data-ttu-id="dc361-153">是</span><span class="sxs-lookup"><span data-stu-id="dc361-153">Yes</span></span>         | <span data-ttu-id="dc361-154">值**FromRole**对应于要映射的关联的导航属性的属性。</span><span class="sxs-lookup"><span data-stu-id="dc361-154">The value of the **FromRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="dc361-155">有关详细信息，请参阅 NavigationProperty 元素 (CSDL)。</span><span class="sxs-lookup"><span data-stu-id="dc361-155">For more information, see NavigationProperty Element (CSDL).</span></span> |
| <span data-ttu-id="dc361-156">**若要**</span><span class="sxs-lookup"><span data-stu-id="dc361-156">**To**</span></span>             | <span data-ttu-id="dc361-157">是</span><span class="sxs-lookup"><span data-stu-id="dc361-157">Yes</span></span>         | <span data-ttu-id="dc361-158">值**ToRole**对应于要映射的关联的导航属性的属性。</span><span class="sxs-lookup"><span data-stu-id="dc361-158">The value of the **ToRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="dc361-159">有关详细信息，请参阅 NavigationProperty 元素 (CSDL)。</span><span class="sxs-lookup"><span data-stu-id="dc361-159">For more information, see NavigationProperty Element (CSDL).</span></span>   |

### <a name="example"></a><span data-ttu-id="dc361-160">示例</span><span class="sxs-lookup"><span data-stu-id="dc361-160">Example</span></span>

<span data-ttu-id="dc361-161">请考虑使用以下概念模型实体类型：</span><span class="sxs-lookup"><span data-stu-id="dc361-161">Consider the following conceptual model entity type:</span></span>

``` xml
 <EntityType Name="Course">
   <Key>
     <PropertyRef Name="CourseID" />
   </Key>
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" MaxLength="100"
             FixedLength="false" Unicode="true" />
   <Property Type="Int32" Name="Credits" Nullable="false" />
   <NavigationProperty Name="Department"
                       Relationship="SchoolModel.FK_Course_Department"
                       FromRole="Course" ToRole="Department" />
 </EntityType>
```

<span data-ttu-id="dc361-162">另请考虑使用以下存储过程：</span><span class="sxs-lookup"><span data-stu-id="dc361-162">Also consider the following stored procedure:</span></span>

``` SQL
 CREATE PROCEDURE [dbo].[UpdateCourse]
                                @CourseID int,
                                @Title nvarchar(50),
                                @Credits int,
                                @DepartmentID int
                                AS
                                UPDATE Course SET Title=@Title,
                                                              Credits=@Credits,
                                                              DepartmentID=@DepartmentID
                                WHERE CourseID=@CourseID;
```

<span data-ttu-id="dc361-163">若要映射的更新函数`Course`实体与此存储过程，必须提供的值**DepartmentID**参数。</span><span class="sxs-lookup"><span data-stu-id="dc361-163">In order to map the update function of the `Course` entity to this stored procedure, you must supply a value to the **DepartmentID** parameter.</span></span> <span data-ttu-id="dc361-164">`DepartmentID` 的值与实体类型中的某个属性不对应；该值包含在独立的关联中，此关联的映射如下所示：</span><span class="sxs-lookup"><span data-stu-id="dc361-164">The value for `DepartmentID` does not correspond to a property on the entity type; it is contained in an independent association whose mapping is shown here:</span></span>

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
 </AssociationSetMapping>
```

<span data-ttu-id="dc361-165">下面的代码演示**AssociationEnd**元素用于映射**DepartmentID**属性**FK\_课程\_部门**关联要**UpdateCourse**存储过程 (所属的更新函数**课程**实体类型映射):</span><span class="sxs-lookup"><span data-stu-id="dc361-165">The following code shows the **AssociationEnd** element used to map the **DepartmentID** property of the **FK\_Course\_Department** association to the **UpdateCourse** stored procedure (to which the update function of the **Course** entity type is mapped):</span></span>

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <ModificationFunctionMapping>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdateCourse">
         <AssociationEnd AssociationSet="FK_Course_Department"
                         From="Course" To="Department">
           <ScalarProperty Name="DepartmentID"
                           ParameterName="DepartmentID"
                           Version="Current" />
         </AssociationEnd>
         <ScalarProperty Name="Credits" ParameterName="Credits"
                         Version="Current" />
         <ScalarProperty Name="Title" ParameterName="Title"
                         Version="Current" />
         <ScalarProperty Name="CourseID" ParameterName="CourseID"
                         Version="Current" />
       </UpdateFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="associationsetmapping-element-msl"></a><span data-ttu-id="dc361-166">AssociationSetMapping 元素 (MSL)</span><span class="sxs-lookup"><span data-stu-id="dc361-166">AssociationSetMapping Element (MSL)</span></span>

<span data-ttu-id="dc361-167">**AssociationSetMapping**以映射规范语言 （msl） 表示的元素定义概念模型和表基础数据库中的列中的关联之间的映射。</span><span class="sxs-lookup"><span data-stu-id="dc361-167">The **AssociationSetMapping** element in mapping specification language (MSL) defines the mapping between an association in the conceptual model and table columns in the underlying database.</span></span>

<span data-ttu-id="dc361-168">概念模型中的关联是一些属性表示基础数据库中的主键列和外键列的类型。</span><span class="sxs-lookup"><span data-stu-id="dc361-168">Associations in the conceptual model are types whose properties represent primary and foreign key columns in the underlying database.</span></span> <span data-ttu-id="dc361-169">**AssociationSetMapping**元素使用两个 EndProperty 元素用于在数据库中定义关联类型属性与列之间的映射。</span><span class="sxs-lookup"><span data-stu-id="dc361-169">The **AssociationSetMapping** element uses two EndProperty elements to define the mappings between association type properties and columns in the database.</span></span> <span data-ttu-id="dc361-170">可以将条件放在条件元素具有这些映射。</span><span class="sxs-lookup"><span data-stu-id="dc361-170">You can place conditions on these mappings with the Condition element.</span></span> <span data-ttu-id="dc361-171">将 insert、 update 和 delete 函数的关联映射到 ModificationFunctionMapping 元素与数据库中的存储过程。</span><span class="sxs-lookup"><span data-stu-id="dc361-171">Map the insert, update, and delete functions for associations to stored procedures in the database with the ModificationFunctionMapping element.</span></span> <span data-ttu-id="dc361-172">QueryView 元素中使用实体 SQL 字符串来定义关联与表列之间的只读映射。</span><span class="sxs-lookup"><span data-stu-id="dc361-172">Define read-only mappings between associations and table columns by using an Entity SQL string in a QueryView element.</span></span>

> [!NOTE]
> <span data-ttu-id="dc361-173">如果概念模型中的某个关联定义引用约束，则关联不需要通过映射**AssociationSetMapping**元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-173">If a referential constraint is defined for an association in the conceptual model, the association does not need to be mapped with an **AssociationSetMapping** element.</span></span> <span data-ttu-id="dc361-174">如果**AssociationSetMapping**元素具有引用约束的关联存在，则中定义的映射**AssociationSetMapping**元素将被忽略。</span><span class="sxs-lookup"><span data-stu-id="dc361-174">If an **AssociationSetMapping** element is present for an association that has a referential constraint, the mappings defined in the **AssociationSetMapping** element will be ignored.</span></span> <span data-ttu-id="dc361-175">有关详细信息，请参阅 ReferentialConstraint 元素 (CSDL)。</span><span class="sxs-lookup"><span data-stu-id="dc361-175">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="dc361-176">**AssociationSetMapping**元素可以具有以下子元素</span><span class="sxs-lookup"><span data-stu-id="dc361-176">The **AssociationSetMapping** element can have the following child elements</span></span>

-   <span data-ttu-id="dc361-177">QueryView （零个或一个）</span><span class="sxs-lookup"><span data-stu-id="dc361-177">QueryView (zero or one)</span></span>
-   <span data-ttu-id="dc361-178">EndProperty （零个或两个）</span><span class="sxs-lookup"><span data-stu-id="dc361-178">EndProperty (zero or two)</span></span>
-   <span data-ttu-id="dc361-179">条件 （零个或多个）</span><span class="sxs-lookup"><span data-stu-id="dc361-179">Condition (zero or more)</span></span>
-   <span data-ttu-id="dc361-180">ModificationFunctionMapping （零个或一个）</span><span class="sxs-lookup"><span data-stu-id="dc361-180">ModificationFunctionMapping (zero or one)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="dc361-181">适用的特性</span><span class="sxs-lookup"><span data-stu-id="dc361-181">Applicable Attributes</span></span>

<span data-ttu-id="dc361-182">下表介绍可应用于的特性**AssociationSetMapping**元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-182">The following table describes the attributes that can be applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="dc361-183">特性名</span><span class="sxs-lookup"><span data-stu-id="dc361-183">Attribute Name</span></span>     | <span data-ttu-id="dc361-184">是否必需</span><span class="sxs-lookup"><span data-stu-id="dc361-184">Is Required</span></span> | <span data-ttu-id="dc361-185">“值”</span><span class="sxs-lookup"><span data-stu-id="dc361-185">Value</span></span>                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| <span data-ttu-id="dc361-186">**名称**</span><span class="sxs-lookup"><span data-stu-id="dc361-186">**Name**</span></span>           | <span data-ttu-id="dc361-187">是</span><span class="sxs-lookup"><span data-stu-id="dc361-187">Yes</span></span>         | <span data-ttu-id="dc361-188">要映射的概念模型关联集的名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-188">The name of the conceptual model association set that is being mapped.</span></span>                      |
| <span data-ttu-id="dc361-189">**类型名称**</span><span class="sxs-lookup"><span data-stu-id="dc361-189">**TypeName**</span></span>       | <span data-ttu-id="dc361-190">否</span><span class="sxs-lookup"><span data-stu-id="dc361-190">No</span></span>          | <span data-ttu-id="dc361-191">要映射的概念模型关联类型的命名空间限定的名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-191">The namespace-qualified name of the conceptual model association type that is being mapped.</span></span> |
| <span data-ttu-id="dc361-192">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="dc361-192">**StoreEntitySet**</span></span> | <span data-ttu-id="dc361-193">否</span><span class="sxs-lookup"><span data-stu-id="dc361-193">No</span></span>          | <span data-ttu-id="dc361-194">要映射的表的名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-194">The name of the table that is being mapped.</span></span>                                                 |

### <a name="example"></a><span data-ttu-id="dc361-195">示例</span><span class="sxs-lookup"><span data-stu-id="dc361-195">Example</span></span>

<span data-ttu-id="dc361-196">下面的示例演示**AssociationSetMapping**中的元素**FK\_课程\_部门**中概念模型关联集映射到**课程**数据库表中的。</span><span class="sxs-lookup"><span data-stu-id="dc361-196">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association set in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="dc361-197">关联类型属性与表列之间的映射指定子级**EndProperty**元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-197">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
 </AssociationSetMapping>
``` 

## <a name="complexproperty-element-msl"></a><span data-ttu-id="dc361-198">ComplexProperty 元素 (MSL)</span><span class="sxs-lookup"><span data-stu-id="dc361-198">ComplexProperty Element (MSL)</span></span>

<span data-ttu-id="dc361-199">一个**ComplexProperty**以映射规范语言 （msl） 表示的元素定义概念模型实体类型的表列基础数据库中的复杂类型属性之间的映射。</span><span class="sxs-lookup"><span data-stu-id="dc361-199">A **ComplexProperty** element in mapping specification language (MSL) defines the mapping between a complex type property on a conceptual model entity type and table columns in the underlying database.</span></span> <span data-ttu-id="dc361-200">在子 ScalarProperty 元素中指定属性-列映射。</span><span class="sxs-lookup"><span data-stu-id="dc361-200">The property-column mappings are specified in child ScalarProperty elements.</span></span>

<span data-ttu-id="dc361-201">**ComplexType**属性元素可以具有以下子元素：</span><span class="sxs-lookup"><span data-stu-id="dc361-201">The **ComplexType** property element can have the following child elements:</span></span>

-   <span data-ttu-id="dc361-202">ScalarProperty （零个或多个）</span><span class="sxs-lookup"><span data-stu-id="dc361-202">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="dc361-203">**ComplexProperty** （零个或多个）</span><span class="sxs-lookup"><span data-stu-id="dc361-203">**ComplexProperty** (zero or more)</span></span>
-   <span data-ttu-id="dc361-204">ComplextTypeMapping （零个或多个）</span><span class="sxs-lookup"><span data-stu-id="dc361-204">ComplextTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="dc361-205">条件 （零个或多个）</span><span class="sxs-lookup"><span data-stu-id="dc361-205">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="dc361-206">适用的特性</span><span class="sxs-lookup"><span data-stu-id="dc361-206">Applicable Attributes</span></span>

<span data-ttu-id="dc361-207">下表描述了适用于的特性**ComplexProperty**元素：</span><span class="sxs-lookup"><span data-stu-id="dc361-207">The following table describes the attributes that are applicable to the **ComplexProperty** element:</span></span>

| <span data-ttu-id="dc361-208">特性名</span><span class="sxs-lookup"><span data-stu-id="dc361-208">Attribute Name</span></span> | <span data-ttu-id="dc361-209">是否必需</span><span class="sxs-lookup"><span data-stu-id="dc361-209">Is Required</span></span> | <span data-ttu-id="dc361-210">“值”</span><span class="sxs-lookup"><span data-stu-id="dc361-210">Value</span></span>                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="dc361-211">**名称**</span><span class="sxs-lookup"><span data-stu-id="dc361-211">**Name**</span></span>       | <span data-ttu-id="dc361-212">是</span><span class="sxs-lookup"><span data-stu-id="dc361-212">Yes</span></span>         | <span data-ttu-id="dc361-213">概念模型中要映射的实体类型的复杂属性的名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-213">The name of the complex property of an entity type in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="dc361-214">**类型名称**</span><span class="sxs-lookup"><span data-stu-id="dc361-214">**TypeName**</span></span>   | <span data-ttu-id="dc361-215">否</span><span class="sxs-lookup"><span data-stu-id="dc361-215">No</span></span>          | <span data-ttu-id="dc361-216">概念模型属性类型的命名空间限定名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-216">The namespace-qualified name of the conceptual model property type.</span></span>                              |

### <a name="example"></a><span data-ttu-id="dc361-217">示例</span><span class="sxs-lookup"><span data-stu-id="dc361-217">Example</span></span>

<span data-ttu-id="dc361-218">下面的示例基于 School 模型。</span><span class="sxs-lookup"><span data-stu-id="dc361-218">The following example is based on the School model.</span></span> <span data-ttu-id="dc361-219">下面的复杂类型已添加到概念模型中：</span><span class="sxs-lookup"><span data-stu-id="dc361-219">The following complex type has been added to the conceptual model:</span></span>

``` xml
 <ComplexType Name="FullName">
   <Property Type="String" Name="LastName"
             Nullable="false" MaxLength="50"
             FixedLength="false" Unicode="true" />
   <Property Type="String" Name="FirstName"
             Nullable="false" MaxLength="50"
             FixedLength="false" Unicode="true" />
 </ComplexType>
```

<span data-ttu-id="dc361-220">**LastName**并**FirstName**的属性**人员**实体类型已被替换为一个复杂属性**名称**:</span><span class="sxs-lookup"><span data-stu-id="dc361-220">The **LastName** and **FirstName** properties of the **Person** entity type have been replaced with one complex property, **Name**:</span></span>

``` xml
 <EntityType Name="Person">
   <Key>
     <PropertyRef Name="PersonID" />
   </Key>
   <Property Name="PersonID" Type="Int32" Nullable="false"
             annotation:StoreGeneratedPattern="Identity" />
   <Property Name="HireDate" Type="DateTime" />
   <Property Name="EnrollmentDate" Type="DateTime" />
   <Property Name="Name" Type="SchoolModel.FullName" Nullable="false" />
 </EntityType>
```

<span data-ttu-id="dc361-221">下面的 MSL 演示**ComplexProperty**元素用于映射**名称**到基础数据库中的列的属性：</span><span class="sxs-lookup"><span data-stu-id="dc361-221">The following MSL shows the **ComplexProperty** element used to map the **Name** property to columns in the underlying database:</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate" ColumnName="EnrollmentDate" />
       <ComplexProperty Name="Name" TypeName="SchoolModel.FullName">
         <ScalarProperty Name="FirstName" ColumnName="FirstName" />
         <ScalarProperty Name="LastName" ColumnName="LastName" />  
       </ComplexProperty>
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="complextypemapping-element-msl"></a><span data-ttu-id="dc361-222">ComplexTypeMapping 元素 (MSL)</span><span class="sxs-lookup"><span data-stu-id="dc361-222">ComplexTypeMapping Element (MSL)</span></span>

<span data-ttu-id="dc361-223">**ComplexTypeMapping**以映射规范语言 （msl） 表示的元素是 ResultMapping 元素的子元素，定义中的基础概念模型中的函数导入和存储的过程之间的映射当满足以下条件的数据库：</span><span class="sxs-lookup"><span data-stu-id="dc361-223">The **ComplexTypeMapping** element in mapping specification language (MSL) is a child of the ResultMapping element and defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="dc361-224">函数导入返回一个概念复杂类型。</span><span class="sxs-lookup"><span data-stu-id="dc361-224">The function import returns a conceptual complex type.</span></span>
-   <span data-ttu-id="dc361-225">存储过程返回的列的名称与复杂类型的属性名称不完全匹配。</span><span class="sxs-lookup"><span data-stu-id="dc361-225">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the complex type.</span></span>

<span data-ttu-id="dc361-226">默认情况下，存储过程返回的列与复杂类型返回的列之间的映射基于列名称和属性名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-226">By default, the mapping between the columns returned by a stored procedure and a complex type is based on column and property names.</span></span> <span data-ttu-id="dc361-227">如果列名称与属性名称不完全匹配，则必须使用**ComplexTypeMapping**元素定义的映射。</span><span class="sxs-lookup"><span data-stu-id="dc361-227">If column names do not exactly match property names, you must use the **ComplexTypeMapping** element to define the mapping.</span></span> <span data-ttu-id="dc361-228">有关默认映射的示例，请参阅 FunctionImportMapping 元素 (MSL)。</span><span class="sxs-lookup"><span data-stu-id="dc361-228">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="dc361-229">**ComplexTypeMapping**元素可以具有以下子元素：</span><span class="sxs-lookup"><span data-stu-id="dc361-229">The **ComplexTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="dc361-230">ScalarProperty （零个或多个）</span><span class="sxs-lookup"><span data-stu-id="dc361-230">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="dc361-231">适用的特性</span><span class="sxs-lookup"><span data-stu-id="dc361-231">Applicable Attributes</span></span>

<span data-ttu-id="dc361-232">下表描述了适用于的特性**ComplexTypeMapping**元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-232">The following table describes the attributes that are applicable to the **ComplexTypeMapping** element.</span></span>

| <span data-ttu-id="dc361-233">特性名</span><span class="sxs-lookup"><span data-stu-id="dc361-233">Attribute Name</span></span> | <span data-ttu-id="dc361-234">是否必需</span><span class="sxs-lookup"><span data-stu-id="dc361-234">Is Required</span></span> | <span data-ttu-id="dc361-235">“值”</span><span class="sxs-lookup"><span data-stu-id="dc361-235">Value</span></span>                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| <span data-ttu-id="dc361-236">**类型名称**</span><span class="sxs-lookup"><span data-stu-id="dc361-236">**TypeName**</span></span>   | <span data-ttu-id="dc361-237">是</span><span class="sxs-lookup"><span data-stu-id="dc361-237">Yes</span></span>         | <span data-ttu-id="dc361-238">要映射的复杂类型的命名空间限定的名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-238">The namespace-qualified name of the complex type that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="dc361-239">示例</span><span class="sxs-lookup"><span data-stu-id="dc361-239">Example</span></span>

<span data-ttu-id="dc361-240">请考虑使用以下存储过程：</span><span class="sxs-lookup"><span data-stu-id="dc361-240">Consider the following stored procedure:</span></span>

``` SQL
 CREATE PROCEDURE [dbo].[GetGrades]
             @student_Id int
             AS
             SELECT     EnrollmentID as enroll_id,
                                                                             Grade as grade,
                                                                             CourseID as course_id,
                                                                             StudentID as student_id
                                               FROM dbo.StudentGrade
             WHERE StudentID = @student_Id
```

<span data-ttu-id="dc361-241">另请考虑使用以下概念模型复杂类型：</span><span class="sxs-lookup"><span data-stu-id="dc361-241">Also consider the following conceptual model complex type:</span></span>

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

<span data-ttu-id="dc361-242">若要创建返回前一个复杂类型的实例的函数导入，各列之间的映射存储过程返回的和必须中定义的实体类型**ComplexTypeMapping**元素：</span><span class="sxs-lookup"><span data-stu-id="dc361-242">In order to create a function import that returns instances of the previous complex type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ComplexTypeMapping** element:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetGrades"
                        FunctionName="SchoolModel.Store.GetGrades" >
   <ResultMapping>
     <ComplexTypeMapping TypeName="SchoolModel.GradeInfo">
       <ScalarProperty Name="EnrollmentID" ColumnName="enroll_id"/>
       <ScalarProperty Name="CourseID" ColumnName="course_id"/>
       <ScalarProperty Name="StudentID" ColumnName="student_id"/>
       <ScalarProperty Name="Grade" ColumnName="grade"/>
     </ComplexTypeMapping>
   </ResultMapping>
 </FunctionImportMapping>
```

## <a name="condition-element-msl"></a><span data-ttu-id="dc361-243">Condition 元素 (MSL)</span><span class="sxs-lookup"><span data-stu-id="dc361-243">Condition Element (MSL)</span></span>

<span data-ttu-id="dc361-244">**条件**以映射规范语言 （msl） 表示的元素的条件置于概念模型与基础数据库之间的映射。</span><span class="sxs-lookup"><span data-stu-id="dc361-244">The **Condition** element in mapping specification language (MSL) places conditions on mappings between the conceptual model and the underlying database.</span></span> <span data-ttu-id="dc361-245">XML 节点内定义的映射是有效，如果所有条件，即在指定的子**条件**满足元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-245">The mapping that is defined within an XML node is valid if all conditions, as specified in child **Condition** elements, are met.</span></span> <span data-ttu-id="dc361-246">否则，该映射无效。</span><span class="sxs-lookup"><span data-stu-id="dc361-246">Otherwise, the mapping is not valid.</span></span> <span data-ttu-id="dc361-247">例如，如果 MappingFragment 元素包含一个或多个**条件**子元素内定义的映射**MappingFragment**节点将仅在如果所有有效的子条件**条件**满足元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-247">For example, if a MappingFragment element contains one or more **Condition** child elements, the mapping defined within the **MappingFragment** node will only be valid if all the conditions of the child **Condition** elements are met.</span></span>

<span data-ttu-id="dc361-248">每个条件均可应用于**名称**(由指定的概念模型实体属性的名称**名称**属性)，或**ColumnName** （中的列的名称指定的数据库**ColumnName**属性)。</span><span class="sxs-lookup"><span data-stu-id="dc361-248">Each condition can apply to either a **Name** (the name of a conceptual model entity property, specified by the **Name** attribute), or a **ColumnName** (the name of a column in the database, specified by the **ColumnName** attribute).</span></span> <span data-ttu-id="dc361-249">当**名称**设置属性，根据实体属性值检查条件。</span><span class="sxs-lookup"><span data-stu-id="dc361-249">When the **Name** attribute is set, the condition is checked against an entity property value.</span></span> <span data-ttu-id="dc361-250">当**ColumnName**属性设置，条件检查针对某列的值。</span><span class="sxs-lookup"><span data-stu-id="dc361-250">When the **ColumnName** attribute is set, the condition is checked against a column value.</span></span> <span data-ttu-id="dc361-251">只有一个**名称**或**ColumnName**中，可以指定属性**条件**元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-251">Only one of the **Name** or **ColumnName** attribute can be specified in a **Condition** element.</span></span>

> [!NOTE]
> <span data-ttu-id="dc361-252">当**条件**元素内 FunctionImportMapping 元素，仅使用**名称**特性不适用。</span><span class="sxs-lookup"><span data-stu-id="dc361-252">When the **Condition** element is used within a FunctionImportMapping element, only the **Name** attribute is not applicable.</span></span>

<span data-ttu-id="dc361-253">**条件**元素可以是以下元素的子元素：</span><span class="sxs-lookup"><span data-stu-id="dc361-253">The **Condition** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="dc361-254">AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="dc361-254">AssociationSetMapping</span></span>
-   <span data-ttu-id="dc361-255">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="dc361-255">ComplexProperty</span></span>
-   <span data-ttu-id="dc361-256">EntitySetMapping</span><span class="sxs-lookup"><span data-stu-id="dc361-256">EntitySetMapping</span></span>
-   <span data-ttu-id="dc361-257">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="dc361-257">MappingFragment</span></span>
-   <span data-ttu-id="dc361-258">EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="dc361-258">EntityTypeMapping</span></span>

<span data-ttu-id="dc361-259">**条件**元素可以具有任何子元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-259">The **Condition** element can have no child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="dc361-260">适用的特性</span><span class="sxs-lookup"><span data-stu-id="dc361-260">Applicable Attributes</span></span>

<span data-ttu-id="dc361-261">下表描述了适用于的特性**条件**元素：</span><span class="sxs-lookup"><span data-stu-id="dc361-261">The following table describes the attributes that are applicable to the **Condition** element:</span></span>

| <span data-ttu-id="dc361-262">特性名</span><span class="sxs-lookup"><span data-stu-id="dc361-262">Attribute Name</span></span> | <span data-ttu-id="dc361-263">是否必需</span><span class="sxs-lookup"><span data-stu-id="dc361-263">Is Required</span></span> | <span data-ttu-id="dc361-264">“值”</span><span class="sxs-lookup"><span data-stu-id="dc361-264">Value</span></span>                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="dc361-265">**列名称**</span><span class="sxs-lookup"><span data-stu-id="dc361-265">**ColumnName**</span></span> | <span data-ttu-id="dc361-266">否</span><span class="sxs-lookup"><span data-stu-id="dc361-266">No</span></span>          | <span data-ttu-id="dc361-267">表列的名称，其值用于计算条件。</span><span class="sxs-lookup"><span data-stu-id="dc361-267">The name of the table column whose value is used to evaluate the condition.</span></span>                                                                                                                                                                                                                   |
| <span data-ttu-id="dc361-268">**IsNull**</span><span class="sxs-lookup"><span data-stu-id="dc361-268">**IsNull**</span></span>     | <span data-ttu-id="dc361-269">否</span><span class="sxs-lookup"><span data-stu-id="dc361-269">No</span></span>          | <span data-ttu-id="dc361-270">**True**或**False**。</span><span class="sxs-lookup"><span data-stu-id="dc361-270">**True** or **False**.</span></span> <span data-ttu-id="dc361-271">如果值为 **，则返回 True**和列的值是**null**，或如果值为**False**和列的值不是**null**，条件为 true.</span><span class="sxs-lookup"><span data-stu-id="dc361-271">If the value is **True** and the column value is **null**, or if the value is **False** and the column value is not **null**, the condition is true.</span></span> <span data-ttu-id="dc361-272">否则，条件为 False。</span><span class="sxs-lookup"><span data-stu-id="dc361-272">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="dc361-273">**IsNull**并**值**属性不能在同一时间使用。</span><span class="sxs-lookup"><span data-stu-id="dc361-273">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span> |
| <span data-ttu-id="dc361-274">**值**</span><span class="sxs-lookup"><span data-stu-id="dc361-274">**Value**</span></span>      | <span data-ttu-id="dc361-275">否</span><span class="sxs-lookup"><span data-stu-id="dc361-275">No</span></span>          | <span data-ttu-id="dc361-276">要与列值进行比较的值。</span><span class="sxs-lookup"><span data-stu-id="dc361-276">The value with which the column value is compared.</span></span> <span data-ttu-id="dc361-277">如果值不同，则该条件为 True。</span><span class="sxs-lookup"><span data-stu-id="dc361-277">If the values are the same, the condition is true.</span></span> <span data-ttu-id="dc361-278">否则，条件为 False。</span><span class="sxs-lookup"><span data-stu-id="dc361-278">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="dc361-279">**IsNull**并**值**属性不能在同一时间使用。</span><span class="sxs-lookup"><span data-stu-id="dc361-279">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span>                                                                       |
| <span data-ttu-id="dc361-280">**名称**</span><span class="sxs-lookup"><span data-stu-id="dc361-280">**Name**</span></span>       | <span data-ttu-id="dc361-281">否</span><span class="sxs-lookup"><span data-stu-id="dc361-281">No</span></span>          | <span data-ttu-id="dc361-282">概念模型实体属性的名称，其值用于计算条件。</span><span class="sxs-lookup"><span data-stu-id="dc361-282">The name of the conceptual model entity property whose value is used to evaluate the condition.</span></span> <br/> <span data-ttu-id="dc361-283">此属性不适用如果**条件**FunctionImportMapping 元素中使用元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-283">This attribute is not applicable if the **Condition** element is used within a FunctionImportMapping element.</span></span>                                                                           |

### <a name="example"></a><span data-ttu-id="dc361-284">示例</span><span class="sxs-lookup"><span data-stu-id="dc361-284">Example</span></span>

<span data-ttu-id="dc361-285">下面的示例演示**条件**元素的子级作为**MappingFragment**元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-285">The following example shows **Condition** elements as children of **MappingFragment** elements.</span></span> <span data-ttu-id="dc361-286">当**HireDate**不为 null 和**EnrollmentDate**是为 null，将数据映射之间**SchoolModel.Instructor**类型和**PersonID**并**HireDate**的列**人员**表。</span><span class="sxs-lookup"><span data-stu-id="dc361-286">When **HireDate** is not null and **EnrollmentDate** is null, data is mapped between the **SchoolModel.Instructor** type and the **PersonID** and **HireDate** columns of the **Person** table.</span></span> <span data-ttu-id="dc361-287">当**EnrollmentDate**不为 null 和**HireDate**是为 null，将数据映射之间**SchoolModel.Student**类型和**PersonID**并**注册**的列**人员**表。</span><span class="sxs-lookup"><span data-stu-id="dc361-287">When **EnrollmentDate** is not null and **HireDate** is null, data is mapped between the **SchoolModel.Student** type and the **PersonID** and **Enrollment** columns of the **Person** table.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Person)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Instructor)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <Condition ColumnName="HireDate" IsNull="false" />
       <Condition ColumnName="EnrollmentDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Student)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
       <Condition ColumnName="EnrollmentDate" IsNull="false" />
       <Condition ColumnName="HireDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="deletefunction-element-msl"></a><span data-ttu-id="dc361-288">DeleteFunction 元素 (MSL)</span><span class="sxs-lookup"><span data-stu-id="dc361-288">DeleteFunction Element (MSL)</span></span>

<span data-ttu-id="dc361-289">**DeleteFunction**以映射规范语言 （msl） 表示的元素将概念模型中的实体类型或关联的删除函数映射到基础数据库中的存储过程。</span><span class="sxs-lookup"><span data-stu-id="dc361-289">The **DeleteFunction** element in mapping specification language (MSL) maps the delete function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="dc361-290">将修改函数映射到其中的存储过程必须在存储模型中声明。</span><span class="sxs-lookup"><span data-stu-id="dc361-290">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="dc361-291">有关详细信息，请参阅函数元素 (SSDL)。</span><span class="sxs-lookup"><span data-stu-id="dc361-291">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="dc361-292">如果不将所有这三个插入、 更新或删除一个实体类型到存储过程的操作、 映射的操作将失败，则在运行时执行和 UpdateException 引发。</span><span class="sxs-lookup"><span data-stu-id="dc361-292">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

### <a name="deletefunction-applied-to-entitytypemapping"></a><span data-ttu-id="dc361-293">应用于 EntityTypeMapping 的 DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="dc361-293">DeleteFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="dc361-294">当应用于 EntityTypeMapping 元素**DeleteFunction**元素将概念模型中的实体类型的删除函数映射到存储过程。</span><span class="sxs-lookup"><span data-stu-id="dc361-294">When applied to the EntityTypeMapping element, the **DeleteFunction** element maps the delete function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="dc361-295">**DeleteFunction**元素可以具有以下子元素时应用于**EntityTypeMapping**元素：</span><span class="sxs-lookup"><span data-stu-id="dc361-295">The **DeleteFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="dc361-296">AssociationEnd （零个或多个）</span><span class="sxs-lookup"><span data-stu-id="dc361-296">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="dc361-297">ComplexProperty （零个或多个）</span><span class="sxs-lookup"><span data-stu-id="dc361-297">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="dc361-298">ScarlarProperty （零个或多个）</span><span class="sxs-lookup"><span data-stu-id="dc361-298">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="dc361-299">适用的特性</span><span class="sxs-lookup"><span data-stu-id="dc361-299">Applicable Attributes</span></span>

<span data-ttu-id="dc361-300">下表介绍可应用于的特性**DeleteFunction**时应用于元素**EntityTypeMapping**元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-300">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="dc361-301">特性名</span><span class="sxs-lookup"><span data-stu-id="dc361-301">Attribute Name</span></span>            | <span data-ttu-id="dc361-302">是否必需</span><span class="sxs-lookup"><span data-stu-id="dc361-302">Is Required</span></span> | <span data-ttu-id="dc361-303">“值”</span><span class="sxs-lookup"><span data-stu-id="dc361-303">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="dc361-304">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="dc361-304">**FunctionName**</span></span>          | <span data-ttu-id="dc361-305">是</span><span class="sxs-lookup"><span data-stu-id="dc361-305">Yes</span></span>         | <span data-ttu-id="dc361-306">删除函数要映射到的存储过程的命名空间限定名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-306">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="dc361-307">存储过程必须在存储模型中声明。</span><span class="sxs-lookup"><span data-stu-id="dc361-307">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="dc361-308">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="dc361-308">**RowsAffectedParameter**</span></span> | <span data-ttu-id="dc361-309">否</span><span class="sxs-lookup"><span data-stu-id="dc361-309">No</span></span>          | <span data-ttu-id="dc361-310">返回受影响行数的输出参数的名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-310">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="dc361-311">示例</span><span class="sxs-lookup"><span data-stu-id="dc361-311">Example</span></span>

<span data-ttu-id="dc361-312">以下示例基于 School 模型，并显示**DeleteFunction**元素的删除函数映射**人员**实体类型设置为**DeletePerson**存储的过程。</span><span class="sxs-lookup"><span data-stu-id="dc361-312">The following example is based on the School model and shows the **DeleteFunction** element mapping the delete function of the **Person** entity type to the **DeletePerson** stored procedure.</span></span> <span data-ttu-id="dc361-313">**DeletePerson**在存储模型中声明的存储的过程。</span><span class="sxs-lookup"><span data-stu-id="dc361-313">The **DeletePerson** stored procedure is declared in the storage model.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="deletefunction-applied-to-associationsetmapping"></a><span data-ttu-id="dc361-314">应用于 AssociationSetMapping 的 DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="dc361-314">DeleteFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="dc361-315">当应用于 AssociationSetMapping 元素**DeleteFunction**元素将概念模型中的关联的删除函数映射到存储过程。</span><span class="sxs-lookup"><span data-stu-id="dc361-315">When applied to the AssociationSetMapping element, the **DeleteFunction** element maps the delete function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="dc361-316">**DeleteFunction**元素可以具有以下子元素时应用于**AssociationSetMapping**元素：</span><span class="sxs-lookup"><span data-stu-id="dc361-316">The **DeleteFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="dc361-317">EndProperty</span><span class="sxs-lookup"><span data-stu-id="dc361-317">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="dc361-318">适用的特性</span><span class="sxs-lookup"><span data-stu-id="dc361-318">Applicable Attributes</span></span>

<span data-ttu-id="dc361-319">下表介绍可应用于的特性**DeleteFunction**时应用于元素**AssociationSetMapping**元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-319">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="dc361-320">特性名</span><span class="sxs-lookup"><span data-stu-id="dc361-320">Attribute Name</span></span>            | <span data-ttu-id="dc361-321">是否必需</span><span class="sxs-lookup"><span data-stu-id="dc361-321">Is Required</span></span> | <span data-ttu-id="dc361-322">“值”</span><span class="sxs-lookup"><span data-stu-id="dc361-322">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="dc361-323">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="dc361-323">**FunctionName**</span></span>          | <span data-ttu-id="dc361-324">是</span><span class="sxs-lookup"><span data-stu-id="dc361-324">Yes</span></span>         | <span data-ttu-id="dc361-325">删除函数要映射到的存储过程的命名空间限定名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-325">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="dc361-326">存储过程必须在存储模型中声明。</span><span class="sxs-lookup"><span data-stu-id="dc361-326">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="dc361-327">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="dc361-327">**RowsAffectedParameter**</span></span> | <span data-ttu-id="dc361-328">否</span><span class="sxs-lookup"><span data-stu-id="dc361-328">No</span></span>          | <span data-ttu-id="dc361-329">返回受影响行数的输出参数的名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-329">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="dc361-330">示例</span><span class="sxs-lookup"><span data-stu-id="dc361-330">Example</span></span>

<span data-ttu-id="dc361-331">以下示例基于 School 模型，并显示**DeleteFunction**用于的删除函数映射元素**CourseInstructor**关联到**DeleteCourseInstructor**存储过程。</span><span class="sxs-lookup"><span data-stu-id="dc361-331">The following example is based on the School model and shows the **DeleteFunction** element used to map delete function of the **CourseInstructor** association to the **DeleteCourseInstructor** stored procedure.</span></span> <span data-ttu-id="dc361-332">**DeleteCourseInstructor**在存储模型中声明的存储的过程。</span><span class="sxs-lookup"><span data-stu-id="dc361-332">The **DeleteCourseInstructor** stored procedure is declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="endproperty-element-msl"></a><span data-ttu-id="dc361-333">EndProperty 元素 (MSL)</span><span class="sxs-lookup"><span data-stu-id="dc361-333">EndProperty Element (MSL)</span></span>

<span data-ttu-id="dc361-334">**EndProperty**以映射规范语言 （msl） 表示的元素定义结束或修改函数的概念模型关联和基础数据库之间的映射。</span><span class="sxs-lookup"><span data-stu-id="dc361-334">The **EndProperty** element in mapping specification language (MSL) defines the mapping between an end or a modification function of a conceptual model association and the underlying database.</span></span> <span data-ttu-id="dc361-335">子 ScalarProperty 元素中指定的属性列映射。</span><span class="sxs-lookup"><span data-stu-id="dc361-335">The property-column mapping is specified in a child ScalarProperty element.</span></span>

<span data-ttu-id="dc361-336">当**EndProperty**元素用于定义概念模型关联端的映射，它是 AssociationSetMapping 元素的子元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-336">When an **EndProperty** element is used to define the mapping for the end of a conceptual model association, it is a child of an AssociationSetMapping element.</span></span> <span data-ttu-id="dc361-337">当**EndProperty**元素用于定义概念模型关联的修改函数映射，它是 InsertFunction 元素或 DeleteFunction 元素的子级。</span><span class="sxs-lookup"><span data-stu-id="dc361-337">When the **EndProperty** element is used to define the mapping for a modification function of a conceptual model association, it is a child of an InsertFunction element or DeleteFunction element.</span></span>

<span data-ttu-id="dc361-338">**EndProperty**元素可以具有以下子元素：</span><span class="sxs-lookup"><span data-stu-id="dc361-338">The **EndProperty** element can have the following child elements:</span></span>

-   <span data-ttu-id="dc361-339">ScalarProperty （零个或多个）</span><span class="sxs-lookup"><span data-stu-id="dc361-339">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="dc361-340">适用的特性</span><span class="sxs-lookup"><span data-stu-id="dc361-340">Applicable Attributes</span></span>

<span data-ttu-id="dc361-341">下表描述了适用于的特性**EndProperty**元素：</span><span class="sxs-lookup"><span data-stu-id="dc361-341">The following table describes the attributes that are applicable to the **EndProperty** element:</span></span>

| <span data-ttu-id="dc361-342">特性名</span><span class="sxs-lookup"><span data-stu-id="dc361-342">Attribute Name</span></span> | <span data-ttu-id="dc361-343">是否必需</span><span class="sxs-lookup"><span data-stu-id="dc361-343">Is Required</span></span> | <span data-ttu-id="dc361-344">“值”</span><span class="sxs-lookup"><span data-stu-id="dc361-344">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="dc361-345">name</span><span class="sxs-lookup"><span data-stu-id="dc361-345">Name</span></span>           | <span data-ttu-id="dc361-346">是</span><span class="sxs-lookup"><span data-stu-id="dc361-346">Yes</span></span>         | <span data-ttu-id="dc361-347">要映射的关联端的名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-347">The name of the association end that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="dc361-348">示例</span><span class="sxs-lookup"><span data-stu-id="dc361-348">Example</span></span>

<span data-ttu-id="dc361-349">下面的示例演示**AssociationSetMapping**中的元素**FK\_课程\_部门**概念模型中的关联映射到**课程**数据库表中的。</span><span class="sxs-lookup"><span data-stu-id="dc361-349">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="dc361-350">关联类型属性与表列之间的映射指定子级**EndProperty**元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-350">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
 </AssociationSetMapping>
```

### <a name="example"></a><span data-ttu-id="dc361-351">示例</span><span class="sxs-lookup"><span data-stu-id="dc361-351">Example</span></span>

<span data-ttu-id="dc361-352">下面的示例演示**EndProperty**元素关联的插入和删除函数映射 (**CourseInstructor**) 到基础数据库中的存储过程。</span><span class="sxs-lookup"><span data-stu-id="dc361-352">The following example shows the **EndProperty** element mapping the insert and delete functions of an association (**CourseInstructor**) to stored procedures in the underlying database.</span></span> <span data-ttu-id="dc361-353">映射到的函数在存储模型中声明。</span><span class="sxs-lookup"><span data-stu-id="dc361-353">The functions that are mapped to are declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="entitycontainermapping-element-msl"></a><span data-ttu-id="dc361-354">EntityContainerMapping 元素 (MSL)</span><span class="sxs-lookup"><span data-stu-id="dc361-354">EntityContainerMapping Element (MSL)</span></span>

<span data-ttu-id="dc361-355">**EntityContainerMapping**以映射规范语言 （msl） 表示的元素将概念模型中的实体容器映射到存储模型中的实体容器。</span><span class="sxs-lookup"><span data-stu-id="dc361-355">The **EntityContainerMapping** element in mapping specification language (MSL) maps the entity container in the conceptual model to the entity container in the storage model.</span></span> <span data-ttu-id="dc361-356">**EntityContainerMapping**元素是映射元素的子元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-356">The **EntityContainerMapping** element is a child of the Mapping element.</span></span>

<span data-ttu-id="dc361-357">**EntityContainerMapping**元素可以具有下列子元素 （按所列顺序）：</span><span class="sxs-lookup"><span data-stu-id="dc361-357">The **EntityContainerMapping** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="dc361-358">EntitySetMapping （零个或多个）</span><span class="sxs-lookup"><span data-stu-id="dc361-358">EntitySetMapping (zero or more)</span></span>
-   <span data-ttu-id="dc361-359">AssociationSetMapping （零个或多个）</span><span class="sxs-lookup"><span data-stu-id="dc361-359">AssociationSetMapping (zero or more)</span></span>
-   <span data-ttu-id="dc361-360">FunctionImportMapping （零个或多个）</span><span class="sxs-lookup"><span data-stu-id="dc361-360">FunctionImportMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="dc361-361">适用的特性</span><span class="sxs-lookup"><span data-stu-id="dc361-361">Applicable Attributes</span></span>

<span data-ttu-id="dc361-362">下表介绍可应用于的特性**EntityContainerMapping**元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-362">The following table describes the attributes that can be applied to the **EntityContainerMapping** element.</span></span>

| <span data-ttu-id="dc361-363">特性名</span><span class="sxs-lookup"><span data-stu-id="dc361-363">Attribute Name</span></span>            | <span data-ttu-id="dc361-364">是否必需</span><span class="sxs-lookup"><span data-stu-id="dc361-364">Is Required</span></span> | <span data-ttu-id="dc361-365">“值”</span><span class="sxs-lookup"><span data-stu-id="dc361-365">Value</span></span>                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="dc361-366">**StorageModelContainer**</span><span class="sxs-lookup"><span data-stu-id="dc361-366">**StorageModelContainer**</span></span> | <span data-ttu-id="dc361-367">是</span><span class="sxs-lookup"><span data-stu-id="dc361-367">Yes</span></span>         | <span data-ttu-id="dc361-368">要映射到的存储模型实体容器的名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-368">The name of the storage model entity container that is being mapped.</span></span>                                                                                                                                                                                     |
| <span data-ttu-id="dc361-369">**CdmEntityContainer**</span><span class="sxs-lookup"><span data-stu-id="dc361-369">**CdmEntityContainer**</span></span>    | <span data-ttu-id="dc361-370">是</span><span class="sxs-lookup"><span data-stu-id="dc361-370">Yes</span></span>         | <span data-ttu-id="dc361-371">要映射的概念模型实体容器的名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-371">The name of the conceptual model entity container that is being mapped.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="dc361-372">**GenerateUpdateViews**</span><span class="sxs-lookup"><span data-stu-id="dc361-372">**GenerateUpdateViews**</span></span>   | <span data-ttu-id="dc361-373">否</span><span class="sxs-lookup"><span data-stu-id="dc361-373">No</span></span>          | <span data-ttu-id="dc361-374">**True**或**False**。</span><span class="sxs-lookup"><span data-stu-id="dc361-374">**True** or **False**.</span></span> <span data-ttu-id="dc361-375">如果**False**，会生成任何更新视图。</span><span class="sxs-lookup"><span data-stu-id="dc361-375">If **False**, no update views are generated.</span></span> <span data-ttu-id="dc361-376">此属性应设置为**False**后将会无效，因为数据可能会往返不成功的只读映射。</span><span class="sxs-lookup"><span data-stu-id="dc361-376">This attribute should be set to **False** when you have a read-only mapping that would be invalid because data may not round-trip successfully.</span></span> <br/> <span data-ttu-id="dc361-377">默认值是 **，则返回 True**。</span><span class="sxs-lookup"><span data-stu-id="dc361-377">The default value is **True**.</span></span> |

### <a name="example"></a><span data-ttu-id="dc361-378">示例</span><span class="sxs-lookup"><span data-stu-id="dc361-378">Example</span></span>

<span data-ttu-id="dc361-379">下面的示例演示**EntityContainerMapping**映射元素**SchoolModelEntities**容器 （概念模型实体容器） 到**SchoolModelStoreContainer**容器 （存储模型实体容器）：</span><span class="sxs-lookup"><span data-stu-id="dc361-379">The following example shows an **EntityContainerMapping** element that maps the **SchoolModelEntities** container (the conceptual model entity container) to the **SchoolModelStoreContainer** container (the storage model entity container):</span></span>

``` xml
 <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                         CdmEntityContainer="SchoolModelEntities">
   <EntitySetMapping Name="Courses">
     <EntityTypeMapping TypeName="c.Course">
       <MappingFragment StoreEntitySet="Course">
         <ScalarProperty Name="CourseID" ColumnName="CourseID" />
         <ScalarProperty Name="Title" ColumnName="Title" />
         <ScalarProperty Name="Credits" ColumnName="Credits" />
         <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
       </MappingFragment>
     </EntityTypeMapping>
   </EntitySetMapping>
   <EntitySetMapping Name="Departments">
     <EntityTypeMapping TypeName="c.Department">
       <MappingFragment StoreEntitySet="Department">
         <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         <ScalarProperty Name="Name" ColumnName="Name" />
         <ScalarProperty Name="Budget" ColumnName="Budget" />
         <ScalarProperty Name="StartDate" ColumnName="StartDate" />
         <ScalarProperty Name="Administrator" ColumnName="Administrator" />
       </MappingFragment>
     </EntityTypeMapping>
   </EntitySetMapping>
 </EntityContainerMapping>
```

## <a name="entitysetmapping-element-msl"></a><span data-ttu-id="dc361-380">EntitySetMapping 元素 (MSL)</span><span class="sxs-lookup"><span data-stu-id="dc361-380">EntitySetMapping Element (MSL)</span></span>

<span data-ttu-id="dc361-381">**EntitySetMapping**的存储模型中的元素以映射规范语言 (MSL) 映射的概念模型实体中的所有类型都设置为实体集。</span><span class="sxs-lookup"><span data-stu-id="dc361-381">The **EntitySetMapping** element in mapping specification language (MSL) maps all types in a conceptual model entity set to entity sets in the storage model.</span></span> <span data-ttu-id="dc361-382">概念模型中的实体集是为的逻辑容器相同的类型 （和派生的类型） 的实体的实例。</span><span class="sxs-lookup"><span data-stu-id="dc361-382">An entity set in the conceptual model is a logical container for instances of entities of the same type (and derived types).</span></span> <span data-ttu-id="dc361-383">存储模型中的实体集表示表或视图基础数据库中。</span><span class="sxs-lookup"><span data-stu-id="dc361-383">An entity set in the storage model represents a table or view in the underlying database.</span></span> <span data-ttu-id="dc361-384">概念模型实体集指定的值**名称**的属性**EntitySetMapping**元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-384">The conceptual model entity set is specified by the value of the **Name** attribute of the **EntitySetMapping** element.</span></span> <span data-ttu-id="dc361-385">指定映射到的表或视图指定**StoreEntitySet**属性在每个子 MappingFragment 元素中或在**EntitySetMapping**元素本身。</span><span class="sxs-lookup"><span data-stu-id="dc361-385">The mapped-to table or view is specified by the **StoreEntitySet** attribute in each child MappingFragment element or in the **EntitySetMapping** element itself.</span></span>

<span data-ttu-id="dc361-386">**EntitySetMapping**元素可以具有以下子元素：</span><span class="sxs-lookup"><span data-stu-id="dc361-386">The **EntitySetMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="dc361-387">EntityTypeMapping （零个或多个）</span><span class="sxs-lookup"><span data-stu-id="dc361-387">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="dc361-388">QueryView （零个或一个）</span><span class="sxs-lookup"><span data-stu-id="dc361-388">QueryView (zero or one)</span></span>
-   <span data-ttu-id="dc361-389">MappingFragment （零个或多个）</span><span class="sxs-lookup"><span data-stu-id="dc361-389">MappingFragment (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="dc361-390">适用的特性</span><span class="sxs-lookup"><span data-stu-id="dc361-390">Applicable Attributes</span></span>

<span data-ttu-id="dc361-391">下表介绍可应用于的特性**EntitySetMapping**元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-391">The following table describes the attributes that can be applied to the **EntitySetMapping** element.</span></span>

| <span data-ttu-id="dc361-392">特性名</span><span class="sxs-lookup"><span data-stu-id="dc361-392">Attribute Name</span></span>           | <span data-ttu-id="dc361-393">是否必需</span><span class="sxs-lookup"><span data-stu-id="dc361-393">Is Required</span></span> | <span data-ttu-id="dc361-394">“值”</span><span class="sxs-lookup"><span data-stu-id="dc361-394">Value</span></span>                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="dc361-395">**名称**</span><span class="sxs-lookup"><span data-stu-id="dc361-395">**Name**</span></span>                 | <span data-ttu-id="dc361-396">是</span><span class="sxs-lookup"><span data-stu-id="dc361-396">Yes</span></span>         | <span data-ttu-id="dc361-397">要映射的概念模型实体集的名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-397">The name of the conceptual model entity set that is being mapped.</span></span>                                                                                                                                                             |
| <span data-ttu-id="dc361-398">**TypeName** **1**</span><span class="sxs-lookup"><span data-stu-id="dc361-398">**TypeName** **1**</span></span>       | <span data-ttu-id="dc361-399">否</span><span class="sxs-lookup"><span data-stu-id="dc361-399">No</span></span>          | <span data-ttu-id="dc361-400">要映射的概念模型实体类型的名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-400">The name of the conceptual model entity type that is being mapped.</span></span>                                                                                                                                                            |
| <span data-ttu-id="dc361-401">**StoreEntitySet** **1**</span><span class="sxs-lookup"><span data-stu-id="dc361-401">**StoreEntitySet** **1**</span></span> | <span data-ttu-id="dc361-402">否</span><span class="sxs-lookup"><span data-stu-id="dc361-402">No</span></span>          | <span data-ttu-id="dc361-403">要映射到的存储模型实体集的名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-403">The name of the storage model entity set that is being mapped to.</span></span>                                                                                                                                                             |
| <span data-ttu-id="dc361-404">**MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="dc361-404">**MakeColumnsDistinct**</span></span>  | <span data-ttu-id="dc361-405">否</span><span class="sxs-lookup"><span data-stu-id="dc361-405">No</span></span>          | <span data-ttu-id="dc361-406">**True**或**False**具体取决于是否返回仅非重复行。</span><span class="sxs-lookup"><span data-stu-id="dc361-406">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="dc361-407">如果此属性设置为 **，则返回 True**，则**GenerateUpdateViews** EntityContainerMapping 元素的属性必须设置为**False**。</span><span class="sxs-lookup"><span data-stu-id="dc361-407">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

 

<span data-ttu-id="dc361-408">**1** **TypeName**并**StoreEntitySet**属性可用于代替 EntityTypeMapping 和 MappingFragment 子元素将单个实体类型映射到单个表。</span><span class="sxs-lookup"><span data-stu-id="dc361-408">**1** The **TypeName** and **StoreEntitySet** attributes can be used in place of the EntityTypeMapping and MappingFragment child elements to map a single entity type to a single table.</span></span>

### <a name="example"></a><span data-ttu-id="dc361-409">示例</span><span class="sxs-lookup"><span data-stu-id="dc361-409">Example</span></span>

<span data-ttu-id="dc361-410">下面的示例演示**EntitySetMapping**将三种类型 （基类型和两个派生的类型） 映射中的元素**课程**的三个不同表中对概念模型实体集基础数据库。</span><span class="sxs-lookup"><span data-stu-id="dc361-410">The following example shows an **EntitySetMapping** element that maps three types (a base type and two derived types) in the **Courses** entity set of the conceptual model to three different tables in the underlying database.</span></span> <span data-ttu-id="dc361-411">通过指定表**StoreEntitySet**中每个属性**MappingFragment**元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-411">The tables are specified by the **StoreEntitySet** attribute in each **MappingFragment** element.</span></span>

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.Course)">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="Title" ColumnName="Title" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.OnlineCourse)">
     <MappingFragment StoreEntitySet="OnlineCourse">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="URL" ColumnName="URL" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.OnsiteCourse)">
     <MappingFragment StoreEntitySet="OnsiteCourse">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Time" ColumnName="Time" />
       <ScalarProperty Name="Days" ColumnName="Days" />
       <ScalarProperty Name="Location" ColumnName="Location" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="entitytypemapping-element-msl"></a><span data-ttu-id="dc361-412">EntityTypeMapping 元素 (MSL)</span><span class="sxs-lookup"><span data-stu-id="dc361-412">EntityTypeMapping Element (MSL)</span></span>

<span data-ttu-id="dc361-413">**EntityTypeMapping**元素以映射规范语言 （msl） 表示基础数据库中定义的概念模型和表中的实体类型或视图之间的映射。</span><span class="sxs-lookup"><span data-stu-id="dc361-413">The **EntityTypeMapping** element in mapping specification language (MSL) defines the mapping between an entity type in the conceptual model and tables or views in the underlying database.</span></span> <span data-ttu-id="dc361-414">有关概念模型实体类型和基础数据库表或视图的信息，请参阅 EntityType 元素 (CSDL) 和 EntitySet 元素 (SSDL)。</span><span class="sxs-lookup"><span data-stu-id="dc361-414">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="dc361-415">指定要映射的概念模型实体类型**TypeName**的属性**EntityTypeMapping**元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-415">The conceptual model entity type that is being mapped is specified by the **TypeName** attribute of the **EntityTypeMapping** element.</span></span> <span data-ttu-id="dc361-416">指定的表或视图要映射**StoreEntitySet**子 MappingFragment 元素的属性。</span><span class="sxs-lookup"><span data-stu-id="dc361-416">The table or view that is being mapped is specified by the **StoreEntitySet** attribute of the child MappingFragment element.</span></span>

<span data-ttu-id="dc361-417">ModificationFunctionMapping 子元素可用于映射插入、 更新或删除实体类型到数据库中的存储过程的函数。</span><span class="sxs-lookup"><span data-stu-id="dc361-417">The ModificationFunctionMapping child element can be used to map the insert, update, or delete functions of entity types to stored procedures in the database.</span></span>

<span data-ttu-id="dc361-418">**EntityTypeMapping**元素可以具有以下子元素：</span><span class="sxs-lookup"><span data-stu-id="dc361-418">The **EntityTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="dc361-419">MappingFragment （零个或多个）</span><span class="sxs-lookup"><span data-stu-id="dc361-419">MappingFragment (zero or more)</span></span>
-   <span data-ttu-id="dc361-420">ModificationFunctionMapping （零个或一个）</span><span class="sxs-lookup"><span data-stu-id="dc361-420">ModificationFunctionMapping (zero or one)</span></span>
-   <span data-ttu-id="dc361-421">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="dc361-421">ScalarProperty</span></span>
-   <span data-ttu-id="dc361-422">条件</span><span class="sxs-lookup"><span data-stu-id="dc361-422">Condition</span></span>

> [!NOTE]
> <span data-ttu-id="dc361-423">**MappingFragment**并**ModificationFunctionMapping**元素的子元素不能**EntityTypeMapping**在同一时间元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-423">**MappingFragment** and **ModificationFunctionMapping** elements cannot be child elements of the **EntityTypeMapping** element at the same time.</span></span>


> [!NOTE]
> <span data-ttu-id="dc361-424">**ScalarProperty**并**条件**元素只能是子元素的**EntityTypeMapping** FunctionImportMapping 元素内使用时的元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-424">The **ScalarProperty** and **Condition** elements can only be child elements of the **EntityTypeMapping** element when it is used within a FunctionImportMapping element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="dc361-425">适用的特性</span><span class="sxs-lookup"><span data-stu-id="dc361-425">Applicable Attributes</span></span>

<span data-ttu-id="dc361-426">下表介绍可应用于的特性**EntityTypeMapping**元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-426">The following table describes the attributes that can be applied to the **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="dc361-427">特性名</span><span class="sxs-lookup"><span data-stu-id="dc361-427">Attribute Name</span></span> | <span data-ttu-id="dc361-428">是否必需</span><span class="sxs-lookup"><span data-stu-id="dc361-428">Is Required</span></span> | <span data-ttu-id="dc361-429">“值”</span><span class="sxs-lookup"><span data-stu-id="dc361-429">Value</span></span>                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="dc361-430">**类型名称**</span><span class="sxs-lookup"><span data-stu-id="dc361-430">**TypeName**</span></span>   | <span data-ttu-id="dc361-431">是</span><span class="sxs-lookup"><span data-stu-id="dc361-431">Yes</span></span>         | <span data-ttu-id="dc361-432">要映射的概念模型实体类型的命名空间限定的名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-432">The namespace-qualified name of the conceptual model entity type that is being mapped.</span></span> <br/> <span data-ttu-id="dc361-433">如果类型为抽象类型或派生类型，则值必须为 `IsOfType(Namespace-qualified_type_name)`。</span><span class="sxs-lookup"><span data-stu-id="dc361-433">If the type is abstract or a derived type, the value must be `IsOfType(Namespace-qualified_type_name)`.</span></span> |

### <a name="example"></a><span data-ttu-id="dc361-434">示例</span><span class="sxs-lookup"><span data-stu-id="dc361-434">Example</span></span>

<span data-ttu-id="dc361-435">下面的示例演示具有两个子的 EntitySetMapping 元素**EntityTypeMapping**元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-435">The following example shows an EntitySetMapping element with two child **EntityTypeMapping** elements.</span></span> <span data-ttu-id="dc361-436">在第一个**EntityTypeMapping**元素， **SchoolModel.Person**实体类型映射到**人员**表。</span><span class="sxs-lookup"><span data-stu-id="dc361-436">In the first **EntityTypeMapping** element, the **SchoolModel.Person** entity type is mapped to the **Person** table.</span></span> <span data-ttu-id="dc361-437">在第二个**EntityTypeMapping**元素中的更新功能**SchoolModel.Person**类型映射到存储过程**UpdatePerson**，在数据库中.</span><span class="sxs-lookup"><span data-stu-id="dc361-437">In the second **EntityTypeMapping** element, the update functionality of the **SchoolModel.Person** type is mapped to a stored procedure, **UpdatePerson**, in the database.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate" ColumnName="EnrollmentDate" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate" ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a><span data-ttu-id="dc361-438">示例</span><span class="sxs-lookup"><span data-stu-id="dc361-438">Example</span></span>

<span data-ttu-id="dc361-439">下一示例演示其中根类型为抽象类型的类型层次结构的映射。</span><span class="sxs-lookup"><span data-stu-id="dc361-439">The next example shows the mapping of a type hierarchy in which the root type is abstract.</span></span> <span data-ttu-id="dc361-440">请注意，使用`IsOfType`语法**TypeName**属性。</span><span class="sxs-lookup"><span data-stu-id="dc361-440">Note the use of the `IsOfType` syntax for the **TypeName** attributes.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Person)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Instructor)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <Condition ColumnName="HireDate" IsNull="false" />
       <Condition ColumnName="EnrollmentDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Student)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
       <Condition ColumnName="EnrollmentDate" IsNull="false" />
       <Condition ColumnName="HireDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="functionimportmapping-element-msl"></a><span data-ttu-id="dc361-441">FunctionImportMapping 元素 (MSL)</span><span class="sxs-lookup"><span data-stu-id="dc361-441">FunctionImportMapping Element (MSL)</span></span>

<span data-ttu-id="dc361-442">**FunctionImportMapping**元素以映射规范语言 （msl） 表示基础数据库中定义的函数导入在概念模型和存储的过程或函数之间的映射。</span><span class="sxs-lookup"><span data-stu-id="dc361-442">The **FunctionImportMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure or function in the underlying database.</span></span> <span data-ttu-id="dc361-443">函数导入必须在概念模型中进行声明，存储的过程必须在存储模型中进行声明。</span><span class="sxs-lookup"><span data-stu-id="dc361-443">Function imports must be declared in the conceptual model and stored procedures must be declared in the storage model.</span></span> <span data-ttu-id="dc361-444">有关详细信息，请参阅 FunctionImport 元素 (CSDL) 和函数元素 (SSDL)。</span><span class="sxs-lookup"><span data-stu-id="dc361-444">For more information, see FunctionImport Element (CSDL) and Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="dc361-445">默认情况下，如果函数导入返回概念模型实体类型或复杂类型，则基础存储过程返回的列名称必须与概念模型类型中的属性名称完全匹配。</span><span class="sxs-lookup"><span data-stu-id="dc361-445">By default, if a function import returns a conceptual model entity type or complex type, then the names of the columns returned by the underlying stored procedure must exactly match the names of the properties on the conceptual model type.</span></span> <span data-ttu-id="dc361-446">如果列名称与属性名称不完全匹配，则必须 ResultMapping 元素中定义映射。</span><span class="sxs-lookup"><span data-stu-id="dc361-446">If the column names do not exactly match the property names, the mapping must be defined in a ResultMapping element.</span></span>

<span data-ttu-id="dc361-447">**FunctionImportMapping**元素可以具有以下子元素：</span><span class="sxs-lookup"><span data-stu-id="dc361-447">The **FunctionImportMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="dc361-448">ResultMapping （零个或多个）</span><span class="sxs-lookup"><span data-stu-id="dc361-448">ResultMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="dc361-449">适用的特性</span><span class="sxs-lookup"><span data-stu-id="dc361-449">Applicable Attributes</span></span>

<span data-ttu-id="dc361-450">下表描述了适用于的特性**FunctionImportMapping**元素：</span><span class="sxs-lookup"><span data-stu-id="dc361-450">The following table describes the attributes that are applicable to the **FunctionImportMapping** element:</span></span>

| <span data-ttu-id="dc361-451">特性名</span><span class="sxs-lookup"><span data-stu-id="dc361-451">Attribute Name</span></span>         | <span data-ttu-id="dc361-452">是否必需</span><span class="sxs-lookup"><span data-stu-id="dc361-452">Is Required</span></span> | <span data-ttu-id="dc361-453">“值”</span><span class="sxs-lookup"><span data-stu-id="dc361-453">Value</span></span>                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| <span data-ttu-id="dc361-454">**FunctionImportName**</span><span class="sxs-lookup"><span data-stu-id="dc361-454">**FunctionImportName**</span></span> | <span data-ttu-id="dc361-455">是</span><span class="sxs-lookup"><span data-stu-id="dc361-455">Yes</span></span>         | <span data-ttu-id="dc361-456">概念模型中要映射的函数导入的名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-456">The name of the function import in the conceptual model that is being mapped.</span></span>           |
| <span data-ttu-id="dc361-457">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="dc361-457">**FunctionName**</span></span>       | <span data-ttu-id="dc361-458">是</span><span class="sxs-lookup"><span data-stu-id="dc361-458">Yes</span></span>         | <span data-ttu-id="dc361-459">存储模型中要映射的函数的命名空间限定名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-459">The namespace-qualified name of the function in the storage model that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="dc361-460">示例</span><span class="sxs-lookup"><span data-stu-id="dc361-460">Example</span></span>

<span data-ttu-id="dc361-461">下面的示例基于 School 模型。</span><span class="sxs-lookup"><span data-stu-id="dc361-461">The following example is based on the School model.</span></span> <span data-ttu-id="dc361-462">请考虑在存储模型中使用以下函数：</span><span class="sxs-lookup"><span data-stu-id="dc361-462">Consider the following function in the storage model:</span></span>

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

<span data-ttu-id="dc361-463">另请考虑在概念模型中使用此函数导入：</span><span class="sxs-lookup"><span data-stu-id="dc361-463">Also consider this function import in the conceptual model:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

<span data-ttu-id="dc361-464">下面的示例演示**FunctionImportMapping**元素用于映射函数和函数导入上面彼此：</span><span class="sxs-lookup"><span data-stu-id="dc361-464">The following example show a **FunctionImportMapping** element used to map the function and function import above to each other:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a><span data-ttu-id="dc361-465">InsertFunction 元素 (MSL)</span><span class="sxs-lookup"><span data-stu-id="dc361-465">InsertFunction Element (MSL)</span></span>

<span data-ttu-id="dc361-466">**InsertFunction**以映射规范语言 （msl） 表示的元素将概念模型中的实体类型或关联的插入函数映射到基础数据库中的存储过程。</span><span class="sxs-lookup"><span data-stu-id="dc361-466">The **InsertFunction** element in mapping specification language (MSL) maps the insert function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="dc361-467">将修改函数映射到其中的存储过程必须在存储模型中声明。</span><span class="sxs-lookup"><span data-stu-id="dc361-467">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="dc361-468">有关详细信息，请参阅函数元素 (SSDL)。</span><span class="sxs-lookup"><span data-stu-id="dc361-468">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="dc361-469">如果不将所有这三个插入、 更新或删除一个实体类型到存储过程的操作、 映射的操作将失败，则在运行时执行和 UpdateException 引发。</span><span class="sxs-lookup"><span data-stu-id="dc361-469">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="dc361-470">**InsertFunction**元素可以是 ModificationFunctionMapping 元素的子元素，并应用于 EntityTypeMapping 元素或 AssociationSetMapping 元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-470">The **InsertFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

### <a name="insertfunction-applied-to-entitytypemapping"></a><span data-ttu-id="dc361-471">应用于 EntityTypeMapping 的 InsertFunction</span><span class="sxs-lookup"><span data-stu-id="dc361-471">InsertFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="dc361-472">当应用于 EntityTypeMapping 元素**InsertFunction**元素将概念模型中的实体类型的插入函数映射到存储过程。</span><span class="sxs-lookup"><span data-stu-id="dc361-472">When applied to the EntityTypeMapping element, the **InsertFunction** element maps the insert function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="dc361-473">**InsertFunction**元素可以具有以下子元素时应用于**EntityTypeMapping**元素：</span><span class="sxs-lookup"><span data-stu-id="dc361-473">The **InsertFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="dc361-474">AssociationEnd （零个或多个）</span><span class="sxs-lookup"><span data-stu-id="dc361-474">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="dc361-475">ComplexProperty （零个或多个）</span><span class="sxs-lookup"><span data-stu-id="dc361-475">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="dc361-476">ResultBinding （零个或一个）</span><span class="sxs-lookup"><span data-stu-id="dc361-476">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="dc361-477">ScarlarProperty （零个或多个）</span><span class="sxs-lookup"><span data-stu-id="dc361-477">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="dc361-478">适用的特性</span><span class="sxs-lookup"><span data-stu-id="dc361-478">Applicable Attributes</span></span>

<span data-ttu-id="dc361-479">下表介绍可应用于的特性**InsertFunction**元素时应用于**EntityTypeMapping**元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-479">The following table describes the attributes that can be applied to the **InsertFunction** element when applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="dc361-480">特性名</span><span class="sxs-lookup"><span data-stu-id="dc361-480">Attribute Name</span></span>            | <span data-ttu-id="dc361-481">是否必需</span><span class="sxs-lookup"><span data-stu-id="dc361-481">Is Required</span></span> | <span data-ttu-id="dc361-482">“值”</span><span class="sxs-lookup"><span data-stu-id="dc361-482">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="dc361-483">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="dc361-483">**FunctionName**</span></span>          | <span data-ttu-id="dc361-484">是</span><span class="sxs-lookup"><span data-stu-id="dc361-484">Yes</span></span>         | <span data-ttu-id="dc361-485">插入函数要映射到的存储过程的命名空间限定名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-485">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="dc361-486">存储过程必须在存储模型中声明。</span><span class="sxs-lookup"><span data-stu-id="dc361-486">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="dc361-487">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="dc361-487">**RowsAffectedParameter**</span></span> | <span data-ttu-id="dc361-488">否</span><span class="sxs-lookup"><span data-stu-id="dc361-488">No</span></span>          | <span data-ttu-id="dc361-489">返回受影响行数的输出参数的名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-489">The name of the output parameter that returns the number of affected rows.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="dc361-490">示例</span><span class="sxs-lookup"><span data-stu-id="dc361-490">Example</span></span>

<span data-ttu-id="dc361-491">以下示例基于 School 模型，并显示**InsertFunction**元素用于将 Person 实体类型的插入函数映射**InsertPerson**存储过程。</span><span class="sxs-lookup"><span data-stu-id="dc361-491">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the Person entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="dc361-492">**InsertPerson**在存储模型中声明的存储的过程。</span><span class="sxs-lookup"><span data-stu-id="dc361-492">The **InsertPerson** stored procedure is declared in the storage model.</span></span>

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```
### <a name="insertfunction-applied-to-associationsetmapping"></a><span data-ttu-id="dc361-493">应用于 AssociationSetMapping 的 InsertFunction</span><span class="sxs-lookup"><span data-stu-id="dc361-493">InsertFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="dc361-494">当应用于 AssociationSetMapping 元素**InsertFunction**元素将概念模型中的关联的插入函数映射到存储过程。</span><span class="sxs-lookup"><span data-stu-id="dc361-494">When applied to the AssociationSetMapping element, the **InsertFunction** element maps the insert function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="dc361-495">**InsertFunction**元素可以具有以下子元素时应用于**AssociationSetMapping**元素：</span><span class="sxs-lookup"><span data-stu-id="dc361-495">The **InsertFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="dc361-496">EndProperty</span><span class="sxs-lookup"><span data-stu-id="dc361-496">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="dc361-497">适用的特性</span><span class="sxs-lookup"><span data-stu-id="dc361-497">Applicable Attributes</span></span>

<span data-ttu-id="dc361-498">下表介绍可应用于的特性**InsertFunction**时应用于元素**AssociationSetMapping**元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-498">The following table describes the attributes that can be applied to the **InsertFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="dc361-499">特性名</span><span class="sxs-lookup"><span data-stu-id="dc361-499">Attribute Name</span></span>            | <span data-ttu-id="dc361-500">是否必需</span><span class="sxs-lookup"><span data-stu-id="dc361-500">Is Required</span></span> | <span data-ttu-id="dc361-501">“值”</span><span class="sxs-lookup"><span data-stu-id="dc361-501">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="dc361-502">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="dc361-502">**FunctionName**</span></span>          | <span data-ttu-id="dc361-503">是</span><span class="sxs-lookup"><span data-stu-id="dc361-503">Yes</span></span>         | <span data-ttu-id="dc361-504">插入函数要映射到的存储过程的命名空间限定名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-504">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="dc361-505">存储过程必须在存储模型中声明。</span><span class="sxs-lookup"><span data-stu-id="dc361-505">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="dc361-506">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="dc361-506">**RowsAffectedParameter**</span></span> | <span data-ttu-id="dc361-507">否</span><span class="sxs-lookup"><span data-stu-id="dc361-507">No</span></span>          | <span data-ttu-id="dc361-508">返回受影响行数的输出参数的名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-508">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="dc361-509">示例</span><span class="sxs-lookup"><span data-stu-id="dc361-509">Example</span></span>

<span data-ttu-id="dc361-510">以下示例基于 School 模型，并显示**InsertFunction**元素使用的插入函数映射**CourseInstructor**关联到**InsertCourseInstructor**存储过程。</span><span class="sxs-lookup"><span data-stu-id="dc361-510">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the **CourseInstructor** association to the **InsertCourseInstructor** stored procedure.</span></span> <span data-ttu-id="dc361-511">**InsertCourseInstructor**在存储模型中声明的存储的过程。</span><span class="sxs-lookup"><span data-stu-id="dc361-511">The **InsertCourseInstructor** stored procedure is declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="mapping-element-msl"></a><span data-ttu-id="dc361-512">Mapping 元素 (MSL)</span><span class="sxs-lookup"><span data-stu-id="dc361-512">Mapping Element (MSL)</span></span>

<span data-ttu-id="dc361-513">**映射**以映射规范语言 （msl） 表示的元素包含对象 （如存储模型中所述） 到数据库的概念模型中定义的映射的信息。</span><span class="sxs-lookup"><span data-stu-id="dc361-513">The **Mapping** element in mapping specification language (MSL) contains information for mapping objects that are defined in a conceptual model to a database (as described in a storage model).</span></span> <span data-ttu-id="dc361-514">有关详细信息，请参阅 CSDL 规范和 SSDL 规范。</span><span class="sxs-lookup"><span data-stu-id="dc361-514">For more information, see CSDL Specification and SSDL Specification.</span></span>

<span data-ttu-id="dc361-515">**映射**元素是映射规范的根元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-515">The **Mapping** element is the root element for a mapping specification.</span></span> <span data-ttu-id="dc361-516">映射规范的 XML 命名空间是 http://schemas.microsoft.com/ado/2009/11/mapping/cs 。</span><span class="sxs-lookup"><span data-stu-id="dc361-516">The XML namespace for mapping specifications is http://schemas.microsoft.com/ado/2009/11/mapping/cs.</span></span>

<span data-ttu-id="dc361-517">映射元素可以具有以下子元素（按所列顺序）：</span><span class="sxs-lookup"><span data-stu-id="dc361-517">The mapping element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="dc361-518">别名 （零个或多个）</span><span class="sxs-lookup"><span data-stu-id="dc361-518">Alias (zero or more)</span></span>
-   <span data-ttu-id="dc361-519">EntityContainerMapping （恰好一个）</span><span class="sxs-lookup"><span data-stu-id="dc361-519">EntityContainerMapping (exactly one)</span></span>

<span data-ttu-id="dc361-520">MSL 中引用的概念模型类型和存储模型类型的名称必须由其相应的命名空间名称限定。</span><span class="sxs-lookup"><span data-stu-id="dc361-520">Names of conceptual and storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="dc361-521">有关概念模型命名空间名称的信息，请参阅架构元素 (CSDL)。</span><span class="sxs-lookup"><span data-stu-id="dc361-521">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="dc361-522">有关存储模型命名空间名称的信息，请参阅架构元素 (SSDL)。</span><span class="sxs-lookup"><span data-stu-id="dc361-522">For information about the storage model namespace name, see Schema Element (SSDL).</span></span> <span data-ttu-id="dc361-523">可以使用别名元素定义 MSL 中使用的命名空间的别名。</span><span class="sxs-lookup"><span data-stu-id="dc361-523">Aliases for namespaces that are used in MSL can be defined with the Alias element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="dc361-524">适用的特性</span><span class="sxs-lookup"><span data-stu-id="dc361-524">Applicable Attributes</span></span>

<span data-ttu-id="dc361-525">下表描述了可应用于的特性**映射**元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-525">The table below describes the attributes that can be applied to the **Mapping** element.</span></span>

| <span data-ttu-id="dc361-526">特性名</span><span class="sxs-lookup"><span data-stu-id="dc361-526">Attribute Name</span></span> | <span data-ttu-id="dc361-527">是否必需</span><span class="sxs-lookup"><span data-stu-id="dc361-527">Is Required</span></span> | <span data-ttu-id="dc361-528">“值”</span><span class="sxs-lookup"><span data-stu-id="dc361-528">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="dc361-529">空格键</span><span class="sxs-lookup"><span data-stu-id="dc361-529">**Space**</span></span>      | <span data-ttu-id="dc361-530">是</span><span class="sxs-lookup"><span data-stu-id="dc361-530">Yes</span></span>         | <span data-ttu-id="dc361-531">**C S**。</span><span class="sxs-lookup"><span data-stu-id="dc361-531">**C-S**.</span></span> <span data-ttu-id="dc361-532">这是固定值，因此不能更改。</span><span class="sxs-lookup"><span data-stu-id="dc361-532">This is a fixed value and cannot be changed.</span></span> |

### <a name="example"></a><span data-ttu-id="dc361-533">示例</span><span class="sxs-lookup"><span data-stu-id="dc361-533">Example</span></span>

<span data-ttu-id="dc361-534">下面的示例演示**映射**基于 School 模型的一部分的元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-534">The following example shows a **Mapping** element that is based on part of the School model.</span></span> <span data-ttu-id="dc361-535">有关 School 模型的详细信息，请参阅快速入门 （实体框架）：</span><span class="sxs-lookup"><span data-stu-id="dc361-535">For more information about the School model, see Quickstart (Entity Framework):</span></span>

``` xml
 <Mapping Space="C-S"
          xmlns="http://schemas.microsoft.com/ado/2009/11/mapping/cs">
   <Alias Key="c" Value="SchoolModel"/>
   <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                           CdmEntityContainer="SchoolModelEntities">
     <EntitySetMapping Name="Courses">
       <EntityTypeMapping TypeName="c.Course">
         <MappingFragment StoreEntitySet="Course">
           <ScalarProperty Name="CourseID" ColumnName="CourseID" />
           <ScalarProperty Name="Title" ColumnName="Title" />
           <ScalarProperty Name="Credits" ColumnName="Credits" />
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
     <EntitySetMapping Name="Departments">
       <EntityTypeMapping TypeName="c.Department">
         <MappingFragment StoreEntitySet="Department">
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
           <ScalarProperty Name="Name" ColumnName="Name" />
           <ScalarProperty Name="Budget" ColumnName="Budget" />
           <ScalarProperty Name="StartDate" ColumnName="StartDate" />
           <ScalarProperty Name="Administrator" ColumnName="Administrator" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
   </EntityContainerMapping>
 </Mapping>
```

## <a name="mappingfragment-element-msl"></a><span data-ttu-id="dc361-536">MappingFragment 元素 (MSL)</span><span class="sxs-lookup"><span data-stu-id="dc361-536">MappingFragment Element (MSL)</span></span>

<span data-ttu-id="dc361-537">**MappingFragment**以映射规范语言 （msl） 表示的元素定义概念模型实体类型的表或视图的数据库中的属性之间的映射。</span><span class="sxs-lookup"><span data-stu-id="dc361-537">The **MappingFragment** element in mapping specification language (MSL) defines the mapping between the properties of a conceptual model entity type and a table or view in the database.</span></span> <span data-ttu-id="dc361-538">有关概念模型实体类型和基础数据库表或视图的信息，请参阅 EntityType 元素 (CSDL) 和 EntitySet 元素 (SSDL)。</span><span class="sxs-lookup"><span data-stu-id="dc361-538">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="dc361-539">**MappingFragment**可以是 EntityTypeMapping 元素或 EntitySetMapping 元素的子元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-539">The **MappingFragment** can be a child element of the EntityTypeMapping element or the EntitySetMapping element.</span></span>

<span data-ttu-id="dc361-540">**MappingFragment**元素可以具有以下子元素：</span><span class="sxs-lookup"><span data-stu-id="dc361-540">The **MappingFragment** element can have the following child elements:</span></span>

-   <span data-ttu-id="dc361-541">复杂类型 （零个或多个）</span><span class="sxs-lookup"><span data-stu-id="dc361-541">ComplexType (zero or more)</span></span>
-   <span data-ttu-id="dc361-542">ScalarProperty （零个或多个）</span><span class="sxs-lookup"><span data-stu-id="dc361-542">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="dc361-543">条件 （零个或多个）</span><span class="sxs-lookup"><span data-stu-id="dc361-543">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="dc361-544">适用的特性</span><span class="sxs-lookup"><span data-stu-id="dc361-544">Applicable Attributes</span></span>

<span data-ttu-id="dc361-545">下表介绍可应用于的特性**MappingFragment**元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-545">The following table describes the attributes that can be applied to the **MappingFragment** element.</span></span>

| <span data-ttu-id="dc361-546">特性名</span><span class="sxs-lookup"><span data-stu-id="dc361-546">Attribute Name</span></span>          | <span data-ttu-id="dc361-547">是否必需</span><span class="sxs-lookup"><span data-stu-id="dc361-547">Is Required</span></span> | <span data-ttu-id="dc361-548">“值”</span><span class="sxs-lookup"><span data-stu-id="dc361-548">Value</span></span>                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="dc361-549">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="dc361-549">**StoreEntitySet**</span></span>      | <span data-ttu-id="dc361-550">是</span><span class="sxs-lookup"><span data-stu-id="dc361-550">Yes</span></span>         | <span data-ttu-id="dc361-551">要映射的表或视图的名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-551">The name of the table or view that is being mapped.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="dc361-552">**MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="dc361-552">**MakeColumnsDistinct**</span></span> | <span data-ttu-id="dc361-553">否</span><span class="sxs-lookup"><span data-stu-id="dc361-553">No</span></span>          | <span data-ttu-id="dc361-554">**True**或**False**具体取决于是否返回仅非重复行。</span><span class="sxs-lookup"><span data-stu-id="dc361-554">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="dc361-555">如果此属性设置为 **，则返回 True**，则**GenerateUpdateViews** EntityContainerMapping 元素的属性必须设置为**False**。</span><span class="sxs-lookup"><span data-stu-id="dc361-555">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

### <a name="example"></a><span data-ttu-id="dc361-556">示例</span><span class="sxs-lookup"><span data-stu-id="dc361-556">Example</span></span>

<span data-ttu-id="dc361-557">下面的示例演示**MappingFragment**元素的子级作为**EntityTypeMapping**元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-557">The following example shows a **MappingFragment** element as the child of an **EntityTypeMapping** element.</span></span> <span data-ttu-id="dc361-558">在此示例中，属性**课程**概念模型中的类型映射到的列**课程**数据库表中的。</span><span class="sxs-lookup"><span data-stu-id="dc361-558">In this example, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a><span data-ttu-id="dc361-559">示例</span><span class="sxs-lookup"><span data-stu-id="dc361-559">Example</span></span>

<span data-ttu-id="dc361-560">下面的示例演示**MappingFragment**元素的子级作为**EntitySetMapping**元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-560">The following example shows a **MappingFragment** element as the child of an **EntitySetMapping** element.</span></span> <span data-ttu-id="dc361-561">如上面的属性的示例中所示**课程**概念模型中的类型映射到的列**课程**数据库表中的。</span><span class="sxs-lookup"><span data-stu-id="dc361-561">As in the example above, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

``` xml
 <EntitySetMapping Name="Courses" TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
     </MappingFragment>
 </EntitySetMapping>
```

## <a name="modificationfunctionmapping-element-msl"></a><span data-ttu-id="dc361-562">ModificationFunctionMapping 元素 (MSL)</span><span class="sxs-lookup"><span data-stu-id="dc361-562">ModificationFunctionMapping Element (MSL)</span></span>

<span data-ttu-id="dc361-563">**ModificationFunctionMapping**以映射规范语言 （msl） 表示的元素映射插入、 更新和删除函数的概念模型实体类型到基础数据库中的存储过程。</span><span class="sxs-lookup"><span data-stu-id="dc361-563">The **ModificationFunctionMapping** element in mapping specification language (MSL) maps the insert, update, and delete functions of a conceptual model entity type to stored procedures in the underlying database.</span></span> <span data-ttu-id="dc361-564">**ModificationFunctionMapping**元素还可以将映射插入和删除到基础数据库中的存储过程概念模型中的多对多关联的函数。</span><span class="sxs-lookup"><span data-stu-id="dc361-564">The **ModificationFunctionMapping** element can also map the insert and delete functions for many-to-many associations in the conceptual model to stored procedures in the underlying database.</span></span> <span data-ttu-id="dc361-565">将修改函数映射到其中的存储过程必须在存储模型中声明。</span><span class="sxs-lookup"><span data-stu-id="dc361-565">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="dc361-566">有关详细信息，请参阅函数元素 (SSDL)。</span><span class="sxs-lookup"><span data-stu-id="dc361-566">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="dc361-567">如果不将所有这三个插入、 更新或删除一个实体类型到存储过程的操作、 映射的操作将失败，则在运行时执行和 UpdateException 引发。</span><span class="sxs-lookup"><span data-stu-id="dc361-567">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>


> [!NOTE]
> <span data-ttu-id="dc361-568">如果将继承层次结构中的一个实体的修改函数映射到存储过程，则必须将该层次结构中所有类型的修改函数都映射到存储过程。</span><span class="sxs-lookup"><span data-stu-id="dc361-568">If the modification functions for one entity in an inheritance hierarchy are mapped to stored procedures, then modification functions for all types in the hierarchy must be mapped to stored procedures.</span></span>

<span data-ttu-id="dc361-569">**ModificationFunctionMapping**元素可以是 EntityTypeMapping 元素或 AssociationSetMapping 元素的子级。</span><span class="sxs-lookup"><span data-stu-id="dc361-569">The **ModificationFunctionMapping** element can be a child of the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

<span data-ttu-id="dc361-570">**ModificationFunctionMapping**元素可以具有以下子元素：</span><span class="sxs-lookup"><span data-stu-id="dc361-570">The **ModificationFunctionMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="dc361-571">DeleteFunction （零个或一个）</span><span class="sxs-lookup"><span data-stu-id="dc361-571">DeleteFunction (zero or one)</span></span>
-   <span data-ttu-id="dc361-572">InsertFunction （零个或一个）</span><span class="sxs-lookup"><span data-stu-id="dc361-572">InsertFunction (zero or one)</span></span>
-   <span data-ttu-id="dc361-573">UpdateFunction （零个或一个）</span><span class="sxs-lookup"><span data-stu-id="dc361-573">UpdateFunction (zero or one)</span></span>

<span data-ttu-id="dc361-574">任何属性都适用于**ModificationFunctionMapping**元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-574">No attributes are applicable to the **ModificationFunctionMapping** element.</span></span>

### <a name="example"></a><span data-ttu-id="dc361-575">示例</span><span class="sxs-lookup"><span data-stu-id="dc361-575">Example</span></span>

<span data-ttu-id="dc361-576">下面的示例演示实体集映射**人**School 模型中实体集。</span><span class="sxs-lookup"><span data-stu-id="dc361-576">The following example shows the entity set mapping for the **People** entity set in the School model.</span></span> <span data-ttu-id="dc361-577">除了的列映射**Person**实体类型的映射的插入、 更新和删除函数**人员**显示类型。</span><span class="sxs-lookup"><span data-stu-id="dc361-577">In addition to the column mapping for the **Person** entity type, the mapping of the insert, update, and delete functions of the **Person** type are shown.</span></span> <span data-ttu-id="dc361-578">映射到的函数在存储模型中声明。</span><span class="sxs-lookup"><span data-stu-id="dc361-578">The functions that are mapped to are declared in the storage model.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a><span data-ttu-id="dc361-579">示例</span><span class="sxs-lookup"><span data-stu-id="dc361-579">Example</span></span>

<span data-ttu-id="dc361-580">下面的示例演示关联集映射**CourseInstructor** School 模型中关联集。</span><span class="sxs-lookup"><span data-stu-id="dc361-580">The following example shows the association set mapping for the **CourseInstructor** association set in the School model.</span></span> <span data-ttu-id="dc361-581">除了的列映射**CourseInstructor**关联的插入和删除函数映射**CourseInstructor**关联显示。</span><span class="sxs-lookup"><span data-stu-id="dc361-581">In addition to the column mapping for the **CourseInstructor** association, the mapping of the insert and delete functions of the **CourseInstructor** association are shown.</span></span> <span data-ttu-id="dc361-582">映射到的函数在存储模型中声明。</span><span class="sxs-lookup"><span data-stu-id="dc361-582">The functions that are mapped to are declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```
 

 

## <a name="queryview-element-msl"></a><span data-ttu-id="dc361-583">QueryView 元素 (MSL)</span><span class="sxs-lookup"><span data-stu-id="dc361-583">QueryView Element (MSL)</span></span>

<span data-ttu-id="dc361-584">**QueryView**以映射规范语言 （msl） 表示的元素定义概念模型和基础数据库中的表中之间对实体类型或关联的只读映射。</span><span class="sxs-lookup"><span data-stu-id="dc361-584">The **QueryView** element in mapping specification language (MSL) defines a read-only mapping between an entity type or association in the conceptual model and a table in the underlying database.</span></span> <span data-ttu-id="dc361-585">使用针对存储模型进行评估的 Entity SQL 查询定义映射，将结果集的实体或关联在概念模型中的表示。</span><span class="sxs-lookup"><span data-stu-id="dc361-585">The mapping is defined with an Entity SQL query that is evaluated against the storage model, and you express the result set in terms of an entity or association in the conceptual model.</span></span> <span data-ttu-id="dc361-586">因为查询视图是只读的，所以不能使用标准更新命令来更新查询视图所定义的类型。</span><span class="sxs-lookup"><span data-stu-id="dc361-586">Because query views are read-only, you cannot use standard update commands to update types that are defined by query views.</span></span> <span data-ttu-id="dc361-587">可以使用修改函数来更新这些类型。</span><span class="sxs-lookup"><span data-stu-id="dc361-587">You can make updates to these types by using modification functions.</span></span> <span data-ttu-id="dc361-588">有关详细信息，请参阅如何： 将修改函数映射到存储过程。</span><span class="sxs-lookup"><span data-stu-id="dc361-588">For more information, see How to: Map Modification Functions to Stored Procedures.</span></span>

> [!NOTE]
> <span data-ttu-id="dc361-589">在中**QueryView**元素中包含的 Entity SQL 表达式**GroupBy**，不支持组聚合或导航属性。</span><span class="sxs-lookup"><span data-stu-id="dc361-589">In the **QueryView** element, Entity SQL expressions that contain **GroupBy**, group aggregates, or navigation properties are not supported.</span></span>

 

<span data-ttu-id="dc361-590">**QueryView**元素可以是 EntitySetMapping 元素或 AssociationSetMapping 元素的子级。</span><span class="sxs-lookup"><span data-stu-id="dc361-590">The **QueryView** element can be a child of the EntitySetMapping element or the AssociationSetMapping element.</span></span> <span data-ttu-id="dc361-591">在前一种情况中，查询视图定义概念模型中实体的只读映射。</span><span class="sxs-lookup"><span data-stu-id="dc361-591">In the former case, the query view defines a read-only mapping for an entity in the conceptual model.</span></span> <span data-ttu-id="dc361-592">在后一种情况中，查询视图定义概念模型中关联的只读映射。</span><span class="sxs-lookup"><span data-stu-id="dc361-592">In the latter case, the query view defines a read-only mapping for an association in the conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="dc361-593">如果**AssociationSetMapping**元素不用于与引用约束的关联**AssociationSetMapping**元素将被忽略。</span><span class="sxs-lookup"><span data-stu-id="dc361-593">If the **AssociationSetMapping** element is for an association with a referential constraint, the **AssociationSetMapping** element is ignored.</span></span> <span data-ttu-id="dc361-594">有关详细信息，请参阅 ReferentialConstraint 元素 (CSDL)。</span><span class="sxs-lookup"><span data-stu-id="dc361-594">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="dc361-595">**QueryView**元素不能具有任何子元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-595">The **QueryView** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="dc361-596">适用的特性</span><span class="sxs-lookup"><span data-stu-id="dc361-596">Applicable Attributes</span></span>

<span data-ttu-id="dc361-597">下表介绍可应用于的特性**QueryView**元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-597">The following table describes the attributes that can be applied to the **QueryView** element.</span></span>

| <span data-ttu-id="dc361-598">特性名</span><span class="sxs-lookup"><span data-stu-id="dc361-598">Attribute Name</span></span> | <span data-ttu-id="dc361-599">是否必需</span><span class="sxs-lookup"><span data-stu-id="dc361-599">Is Required</span></span> | <span data-ttu-id="dc361-600">“值”</span><span class="sxs-lookup"><span data-stu-id="dc361-600">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="dc361-601">**类型名称**</span><span class="sxs-lookup"><span data-stu-id="dc361-601">**TypeName**</span></span>   | <span data-ttu-id="dc361-602">否</span><span class="sxs-lookup"><span data-stu-id="dc361-602">No</span></span>          | <span data-ttu-id="dc361-603">要由查询视图映射的概念模型类型的名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-603">The name of the conceptual model type that is being mapped by the query view.</span></span> |

### <a name="example"></a><span data-ttu-id="dc361-604">示例</span><span class="sxs-lookup"><span data-stu-id="dc361-604">Example</span></span>

<span data-ttu-id="dc361-605">下面的示例演示**QueryView**的子元素**EntitySetMapping**元素，并定义查询视图映射**部门**中的实体类型School 模型。</span><span class="sxs-lookup"><span data-stu-id="dc361-605">The following example shows the **QueryView** element as a child of the **EntitySetMapping** element and defines a query view mapping for the **Department** entity type in the School Model.</span></span>

``` xml
 <EntitySetMapping Name="Departments" >
   <QueryView>
     SELECT VALUE SchoolModel.Department(d.DepartmentID,
                                         d.Name,
                                         d.Budget,
                                         d.StartDate)
     FROM SchoolModelStoreContainer.Department AS d
     WHERE d.Budget > 150000
   </QueryView>
 </EntitySetMapping>
```

<span data-ttu-id="dc361-606">因为该查询仅返回的成员的子集**部门**在存储模型中，类型**部门**School 模型中的类型已被修改，基于此映射，如下所示：</span><span class="sxs-lookup"><span data-stu-id="dc361-606">Because the query only returns a subset of the members of the **Department** type in the storage model, the **Department** type in the School model has been modified based on this mapping as follows:</span></span>

``` xml
 <EntityType Name="Department">
   <Key>
     <PropertyRef Name="DepartmentID" />
   </Key>
   <Property Type="Int32" Name="DepartmentID" Nullable="false" />
   <Property Type="String" Name="Name" Nullable="false"
             MaxLength="50" FixedLength="false" Unicode="true" />
   <Property Type="Decimal" Name="Budget" Nullable="false"
             Precision="19" Scale="4" />
   <Property Type="DateTime" Name="StartDate" Nullable="false" />
   <NavigationProperty Name="Courses"
                       Relationship="SchoolModel.FK_Course_Department"
                       FromRole="Department" ToRole="Course" />
 </EntityType>
```

### <a name="example"></a><span data-ttu-id="dc361-607">示例</span><span class="sxs-lookup"><span data-stu-id="dc361-607">Example</span></span>

<span data-ttu-id="dc361-608">下面的示例说明**QueryView**元素的子级作为**AssociationSetMapping**元素和定义的只读映射`FK_Course_Department`School 模型中的关联。</span><span class="sxs-lookup"><span data-stu-id="dc361-608">The next example shows the **QueryView** element as the child of an **AssociationSetMapping** element and defines a read-only mapping for the `FK_Course_Department` association in the School model.</span></span>

``` xml
 <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                         CdmEntityContainer="SchoolEntities">
   <EntitySetMapping Name="Courses" >
     <QueryView>
       SELECT VALUE SchoolModel.Course(c.CourseID,
                                       c.Title,
                                       c.Credits)
       FROM SchoolModelStoreContainer.Course AS c
     </QueryView>
   </EntitySetMapping>
   <EntitySetMapping Name="Departments" >
     <QueryView>
       SELECT VALUE SchoolModel.Department(d.DepartmentID,
                                           d.Name,
                                           d.Budget,
                                           d.StartDate)
       FROM SchoolModelStoreContainer.Department AS d
       WHERE d.Budget > 150000
     </QueryView>
   </EntitySetMapping>
   <AssociationSetMapping Name="FK_Course_Department" >
     <QueryView>
       SELECT VALUE SchoolModel.FK_Course_Department(
         CREATEREF(SchoolEntities.Departments, row(c.DepartmentID), SchoolModel.Department),
         CREATEREF(SchoolEntities.Courses, row(c.CourseID)) )
       FROM SchoolModelStoreContainer.Course AS c
     </QueryView>
   </AssociationSetMapping>
 </EntityContainerMapping>
```
 
### <a name="comments"></a><span data-ttu-id="dc361-609">注释</span><span class="sxs-lookup"><span data-stu-id="dc361-609">Comments</span></span>

<span data-ttu-id="dc361-610">可以定义查询视图来实现以下方案：</span><span class="sxs-lookup"><span data-stu-id="dc361-610">You can define query views to enable the following scenarios:</span></span>

-   <span data-ttu-id="dc361-611">在概念模型中定义一个实体，该实体不包含存储模型中的实体的所有属性。</span><span class="sxs-lookup"><span data-stu-id="dc361-611">Define an entity in the conceptual model that doesn't include all the properties of the entity in the storage model.</span></span> <span data-ttu-id="dc361-612">这包括不具有默认值并且不支持的属性**null**值。</span><span class="sxs-lookup"><span data-stu-id="dc361-612">This includes properties that do not have default values and do not support **null** values.</span></span>
-   <span data-ttu-id="dc361-613">将存储模型中计算的列映射到概念模型中实体类型的属性。</span><span class="sxs-lookup"><span data-stu-id="dc361-613">Map computed columns in the storage model to properties of entity types in the conceptual model.</span></span>
-   <span data-ttu-id="dc361-614">定义一个映射，其中用于对概念模型中的实体进行分区的条件不基于相等性。</span><span class="sxs-lookup"><span data-stu-id="dc361-614">Define a mapping where conditions used to partition entities in the conceptual model are not based on equality.</span></span> <span data-ttu-id="dc361-615">当指定条件映射使用**条件**元素，提供的条件必须等于指定的值。</span><span class="sxs-lookup"><span data-stu-id="dc361-615">When you specify a conditional mapping using the **Condition** element, the supplied condition must equal the specified value.</span></span> <span data-ttu-id="dc361-616">有关详细信息，请参阅条件元素 (MSL)。</span><span class="sxs-lookup"><span data-stu-id="dc361-616">For more information, see Condition Element (MSL).</span></span>
-   <span data-ttu-id="dc361-617">将存储模型中的同一列映射到概念模型中的多个类型。</span><span class="sxs-lookup"><span data-stu-id="dc361-617">Map the same column in the storage model to multiple types in the conceptual model.</span></span>
-   <span data-ttu-id="dc361-618">将多个类型映射到同一个表。</span><span class="sxs-lookup"><span data-stu-id="dc361-618">Map multiple types to the same table.</span></span>
-   <span data-ttu-id="dc361-619">在概念模型中定义不基于关系架构中的外键的关联。</span><span class="sxs-lookup"><span data-stu-id="dc361-619">Define associations in the conceptual model that are not based on foreign keys in the relational schema.</span></span>
-   <span data-ttu-id="dc361-620">使用自定义业务逻辑设置概念模型中的属性值。</span><span class="sxs-lookup"><span data-stu-id="dc361-620">Use custom business logic to set the value of properties in the conceptual model.</span></span> <span data-ttu-id="dc361-621">例如，你可以将字符串值"T"的值的数据源中的映射 **，则返回 true**，一个布尔值，在概念模型中的。</span><span class="sxs-lookup"><span data-stu-id="dc361-621">For example, you could map the string value "T" in the data source to a value of **true**, a Boolean, in the conceptual model.</span></span>
-   <span data-ttu-id="dc361-622">为查询结果定义条件筛选器。</span><span class="sxs-lookup"><span data-stu-id="dc361-622">Define conditional filters for query results.</span></span>
-   <span data-ttu-id="dc361-623">强制对概念模型中的数据施加比存储模型中更少的限制。</span><span class="sxs-lookup"><span data-stu-id="dc361-623">Enforce fewer restrictions on data in the conceptual model than in the storage model.</span></span> <span data-ttu-id="dc361-624">例如，您可以制作属性在概念模型中可以为 null 即使映射到的列不支持**null**值。</span><span class="sxs-lookup"><span data-stu-id="dc361-624">For example, you could make a property in the conceptual model nullable even if the column to which it is mapped does not support **null**values.</span></span>

<span data-ttu-id="dc361-625">为实体定义查询视图时需要考虑以下注意事项：</span><span class="sxs-lookup"><span data-stu-id="dc361-625">The following considerations apply when you define query views for entities:</span></span>

-   <span data-ttu-id="dc361-626">查询视图是只读的。</span><span class="sxs-lookup"><span data-stu-id="dc361-626">Query views are read-only.</span></span> <span data-ttu-id="dc361-627">只能使用修改函数来更新实体。</span><span class="sxs-lookup"><span data-stu-id="dc361-627">You can only make updates to entities by using modification functions.</span></span>
-   <span data-ttu-id="dc361-628">通过查询视图定义实体类型时，必须也通过查询视图来定义所有相关实体。</span><span class="sxs-lookup"><span data-stu-id="dc361-628">When you define an entity type by a query view, you must also define all related entities by query views.</span></span>
-   <span data-ttu-id="dc361-629">当您映射到表示关系架构中的链接表的存储模型中实体的多对多关联时，必须定义**QueryView**中的元素**AssociationSetMapping**此链接表的的元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-629">When you map a many-to-many association to an entity in the storage model that represents a link table in the relational schema, you must define a **QueryView** element in the **AssociationSetMapping** element for this link table.</span></span>
-   <span data-ttu-id="dc361-630">必须为类型层次结构中的所有类型定义查询视图。</span><span class="sxs-lookup"><span data-stu-id="dc361-630">Query views must be defined for all types in a type hierarchy.</span></span> <span data-ttu-id="dc361-631">可以使用下列方式来实现：</span><span class="sxs-lookup"><span data-stu-id="dc361-631">You can do this in the following ways:</span></span>
-   -   <span data-ttu-id="dc361-632">使用单个**QueryView**元素，它指定返回层次结构中的所有实体类型的联合的单一实体 SQL 查询。</span><span class="sxs-lookup"><span data-stu-id="dc361-632">With a single **QueryView** element that specifies a single Entity SQL query that returns a union of all of the entity types in the hierarchy.</span></span>
    -   <span data-ttu-id="dc361-633">使用单个**QueryView**指定单个实体 SQL 查询使用 CASE 运算符可在层次结构中返回的特定实体类型的元素根据特定条件。</span><span class="sxs-lookup"><span data-stu-id="dc361-633">With a single **QueryView** element that specifies a single Entity SQL query that uses the CASE operator to return a specific entity type in the hierarchy based on a specific condition.</span></span>
    -   <span data-ttu-id="dc361-634">带有附加**QueryView**层次结构中的特定类型的元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-634">With an additional **QueryView** element for a specific type in the hierarchy.</span></span> <span data-ttu-id="dc361-635">在这种情况下，使用**TypeName**的属性**QueryView**元素来指定每个视图的实体类型。</span><span class="sxs-lookup"><span data-stu-id="dc361-635">In this case, use the **TypeName** attribute of the **QueryView** element to specify the entity type for each view.</span></span>
-   <span data-ttu-id="dc361-636">当定义查询视图时，不能指定**StorageSetName**特性，可以在**EntitySetMapping**元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-636">When a query view is defined, you cannot specify the **StorageSetName** attribute on the **EntitySetMapping** element.</span></span>
-   <span data-ttu-id="dc361-637">当定义查询视图时， **EntitySetMapping**元素不能同时包含**属性**映射。</span><span class="sxs-lookup"><span data-stu-id="dc361-637">When a query view is defined, the **EntitySetMapping**element cannot also contain **Property** mappings.</span></span>

## <a name="resultbinding-element-msl"></a><span data-ttu-id="dc361-638">ResultBinding 元素 (MSL)</span><span class="sxs-lookup"><span data-stu-id="dc361-638">ResultBinding Element (MSL)</span></span>

<span data-ttu-id="dc361-639">**ResultBinding**以映射规范语言 （msl） 表示的元素映射列的值由存储过程到概念模型中的实体属性时返回实体类型修改函数映射到存储基础数据库中的过程。</span><span class="sxs-lookup"><span data-stu-id="dc361-639">The **ResultBinding** element in mapping specification language (MSL) maps column values that are returned by stored procedures to entity properties in the conceptual model when entity type modification functions are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="dc361-640">例如，当标识列的值返回插入的存储过程中， **ResultBinding**元素映射到概念模型中实体类型属性返回的值。</span><span class="sxs-lookup"><span data-stu-id="dc361-640">For example, when the value of an identity column is returned by an insert stored procedure, the **ResultBinding** element maps the returned value to an entity type property in the conceptual model.</span></span>

<span data-ttu-id="dc361-641">**ResultBinding**元素可以是 InsertFunction 元素或 UpdateFunction 元素的子级。</span><span class="sxs-lookup"><span data-stu-id="dc361-641">The **ResultBinding** element can be child of the InsertFunction element or the UpdateFunction element.</span></span>

<span data-ttu-id="dc361-642">**ResultBinding**元素不能具有任何子元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-642">The **ResultBinding** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="dc361-643">适用的特性</span><span class="sxs-lookup"><span data-stu-id="dc361-643">Applicable Attributes</span></span>

<span data-ttu-id="dc361-644">下表描述了适用于的特性**ResultBinding**元素：</span><span class="sxs-lookup"><span data-stu-id="dc361-644">The following table describes the attributes that are applicable to the **ResultBinding** element:</span></span>

| <span data-ttu-id="dc361-645">特性名</span><span class="sxs-lookup"><span data-stu-id="dc361-645">Attribute Name</span></span> | <span data-ttu-id="dc361-646">是否必需</span><span class="sxs-lookup"><span data-stu-id="dc361-646">Is Required</span></span> | <span data-ttu-id="dc361-647">“值”</span><span class="sxs-lookup"><span data-stu-id="dc361-647">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="dc361-648">**名称**</span><span class="sxs-lookup"><span data-stu-id="dc361-648">**Name**</span></span>       | <span data-ttu-id="dc361-649">是</span><span class="sxs-lookup"><span data-stu-id="dc361-649">Yes</span></span>         | <span data-ttu-id="dc361-650">概念模型中要映射的实体属性的名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-650">The name of the entity property in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="dc361-651">**列名称**</span><span class="sxs-lookup"><span data-stu-id="dc361-651">**ColumnName**</span></span> | <span data-ttu-id="dc361-652">是</span><span class="sxs-lookup"><span data-stu-id="dc361-652">Yes</span></span>         | <span data-ttu-id="dc361-653">要映射的列的名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-653">The name of the column being mapped.</span></span>                                          |

### <a name="example"></a><span data-ttu-id="dc361-654">示例</span><span class="sxs-lookup"><span data-stu-id="dc361-654">Example</span></span>

<span data-ttu-id="dc361-655">以下示例基于 School 模型，并显示**InsertFunction**元素用于映射的插入函数**人员**实体类型设置为**InsertPerson**存储的过程。</span><span class="sxs-lookup"><span data-stu-id="dc361-655">The following example is based on the School model and shows an **InsertFunction** element used to map the insert function of the **Person** entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="dc361-656">( **InsertPerson**存储的过程如下所示，在存储模型中声明。)一个**ResultBinding**元素用于映射存储过程返回的列值 (**NewPersonID**) 到实体类型属性 (**PersonID**)。</span><span class="sxs-lookup"><span data-stu-id="dc361-656">(The **InsertPerson** stored procedure is shown below and is declared in the storage model.) A **ResultBinding** element is used to map a column value that is returned by the stored procedure (**NewPersonID**) to an entity type property (**PersonID**).</span></span>

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```

<span data-ttu-id="dc361-657">以下 TRANSACT-SQL 描述**InsertPerson**存储过程：</span><span class="sxs-lookup"><span data-stu-id="dc361-657">The following Transact-SQL describes the **InsertPerson** stored procedure:</span></span>

``` SQL
 CREATE PROCEDURE [dbo].[InsertPerson]
                                @LastName nvarchar(50),
                                @FirstName nvarchar(50),
                                @HireDate datetime,
                                @EnrollmentDate datetime
                                AS
                                INSERT INTO dbo.Person (LastName,
                                                                             FirstName,
                                                                             HireDate,
                                                                             EnrollmentDate)
                                VALUES (@LastName,
                                               @FirstName,
                                               @HireDate,
                                               @EnrollmentDate);
                                SELECT SCOPE_IDENTITY() as NewPersonID;
```

## <a name="resultmapping-element-msl"></a><span data-ttu-id="dc361-658">ResultMapping 元素 (MSL)</span><span class="sxs-lookup"><span data-stu-id="dc361-658">ResultMapping Element (MSL)</span></span>

<span data-ttu-id="dc361-659">**ResultMapping**元素以映射规范语言 （msl） 表示基础数据库中定义概念模型中的函数导入和存储的过程之间的映射，当满足以下条件：</span><span class="sxs-lookup"><span data-stu-id="dc361-659">The **ResultMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="dc361-660">函数导入返回一个概念模型实体类型或复杂类型。</span><span class="sxs-lookup"><span data-stu-id="dc361-660">The function import returns a conceptual model entity type or complex type.</span></span>
-   <span data-ttu-id="dc361-661">存储过程返回的列的名称与实体类型或复杂类型中的属性名称不完全匹配。</span><span class="sxs-lookup"><span data-stu-id="dc361-661">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the entity type or complex type.</span></span>

<span data-ttu-id="dc361-662">默认情况下，存储过程返回的列与实体类型或复杂类型中的属性之间的映射基于列名称和属性名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-662">By default, the mapping between the columns returned by a stored procedure and an entity type or complex type is based on column and property names.</span></span> <span data-ttu-id="dc361-663">如果列名称与属性名称不完全匹配，则必须使用**ResultMapping**元素定义的映射。</span><span class="sxs-lookup"><span data-stu-id="dc361-663">If column names do not exactly match property names, you must use the **ResultMapping** element to define the mapping.</span></span> <span data-ttu-id="dc361-664">有关默认映射的示例，请参阅 FunctionImportMapping 元素 (MSL)。</span><span class="sxs-lookup"><span data-stu-id="dc361-664">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="dc361-665">**ResultMapping**元素是 FunctionImportMapping 元素的子元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-665">The **ResultMapping** element is a child element of the FunctionImportMapping element.</span></span>

<span data-ttu-id="dc361-666">**ResultMapping**元素可以具有以下子元素：</span><span class="sxs-lookup"><span data-stu-id="dc361-666">The **ResultMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="dc361-667">EntityTypeMapping （零个或多个）</span><span class="sxs-lookup"><span data-stu-id="dc361-667">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="dc361-668">ComplexTypeMapping</span><span class="sxs-lookup"><span data-stu-id="dc361-668">ComplexTypeMapping</span></span>

<span data-ttu-id="dc361-669">任何属性都适用于**ResultMapping**元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-669">No attributes are applicable to the **ResultMapping** Element.</span></span>

### <a name="example"></a><span data-ttu-id="dc361-670">示例</span><span class="sxs-lookup"><span data-stu-id="dc361-670">Example</span></span>

<span data-ttu-id="dc361-671">请考虑使用以下存储过程：</span><span class="sxs-lookup"><span data-stu-id="dc361-671">Consider the following stored procedure:</span></span>

``` SQL
 CREATE PROCEDURE [dbo].[GetGrades]
             @student_Id int
             AS
             SELECT     EnrollmentID as enroll_id,
                                                                             Grade as grade,
                                                                             CourseID as course_id,
                                                                             StudentID as student_id
                                               FROM dbo.StudentGrade
             WHERE StudentID = @student_Id
```

<span data-ttu-id="dc361-672">另请考虑使用以下概念模型实体类型：</span><span class="sxs-lookup"><span data-stu-id="dc361-672">Also consider the following conceptual model entity type:</span></span>

``` xml
 <EntityType Name="StudentGrade">
   <Key>
     <PropertyRef Name="EnrollmentID" />
   </Key>
   <Property Name="EnrollmentID" Type="Int32" Nullable="false"
             annotation:StoreGeneratedPattern="Identity" />
   <Property Name="CourseID" Type="Int32" Nullable="false" />
   <Property Name="StudentID" Type="Int32" Nullable="false" />
   <Property Name="Grade" Type="Decimal" Precision="3" Scale="2" />
 </EntityType>
```

<span data-ttu-id="dc361-673">若要创建函数导入返回上一个实体类型的实例，列之间的映射存储过程返回的和必须中定义的实体类型**ResultMapping**元素：</span><span class="sxs-lookup"><span data-stu-id="dc361-673">In order to create a function import that returns instances of the previous entity type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ResultMapping** element:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetGrades"
                        FunctionName="SchoolModel.Store.GetGrades" >
   <ResultMapping>
     <EntityTypeMapping TypeName="SchoolModel.StudentGrade">
       <ScalarProperty Name="EnrollmentID" ColumnName="enroll_id"/>
       <ScalarProperty Name="CourseID" ColumnName="course_id"/>
       <ScalarProperty Name="StudentID" ColumnName="student_id"/>
       <ScalarProperty Name="Grade" ColumnName="grade"/>
     </EntityTypeMapping>
   </ResultMapping>
 </FunctionImportMapping>
```

## <a name="scalarproperty-element-msl"></a><span data-ttu-id="dc361-674">ScalarProperty 元素 (MSL)</span><span class="sxs-lookup"><span data-stu-id="dc361-674">ScalarProperty Element (MSL)</span></span>

<span data-ttu-id="dc361-675">**ScalarProperty**以映射规范语言 （msl） 表示的元素将概念模型实体类型、 复杂类型或关联的属性映射到表列或基础数据库中的存储的过程参数。</span><span class="sxs-lookup"><span data-stu-id="dc361-675">The **ScalarProperty** element in mapping specification language (MSL) maps a property on a conceptual model entity type, complex type, or association to a table column or stored procedure parameter in the underlying database.</span></span>

> [!NOTE]
> <span data-ttu-id="dc361-676">将修改函数映射到其中的存储过程必须在存储模型中声明。</span><span class="sxs-lookup"><span data-stu-id="dc361-676">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="dc361-677">有关详细信息，请参阅函数元素 (SSDL)。</span><span class="sxs-lookup"><span data-stu-id="dc361-677">For more information, see Function Element (SSDL).</span></span>

<span data-ttu-id="dc361-678">**ScalarProperty**元素可以是以下元素的子元素：</span><span class="sxs-lookup"><span data-stu-id="dc361-678">The **ScalarProperty** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="dc361-679">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="dc361-679">MappingFragment</span></span>
-   <span data-ttu-id="dc361-680">InsertFunction</span><span class="sxs-lookup"><span data-stu-id="dc361-680">InsertFunction</span></span>
-   <span data-ttu-id="dc361-681">UpdateFunction</span><span class="sxs-lookup"><span data-stu-id="dc361-681">UpdateFunction</span></span>
-   <span data-ttu-id="dc361-682">DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="dc361-682">DeleteFunction</span></span>
-   <span data-ttu-id="dc361-683">EndProperty</span><span class="sxs-lookup"><span data-stu-id="dc361-683">EndProperty</span></span>
-   <span data-ttu-id="dc361-684">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="dc361-684">ComplexProperty</span></span>
-   <span data-ttu-id="dc361-685">ResultMapping</span><span class="sxs-lookup"><span data-stu-id="dc361-685">ResultMapping</span></span>

<span data-ttu-id="dc361-686">为的子**MappingFragment**， **ComplexProperty**，或**EndProperty**元素**ScalarProperty**元素的属性映射在数据库中的列对概念模型。</span><span class="sxs-lookup"><span data-stu-id="dc361-686">As a child of the **MappingFragment**, **ComplexProperty**, or **EndProperty** element, the **ScalarProperty** element maps a property in the conceptual model to a column in the database.</span></span> <span data-ttu-id="dc361-687">为的子**InsertFunction**， **UpdateFunction**，或**DeleteFunction**元素**ScalarProperty**元素的属性映射在存储的过程参数对概念模型。</span><span class="sxs-lookup"><span data-stu-id="dc361-687">As a child of the **InsertFunction**, **UpdateFunction**, or **DeleteFunction** element, the **ScalarProperty** element maps a property in the conceptual model to a stored procedure parameter.</span></span>

<span data-ttu-id="dc361-688">**ScalarProperty**元素不能具有任何子元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-688">The **ScalarProperty** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="dc361-689">适用的特性</span><span class="sxs-lookup"><span data-stu-id="dc361-689">Applicable Attributes</span></span>

<span data-ttu-id="dc361-690">将应用于的特性**ScalarProperty**元素而异的元素的角色。</span><span class="sxs-lookup"><span data-stu-id="dc361-690">The attributes that apply to the **ScalarProperty** element differ depending on the role of the element.</span></span>

<span data-ttu-id="dc361-691">下表描述了所适用的属性**ScalarProperty**元素用于将概念模型属性映射到数据库中的列：</span><span class="sxs-lookup"><span data-stu-id="dc361-691">The following table describes the attributes that are applicable when the **ScalarProperty** element is used to map a conceptual model property to a column in the database:</span></span>

| <span data-ttu-id="dc361-692">特性名</span><span class="sxs-lookup"><span data-stu-id="dc361-692">Attribute Name</span></span> | <span data-ttu-id="dc361-693">是否必需</span><span class="sxs-lookup"><span data-stu-id="dc361-693">Is Required</span></span> | <span data-ttu-id="dc361-694">“值”</span><span class="sxs-lookup"><span data-stu-id="dc361-694">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="dc361-695">**名称**</span><span class="sxs-lookup"><span data-stu-id="dc361-695">**Name**</span></span>       | <span data-ttu-id="dc361-696">是</span><span class="sxs-lookup"><span data-stu-id="dc361-696">Yes</span></span>         | <span data-ttu-id="dc361-697">要映射的概念模型属性的名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-697">The name of the conceptual model property that is being mapped.</span></span> |
| <span data-ttu-id="dc361-698">**列名称**</span><span class="sxs-lookup"><span data-stu-id="dc361-698">**ColumnName**</span></span> | <span data-ttu-id="dc361-699">是</span><span class="sxs-lookup"><span data-stu-id="dc361-699">Yes</span></span>         | <span data-ttu-id="dc361-700">要映射的表列的名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-700">The name of the table column that is being mapped.</span></span>              |

<span data-ttu-id="dc361-701">下表描述了适用于的特性**ScalarProperty**元素用于将概念模型属性映射到存储的过程参数时：</span><span class="sxs-lookup"><span data-stu-id="dc361-701">The following table describes the attributes that are applicable to the **ScalarProperty** element when it is used to map a conceptual model property to a stored procedure parameter:</span></span>

| <span data-ttu-id="dc361-702">特性名</span><span class="sxs-lookup"><span data-stu-id="dc361-702">Attribute Name</span></span>    | <span data-ttu-id="dc361-703">是否必需</span><span class="sxs-lookup"><span data-stu-id="dc361-703">Is Required</span></span> | <span data-ttu-id="dc361-704">“值”</span><span class="sxs-lookup"><span data-stu-id="dc361-704">Value</span></span>                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="dc361-705">**名称**</span><span class="sxs-lookup"><span data-stu-id="dc361-705">**Name**</span></span>          | <span data-ttu-id="dc361-706">是</span><span class="sxs-lookup"><span data-stu-id="dc361-706">Yes</span></span>         | <span data-ttu-id="dc361-707">要映射的概念模型属性的名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-707">The name of the conceptual model property that is being mapped.</span></span>                                                                                 |
| <span data-ttu-id="dc361-708">**参数名**</span><span class="sxs-lookup"><span data-stu-id="dc361-708">**ParameterName**</span></span> | <span data-ttu-id="dc361-709">是</span><span class="sxs-lookup"><span data-stu-id="dc361-709">Yes</span></span>         | <span data-ttu-id="dc361-710">正在映射的参数的名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-710">The name of the parameter that is being mapped.</span></span>                                                                                                 |
| <span data-ttu-id="dc361-711">**Version**</span><span class="sxs-lookup"><span data-stu-id="dc361-711">**Version**</span></span>       | <span data-ttu-id="dc361-712">否</span><span class="sxs-lookup"><span data-stu-id="dc361-712">No</span></span>          | <span data-ttu-id="dc361-713">**当前**或**原始**具体取决于是否当前值或属性的原始值应使用为了进行并发检查。</span><span class="sxs-lookup"><span data-stu-id="dc361-713">**Current** or **Original** depending on whether the current value or the original value of the property should be used for concurrency checks.</span></span> |

### <a name="example"></a><span data-ttu-id="dc361-714">示例</span><span class="sxs-lookup"><span data-stu-id="dc361-714">Example</span></span>

<span data-ttu-id="dc361-715">下面的示例演示**ScalarProperty**元素使用以下两种方式：</span><span class="sxs-lookup"><span data-stu-id="dc361-715">The following example shows the **ScalarProperty** element used in two ways:</span></span>

-   <span data-ttu-id="dc361-716">若要映射的属性**Person**实体类型的列**人员**表。</span><span class="sxs-lookup"><span data-stu-id="dc361-716">To map the properties of the **Person** entity type to the columns of the **Person**table.</span></span>
-   <span data-ttu-id="dc361-717">若要映射的属性**Person**实体类型的参数**UpdatePerson**存储过程。</span><span class="sxs-lookup"><span data-stu-id="dc361-717">To map the properties of the **Person** entity type to the parameters of the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="dc361-718">存储过程在存储模型中声明。</span><span class="sxs-lookup"><span data-stu-id="dc361-718">The stored procedures are declared in the storage model.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a><span data-ttu-id="dc361-719">示例</span><span class="sxs-lookup"><span data-stu-id="dc361-719">Example</span></span>

<span data-ttu-id="dc361-720">下面的示例说明**ScalarProperty**元素用于映射插入和删除函数的概念模型关联到数据库中的存储过程。</span><span class="sxs-lookup"><span data-stu-id="dc361-720">The next example shows the **ScalarProperty** element used to map the insert and delete functions of a conceptual model association to stored procedures in the database.</span></span> <span data-ttu-id="dc361-721">存储过程在存储模型中声明。</span><span class="sxs-lookup"><span data-stu-id="dc361-721">The stored procedures are declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="updatefunction-element-msl"></a><span data-ttu-id="dc361-722">UpdateFunction 元素 (MSL)</span><span class="sxs-lookup"><span data-stu-id="dc361-722">UpdateFunction Element (MSL)</span></span>

<span data-ttu-id="dc361-723">**UpdateFunction**以映射规范语言 （msl） 表示的元素将概念模型中的实体类型的更新函数映射到基础数据库中的存储过程。</span><span class="sxs-lookup"><span data-stu-id="dc361-723">The **UpdateFunction** element in mapping specification language (MSL) maps the update function of an entity type in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="dc361-724">将修改函数映射到其中的存储过程必须在存储模型中声明。</span><span class="sxs-lookup"><span data-stu-id="dc361-724">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="dc361-725">有关详细信息，请参阅函数元素 (SSDL)。</span><span class="sxs-lookup"><span data-stu-id="dc361-725">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
>  <span data-ttu-id="dc361-726">如果不将所有这三个插入、 更新或删除一个实体类型到存储过程的操作、 映射的操作将失败，则在运行时执行和 UpdateException 引发。</span><span class="sxs-lookup"><span data-stu-id="dc361-726">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="dc361-727">**UpdateFunction**元素可以是 ModificationFunctionMapping 元素的子元素，并应用于 EntityTypeMapping 元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-727">The **UpdateFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element.</span></span>

<span data-ttu-id="dc361-728">**UpdateFunction**元素可以具有以下子元素：</span><span class="sxs-lookup"><span data-stu-id="dc361-728">The **UpdateFunction** element can have the following child elements:</span></span>

-   <span data-ttu-id="dc361-729">AssociationEnd （零个或多个）</span><span class="sxs-lookup"><span data-stu-id="dc361-729">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="dc361-730">ComplexProperty （零个或多个）</span><span class="sxs-lookup"><span data-stu-id="dc361-730">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="dc361-731">ResultBinding （零个或一个）</span><span class="sxs-lookup"><span data-stu-id="dc361-731">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="dc361-732">ScarlarProperty （零个或多个）</span><span class="sxs-lookup"><span data-stu-id="dc361-732">ScarlarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="dc361-733">适用的特性</span><span class="sxs-lookup"><span data-stu-id="dc361-733">Applicable Attributes</span></span>

<span data-ttu-id="dc361-734">下表介绍可应用于的特性**UpdateFunction**元素。</span><span class="sxs-lookup"><span data-stu-id="dc361-734">The following table describes the attributes that can be applied to the **UpdateFunction** element.</span></span>

| <span data-ttu-id="dc361-735">特性名</span><span class="sxs-lookup"><span data-stu-id="dc361-735">Attribute Name</span></span>            | <span data-ttu-id="dc361-736">是否必需</span><span class="sxs-lookup"><span data-stu-id="dc361-736">Is Required</span></span> | <span data-ttu-id="dc361-737">“值”</span><span class="sxs-lookup"><span data-stu-id="dc361-737">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="dc361-738">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="dc361-738">**FunctionName**</span></span>          | <span data-ttu-id="dc361-739">是</span><span class="sxs-lookup"><span data-stu-id="dc361-739">Yes</span></span>         | <span data-ttu-id="dc361-740">更新函数要映射到的存储过程的命名空间限定名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-740">The namespace-qualified name of the stored procedure to which the update function is mapped.</span></span> <span data-ttu-id="dc361-741">存储过程必须在存储模型中声明。</span><span class="sxs-lookup"><span data-stu-id="dc361-741">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="dc361-742">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="dc361-742">**RowsAffectedParameter**</span></span> | <span data-ttu-id="dc361-743">否</span><span class="sxs-lookup"><span data-stu-id="dc361-743">No</span></span>          | <span data-ttu-id="dc361-744">返回受影响行数的输出参数的名称。</span><span class="sxs-lookup"><span data-stu-id="dc361-744">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

### <a name="example"></a><span data-ttu-id="dc361-745">示例</span><span class="sxs-lookup"><span data-stu-id="dc361-745">Example</span></span>

<span data-ttu-id="dc361-746">以下示例基于 School 模型，并显示**UpdateFunction**元素使用的更新函数映射**人员**实体类型设置为**UpdatePerson**存储的过程。</span><span class="sxs-lookup"><span data-stu-id="dc361-746">The following example is based on the School model and shows the **UpdateFunction** element used to map update function of the **Person** entity type to the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="dc361-747">**UpdatePerson**在存储模型中声明的存储的过程。</span><span class="sxs-lookup"><span data-stu-id="dc361-747">The **UpdatePerson** stored procedure is declared in the storage model.</span></span>

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```
