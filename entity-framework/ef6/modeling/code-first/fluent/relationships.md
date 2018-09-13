---
title: Fluent API 的关系-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: fd73b4f8-16d5-40f1-9640-885ceafe67a1
ms.openlocfilehash: 05f282c02699f8bf3c71197ac5e01000f1855917
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490462"
---
# <a name="fluent-api---relationships"></a><span data-ttu-id="5d080-102">Fluent API 的关系</span><span class="sxs-lookup"><span data-stu-id="5d080-102">Fluent API - Relationships</span></span>
> [!NOTE]
> <span data-ttu-id="5d080-103">此页提供了有关设置关系中第一个代码模型使用 fluent API 的信息。</span><span class="sxs-lookup"><span data-stu-id="5d080-103">This page provides information about setting up relationships in your Code First model using the fluent API.</span></span> <span data-ttu-id="5d080-104">有关在 EF 和如何访问和处理数据使用关系的关系的常规信息，请参阅[关系和导航属性](~/ef6/fundamentals/relationships.md)。</span><span class="sxs-lookup"><span data-stu-id="5d080-104">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).</span></span>  

<span data-ttu-id="5d080-105">在使用 Code First，可以通过定义域 CLR 类定义您的模型。</span><span class="sxs-lookup"><span data-stu-id="5d080-105">When working with Code First, you define your model by defining your domain CLR classes.</span></span> <span data-ttu-id="5d080-106">默认情况下，实体框架使用 Code First 约定来将您的类映射到数据库架构。</span><span class="sxs-lookup"><span data-stu-id="5d080-106">By default, Entity Framework uses the Code First conventions to map your classes to the database schema.</span></span> <span data-ttu-id="5d080-107">如果使用 Code First 的命名约定，在大多数情况下您可以依赖于 Code First 来设置基于外键和导航属性在类定义的表之间的关系。</span><span class="sxs-lookup"><span data-stu-id="5d080-107">If you use the Code First naming conventions, in most cases you can rely on Code First to set up relationships between your tables based on the foreign keys and navigation properties that you define on the classes.</span></span> <span data-ttu-id="5d080-108">如果定义您的类时不遵循约定或约定如果你想要更改的方式工作，可以使用 fluent API 或数据注释来配置您的类，以便代码优先可以映射在表之间的关系。</span><span class="sxs-lookup"><span data-stu-id="5d080-108">If you do not follow the conventions when defining your classes, or if you want to change the way the conventions work, you can use the fluent API or data annotations to configure your classes so Code First can map the relationships between your tables.</span></span>  

## <a name="introduction"></a><span data-ttu-id="5d080-109">介绍</span><span class="sxs-lookup"><span data-stu-id="5d080-109">Introduction</span></span>  

<span data-ttu-id="5d080-110">在通过 fluent API 配置关系时，开始 EntityTypeConfiguration 实例，然后使用 HasRequired、 HasOptional 或 HasMany 方法指定的此实体所参与的关系的类型。</span><span class="sxs-lookup"><span data-stu-id="5d080-110">When configuring a relationship with the fluent API, you start with the EntityTypeConfiguration instance and then use the HasRequired, HasOptional, or HasMany method to specify the type of relationship this entity participates in.</span></span> <span data-ttu-id="5d080-111">这些 HasRequired 和 HasOptional 方法采用表示引用导航属性的 lambda 表达式。</span><span class="sxs-lookup"><span data-stu-id="5d080-111">The HasRequired and HasOptional methods take a lambda expression that represents a reference navigation property.</span></span> <span data-ttu-id="5d080-112">HasMany 方法采用 lambda 表达式，表示集合导航属性。</span><span class="sxs-lookup"><span data-stu-id="5d080-112">The HasMany method takes a lambda expression that represents a collection navigation property.</span></span> <span data-ttu-id="5d080-113">然后可以使用 WithRequired、 WithOptional 和 WithMany 方法配置一个反向导航属性。</span><span class="sxs-lookup"><span data-stu-id="5d080-113">You can then configure an inverse navigation property by using the WithRequired, WithOptional, and WithMany methods.</span></span> <span data-ttu-id="5d080-114">这些方法具有重载，它们不采用自变量和可用于单向导航使用指定的基数。</span><span class="sxs-lookup"><span data-stu-id="5d080-114">These methods have overloads that do not take arguments and can be used to specify cardinality with unidirectional navigations.</span></span>  

