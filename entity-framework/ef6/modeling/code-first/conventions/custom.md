---
title: 自定义代码优先约定-EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: dd2bdbd9-ae9e-470a-aeb8-d0ba160499b7
caps.latest.revision: 3
ms.openlocfilehash: 24d6f1bd5eb2ff8be59b9eedd1c4156709fa42fb
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/09/2018
ms.locfileid: "39120484"
---
# <a name="custom-code-first-conventions"></a><span data-ttu-id="04824-102">自定义代码优先约定</span><span class="sxs-lookup"><span data-stu-id="04824-102">Custom Code First Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="04824-103">**仅限 EF6 及更高版本** - 此页面中讨论的功能、API 等已引入实体框架 6。</span><span class="sxs-lookup"><span data-stu-id="04824-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="04824-104">如果使用的是早期版本，则部分或全部信息不适用。</span><span class="sxs-lookup"><span data-stu-id="04824-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="04824-105">使用 Code First 时您的模型会计算从您使用的一组约定的类。</span><span class="sxs-lookup"><span data-stu-id="04824-105">When using Code First your model is calculated from your classes using a set of conventions.</span></span> <span data-ttu-id="04824-106">默认值[Code First 约定](~/ef6/modeling/code-first/conventions/built-in.md)确定某些事宜等的属性变为实体的主键、 表实体映射到，并在默认情况下哪些精度和小数位数有 decimal 列的名称。</span><span class="sxs-lookup"><span data-stu-id="04824-106">The default [Code First Conventions](~/ef6/modeling/code-first/conventions/built-in.md) determine things like which property becomes the primary key of an entity, the name of the table an entity maps to, and what precision and scale a decimal column has by default.</span></span>

<span data-ttu-id="04824-107">有时不适合您的模型，这些默认约定，必须先解决它们通过配置多个使用数据注释或 Fluent API 的单个实体。</span><span class="sxs-lookup"><span data-stu-id="04824-107">Sometimes these default conventions are not ideal for your model, and you have to work around them by configuring many individual entities using Data Annotations or the Fluent API.</span></span> <span data-ttu-id="04824-108">自定义代码优先约定，可以定义您自己提供配置的默认值为您的模型的约定。</span><span class="sxs-lookup"><span data-stu-id="04824-108">Custom Code First Conventions let you define your own conventions that provide configuration defaults for your model.</span></span> <span data-ttu-id="04824-109">在本演练中，我们将了解不同类型的自定义约定以及如何创建每个。</span><span class="sxs-lookup"><span data-stu-id="04824-109">In this walkthrough, we will explore the different types of custom conventions and how to create each of them.</span></span>


## <a name="model-based-conventions"></a><span data-ttu-id="04824-110">基于模型的约定</span><span class="sxs-lookup"><span data-stu-id="04824-110">Model-Based Conventions</span></span>

<span data-ttu-id="04824-111">本页介绍如何自定义约定 DbModelBuilder API。</span><span class="sxs-lookup"><span data-stu-id="04824-111">This page covers the DbModelBuilder API for custom conventions.</span></span> <span data-ttu-id="04824-112">此 API 应足以满足创作大多数自定义约定。</span><span class="sxs-lookup"><span data-stu-id="04824-112">This API should be sufficient for authoring most custom conventions.</span></span> <span data-ttu-id="04824-113">但是，还有创作模型基于约定的操作的最终模型，在创建后的约定-若要处理的高级的方案的功能。</span><span class="sxs-lookup"><span data-stu-id="04824-113">However, there is also the ability to author model-based conventions - conventions that manipulate the final model once it is created - to handle advanced scenarios.</span></span> <span data-ttu-id="04824-114">有关详细信息，请参阅[基于模型的约定](~/ef6/modeling/code-first/conventions/model.md)。</span><span class="sxs-lookup"><span data-stu-id="04824-114">For more information, see [Model-Based Conventions](~/ef6/modeling/code-first/conventions/model.md).</span></span>

 

