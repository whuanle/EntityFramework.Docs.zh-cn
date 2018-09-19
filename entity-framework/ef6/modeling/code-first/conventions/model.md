---
title: 基于模型的约定的 EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
ms.openlocfilehash: 80b722730b4ca6c9d00a8611b6c9027e8bc9fe61
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283702"
---
# <a name="model-based-conventions"></a><span data-ttu-id="88984-102">基于模型的约定</span><span class="sxs-lookup"><span data-stu-id="88984-102">Model-Based Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="88984-103">**仅限 EF6 及更高版本** - 此页面中讨论的功能、API 等已引入实体框架 6。</span><span class="sxs-lookup"><span data-stu-id="88984-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="88984-104">如果使用的是早期版本，则部分或全部信息不适用。</span><span class="sxs-lookup"><span data-stu-id="88984-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="88984-105">基于模型约定是一种高级的基于约定模型配置方法。</span><span class="sxs-lookup"><span data-stu-id="88984-105">Model based conventions are an advanced method of convention based model configuration.</span></span> <span data-ttu-id="88984-106">在大多数情况下[自定义 Code First 约定 API，在 DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md)应使用。</span><span class="sxs-lookup"><span data-stu-id="88984-106">For most scenarios the [Custom Code First Convention API on DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) should be used.</span></span> <span data-ttu-id="88984-107">约定 DbModelBuilder API 了解建议使用基于模型约定之前。</span><span class="sxs-lookup"><span data-stu-id="88984-107">An understanding of the DbModelBuilder API for conventions is recommended before using model based conventions.</span></span>  

<span data-ttu-id="88984-108">基于模型约定允许创建的约定会影响属性和不能通过标准约定配置的表。</span><span class="sxs-lookup"><span data-stu-id="88984-108">Model based conventions allow the creation of conventions that affect properties and tables which are not configurable through standard conventions.</span></span> <span data-ttu-id="88984-109">其中的示例为每个层次结构模型表中的鉴别器列和独立关联中的列。</span><span class="sxs-lookup"><span data-stu-id="88984-109">Examples of these are discriminator columns in table per hierarchy models and Independent Association columns.</span></span>  

## <a name="creating-a-convention"></a><span data-ttu-id="88984-110">创建约定</span><span class="sxs-lookup"><span data-stu-id="88984-110">Creating a Convention</span></span>   

<span data-ttu-id="88984-111">创建模型基于约定的第一步选择时需要在管道中应用于模型约定。</span><span class="sxs-lookup"><span data-stu-id="88984-111">The first step in creating a model based convention is choosing when in the pipeline the convention needs to be applied to the model.</span></span> <span data-ttu-id="88984-112">有两种类型的模型约定，概念 （C-空间） 和应用商店 （S-空间）。</span><span class="sxs-lookup"><span data-stu-id="88984-112">There are two types of model conventions, Conceptual (C-Space) and Store (S-Space).</span></span> <span data-ttu-id="88984-113">C 空间约定应用于应用程序生成，而非 S 空间约定应用于表示的数据库的模型的版本和控件等如何自动生成的列名为的模型。</span><span class="sxs-lookup"><span data-stu-id="88984-113">A C-Space convention is applied to the model that the application builds, whereas an S-Space convention is applied to the version of the model that represents the database and controls things such as how automatically-generated columns are named.</span></span>  

<span data-ttu-id="88984-114">模型约定是从 IConceptualModelConvention 或 IStoreModelConvention 扩展的类。</span><span class="sxs-lookup"><span data-stu-id="88984-114">A model convention is a class that extends from either IConceptualModelConvention or IStoreModelConvention.</span></span>  <span data-ttu-id="88984-115">两者都接受的泛型类型，可以为这些接口类型 MetadataItem 用于筛选约定适用于的数据类型。</span><span class="sxs-lookup"><span data-stu-id="88984-115">These interfaces both accept a generic type that can be of type MetadataItem which is used to filter the data type that the convention applies to.</span></span>  

## <a name="adding-a-convention"></a><span data-ttu-id="88984-116">添加一种约定</span><span class="sxs-lookup"><span data-stu-id="88984-116">Adding a Convention</span></span>   