<span data-ttu-id="5d080-115">然后可以使用 HasForeignKey 方法来配置外键属性。</span><span class="sxs-lookup"><span data-stu-id="5d080-115">You can then configure foreign key properties by using the HasForeignKey method.</span></span> <span data-ttu-id="5d080-116">此方法采用 lambda 表达式，表示要用作外键的属性。</span><span class="sxs-lookup"><span data-stu-id="5d080-116">This method takes a lambda expression that represents the property to be used as the foreign key.</span></span>  

## <a name="configuring-a-required-to-optional-relationship-one-tozero-or-one"></a><span data-ttu-id="5d080-117">配置所需-到-可选关系 （一个-到-零或一）</span><span class="sxs-lookup"><span data-stu-id="5d080-117">Configuring a Required-to-Optional Relationship (One-to–Zero-or-One)</span></span>  

<span data-ttu-id="5d080-118">下面的示例配置对零或一一关系。</span><span class="sxs-lookup"><span data-stu-id="5d080-118">The following example configures a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="5d080-119">OfficeAssignment 具有 primary key 和 foreign key 的 InstructorID 属性，因为该属性的名称不遵循 HasKey 方法用于配置为主键的约定。</span><span class="sxs-lookup"><span data-stu-id="5d080-119">The OfficeAssignment has the InstructorID property that is a primary key and a foreign key, because the name of the property does not follow the convention the HasKey method is used to configure the primary key.</span></span>  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

// Map one-to-zero or one relationship
modelBuilder.Entity<OfficeAssignment>()
    .HasRequired(t => t.Instructor)
    .WithOptional(t => t.OfficeAssignment);
```  

## <a name="configuring-a-relationship-where-both-ends-are-required-one-to-one"></a><span data-ttu-id="5d080-120">配置两个边界所需 （一对一） 的关系</span><span class="sxs-lookup"><span data-stu-id="5d080-120">Configuring a Relationship Where Both Ends Are Required (One-to-One)</span></span>  

<span data-ttu-id="5d080-121">在大多数情况下 Entity Framework 可以推断出哪种类型是依赖项以及哪一个是一种关系中的主体。</span><span class="sxs-lookup"><span data-stu-id="5d080-121">In most cases Entity Framework can infer which type is the dependent and which is the principal in a relationship.</span></span> <span data-ttu-id="5d080-122">但是，当同时关系所需或两侧均可选实体框架无法识别依赖和主体可以。</span><span class="sxs-lookup"><span data-stu-id="5d080-122">However, when both ends of the relationship are required or both sides are optional Entity Framework cannot identify the dependent and principal.</span></span> <span data-ttu-id="5d080-123">当此关系的两端都是必需的时之后，使用 WithRequiredPrincipal 或 WithRequiredDependent HasRequired 方法。</span><span class="sxs-lookup"><span data-stu-id="5d080-123">When both ends of the relationship are required, use WithRequiredPrincipal or WithRequiredDependent after the HasRequired method.</span></span> <span data-ttu-id="5d080-124">当此关系的两端都是可选的时之后，使用 WithOptionalPrincipal 或 WithOptionalDependent HasOptional 方法。</span><span class="sxs-lookup"><span data-stu-id="5d080-124">When both ends of the relationship are optional, use WithOptionalPrincipal or WithOptionalDependent after the HasOptional method.</span></span>  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);
```  

## <a name="configuring-a-many-to-many-relationship"></a><span data-ttu-id="5d080-125">配置多对多关系</span><span class="sxs-lookup"><span data-stu-id="5d080-125">Configuring a Many-to-Many Relationship</span></span>  