## <a name="our-model"></a><span data-ttu-id="04824-115">我们的模型</span><span class="sxs-lookup"><span data-stu-id="04824-115">Our Model</span></span>

<span data-ttu-id="04824-116">让我们首先定义一个简单的模型，我们可以使用与我们的约定。</span><span class="sxs-lookup"><span data-stu-id="04824-116">Let's start by defining a simple model that we can use with our conventions.</span></span> <span data-ttu-id="04824-117">将以下类添加到你的项目。</span><span class="sxs-lookup"><span data-stu-id="04824-117">Add the following classes to your project.</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;

    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }
    }

    public class Product
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public decimal? Price { get; set; }
        public DateTime? ReleaseDate { get; set; }
        public ProductCategory Category { get; set; }
    }

    public class ProductCategory
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public List<Product> Products { get; set; }
    }
```

 

## <a name="introducing-custom-conventions"></a><span data-ttu-id="04824-118">引入了自定义约定</span><span class="sxs-lookup"><span data-stu-id="04824-118">Introducing Custom Conventions</span></span>

<span data-ttu-id="04824-119">让我们来编写配置已命名密钥为其实体类型的主键的任何属性的约定。</span><span class="sxs-lookup"><span data-stu-id="04824-119">Let’s write a convention that configures any property named Key to be the primary key for its entity type.</span></span>

<span data-ttu-id="04824-120">模型生成器中，可通过在上下文中重写 OnModelCreating 上启用了约定。</span><span class="sxs-lookup"><span data-stu-id="04824-120">Conventions are enabled on the model builder, which can be accessed by overriding OnModelCreating in the context.</span></span> <span data-ttu-id="04824-121">按如下所示更新 ProductContext 类：</span><span class="sxs-lookup"><span data-stu-id="04824-121">Update the ProductContext class as follows:</span></span>

``` csharp
    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Properties()
                        .Where(p => p.Name == "Key")
                        .Configure(p => p.IsKey());
        }
    }
```

<span data-ttu-id="04824-122">现在，我们已命名密钥的模型中的任何属性将配置为所有实体的主键的一部分。</span><span class="sxs-lookup"><span data-stu-id="04824-122">Now, any property in our model named Key will be configured as the primary key of whatever entity its part of.</span></span>

<span data-ttu-id="04824-123">此外可以使通过筛选我们要配置的属性的类型更具体是将约定：</span><span class="sxs-lookup"><span data-stu-id="04824-123">We could also make our conventions more specific by filtering on the type of property that we are going to configure:</span></span>

``` csharp
    modelBuilder.Properties<int>()
                .Where(p => p.Name == "Key")
                .Configure(p => p.IsKey());
```

<span data-ttu-id="04824-124">这会配置其实体的键，但前提是一个整数，这些调用要为主键的所有属性。</span><span class="sxs-lookup"><span data-stu-id="04824-124">This will configure all properties called Key to be the primary key of their entity, but only if they are an integer.</span></span>

<span data-ttu-id="04824-125">IsKey 方法的一个有趣功能是，它是累加性。</span><span class="sxs-lookup"><span data-stu-id="04824-125">An interesting feature of the IsKey method is that it is additive.</span></span> <span data-ttu-id="04824-126">这意味着，如果您对多个属性调用 IsKey，它们将成为复合键的一部分。</span><span class="sxs-lookup"><span data-stu-id="04824-126">Which means that if you call IsKey on multiple properties and they will all become part of a composite key.</span></span> <span data-ttu-id="04824-127">对于这一点是，指定键的多个属性时，还必须指定这些属性的顺序。</span><span class="sxs-lookup"><span data-stu-id="04824-127">The one caveat for this is that when you specify multiple properties for a key you must also specify an order for those properties.</span></span> <span data-ttu-id="04824-128">可以通过调用方法如下所示的 HasColumnOrder 来执行此操作：</span><span class="sxs-lookup"><span data-stu-id="04824-128">You can do this by calling the HasColumnOrder method like below:</span></span>

``` csharp
    modelBuilder.Properties<int>()
                .Where(x => x.Name == "Key")
                .Configure(x => x.IsKey().HasColumnOrder(1));

    modelBuilder.Properties()
                .Where(x => x.Name == "Name")
                .Configure(x => x.IsKey().HasColumnOrder(2));