<span data-ttu-id="88984-117">模型约定添加为正则约定类相同的方式。</span><span class="sxs-lookup"><span data-stu-id="88984-117">Model conventions are added in the same way as regular conventions classes.</span></span> <span data-ttu-id="88984-118">在中**OnModelCreating**方法中，将约定添加到模型的约定的列表。</span><span class="sxs-lookup"><span data-stu-id="88984-118">In the **OnModelCreating** method, add the convention to the list of conventions for a model.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

public class BlogContext : DbContext  
{  
    public DbSet<Post> Posts { get; set; }  
    public DbSet<Comment> Comments { get; set; }  

    protected override void OnModelCreating(DbModelBuilder modelBuilder)  
    {  
        modelBuilder.Conventions.Add<MyModelBasedConvention>();  
    }  
}
```  

<span data-ttu-id="88984-119">一种约定还可以添加与另一个惯例使用 Conventions.AddBefore\< \>或 Conventions.AddAfter\< \>方法。</span><span class="sxs-lookup"><span data-stu-id="88984-119">A convention can also be added in relation to another convention using the Conventions.AddBefore\<\> or Conventions.AddAfter\<\> methods.</span></span> <span data-ttu-id="88984-120">有关实体框架应用的约定的详细信息请参阅注释部分。</span><span class="sxs-lookup"><span data-stu-id="88984-120">For more information about the conventions that Entity Framework applies see the notes section.</span></span>  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a><span data-ttu-id="88984-121">示例： 鉴别器模型约定</span><span class="sxs-lookup"><span data-stu-id="88984-121">Example: Discriminator Model Convention</span></span>  

<span data-ttu-id="88984-122">重命名列生成的 EF 是示例就不能与其他约定 Api。</span><span class="sxs-lookup"><span data-stu-id="88984-122">Renaming columns generated by EF is an example of something that you can’t do with the other conventions APIs.</span></span>  <span data-ttu-id="88984-123">这是一种情况其中使用模型约定是唯一的选项。</span><span class="sxs-lookup"><span data-stu-id="88984-123">This is a situation where using model conventions is your only option.</span></span>  

<span data-ttu-id="88984-124">如何使用模型基于约定配置生成的列的示例自定义鉴别器列的命名的方法。</span><span class="sxs-lookup"><span data-stu-id="88984-124">An example of how to use a model based convention to configure generated columns is customizing the way discriminator columns are named.</span></span>  <span data-ttu-id="88984-125">下面是将名为"Discriminator"的模型中的每列重命名为"EntityType"简单的模型基于约定的示例。</span><span class="sxs-lookup"><span data-stu-id="88984-125">Below is an example of a simple model based convention that renames every column in the model named “Discriminator” to “EntityType”.</span></span>  <span data-ttu-id="88984-126">这包括开发人员只需命名为"Discriminator"的列。</span><span class="sxs-lookup"><span data-stu-id="88984-126">This includes columns that the developer simply named “Discriminator”.</span></span> <span data-ttu-id="88984-127">由于"Discriminator"列是生成的列，这需要在 S 空间中运行。</span><span class="sxs-lookup"><span data-stu-id="88984-127">Since the “Discriminator” column is a generated column this needs to run in S-Space.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

class DiscriminatorRenamingConvention : IStoreModelConvention<EdmProperty>  
{  
    public void Apply(EdmProperty property, DbModel model)  
    {            
        if (property.Name == "Discriminator")  
        {  
            property.Name = "EntityType";  
        }  
    }  
}
```  

## <a name="example-general-ia-renaming-convention"></a><span data-ttu-id="88984-128">示例： 常规 IA 重命名约定</span><span class="sxs-lookup"><span data-stu-id="88984-128">Example: General IA Renaming Convention</span></span>   

<span data-ttu-id="88984-129">在操作中的模型基于约定的另一个更复杂的示例是配置为独立关联 (IAs) 的方式。</span><span class="sxs-lookup"><span data-stu-id="88984-129">Another more complicated example of model based conventions in action is to configure the way that Independent Associations (IAs) are named.</span></span>  <span data-ttu-id="88984-130">这是模型约定都适用，因为 IAs 都由 EF 且 DbModelBuilder API 可以访问的模型中不存在。</span><span class="sxs-lookup"><span data-stu-id="88984-130">This is a situation where Model conventions are applicable because IAs are generated by EF and aren’t present in the model that the DbModelBuilder API can access.</span></span>  

