---
title: 代码优先约定-EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: bc644573-c2b2-4ed7-8745-3c37c41058ad
caps.latest.revision: 4
ms.openlocfilehash: b124a034ba900780cc4d7e1354408cd3995e874e
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/08/2018
ms.locfileid: "39119975"
---
# <a name="code-first-conventions"></a><span data-ttu-id="eb3fb-102">代码优先约定</span><span class="sxs-lookup"><span data-stu-id="eb3fb-102">Code First Conventions</span></span>
<span data-ttu-id="eb3fb-103">Code First 允许使用 C# 或 Visual Basic.NET 类的描述模型。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-103">Code First enables you to describe a model by using C# or Visual Basic .NET classes.</span></span> <span data-ttu-id="eb3fb-104">通过使用约定来检测到该模型的基本形状。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-104">The basic shape of the model is detected by using conventions.</span></span> <span data-ttu-id="eb3fb-105">约定是用于自动配置基于类定义时使用 Code First 的概念模型的规则集。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-105">Conventions are sets of rules that are used to automatically configure a conceptual model based on class definitions when working with Code First.</span></span> <span data-ttu-id="eb3fb-106">约定 System.Data.Entity.ModelConfiguration.Conventions 命名空间中定义。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-106">The conventions are defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>  

<span data-ttu-id="eb3fb-107">通过使用数据注释或 fluent API，可以进一步配置您的模型。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-107">You can further configure your model by using data annotations or the fluent API.</span></span> <span data-ttu-id="eb3fb-108">优先级通过 fluent API 跟数据注释，然后约定的配置。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-108">Precedence is given to configuration through the fluent API followed by data annotations and then conventions.</span></span> <span data-ttu-id="eb3fb-109">有关详细信息请参阅[数据批注](~/ef6/modeling/code-first/data-annotations.md)， [Fluent API-关系](~/ef6/modeling/code-first/fluent/relationships.md)， [Fluent API 的类型和属性](~/ef6/modeling/code-first/fluent/types-and-properties.md)和[VB.NET使用FluentAPI](~/ef6/modeling/code-first/fluent/vb.md).</span><span class="sxs-lookup"><span data-stu-id="eb3fb-109">For more information see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md), [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md), [Fluent API - Types & Properties](~/ef6/modeling/code-first/fluent/types-and-properties.md) and [Fluent API with VB.NET](~/ef6/modeling/code-first/fluent/vb.md).</span></span>  

<span data-ttu-id="eb3fb-110">Code First 约定的详细的列表现已推出[API 文档](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx)。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-110">A detailed list of Code First conventions is available in the [API Documentation](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span> <span data-ttu-id="eb3fb-111">本主题概述使用 Code First 的约定。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-111">This topic provides an overview of the conventions used by Code First.</span></span>  

## <a name="type-discovery"></a><span data-ttu-id="eb3fb-112">类型发现</span><span class="sxs-lookup"><span data-stu-id="eb3fb-112">Type Discovery</span></span>  

<span data-ttu-id="eb3fb-113">使用 Code First 开发时通常首先编写.NET Framework 类定义概念 （域） 模型。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-113">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="eb3fb-114">除了定义类，您还需要让**DbContext**知道你想要在模型中包含的类型。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-114">In addition to defining the classes, you also need to let **DbContext** know which types you want to include in the model.</span></span> <span data-ttu-id="eb3fb-115">若要执行此操作，您定义派生上下文类**DbContext** ，并公开**DbSet**你想要将模型的一部分的类型的属性。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-115">To do this, you define a context class that derives from **DbContext** and exposes **DbSet** properties for the types that you want to be part of the model.</span></span> <span data-ttu-id="eb3fb-116">代码首先将包括这些类型，并还会中拉入任何引用的类型，即使不同程序集中定义引用的类型。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-116">Code First will include these types and also will pull in any referenced types, even if the referenced types are defined in a different assembly.</span></span>  

<span data-ttu-id="eb3fb-117">如果您的类型参与继承层次结构，它就足以定义**DbSet**它们是否在同一程序集中作为类的基类将自动包含基类和派生的类型的属性。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-117">If your types participate in an inheritance hierarchy, it is enough to define a **DbSet** property for the base class, and the derived types will be automatically included, if they are in the same assembly as the base class.</span></span>  

<span data-ttu-id="eb3fb-118">在以下示例中，都只有一个**DbSet**定义的属性**SchoolEntities**类 (**部门**)。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-118">In the following example, there is only one **DbSet** property defined on the **SchoolEntities** class (**Departments**).</span></span> <span data-ttu-id="eb3fb-119">代码首先使用此属性以发现并请求任何引用的类型。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-119">Code First uses this property to discover and pull in any referenced types.</span></span>  