```

<span data-ttu-id="04824-129">此代码将在模型 int 键列和字符串名称列组成的复合键中配置的类型。</span><span class="sxs-lookup"><span data-stu-id="04824-129">This code will configure the types in our model to have a composite key consisting of the int Key column and the string Name column.</span></span> <span data-ttu-id="04824-130">如果我们查看模型设计器中它将如下所示：</span><span class="sxs-lookup"><span data-stu-id="04824-130">If we view the model in the designer it would look like this:</span></span>

![compositeKey](~/ef6/media/compositekey.png)

<span data-ttu-id="04824-132">属性约定的另一个示例是我要映射到 SQL Server 中而不是日期时间的 datetime2 类型的模型中配置所有日期时间属性。</span><span class="sxs-lookup"><span data-stu-id="04824-132">Another example of property conventions is to configure all DateTime properties in my model to map to the datetime2 type in SQL Server instead of datetime.</span></span> <span data-ttu-id="04824-133">您可以使用以下实现此目的：</span><span class="sxs-lookup"><span data-stu-id="04824-133">You can achieve this with the following:</span></span>

``` csharp
    modelBuilder.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));
```

 

## <a name="convention-classes"></a><span data-ttu-id="04824-134">约定类</span><span class="sxs-lookup"><span data-stu-id="04824-134">Convention Classes</span></span>

<span data-ttu-id="04824-135">定义约定的另一种方法是使用约定类来封装你的约定。</span><span class="sxs-lookup"><span data-stu-id="04824-135">Another way of defining conventions is to use a Convention Class to encapsulate your convention.</span></span> <span data-ttu-id="04824-136">使用约定类时，创建从 System.Data.Entity.ModelConfiguration.Conventions 命名空间中的约定类继承的类型。</span><span class="sxs-lookup"><span data-stu-id="04824-136">When using a Convention Class then you create a type that inherits from the Convention class in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>

<span data-ttu-id="04824-137">我们可以使用我们前面显示通过执行以下的 datetime2 约定创建约定类：</span><span class="sxs-lookup"><span data-stu-id="04824-137">We can create a Convention Class with the datetime2 convention that we showed earlier by doing the following:</span></span>

``` csharp
    public class DateTime2Convention : Convention
    {
        public DateTime2Convention()
        {
            this.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));        
        }
    }
```

<span data-ttu-id="04824-138">若要告诉 EF 使用此约定，将其添加到约定集合在 OnModelCreating，如果您已以及本演练将如下所示：</span><span class="sxs-lookup"><span data-stu-id="04824-138">To tell EF to use this convention you add it to the Conventions collection in OnModelCreating, which if you’ve been following along with the walkthrough will look like this:</span></span>

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Properties<int>()
                    .Where(p => p.Name.EndsWith("Key"))
                    .Configure(p => p.IsKey());

        modelBuilder.Conventions.Add(new DateTime2Convention());
    }
```

<span data-ttu-id="04824-139">您可以看到我们将我们约定的实例添加到约定集合。</span><span class="sxs-lookup"><span data-stu-id="04824-139">As you can see we add an instance of our convention to the conventions collection.</span></span> <span data-ttu-id="04824-140">从约定继承提供了分组和在团队或项目之间共享约定的简便方法。</span><span class="sxs-lookup"><span data-stu-id="04824-140">Inheriting from Convention provides a convenient way of grouping and sharing conventions across teams or projects.</span></span> <span data-ttu-id="04824-141">例如，可以令使用一组通用的您所在组织的所有项目使用的约定。</span><span class="sxs-lookup"><span data-stu-id="04824-141">You could, for example, have a class library with a common set of conventions that all of your organizations projects use.</span></span>

 

## <a name="custom-attributes"></a><span data-ttu-id="04824-142">自定义特性</span><span class="sxs-lookup"><span data-stu-id="04824-142">Custom Attributes</span></span>