<span data-ttu-id="5d080-126">下面的代码配置 Course 和 Instructor 类型之间的多对多关系。</span><span class="sxs-lookup"><span data-stu-id="5d080-126">The following code configures a many-to-many relationship between the Course and Instructor types.</span></span> <span data-ttu-id="5d080-127">在以下示例中，默认 Code First 约定用于创建联接表。</span><span class="sxs-lookup"><span data-stu-id="5d080-127">In the following example, the default Code First conventions are used to create a join table.</span></span> <span data-ttu-id="5d080-128">因此 CourseInstructor 表是使用 Course_CourseID 和 Instructor_InstructorID 列创建。</span><span class="sxs-lookup"><span data-stu-id="5d080-128">As a result the CourseInstructor table is created with Course_CourseID and Instructor_InstructorID columns.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
```  

<span data-ttu-id="5d080-129">如果你想要在表中指定联接表名称和列的名称需要使用 Map 方法来执行其他配置。</span><span class="sxs-lookup"><span data-stu-id="5d080-129">If you want to specify the join table name and the names of the columns in the table you need to do additional configuration by using the Map method.</span></span> <span data-ttu-id="5d080-130">下面的代码生成 CourseID 和 InstructorID 列 CourseInstructor 表。</span><span class="sxs-lookup"><span data-stu-id="5d080-130">The following code generates the CourseInstructor table with CourseID and InstructorID columns.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
    .Map(m =>
    {
        m.ToTable("CourseInstructor");
        m.MapLeftKey("CourseID");
        m.MapRightKey("InstructorID");
    });
```  

## <a name="configuring-a-relationship-with-one-navigation-property"></a><span data-ttu-id="5d080-131">使用一个导航属性配置关系</span><span class="sxs-lookup"><span data-stu-id="5d080-131">Configuring a Relationship with One Navigation Property</span></span>  

<span data-ttu-id="5d080-132">单向 （也称为单向） 仅之一的关系端而不是两者定义导航属性时，关系。</span><span class="sxs-lookup"><span data-stu-id="5d080-132">A one-directional (also called unidirectional) relationship is when a navigation property is defined on only one of the relationship ends and not on both.</span></span> <span data-ttu-id="5d080-133">按照约定，代码优先始终解释为一个多单向关系。</span><span class="sxs-lookup"><span data-stu-id="5d080-133">By convention, Code First always interprets a unidirectional relationship as one-to-many.</span></span> <span data-ttu-id="5d080-134">例如，如果你想一对一关系之间 Instructor 和 OfficeAssignment，其中只有 Instructor 类型有一个导航属性，需要使用 fluent API 来配置此关系。</span><span class="sxs-lookup"><span data-stu-id="5d080-134">For example, if you want a one-to-one relationship between Instructor and OfficeAssignment, where you have a navigation property on only the Instructor type, you need to use the fluent API to configure this relationship.</span></span>  

``` csharp
// Configure the primary Key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal();
```  

## <a name="enabling-cascade-delete"></a><span data-ttu-id="5d080-135">启用级联删除</span><span class="sxs-lookup"><span data-stu-id="5d080-135">Enabling Cascade Delete</span></span>  

<span data-ttu-id="5d080-136">可以通过使用 WillCascadeOnDelete 方法在关系上配置级联删除。</span><span class="sxs-lookup"><span data-stu-id="5d080-136">You can configure cascade delete on a relationship by using the WillCascadeOnDelete method.</span></span> <span data-ttu-id="5d080-137">如果依赖实体的外键不可为 null，则第一个代码设置级联删除的关系。</span><span class="sxs-lookup"><span data-stu-id="5d080-137">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="5d080-138">如果依赖实体的外键是可以为 null，Code First 不会设置级联删除的关系，并且当删除主体将设置外键为 null。</span><span class="sxs-lookup"><span data-stu-id="5d080-138">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span>  

<span data-ttu-id="5d080-139">可通过删除以下级联删除约定：</span><span class="sxs-lookup"><span data-stu-id="5d080-139">You can remove these cascade delete conventions by using:</span></span>  

<span data-ttu-id="5d080-140">modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>（)</span><span class="sxs-lookup"><span data-stu-id="5d080-140">modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>()</span></span>  
<span data-ttu-id="5d080-141">modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>（)</span><span class="sxs-lookup"><span data-stu-id="5d080-141">modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>()</span></span>  

<span data-ttu-id="5d080-142">下面的代码将关系是必需的配置，然后禁用级联删除。</span><span class="sxs-lookup"><span data-stu-id="5d080-142">The following code configures the relationship to be required and then disables cascade delete.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(t => t.Department)
    .WithMany(t => t.Courses)
    .HasForeignKey(d => d.DepartmentID)
    .WillCascadeOnDelete(false);