<span data-ttu-id="88984-131">当 EF 生成 IA 时，它会创建一个名为 EntityType_KeyName 列。</span><span class="sxs-lookup"><span data-stu-id="88984-131">When EF generates an IA, it creates a column named EntityType_KeyName.</span></span> <span data-ttu-id="88984-132">例如，对于键列与名为 Customer 的关联名为客户 id 则会生成一个名为 Customer_CustomerId 列。</span><span class="sxs-lookup"><span data-stu-id="88984-132">For example, for an association named Customer with a key column named CustomerId it would generate a column named Customer_CustomerId.</span></span> <span data-ttu-id="88984-133">下列约定带\_字符超出为 IA.生成的列名称</span><span class="sxs-lookup"><span data-stu-id="88984-133">The following convention strips the ‘\_’ character out of the column name that is generated for the IA.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

// Provides a convention for fixing the independent association (IA) foreign key column names.  
public class ForeignKeyNamingConvention : IStoreModelConvention<AssociationType>
{

    public void Apply(AssociationType association, DbModel model)
    {
        // Identify ForeignKey properties (including IAs)  
        if (association.IsForeignKey)
        {
            // rename FK columns  
            var constraint = association.Constraint;
            if (DoPropertiesHaveDefaultNames(constraint.FromProperties, constraint.ToRole.Name, constraint.ToProperties))
            {
                NormalizeForeignKeyProperties(constraint.FromProperties);
            }
            if (DoPropertiesHaveDefaultNames(constraint.ToProperties, constraint.FromRole.Name, constraint.FromProperties))
            {
                NormalizeForeignKeyProperties(constraint.ToProperties);
            }
        }
    }

    private bool DoPropertiesHaveDefaultNames(ReadOnlyMetadataCollection<EdmProperty> properties, string roleName, ReadOnlyMetadataCollection<EdmProperty> otherEndProperties)
    {
        if (properties.Count != otherEndProperties.Count)
        {
            return false;
        }

        for (int i = 0; i < properties.Count; ++i)
        {
            if (!properties[i].Name.EndsWith("_" + otherEndProperties[i].Name))
            {
                return false;
            }
        }
        return true;
    }

    private void NormalizeForeignKeyProperties(ReadOnlyMetadataCollection<EdmProperty> properties)
    {
        for (int i = 0; i \< properties.Count; ++i)
        {
            int underscoreIndex = properties[i].Name.IndexOf('_');
            if (underscoreIndex > 0)
            {
                properties[i].Name = properties[i].Name.Remove(underscoreIndex, 1);
            }                 
        }
    }
}
```  

## <a name="extending-existing-conventions"></a><span data-ttu-id="88984-134">扩展现有约定</span><span class="sxs-lookup"><span data-stu-id="88984-134">Extending Existing Conventions</span></span>   

<span data-ttu-id="88984-135">如果您需要编写类似于实体框架已适用于您始终可以扩展这一惯例，以免从头开始重新编写的模型的约定的约定。</span><span class="sxs-lookup"><span data-stu-id="88984-135">If you need to write a convention that is similar to one of the conventions that Entity Framework already applies to your model you can always extend that convention to avoid having to rewrite it from scratch.</span></span>  <span data-ttu-id="88984-136">出现这种是替换现有的 Id 匹配与自定义的约定。</span><span class="sxs-lookup"><span data-stu-id="88984-136">An example of this is to replace the existing Id matching convention with a custom one.</span></span>   <span data-ttu-id="88984-137">重写的键的约定的一个好处是，仅当已检测到或显式配置没有键，将会调用重写的方法。</span><span class="sxs-lookup"><span data-stu-id="88984-137">An added benefit to overriding the key convention is that the overridden method will get called only if there is no key already detected or explicitly configured.</span></span> <span data-ttu-id="88984-138">一组约定由实体框架提供了： [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx)。</span><span class="sxs-lookup"><span data-stu-id="88984-138">A list of conventions that used by Entity Framework is available here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;
using System.Linq;  

// Convention to detect primary key properties.
// Recognized naming patterns in order of precedence are:
// 1. 'Key'
// 2. [type name]Key
// Primary key detection is case insensitive.
public class CustomKeyDiscoveryConvention : KeyDiscoveryConvention
{
    private const string Id = "Key";

    protected override IEnumerable<EdmProperty> MatchKeyProperty(
        EntityType entityType, IEnumerable<EdmProperty> primitiveProperties)
    {
        Debug.Assert(entityType != null);
        Debug.Assert(primitiveProperties != null);

        var matches = primitiveProperties
            .Where(p => Id.Equals(p.Name, StringComparison.OrdinalIgnoreCase));

        if (!matches.Any())
       {
            matches = primitiveProperties
                .Where(p => (entityType.Name + Id).Equals(p.Name, StringComparison.OrdinalIgnoreCase));
        }

        // If the number of matches is more than one, then multiple properties matched differing only by
        // case--for example, "Key" and "key".  
        if (matches.Count() > 1)
        {
            throw new InvalidOperationException("Multiple properties match the key convention");
        }

        return matches;
    }
}
```  