<span data-ttu-id="04824-143">约定的另一个出色用途是启用要在配置模型时使用的新属性。</span><span class="sxs-lookup"><span data-stu-id="04824-143">Another great use of conventions is to enable new attributes to be used when configuring a model.</span></span> <span data-ttu-id="04824-144">为了说明这一点，让我们创建一个我们可以使用标记为非 Unicode 字符串属性的属性。</span><span class="sxs-lookup"><span data-stu-id="04824-144">To illustrate this, let’s create an attribute that we can use to mark String properties as non-Unicode.</span></span>

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    public class NonUnicode : Attribute
    {
    }
```

<span data-ttu-id="04824-145">现在，让我们创建的约定，若要将此特性应用于我们的模型：</span><span class="sxs-lookup"><span data-stu-id="04824-145">Now, let’s create a convention to apply this attribute to our model:</span></span>

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<NonUnicode>().Any())
                .Configure(c => c.IsUnicode(false));
```

<span data-ttu-id="04824-146">此约定可添加到任何字符串属性，这意味着数据库中的列使用非 Unicode 属性将存储为 varchar，而不是 nvarchar。</span><span class="sxs-lookup"><span data-stu-id="04824-146">With this convention we can add the NonUnicode attribute to any of our string properties, which means the column in the database will be stored as varchar instead of nvarchar.</span></span>

<span data-ttu-id="04824-147">关于此约定需要注意的一点是，如果将使用非 Unicode 属性放在一个字符串属性以外的任何它将引发异常。</span><span class="sxs-lookup"><span data-stu-id="04824-147">One thing to note about this convention is that if you put the NonUnicode attribute on anything other than a string property then it will throw an exception.</span></span> <span data-ttu-id="04824-148">这是因为您不能配置 IsUnicode 字符串以外的其他任何类型上。</span><span class="sxs-lookup"><span data-stu-id="04824-148">It does this because you cannot configure IsUnicode on any type other than a string.</span></span> <span data-ttu-id="04824-149">如果发生这种情况，然后即可在约定更具体，以便筛选出任何内容不是字符串。</span><span class="sxs-lookup"><span data-stu-id="04824-149">If this happens, then you can make your convention more specific, so that it filters out anything that isn’t a string.</span></span>

<span data-ttu-id="04824-150">尽管上述约定适用于定义自定义属性还有另一个 API，可以是更易于使用，尤其是当你想要使用从特性类的属性。</span><span class="sxs-lookup"><span data-stu-id="04824-150">While the above convention works for defining custom attributes there is another API that can be much easier to use, especially when you want to use properties from the attribute class.</span></span>

<span data-ttu-id="04824-151">对于此示例中我们将更新我们属性并将其更改为 IsUnicode 属性，以便它如下所示：</span><span class="sxs-lookup"><span data-stu-id="04824-151">For this example we are going to update our attribute and change it to an IsUnicode attribute, so it looks like this:</span></span>

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    internal class IsUnicode : Attribute
    {
        public bool Unicode { get; set; }

        public IsUnicode(bool isUnicode)
        {
            Unicode = isUnicode;
        }
    }
```

<span data-ttu-id="04824-152">一旦我们有这，我们可以告诉约定属性应为 Unicode 我们属性上设置一个布尔值。</span><span class="sxs-lookup"><span data-stu-id="04824-152">Once we have this, we can set a bool on our attribute to tell the convention whether or not a property should be Unicode.</span></span> <span data-ttu-id="04824-153">我们无法在我们已通过访问此类配置类的 ClrProperty 的约定来执行此操作：</span><span class="sxs-lookup"><span data-stu-id="04824-153">We could do this in the convention we have already by accessing the ClrProperty of the configuration class like this:</span></span>

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<IsUnicode>().Any())
                .Configure(c => c.IsUnicode(c.ClrPropertyInfo.GetCustomAttribute<IsUnicode>().Unicode));
```