```  

## <a name="configuring-a-composite-foreign-key"></a><span data-ttu-id="5d080-143">配置复合外键</span><span class="sxs-lookup"><span data-stu-id="5d080-143">Configuring a Composite Foreign Key</span></span>  

<span data-ttu-id="5d080-144">如果部门类型的主键包含 DepartmentID 和 Name 属性，则配置为主键部门和课程类型上的外键，如下所示：</span><span class="sxs-lookup"><span data-stu-id="5d080-144">If the primary key on the Department type consisted of DepartmentID and Name properties, you would configure the primary key for the Department and the foreign key on the Course types as follows:</span></span>  

``` csharp
// Composite primary key
modelBuilder.Entity<Department>()
.HasKey(d => new { d.DepartmentID, d.Name });

// Composite foreign key
modelBuilder.Entity<Course>()  
    .HasRequired(c => c.Department)  
    .WithMany(d => d.Courses)
    .HasForeignKey(d => new { d.DepartmentID, d.DepartmentName });
```  

## <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="5d080-145">重命名模型中未定义的外键</span><span class="sxs-lookup"><span data-stu-id="5d080-145">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="5d080-146">如果您选择不在 CLR 类型上定义外键，但想要在数据库中指定它应具有什么名称，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="5d080-146">If you choose not to define a foreign key on the CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

## <a name="configuring-a-foreign-key-name-that-does-not-follow-the-code-first-convention"></a><span data-ttu-id="5d080-147">配置不遵循代码的第一个约定的外键名称</span><span class="sxs-lookup"><span data-stu-id="5d080-147">Configuring a Foreign Key Name That Does Not Follow the Code First Convention</span></span>  

<span data-ttu-id="5d080-148">如果外的键属性课程类上调用而不是 DepartmentID SomeDepartmentID，需要执行以下操作来指定你想要作为外键的 SomeDepartmentID:</span><span class="sxs-lookup"><span data-stu-id="5d080-148">If the foreign key property on the Course class was called SomeDepartmentID instead of DepartmentID, you would need to do the following to specify that you want SomeDepartmentID to be the foreign key:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
         .HasRequired(c => c.Department)
         .WithMany(d => d.Courses)
         .HasForeignKey(c => c.SomeDepartmentID);
```  

## <a name="model-used-in-samples"></a><span data-ttu-id="5d080-149">示例中所用的模型</span><span class="sxs-lookup"><span data-stu-id="5d080-149">Model Used in Samples</span></span>  

<span data-ttu-id="5d080-150">下面的代码优先模型用于在此页上的示例。</span><span class="sxs-lookup"><span data-stu-id="5d080-150">The following Code First model is used for the samples on this page.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.ModelConfiguration.Conventions;
// add a reference to System.ComponentModel.DataAnnotations DLL
using System.ComponentModel.DataAnnotations;
using System.Collections.Generic;
using System;

public class SchoolEntities : DbContext
{
    public DbSet<Course> Courses { get; set; }
    public DbSet<Department> Departments { get; set; }
    public DbSet<Instructor> Instructors { get; set; }
    public DbSet<OfficeAssignment> OfficeAssignments { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        // Configure Code First to ignore PluralizingTableName convention
        // If you keep this convention then the generated tables will have pluralized names.
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
}

public class Department
{
    public Department()
    {
        this.Courses = new HashSet<Course>();
    }
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }
    public decimal Budget { get; set; }
    public System.DateTime StartDate { get; set; }
    public int? Administrator { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; private set; }
}

public class Course
{
    public Course()
    {
        this.Instructors = new HashSet<Instructor>();
    }
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
    public virtual ICollection<Instructor> Instructors { get; private set; }
}

public partial class OnlineCourse : Course
{
    public string URL { get; set; }
}

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

public class Instructor
{
    public Instructor()
    {
        this.Courses = new List<Course>();
    }

    // Primary key
    public int InstructorID { get; set; }
    public string LastName { get; set; }
    public string FirstName { get; set; }
    public System.DateTime HireDate { get; set; }

    // Navigation properties
    public virtual ICollection<Course> Courses { get; private set; }
}

public class OfficeAssignment
{
    // Specifying InstructorID as a primary
    [Key()]
    public Int32 InstructorID { get; set; }

    public string Location { get; set; }

    // When Entity Framework sees Timestamp attribute
    // it configures ConcurrencyCheck and DatabaseGeneratedPattern=Computed.
    [Timestamp]
    public Byte[] Timestamp { get; set; }

    // Navigation property
    public virtual Instructor Instructor { get; set; }
}
```  