``` csharp
public class SchoolEntities : DbContext
{
    public DbSet<Department> Departments { get; set; }
}

public class Department
{
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; set; }
}

public class Course
{
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
}

public partial class OnlineCourse : Course
{
    public string URL { get; set; }
}

public partial class OnsiteCourse : Course
{
    public string Location { get; set; }
    public string Days { get; set; }
    public System.DateTime Time { get; set; }
}
```  

<span data-ttu-id="eb3fb-120">如果你想要从模型中排除类型，使用**NotMapped**属性或**DbModelBuilder.Ignore** fluent API。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-120">If you want to exclude a type from the model, use the **NotMapped** attribute or the **DbModelBuilder.Ignore** fluent API.</span></span>  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a><span data-ttu-id="eb3fb-121">主要密钥约定</span><span class="sxs-lookup"><span data-stu-id="eb3fb-121">Primary Key Convention</span></span>  

<span data-ttu-id="eb3fb-122">代码首先将推断属性为主键，是否一个类上的属性名为"ID"（不区分大小写），或类名后, 跟"ID"。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-122">Code First infers that a property is a primary key if a property on a class is named “ID” (not case sensitive), or the class name followed by "ID".</span></span> <span data-ttu-id="eb3fb-123">如果主键属性的类型是数值或 GUID 它就是配置为标识列。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-123">If the type of the primary key property is numeric or GUID it will be configured as an identity column.</span></span>  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a><span data-ttu-id="eb3fb-124">关系约定</span><span class="sxs-lookup"><span data-stu-id="eb3fb-124">Relationship Convention</span></span>  

<span data-ttu-id="eb3fb-125">在实体框架中，导航属性提供一种导航两个实体类型之间的关系方法。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-125">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span> <span data-ttu-id="eb3fb-126">针对对象参与到其中的每个关系，各对象均可以具有导航属性。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-126">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="eb3fb-127">导航属性，您可以导航和管理两个方向，返回引用对象中的关系 (在重数为两者中任何一个或零或一) 或集合 （如果重数为多个）。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-127">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="eb3fb-128">代码首先会推断出关系基于您的类型上定义的导航属性。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-128">Code First infers relationships based on the navigation properties defined on your types.</span></span>  

<span data-ttu-id="eb3fb-129">除了导航属性，我们建议您包括表示依赖对象的类型上的外键属性。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-129">In addition to navigation properties, we recommend that you include foreign key properties on the types that represent dependent objects.</span></span> <span data-ttu-id="eb3fb-130">使用作为主体的主键属性相同的数据类型和名称遵循以下格式之一的任何属性表示外的键关系: '\<导航属性名称\>\<主体主键属性名称\>'，'\<主体类名\>\<主键属性名称\>，或\<主体主键属性名称\>。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-130">Any property with the same data type as the principal primary key property and with a name that follows one of the following formats represents a foreign key for the relationship: '\<navigation property name\>\<principal primary key property name\>', '\<principal class name\>\<primary key property name\>', or '\<principal primary key property name\>'.</span></span> <span data-ttu-id="eb3fb-131">如果找到多个匹配项的优先级有以上所列的顺序。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-131">If multiple matches are found then precedence is given in the order listed above.</span></span> <span data-ttu-id="eb3fb-132">外键检测不区分大小写。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-132">Foreign key detection is not case sensitive.</span></span> <span data-ttu-id="eb3fb-133">检测到外键属性时，Code First 推断出关系基于外键的为 null 性的多重性。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-133">When a foreign key property is detected, Code First infers the multiplicity of the relationship based on the nullability of the foreign key.</span></span> <span data-ttu-id="eb3fb-134">如果该属性是可以为 null 关系被注册为可选的;否则注册关系所需的方式。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-134">If the property is nullable then the relationship is registered as optional; otherwise the relationship is registered as required.</span></span>  

<span data-ttu-id="eb3fb-135">如果依赖实体的外键不可为 null，则第一个代码设置级联删除的关系。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-135">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="eb3fb-136">如果依赖实体的外键是可以为 null，Code First 不会设置级联删除的关系，并且当删除主体将设置外键为 null。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-136">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span> <span data-ttu-id="eb3fb-137">重数和级联删除检测到的行为可以使用 fluent API 替代约定。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-137">The multiplicity and cascade delete behavior detected by convention can be overridden by using the fluent API.</span></span>  