<span data-ttu-id="88984-139">然后，我们需要添加的现有密钥约定之前我们新的约定。</span><span class="sxs-lookup"><span data-stu-id="88984-139">We then need to add our new convention before the existing key convention.</span></span> <span data-ttu-id="88984-140">我们添加 CustomKeyDiscoveryConvention 后，我们可以删除 IdKeyDiscoveryConvention。</span><span class="sxs-lookup"><span data-stu-id="88984-140">After we add the CustomKeyDiscoveryConvention, we can remove the IdKeyDiscoveryConvention.</span></span>  <span data-ttu-id="88984-141">如果我们没有删除现有 IdKeyDiscoveryConvention 此约定将仍优先于 Id 发现约定由于它运行第一次，但在其中找到没有"密钥"属性的情况下，将运行的"id"约定。</span><span class="sxs-lookup"><span data-stu-id="88984-141">If we didn’t remove the existing IdKeyDiscoveryConvention this convention would still take precedence over the Id discovery convention since it is run first, but in the case where no “key” property is found, the “id” convention will run.</span></span>  <span data-ttu-id="88984-142">我们看到此行为，因为每个约定会看到该模型，为已更新的以前的约定 （而不是独立操作和所有正在组合在一起），以便如果例如上, 一个约定更新要匹配的内容的列名称感兴趣，为你自定义的约定 （如果在之前的名称是不感兴趣的），则它将应用于该列。</span><span class="sxs-lookup"><span data-stu-id="88984-142">We see this behavior because each convention sees the model as updated by the previous convention (rather than operating on it independently and all being combined together) so that if for example, a previous convention updated a column name to match something of interest to your custom convention (when before that the name was not of interest) then it will apply to that column.</span></span>  

``` csharp
public class BlogContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Comment> Comments { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new CustomKeyDiscoveryConvention());
        modelBuilder.Conventions.Remove<IdKeyDiscoveryConvention>();
    }
}
```  

## <a name="notes"></a><span data-ttu-id="88984-143">说明</span><span class="sxs-lookup"><span data-stu-id="88984-143">Notes</span></span>  

<span data-ttu-id="88984-144">此处的 MSDN 文档中提供了当前应用由实体框架的约定的列表： [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx)。</span><span class="sxs-lookup"><span data-stu-id="88984-144">A list of conventions that are currently applied by Entity Framework is available in the MSDN documentation here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  <span data-ttu-id="88984-145">直接从我们的源代码拉取此列表。</span><span class="sxs-lookup"><span data-stu-id="88984-145">This list is pulled directly from our source code.</span></span>  <span data-ttu-id="88984-146">Entity Framework 6 的源代码位于[GitHub](https://github.com/aspnet/entityframework6/)和许多实体框架使用的约定是很好的自定义模型起点基于约定。</span><span class="sxs-lookup"><span data-stu-id="88984-146">The source code for Entity Framework 6 is available on [GitHub](https://github.com/aspnet/entityframework6/) and many of the conventions used by Entity Framework are good starting points for custom model based conventions.</span></span>  