<span data-ttu-id="04824-154">这非常简单，但没有实现此目的通过使用具有更简洁的方式的约定 API 的方法。</span><span class="sxs-lookup"><span data-stu-id="04824-154">This is easy enough, but there is a more succinct way of achieving this by using the Having method of the conventions API.</span></span> <span data-ttu-id="04824-155">无方法具有的参数键入 Func&lt;PropertyInfo、 T&gt; ，它接受 PropertyInfo Where 相同方法，但应返回一个对象。</span><span class="sxs-lookup"><span data-stu-id="04824-155">The Having method has a parameter of type Func&lt;PropertyInfo, T&gt; which accepts the PropertyInfo the same as the Where method, but is expected to return an object.</span></span> <span data-ttu-id="04824-156">如果返回的对象为 null，则将不会配置属性，这意味着您可以筛选出属性与之类似的位置，但它的不同在于它还将捕获返回的对象，并将其传递给配置方法。</span><span class="sxs-lookup"><span data-stu-id="04824-156">If the returned object is null then the property will not be configured, which means you can filter out properties with it just like Where, but it is different in that it will also capture the returned object and pass it to the Configure method.</span></span> <span data-ttu-id="04824-157">此工作原理如下所示：</span><span class="sxs-lookup"><span data-stu-id="04824-157">This works like the following:</span></span>

``` csharp
    modelBuilder.Properties()
                .Having(x =>x.GetCustomAttributes(false).OfType<IsUnicode>().FirstOrDefault())
                .Configure((config, att) => config.IsUnicode(att.Unicode));
```

<span data-ttu-id="04824-158">自定义特性不是唯一的原因，若要使用 Having 方法，它可推论内容对其进行筛选配置类型或属性时所需的任何位置。</span><span class="sxs-lookup"><span data-stu-id="04824-158">Custom attributes are not the only reason to use the Having method, it is useful anywhere that you need to reason about something that you are filtering on when configuring your types or properties.</span></span>

 

## <a name="configuring-types"></a><span data-ttu-id="04824-159">配置类型</span><span class="sxs-lookup"><span data-stu-id="04824-159">Configuring Types</span></span>

<span data-ttu-id="04824-160">到目前为止所有约定已对于属性，但在您的模型中配置类型的约定 API 的另一个区域。</span><span class="sxs-lookup"><span data-stu-id="04824-160">So far all of our conventions have been for properties, but there is another area of the conventions API for configuring the types in your model.</span></span> <span data-ttu-id="04824-161">体验是到目前为止，我们已了解的约定类似但内部配置的选项将在实体而不是属性级别。</span><span class="sxs-lookup"><span data-stu-id="04824-161">The experience is similar to the conventions we have seen so far, but the options inside configure will be at the entity instead of property level.</span></span>

<span data-ttu-id="04824-162">类型级别约定可以是非常有用的事情之一更改表命名约定，以将映射到现有的架构不同于的 EF 默认设置，或创建新的数据库具有不同的命名约定。</span><span class="sxs-lookup"><span data-stu-id="04824-162">One of the things that Type level conventions can be really useful for is changing the table naming convention, either to map to an existing schema that differs from the EF default or to create a new database with a different naming convention.</span></span> <span data-ttu-id="04824-163">若要执行此操作我们首先需要一个方法，可以接受我们的模型中类型的类型信息并返回该类型的表名称应该是什么：</span><span class="sxs-lookup"><span data-stu-id="04824-163">To do this we first need a method that can accept the TypeInfo for a type in our model and return what the table name for that type should be:</span></span>

``` csharp
    private string GetTableName(Type type)
    {
        var result = Regex.Replace(type.Name, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

<span data-ttu-id="04824-164">此方法接受一个类型，并返回而不是驼峰式大小写的下划线开头使用小写的字符串。</span><span class="sxs-lookup"><span data-stu-id="04824-164">This method takes a type and returns a string that uses lower case with underscores instead of CamelCase.</span></span> <span data-ttu-id="04824-165">在我们的模型，这意味着 ProductCategory 类将其映射到名为 product 表\_而不是 ProductCategories 的类别。</span><span class="sxs-lookup"><span data-stu-id="04824-165">In our model this means that the ProductCategory class will be mapped to a table called product\_category instead of ProductCategories.</span></span>

<span data-ttu-id="04824-166">我们有了该方法后我们可以在此类约定中调用它：</span><span class="sxs-lookup"><span data-stu-id="04824-166">Once we have that method we can call it in a convention like this:</span></span>

``` csharp
    modelBuilder.Types()
                .Configure(c => c.ToTable(GetTableName(c.ClrType)));