<span data-ttu-id="eb3fb-138">在下面的示例使用导航属性和外键定义部门和 Course 类之间的关系。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-138">In the following example the navigation properties and a foreign key are used to define the relationship between the Department and Course classes.</span></span>  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; set; }
}

public class Course
{
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
}
```  

> [!NOTE]
> <span data-ttu-id="eb3fb-139">如果有多个相同类型之间的关系 (例如，假设您定义**Person**并**书籍**类，其中**人员**类包含**ReviewedBooks**并**AuthoredBooks**导航属性和**书籍**类包含**作者**和**审阅者**导航属性) 需要使用数据注释或 fluent API 手动配置关系。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-139">If you have multiple relationships between the same types (for example, suppose you define the **Person** and **Book** classes, where the **Person** class contains the **ReviewedBooks** and **AuthoredBooks** navigation properties and the **Book** class contains the **Author** and **Reviewer** navigation properties) you need to manually configure the relationships by using Data Annotations or the fluent API.</span></span> <span data-ttu-id="eb3fb-140">有关详细信息，请参阅[数据注释-关系](~/ef6/modeling/code-first/data-annotations.md)并[Fluent API-关系](~/ef6/modeling/code-first/fluent/relationships.md)。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-140">For more information, see [Data Annotations - Relationships](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>  

## <a name="complex-types-convention"></a><span data-ttu-id="eb3fb-141">复杂类型约定</span><span class="sxs-lookup"><span data-stu-id="eb3fb-141">Complex Types Convention</span></span>  

<span data-ttu-id="eb3fb-142">当 Code First 发现其中主键不能推断出，并通过数据注释或 fluent API 注册没有主键的类定义时，然后该类型自动注册为复杂类型中。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-142">When Code First discovers a class definition where a primary key cannot be inferred, and no primary key is registered through data annotations or the fluent API, then the type is automatically registered as a complex type.</span></span> <span data-ttu-id="eb3fb-143">复杂类型检测也需要类型不具有引用实体类型，并且未从另一种类型的集合属性引用的属性。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-143">Complex type detection also requires that the type does not have properties that reference entity types and is not referenced from a collection property on another type.</span></span> <span data-ttu-id="eb3fb-144">给定以下类定义 Code First 会推断出的**详细信息**是复杂类型，因为它没有主键。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-144">Given the following class definitions Code First would infer that **Details** is a complex type because it has no primary key.</span></span>  

``` csharp
public partial class OnsiteCourse : Course
{
    public OnsiteCourse()
    {
        Details = new Details();
    }

    public Details Details { get; set; }
}

public class Details
{
    public System.DateTime Time { get; set; }
    public string Location { get; set; }
    public string Days { get; set; }
}
```  

## <a name="connection-string-convention"></a><span data-ttu-id="eb3fb-145">连接字符串约定</span><span class="sxs-lookup"><span data-stu-id="eb3fb-145">Connection String Convention</span></span>  

<span data-ttu-id="eb3fb-146">若要了解有关约定使用 DbContext 来发现要使用，请参阅的连接[连接和模型](~/ef6/fundamentals/configuring/connection-strings.md)。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-146">To learn about the conventions that DbContext uses to discover the connection to use see [Connections and Models](~/ef6/fundamentals/configuring/connection-strings.md).</span></span>  

## <a name="removing-conventions"></a><span data-ttu-id="eb3fb-147">删除约定</span><span class="sxs-lookup"><span data-stu-id="eb3fb-147">Removing Conventions</span></span>  

<span data-ttu-id="eb3fb-148">您可以删除任何 System.Data.Entity.ModelConfiguration.Conventions 命名空间中定义的约定。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-148">You can remove any of the conventions defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span> <span data-ttu-id="eb3fb-149">下面的示例移除**PluralizingTableNameConvention**。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-149">The following example removes **PluralizingTableNameConvention**.</span></span>  

``` csharp
public class SchoolEntities : DbContext
{
     . . .

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        // Configure Code First to ignore PluralizingTableName convention
        // If you keep this convention, the generated tables  
        // will have pluralized names.
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
}
```  

## <a name="custom-conventions"></a><span data-ttu-id="eb3fb-150">自定义约定</span><span class="sxs-lookup"><span data-stu-id="eb3fb-150">Custom Conventions</span></span>  

<span data-ttu-id="eb3fb-151">自定义约定支持 EF6 及更高版本。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-151">Custom conventions are supported in EF6 onwards.</span></span> <span data-ttu-id="eb3fb-152">有关详细信息请参阅[自定义代码优先约定](~/ef6/modeling/code-first/conventions/custom.md)。</span><span class="sxs-lookup"><span data-stu-id="eb3fb-152">For more information see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>