```

<span data-ttu-id="04824-167">此约定在我们的模型将映射到从我们 GetTableName 方法返回的表名中配置的每个类型。</span><span class="sxs-lookup"><span data-stu-id="04824-167">This convention configures every type in our model to map to the table name that is returned from our GetTableName method.</span></span> <span data-ttu-id="04824-168">此约定是等效于调用 ToTable 方法使用 Fluent API 在模型中每个实体。</span><span class="sxs-lookup"><span data-stu-id="04824-168">This convention is the equivalent to calling the ToTable method for each entity in the model using the Fluent API.</span></span>

<span data-ttu-id="04824-169">有一点需要注意相关信息是在调用时 ToTable EF 将需要提供作为确切的表名，而无需任何复数形式，它通常执行的操作时确定表名称的字符串。</span><span class="sxs-lookup"><span data-stu-id="04824-169">One thing to note about this is that when you call ToTable EF will take the string that you provide as the exact table name, without any of the pluralization that it would normally do when determining table names.</span></span> <span data-ttu-id="04824-170">这就是原因我们约定中的表名称是产品\_而不是产品类别\_类别。</span><span class="sxs-lookup"><span data-stu-id="04824-170">This is why the table name from our convention is product\_category instead of product\_categories.</span></span> <span data-ttu-id="04824-171">我们可以在我们的约定中解析它，从而对复数化服务的调用自己。</span><span class="sxs-lookup"><span data-stu-id="04824-171">We can resolve that in our convention by making a call to the pluralization service ourselves.</span></span>

<span data-ttu-id="04824-172">下面的代码中我们将使用[依赖项解析](~/ef6/fundamentals/configuring/dependency-resolution.md)EF6 检索会使用 EF 复数化服务中添加的功能和复数化我们表的名称。</span><span class="sxs-lookup"><span data-stu-id="04824-172">In the following code we will use the [Dependency Resolution](~/ef6/fundamentals/configuring/dependency-resolution.md) feature added in EF6 to retrieve the pluralization service that EF would have used and pluralize our table name.</span></span>

``` csharp
    private string GetTableName(Type type)
    {
        var pluralizationService = DbConfiguration.DependencyResolver.GetService<IPluralizationService>();

        var result = pluralizationService.Pluralize(type.Name);

        result = Regex.Replace(result, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

> [!NOTE]
> <span data-ttu-id="04824-173">GetService 的泛型版本是 System.Data.Entity.Infrastructure.DependencyResolution 命名空间中的扩展方法，将需要添加一个 using 语句到上下文中才能使用它。</span><span class="sxs-lookup"><span data-stu-id="04824-173">The generic version of GetService is an extension method in the System.Data.Entity.Infrastructure.DependencyResolution namespace, you will need to add a using statement to your context in order to use it.</span></span>

### <a name="totable-and-inheritance"></a><span data-ttu-id="04824-174">ToTable 和继承</span><span class="sxs-lookup"><span data-stu-id="04824-174">ToTable and Inheritance</span></span>

<span data-ttu-id="04824-175">ToTable 的另一个重要方面是，如果显式将类型映射到给定的表，则可以更改将使用 EF 映射策略。</span><span class="sxs-lookup"><span data-stu-id="04824-175">Another important aspect of ToTable is that if you explicitly map a type to a given table, then you can alter the mapping strategy that EF will use.</span></span> <span data-ttu-id="04824-176">如果对于继承层次结构中每个类型调用 ToTable，将类型名称作为表的名称传递，我们更高版本，然后您将更改默认的每个层次结构一个表 (TPH) 映射策略为每种类型一个表 (TPT)。</span><span class="sxs-lookup"><span data-stu-id="04824-176">If you call ToTable for every type in an inheritance hierarchy, passing the type name as the name of the table like we did above, then you will change the default Table-Per-Hierarchy (TPH) mapping strategy to Table-Per-Type (TPT).</span></span> <span data-ttu-id="04824-177">描述此的最佳方法是有具体的示例：</span><span class="sxs-lookup"><span data-stu-id="04824-177">The best way to describe this is whith a concrete example:</span></span>

``` csharp
    public class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }

    public class Manager : Employee
    {
        public string SectionManaged { get; set; }
    }
```

<span data-ttu-id="04824-178">默认情况下 employee 和 manager 映射到数据库中的同一个表 （员工）。</span><span class="sxs-lookup"><span data-stu-id="04824-178">By default both employee and manager are mapped to the same table (Employees) in the database.</span></span> <span data-ttu-id="04824-179">表将包含员工和管理员都会告诉您哪些类型的实例存储在每行一个鉴别器列。</span><span class="sxs-lookup"><span data-stu-id="04824-179">The table will contain both employees and managers with a discriminator column that will tell you what type of instance is stored in each row.</span></span> <span data-ttu-id="04824-180">这是 TPH 映射，因为没有为层次结构一个表。</span><span class="sxs-lookup"><span data-stu-id="04824-180">This is TPH mapping as there is a single table for the hierarchy.</span></span> <span data-ttu-id="04824-181">但是，如果在这两个类上调用 ToTable 然后每种类型将改为将映射到自己的表，也称为 TPT 由于每个类型都有其自己的表。</span><span class="sxs-lookup"><span data-stu-id="04824-181">However, if you call ToTable on both classe then each type will instead be mapped to its own table, also known as TPT since each type has its own table.</span></span>

``` csharp
    modelBuilder.Types()
                .Configure(c=>c.ToTable(c.ClrType.Name));
```

<span data-ttu-id="04824-182">上面的代码将映射到类似于下面的表结构：</span><span class="sxs-lookup"><span data-stu-id="04824-182">The code above will map to a table structure that looks like the following:</span></span>

![tptExample](~/ef6/media/tptexample.jpg)

<span data-ttu-id="04824-184">可以避免此问题，同时保持几种方法中的默认 TPH 映射：</span><span class="sxs-lookup"><span data-stu-id="04824-184">You can avoid this, and maintain the default TPH mapping, in a couple ways:</span></span>

1.  <span data-ttu-id="04824-185">调用层次结构中每个类型一个表同名 ToTable。</span><span class="sxs-lookup"><span data-stu-id="04824-185">Call ToTable with the same table name for each type in the hierarchy.</span></span>
2.  <span data-ttu-id="04824-186">仅在层次结构，在本例中，将为员工的基本类上调用 ToTable。</span><span class="sxs-lookup"><span data-stu-id="04824-186">Call ToTable only on the base class of the hierarchy, in our example that would be employee.</span></span>

 

## <a name="execution-order"></a><span data-ttu-id="04824-187">执行顺序</span><span class="sxs-lookup"><span data-stu-id="04824-187">Execution Order</span></span>

<span data-ttu-id="04824-188">在最后一个 wins 方式，与 Fluent API 相同操作约定。</span><span class="sxs-lookup"><span data-stu-id="04824-188">Conventions operate in a last wins manner, the same as the Fluent API.</span></span> <span data-ttu-id="04824-189">这意味着，即如果您编写两个约定的配置相同的选项将相同的属性，则最后一个执行 wins。</span><span class="sxs-lookup"><span data-stu-id="04824-189">What this means is that if you write two conventions that configure the same option of the same property, then the last one to execute wins.</span></span> <span data-ttu-id="04824-190">例如，下面的代码中的所有字符串的最大长度设置为 500，但接下来我们配置的最大长度为 250 的模型中名为 Name 的所有属性。</span><span class="sxs-lookup"><span data-stu-id="04824-190">As an example, in the code below the max length of all strings is set to 500 but we then configure all properties called Name in the model to have a max length of 250.</span></span>

``` csharp
    modelBuilder.Properties<string>()
                .Configure(c => c.HasMaxLength(500));

    modelBuilder.Properties<string>()
                .Where(x => x.Name == "Name")
                .Configure(c => c.HasMaxLength(250));
```

<span data-ttu-id="04824-191">由于要设置最大长度为 250 的惯例是之后的所有字符串都设置为 500，在我们的模型名为 Name 的所有属性将具有最大为 250 任何其他字符串，如说明时，为 500。</span><span class="sxs-lookup"><span data-stu-id="04824-191">Because the convention to set max length to 250 is after the one that sets all strings to 500, all the properties called Name in our model will have a MaxLength of 250 while any other strings, such as descriptions, would be 500.</span></span> <span data-ttu-id="04824-192">以这种方式使用约定意味着您可以为类型或模型和然后覆盖属性提供常规约定以不同的子集。</span><span class="sxs-lookup"><span data-stu-id="04824-192">Using conventions in this way means that you can provide a general convention for types or properties in your model and then overide them for subsets that are different.</span></span>

<span data-ttu-id="04824-193">Fluent API 和数据批注还可用来重写中特定用例的约定。</span><span class="sxs-lookup"><span data-stu-id="04824-193">The Fluent API and Data Annotations can also be used to override a convention in specific cases.</span></span> <span data-ttu-id="04824-194">在上例如果我们已使用 Fluent API 来设置属性的最大长度然后我们可能已将它放之前或之后约定，因为更具体的 Fluent API 可以赢取更多常规配置约定。</span><span class="sxs-lookup"><span data-stu-id="04824-194">In our example above if we had used the Fluent API to set the max length of a property then we could have put it before or after the convention, because the more specific Fluent API will win over the more general Configuration Convention.</span></span>

 

## <a name="built-in-conventions"></a><span data-ttu-id="04824-195">内置约定</span><span class="sxs-lookup"><span data-stu-id="04824-195">Built-in Conventions</span></span>

<span data-ttu-id="04824-196">自定义约定可能会影响默认 Code First 约定，因为它可用于添加运行之前或之后另一个约定的约定。</span><span class="sxs-lookup"><span data-stu-id="04824-196">Because custom conventions could be affected by the default Code First conventions, it can be useful to add conventions to run before or after another convention.</span></span> <span data-ttu-id="04824-197">若要执行此操作可以在派生 DbContext 上使用约定集合的 AddBefore 和 AddAfter 方法。</span><span class="sxs-lookup"><span data-stu-id="04824-197">To do this you can use the AddBefore and AddAfter methods of the Conventions collection on your derived DbContext.</span></span> <span data-ttu-id="04824-198">下面的代码将添加我们之前创建，以便它将运行之前的内置约定类密钥发现约定中。</span><span class="sxs-lookup"><span data-stu-id="04824-198">The following code would add the convention class we created earlier so that it will run before the built in key discovery convention.</span></span>

``` csharp
    modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new DateTime2Convention());
```

<span data-ttu-id="04824-199">这要充分利用的添加需要运行之前或之后的内置约定的约定时，一组内置的约定可在此处找到： [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="04824-199">This is going to be of the most use when adding conventions that need to run before or after the built in conventions, a list of the built in conventions can be found here: [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>

<span data-ttu-id="04824-200">此外可以删除不希望应用于您的模型的约定。</span><span class="sxs-lookup"><span data-stu-id="04824-200">You can also remove conventions that you do not want applied to your model.</span></span> <span data-ttu-id="04824-201">若要删除一个约定，请使用 Remove 方法。</span><span class="sxs-lookup"><span data-stu-id="04824-201">To remove a convention, use the Remove method.</span></span> <span data-ttu-id="04824-202">下面是删除 PluralizingTableNameConvention 的示例。</span><span class="sxs-lookup"><span data-stu-id="04824-202">Here is an example of removing the PluralizingTableNameConvention.</span></span>

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
```
