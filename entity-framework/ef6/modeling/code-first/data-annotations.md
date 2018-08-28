---
title: 第一个数据注释-EF6 的代码
author: divega
ms.date: 2016-10-23
ms.assetid: 80abefbd-23c9-4fce-9cd3-520e5df9856e
ms.openlocfilehash: 0ab66afa3babafe657b3ddb32c02c3fba0ae310e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994581"
---
# <a name="code-first-data-annotations"></a><span data-ttu-id="2c842-102">Code First 数据注释</span><span class="sxs-lookup"><span data-stu-id="2c842-102">Code First Data Annotations</span></span>
> [!NOTE]
> <span data-ttu-id="2c842-103">**EF4.1 及更高版本仅**的功能，Api，Entity Framework 4.1 中引入了此页所述的等。</span><span class="sxs-lookup"><span data-stu-id="2c842-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="2c842-104">如果使用的是早期版本，则部分或全部信息不适用。</span><span class="sxs-lookup"><span data-stu-id="2c842-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="2c842-105">此页上的内容改编自文章作者： Julie Lerman 最初编写和 (\<http://thedatafarm.com>)。</span><span class="sxs-lookup"><span data-stu-id="2c842-105">The content on this page is adapted from and article originally written by Julie Lerman (\<http://thedatafarm.com>).</span></span>

<span data-ttu-id="2c842-106">实体框架 Code First 允许您使用您自己的域类来表示 EF 依赖于执行查询，该模型更改跟踪和更新功能。</span><span class="sxs-lookup"><span data-stu-id="2c842-106">Entity Framework Code First allows you to use your own domain classes to represent the model which EF relies on to perform querying, change tracking and updating functions.</span></span> <span data-ttu-id="2c842-107">代码首先利用称为约定对配置的编程模式。</span><span class="sxs-lookup"><span data-stu-id="2c842-107">Code first leverages a programming pattern referred to as convention over configuration.</span></span> <span data-ttu-id="2c842-108">这意味着该代码，则首先将假定您的类遵循 EF 使用的约定。</span><span class="sxs-lookup"><span data-stu-id="2c842-108">What this means is that code first will assume that your classes follow the conventions that EF uses.</span></span> <span data-ttu-id="2c842-109">在这种情况下，EF 将能够执行其作业所需的细节。</span><span class="sxs-lookup"><span data-stu-id="2c842-109">In that case, EF will be able to work out the details it needs to do its job.</span></span> <span data-ttu-id="2c842-110">但是，如果您的类不遵循这些约定，必须将配置添加到您的类与它所需的信息提供 EF 的功能。</span><span class="sxs-lookup"><span data-stu-id="2c842-110">However, if your classes do not follow those conventions, you have the ability to add configurations to your classes to provide EF with the information it needs.</span></span>

<span data-ttu-id="2c842-111">代码首先提供了两种方法将这些配置添加到您的类。</span><span class="sxs-lookup"><span data-stu-id="2c842-111">Code first gives you two ways to add these configurations to your classes.</span></span> <span data-ttu-id="2c842-112">一种使用简单的属性名为 DataAnnotations 和另一种是使用 code first 是 Fluent API，后者为您提供一种在代码中以强制方式，描述配置方法。</span><span class="sxs-lookup"><span data-stu-id="2c842-112">One is using simple attributes called DataAnnotations and the other is using code first’s Fluent API, which provides you with a way to describe configurations imperatively, in code.</span></span>

<span data-ttu-id="2c842-113">本文将重点介绍使用 DataAnnotations （System.ComponentModel.DataAnnotations 命名空间中） 来配置你的类，突出显示的最常见所需的配置。</span><span class="sxs-lookup"><span data-stu-id="2c842-113">This article will focus on using DataAnnotations (in the System.ComponentModel.DataAnnotations namespace) to configure your classes – highlighting the most commonly needed configurations.</span></span> <span data-ttu-id="2c842-114">DataAnnotations 还能够理解的大量的.NET 应用程序，如 ASP.NET MVC 允许这些应用程序可以利用相同的注释进行客户端验证。</span><span class="sxs-lookup"><span data-stu-id="2c842-114">DataAnnotations are also understood by a number of .NET applications, such as ASP.NET MVC which allows these applications to leverage the same annotations for client-side validations.</span></span>


## <a name="the-model"></a><span data-ttu-id="2c842-115">模型</span><span class="sxs-lookup"><span data-stu-id="2c842-115">The model</span></span>

<span data-ttu-id="2c842-116">我将演示代码使用类的一个简单的对第一个 DataAnnotations： 博客和文章。</span><span class="sxs-lookup"><span data-stu-id="2c842-116">I’ll demonstrate code first DataAnnotations with a simple pair of classes: Blog and Post.</span></span>

``` csharp
    public class Blog
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string BloggerName { get; set;}
        public virtual ICollection<Post> Posts { get; set; }
    }

    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public DateTime DateCreated { get; set; }
        public string Content { get; set; }
        public int BlogId { get; set; }
        public ICollection<Comment> Comments { get; set; }
    }
```

<span data-ttu-id="2c842-117">因为它们是在博客和文章类方便地遵循代码的第一个约定和所需帮助 EF 处理它们的调整。</span><span class="sxs-lookup"><span data-stu-id="2c842-117">As they are, the Blog and Post classes conveniently follow code first convention and required no tweaks to help EF work with them.</span></span> <span data-ttu-id="2c842-118">但也可以使用批注来向 EF 提供的详细信息，有关类和它们映射到的数据库。</span><span class="sxs-lookup"><span data-stu-id="2c842-118">But you can also use the annotations to provide more information to EF about the classes and the database that they map to.</span></span>

 

## <a name="key"></a><span data-ttu-id="2c842-119">键</span><span class="sxs-lookup"><span data-stu-id="2c842-119">Key</span></span>

<span data-ttu-id="2c842-120">Entity Framework 依赖于它的跟踪实体使用的密钥值的每个实体。</span><span class="sxs-lookup"><span data-stu-id="2c842-120">Entity Framework relies on every entity having a key value that it uses for tracking entities.</span></span> <span data-ttu-id="2c842-121">首先取决于代码约定之一是如何表示哪个属性是中的每个代码第一类的键。</span><span class="sxs-lookup"><span data-stu-id="2c842-121">One of the conventions that code first depends on is how it implies which property is the key in each of the code first classes.</span></span> <span data-ttu-id="2c842-122">这种约定将查找名为"Id"或其中结合的类名称和"Id"，如"BlogId"的属性。</span><span class="sxs-lookup"><span data-stu-id="2c842-122">That convention is to look for a property named “Id” or one that combines the class name and “Id”, such as “BlogId”.</span></span> <span data-ttu-id="2c842-123">该属性将映射到数据库中的主键列。</span><span class="sxs-lookup"><span data-stu-id="2c842-123">The property will map to a primary key column in the database.</span></span>

<span data-ttu-id="2c842-124">博客和文章类都遵循此约定。</span><span class="sxs-lookup"><span data-stu-id="2c842-124">The Blog and Post classes both follow this convention.</span></span> <span data-ttu-id="2c842-125">但是，如果他们不乐意这样呢？</span><span class="sxs-lookup"><span data-stu-id="2c842-125">But what if they didn’t?</span></span> <span data-ttu-id="2c842-126">如果博客使用名称*PrimaryTrackingKey*改为甚至*foo*？</span><span class="sxs-lookup"><span data-stu-id="2c842-126">What if Blog used the name *PrimaryTrackingKey* instead or even *foo*?</span></span> <span data-ttu-id="2c842-127">如果代码第一次未找到匹配此约定属性它将引发异常，由于实体框架的需求，您必须具有键属性。</span><span class="sxs-lookup"><span data-stu-id="2c842-127">If code first does not find a property that matches this convention it will throw an exception because of Entity Framework’s requirement that you must have a key property.</span></span> <span data-ttu-id="2c842-128">关键批注用于指定哪些属性是要使用的 entitykey。</span><span class="sxs-lookup"><span data-stu-id="2c842-128">You can use the key annotation to specify which property is to be used as the EntityKey.</span></span>

``` csharp
    public class Blog
    {
        [Key]
        public int PrimaryTrackingKey { get; set; }
        public string Title { get; set; }
        public string BloggerName { get; set;}
        public virtual ICollection<Post> Posts { get; set; }
    }
```

<span data-ttu-id="2c842-129">如果您是使用 code first 是数据库生成功能、 博客表具有名为 PrimaryTrackingKey 它默认情况下也定义为标识主键列。</span><span class="sxs-lookup"><span data-stu-id="2c842-129">If you are using code first’s database generation feature, the Blog table will have a primary key column named PrimaryTrackingKey which is also defined as Identity by default.</span></span>

![jj591583_figure01](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a><span data-ttu-id="2c842-131">复合键</span><span class="sxs-lookup"><span data-stu-id="2c842-131">Composite keys</span></span>

<span data-ttu-id="2c842-132">实体框架支持的复合键-主键由多个属性组成。</span><span class="sxs-lookup"><span data-stu-id="2c842-132">Entity Framework supports composite keys - primary keys that consist of more than one property.</span></span> <span data-ttu-id="2c842-133">例如，你可能有其主键为 PassportNumber 和 IssuingCountry 的组合的 Passport 类。</span><span class="sxs-lookup"><span data-stu-id="2c842-133">For example, your could have a Passport class whose primary key is a combination of PassportNumber and IssuingCountry.</span></span>

``` csharp
    public class Passport
    {
        [Key]
        public int PassportNumber { get; set; }
        [Key]
        public string IssuingCountry { get; set; }
        public DateTime Issued { get; set; }
        public DateTime Expires { get; set; }
    }
```

<span data-ttu-id="2c842-134">如果已尝试并在 EF 模型中使用上述类，则会得到一个引发 Invalidoperationexception 指出;</span><span class="sxs-lookup"><span data-stu-id="2c842-134">If you were to try and use the above class in your EF model you wuld get an InvalidOperationExceptions stating;</span></span>

<span data-ttu-id="2c842-135">*无法确定复合主键排序类型 Passport。使用 ColumnAttribute 或 HasKey 方法指定为复合主键的订单。*</span><span class="sxs-lookup"><span data-stu-id="2c842-135">*Unable to determine composite primary key ordering for type 'Passport'. Use the ColumnAttribute or the HasKey method to specify an order for composite primary keys.*</span></span>

<span data-ttu-id="2c842-136">如果您具有复合键，实体框架要求定义键属性的顺序。</span><span class="sxs-lookup"><span data-stu-id="2c842-136">When you have composite keys, Entity Framework requires you to define an order of the key properties.</span></span> <span data-ttu-id="2c842-137">您可以执行此操作使用的列批注指定的顺序。</span><span class="sxs-lookup"><span data-stu-id="2c842-137">You can do this using the Column annotation to specify an order.</span></span>

>[!NOTE]
> <span data-ttu-id="2c842-138">将顺序值是相对 （而非基于索引） 以便可以使用任何值。</span><span class="sxs-lookup"><span data-stu-id="2c842-138">The order value is relative (rather than index based) so any values can be used.</span></span> <span data-ttu-id="2c842-139">例如，100 到 200 个将代替 1 和 2 可接受。</span><span class="sxs-lookup"><span data-stu-id="2c842-139">For example, 100 and 200 would be acceptable in place of 1 and 2.</span></span>

``` csharp
    public class Passport
    {
        [Key]
        [Column(Order=1)]
        public int PassportNumber { get; set; }
        [Key]
        [Column(Order = 2)]
        public string IssuingCountry { get; set; }
        public DateTime Issued { get; set; }
        public DateTime Expires { get; set; }
    }
```

<span data-ttu-id="2c842-140">如果您有包含的复合外键的实体，则必须指定相同的列排序用于相应的主键属性。</span><span class="sxs-lookup"><span data-stu-id="2c842-140">If you have entities with composite foreign keys then you must specify the same column ordering that you used for the corresponding primary key properties.</span></span>

<span data-ttu-id="2c842-141">仅的相对顺序中的外键属性必须是相同的确切的值分配给**顺序**不需要匹配。</span><span class="sxs-lookup"><span data-stu-id="2c842-141">Only the relative ordering within the foreign key properties needs to be the same, the exact values assigned to **Order** do not need to match.</span></span> <span data-ttu-id="2c842-142">例如，在以下类中，3 和 4 可用来代替 1 和 2。</span><span class="sxs-lookup"><span data-stu-id="2c842-142">For example, in the following class, 3 and 4 could be used in place of 1 and 2.</span></span>

``` csharp
    public class PassportStamp
    {
        [Key]
        public int StampId { get; set; }
        public DateTime Stamped { get; set; }
        public string StampingCountry { get; set; }

        [ForeignKey("Passport")]
        [Column(Order = 1)]
        public int PassportNumber { get; set; }

        [ForeignKey("Passport")]
        [Column(Order = 2)]
        public string IssuingCountry { get; set; }

        public Passport Passport { get; set; }
    }
```

## <a name="required"></a><span data-ttu-id="2c842-143">必需</span><span class="sxs-lookup"><span data-stu-id="2c842-143">Required</span></span>

<span data-ttu-id="2c842-144">需要的批注告知 EF 特定的属性是必需。</span><span class="sxs-lookup"><span data-stu-id="2c842-144">The Required annotation tells EF that a particular property is required.</span></span>

<span data-ttu-id="2c842-145">添加所需的 Title 属性将强制 EF （和 MVC） 以确保该属性中包含的数据。</span><span class="sxs-lookup"><span data-stu-id="2c842-145">Adding Required to the Title property will force EF (and MVC) to ensure that the property has data in it.</span></span>

``` csharp
    [Required]
    public string Title { get; set; }
```

<span data-ttu-id="2c842-146">任何其他不使用代码或标记在应用程序中的更改，MVC 应用程序将执行客户端验证，甚至动态生成使用的属性和批注名称的消息。</span><span class="sxs-lookup"><span data-stu-id="2c842-146">With no additional no code or markup changes in the application, an MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![jj591583_figure02](~/ef6/media/jj591583-figure02.png)

<span data-ttu-id="2c842-148">所需的属性也会影响生成的数据库，从而映射的属性不可为 null。</span><span class="sxs-lookup"><span data-stu-id="2c842-148">The Required attribute will also affect the generated database by making the mapped property non-nullable.</span></span> <span data-ttu-id="2c842-149">请注意，标题字段已更改为"not null"。</span><span class="sxs-lookup"><span data-stu-id="2c842-149">Notice that the Title field has changed to “not null”.</span></span>

>[!NOTE]
> <span data-ttu-id="2c842-150">在某些情况下它不可能要为不可为 null，即使该属性是必需的数据库中的列。</span><span class="sxs-lookup"><span data-stu-id="2c842-150">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="2c842-151">例如，当对多个类型使用 TPH 继承策略数据存储在单个表。</span><span class="sxs-lookup"><span data-stu-id="2c842-151">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="2c842-152">如果派生的类型包括必需的属性列不能成为不可为 null 因为层次结构中的不是所有类型都将都具有此属性。</span><span class="sxs-lookup"><span data-stu-id="2c842-152">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>

 

![jj591583_figure03](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a><span data-ttu-id="2c842-154">MaxLength 和 MinLength</span><span class="sxs-lookup"><span data-stu-id="2c842-154">MaxLength and MinLength</span></span>

<span data-ttu-id="2c842-155">MaxLength 和 MinLength 属性，可以指定其他属性验证，像您那样使用所需。</span><span class="sxs-lookup"><span data-stu-id="2c842-155">The MaxLength and MinLength attributes allow you to specify additional property validations, just as you did with Required.</span></span>

<span data-ttu-id="2c842-156">下面是具有长度要求 BloggerName。</span><span class="sxs-lookup"><span data-stu-id="2c842-156">Here is the BloggerName with length requirements.</span></span> <span data-ttu-id="2c842-157">该示例还演示如何组合属性。</span><span class="sxs-lookup"><span data-stu-id="2c842-157">The example also demonstrates how to combine attributes.</span></span>

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="2c842-158">MaxLength 批注会影响数据库的属性的长度设置为 10。</span><span class="sxs-lookup"><span data-stu-id="2c842-158">The MaxLength annotation will impact the database by setting the property’s length to 10.</span></span>

![jj591583_figure04](~/ef6/media/jj591583-figure04.png)

<span data-ttu-id="2c842-160">客户端的批注 MVC 和 EF 4.1 服务器端批注将都遵循此验证，再次动态生成一条错误消息:"字段 BloggerName 具有最大长度为"10"的字符串或数组类型必须是"。该消息是有点长。</span><span class="sxs-lookup"><span data-stu-id="2c842-160">MVC client-side annotation and EF 4.1 server-side annotation will both honor this validation, again dynamically building an error message: “The field BloggerName must be a string or array type with a maximum length of '10'.” That message is a little long.</span></span> <span data-ttu-id="2c842-161">很多批注，可以使用 ErrorMessage 属性指定一条错误消息。</span><span class="sxs-lookup"><span data-stu-id="2c842-161">Many annotations let you specify an error message with the ErrorMessage attribute.</span></span>

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="2c842-162">此外可以在所需的批注中指定错误消息。</span><span class="sxs-lookup"><span data-stu-id="2c842-162">You can also specify ErrorMessage in the Required annotation.</span></span>

![jj591583_figure05](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a><span data-ttu-id="2c842-164">NotMapped</span><span class="sxs-lookup"><span data-stu-id="2c842-164">NotMapped</span></span>

<span data-ttu-id="2c842-165">第一个代码约定决定了是受支持的数据类型的每个属性表示为数据库中。</span><span class="sxs-lookup"><span data-stu-id="2c842-165">Code first convention dictates that every property that is of a supported data type is represented in the database.</span></span> <span data-ttu-id="2c842-166">但这并非总是这种情况在您的应用程序。</span><span class="sxs-lookup"><span data-stu-id="2c842-166">But this isn’t always the case in your applications.</span></span> <span data-ttu-id="2c842-167">例如，可能博客类，用于创建基于字段的标题和 BloggerName 代码中有属性。</span><span class="sxs-lookup"><span data-stu-id="2c842-167">For example you might have a property in the Blog class that creates a code based on the Title and BloggerName fields.</span></span> <span data-ttu-id="2c842-168">该属性可以动态创建实体并且不需要存储。</span><span class="sxs-lookup"><span data-stu-id="2c842-168">That property can be created dynamically and does not need to be stored.</span></span> <span data-ttu-id="2c842-169">可以将标记不会映射到此 BlogCode 属性如 NotMapped 批注与数据库的任何属性。</span><span class="sxs-lookup"><span data-stu-id="2c842-169">You can mark any properties that do not map to the database with the NotMapped annotation such as this BlogCode property.</span></span>

``` csharp
    [NotMapped]
    public string BlogCode
    {
        get
        {
            return Title.Substring(0, 1) + ":" + BloggerName.Substring(0, 1);
        }
    }
```

 

## <a name="complextype"></a><span data-ttu-id="2c842-170">ComplexType</span><span class="sxs-lookup"><span data-stu-id="2c842-170">ComplexType</span></span>

<span data-ttu-id="2c842-171">不常见对域实体跨一组类的描述，然后分层这些类来描述完整实体。</span><span class="sxs-lookup"><span data-stu-id="2c842-171">It’s not uncommon to describe your domain entities across a set of classes and then layer those classes to describe a complete entity.</span></span> <span data-ttu-id="2c842-172">例如，您可以添加到您的模型名 BlogDetails 的类。</span><span class="sxs-lookup"><span data-stu-id="2c842-172">For example, you may add a class called BlogDetails to your model.</span></span>

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="2c842-173">请注意 BlogDetails 不具有任何类型的键属性。</span><span class="sxs-lookup"><span data-stu-id="2c842-173">Notice that BlogDetails does not have any type of key property.</span></span> <span data-ttu-id="2c842-174">在域驱动设计中，BlogDetails 称为值对象。</span><span class="sxs-lookup"><span data-stu-id="2c842-174">In domain driven design, BlogDetails is referred to as a value object.</span></span> <span data-ttu-id="2c842-175">实体框架将值对象称为复杂类型。</span><span class="sxs-lookup"><span data-stu-id="2c842-175">Entity Framework refers to value objects as complex types.</span></span>  <span data-ttu-id="2c842-176">不能自行跟踪复杂类型。</span><span class="sxs-lookup"><span data-stu-id="2c842-176">Complex types cannot be tracked on their own.</span></span>

<span data-ttu-id="2c842-177">但是作为博客类，它将作为博客对象的一部分进行跟踪的 BlogDetails 中的属性。</span><span class="sxs-lookup"><span data-stu-id="2c842-177">However as a property in the Blog class, BlogDetails it will be tracked as part of a Blog object.</span></span> <span data-ttu-id="2c842-178">为了使代码首先将这种情况，你必须将 BlogDetails 类标记为复杂类型。</span><span class="sxs-lookup"><span data-stu-id="2c842-178">In order for code first to recognize this, you must mark the BlogDetails class as a ComplexType.</span></span>

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="2c842-179">现在可以在博客类来表示该博客 BlogDetails 中添加一个属性。</span><span class="sxs-lookup"><span data-stu-id="2c842-179">Now you can add a property in the Blog class to represent the BlogDetails for that blog.</span></span>

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

<span data-ttu-id="2c842-180">在数据库中，博客表将包含的所有博客包括其 BlogDetail 属性中包含的属性的属性。</span><span class="sxs-lookup"><span data-stu-id="2c842-180">In the database, the Blog table will contain all of the properties of the blog including the properties contained in its BlogDetail property.</span></span> <span data-ttu-id="2c842-181">默认情况下，每个前面带有复杂类型，BlogDetail 的名称。</span><span class="sxs-lookup"><span data-stu-id="2c842-181">By default, each one is preceded with the name of the complex type, BlogDetail.</span></span>

![jj591583_figure06](~/ef6/media/jj591583-figure06.png)

<span data-ttu-id="2c842-183">另一个注意事项是，虽然 DateCreated 属性被定义为不可为 null 的日期时间在类中，相关数据库字段是可以为 null。</span><span class="sxs-lookup"><span data-stu-id="2c842-183">Another interesting note is that although the DateCreated property was defined as a non-nullable DateTime in the class, the relevant database field is nullable.</span></span> <span data-ttu-id="2c842-184">如果你想要影响的数据库架构，则必须使用所需的批注。</span><span class="sxs-lookup"><span data-stu-id="2c842-184">You must use the Required annotation if you wish to affect the database schema.</span></span>

 

## <a name="concurrencycheck"></a><span data-ttu-id="2c842-185">ConcurrencyCheck</span><span class="sxs-lookup"><span data-stu-id="2c842-185">ConcurrencyCheck</span></span>

<span data-ttu-id="2c842-186">ConcurrencyCheck 批注可以标记一个或多个要在进行并发检查数据库中，当用户编辑或删除的实体的属性。</span><span class="sxs-lookup"><span data-stu-id="2c842-186">The ConcurrencyCheck annotation allows you to flag one or more properties to be used for concurrency checking in the database when a user edits or deletes an entity.</span></span> <span data-ttu-id="2c842-187">如果您一直使用 EF 设计器，这会将与将属性的 ConcurrencyMode 设置为 Fixed。</span><span class="sxs-lookup"><span data-stu-id="2c842-187">If you've been working with the EF Designer, this aligns with setting a property's ConcurrencyMode to Fixed.</span></span>

<span data-ttu-id="2c842-188">让我们了解 ConcurrencyCheck 通过将其添加到 BloggerName 属性的工作原理。</span><span class="sxs-lookup"><span data-stu-id="2c842-188">Let’s see how ConcurrencyCheck works by adding it to the BloggerName property.</span></span>

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="2c842-189">当调用 SaveChanges 时，由于 ConcurrencyCheck 批注 BloggerName 字段中，将更新中使用该属性的原始值。</span><span class="sxs-lookup"><span data-stu-id="2c842-189">When SaveChanges is called, because of the ConcurrencyCheck annotation on the BloggerName field, the original value of that property will be used in the update.</span></span> <span data-ttu-id="2c842-190">该命令将尝试查找正确的行通过筛选不仅上的密钥值，还在 BloggerName 的原始值。</span><span class="sxs-lookup"><span data-stu-id="2c842-190">The command will attempt to locate the correct row by filtering not only on the key value but also on the original value of BloggerName.</span></span>  <span data-ttu-id="2c842-191">以下是 UPDATE 命令发送到数据库，您可以看到该命令将更新包含 PrimaryTrackingKey 的行的关键部分为 1，该博客从数据库中检索时的原始值的"Julie"BloggerName。</span><span class="sxs-lookup"><span data-stu-id="2c842-191">Here are the critical parts of the UPDATE command sent to the database, where you can see the command will update the row that has a PrimaryTrackingKey is 1 and a BloggerName of “Julie” which was the original value when that blog was retrieved from the database.</span></span>

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

<span data-ttu-id="2c842-192">如果有人已更改该博客的博主名称，请在此期间，此更新将失败，则会将需要处理 DbUpdateConcurrencyException。</span><span class="sxs-lookup"><span data-stu-id="2c842-192">If someone has changed the blogger name for that blog in the meantime, this update will fail and you’ll get a DbUpdateConcurrencyException that you'll need to handle.</span></span>

 

## <a name="timestamp"></a><span data-ttu-id="2c842-193">时间戳</span><span class="sxs-lookup"><span data-stu-id="2c842-193">TimeStamp</span></span>

<span data-ttu-id="2c842-194">它是更常见的是进行并发检查中使用行版本或时间戳字段。</span><span class="sxs-lookup"><span data-stu-id="2c842-194">It's more common to use rowversion or timestamp fields for concurrency checking.</span></span> <span data-ttu-id="2c842-195">但是，而不是使用 ConcurrencyCheck 批注，您可以使用更具体的时间戳批注的属性类型为字节数组。</span><span class="sxs-lookup"><span data-stu-id="2c842-195">But rather than using the ConcurrencyCheck annotation, you can use the more specific TimeStamp annotation as long as the type of the property is byte array.</span></span> <span data-ttu-id="2c842-196">代码首先将相同的处理时间戳属性为 ConcurrencyCheck 属性，但它还将确保代码首先生成的数据库字段是不可为 null。</span><span class="sxs-lookup"><span data-stu-id="2c842-196">Code first will treat Timestamp properties the same as ConcurrencyCheck properties, but it will also ensure that the database field that code first generates is non-nullable.</span></span> <span data-ttu-id="2c842-197">仅可以在给定类中有一个时间戳属性。</span><span class="sxs-lookup"><span data-stu-id="2c842-197">You can only have one timestamp property in a given class.</span></span>

<span data-ttu-id="2c842-198">将以下属性添加到博客类：</span><span class="sxs-lookup"><span data-stu-id="2c842-198">Adding the following property to the Blog class:</span></span>

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

<span data-ttu-id="2c842-199">在代码中首次在数据库表中创建不可以为 null 的时间戳列的结果。</span><span class="sxs-lookup"><span data-stu-id="2c842-199">results in code first creating a non-nullable timestamp column in the database table.</span></span>

![jj591583_figure07](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a><span data-ttu-id="2c842-201">表和列</span><span class="sxs-lookup"><span data-stu-id="2c842-201">Table and Column</span></span>

<span data-ttu-id="2c842-202">如果您让代码优先在创建数据库，可能想要更改的表和列创建的名称。</span><span class="sxs-lookup"><span data-stu-id="2c842-202">If you are letting Code First create the database, you may want to change the name of the tables and columns it is creating.</span></span> <span data-ttu-id="2c842-203">您还可以使用 Code First 与现有数据库。</span><span class="sxs-lookup"><span data-stu-id="2c842-203">You can also use Code First with an existing database.</span></span> <span data-ttu-id="2c842-204">但它并不总是类和你的域中的属性的名称与表和数据库中的列的名称相匹配的情况。</span><span class="sxs-lookup"><span data-stu-id="2c842-204">But it's not always the case that the names of the classes and properties in your domain match the names of the tables and columns in your database.</span></span>

<span data-ttu-id="2c842-205">我的类名为博客，并按照约定，代码首先假定这会映射到名为博客的表。</span><span class="sxs-lookup"><span data-stu-id="2c842-205">My class is named Blog and by convention, code first presumes this will map to a table named Blogs.</span></span> <span data-ttu-id="2c842-206">如果不是这种情况可以使用表属性中指定的表的名称。</span><span class="sxs-lookup"><span data-stu-id="2c842-206">If that's not the case you can specify the name of the table with the Table attribute.</span></span> <span data-ttu-id="2c842-207">此处，批注指定表名 InternalBlogs。</span><span class="sxs-lookup"><span data-stu-id="2c842-207">Here for example, the annotation is specifying that the table name is InternalBlogs.</span></span>

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

<span data-ttu-id="2c842-208">列批注是详细擅长中指定的映射列的特性。</span><span class="sxs-lookup"><span data-stu-id="2c842-208">The Column annotation is a more adept in specifying the attributes of a mapped column.</span></span> <span data-ttu-id="2c842-209">您可以规定名称、 数据类型或甚至一列的表中显示的顺序。</span><span class="sxs-lookup"><span data-stu-id="2c842-209">You can stipulate a name, data type or even the order in which a column appears in the table.</span></span> <span data-ttu-id="2c842-210">下面是列属性的一个示例。</span><span class="sxs-lookup"><span data-stu-id="2c842-210">Here is an example of the Column attribute.</span></span>

``` csharp
    [Column(“BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

<span data-ttu-id="2c842-211">不要混淆列的数据类型 DataAnnotation TypeName 属性。</span><span class="sxs-lookup"><span data-stu-id="2c842-211">Don’t confuse Column’s TypeName attribute with the DataType DataAnnotation.</span></span> <span data-ttu-id="2c842-212">数据类型是 UI 使用一条注释，并忽略由 Code First。</span><span class="sxs-lookup"><span data-stu-id="2c842-212">DataType is an annotation used for the UI and is ignored by Code First.</span></span>

<span data-ttu-id="2c842-213">下面是表后已重新生成。</span><span class="sxs-lookup"><span data-stu-id="2c842-213">Here is the table after it’s been regenerated.</span></span> <span data-ttu-id="2c842-214">表名已更改为 InternalBlogs，从复杂类型说明列现在是 BlogDescription。</span><span class="sxs-lookup"><span data-stu-id="2c842-214">The table name has changed to InternalBlogs and Description column from the complex type is now BlogDescription.</span></span> <span data-ttu-id="2c842-215">因为在批注中指定了名称，但代码首先将不使用的复杂类型的名称开头的列名称的约定。</span><span class="sxs-lookup"><span data-stu-id="2c842-215">Because the name was specified in the annotation, code first will not use the convention of starting the column name with the name of the complex type.</span></span>

![jj591583_figure08](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a><span data-ttu-id="2c842-217">DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="2c842-217">DatabaseGenerated</span></span>

<span data-ttu-id="2c842-218">重要的数据库功能是能够具有计算属性。</span><span class="sxs-lookup"><span data-stu-id="2c842-218">An important database features is the ability to have computed properties.</span></span> <span data-ttu-id="2c842-219">如果您打算映射代码优先类到包含的表的计算列，您不希望实体框架可以尝试更新这些列。</span><span class="sxs-lookup"><span data-stu-id="2c842-219">If you're mapping your Code First classes to tables that contain computed columns, you don't want Entity Framework to try to update those columns.</span></span> <span data-ttu-id="2c842-220">但您希望 EF 从数据库中返回这些值后已插入或更新数据。</span><span class="sxs-lookup"><span data-stu-id="2c842-220">But you do want EF to return those values from the database after you've inserted or updated data.</span></span> <span data-ttu-id="2c842-221">DatabaseGenerated 批注可用于计算枚举以及在类中标记这些属性。</span><span class="sxs-lookup"><span data-stu-id="2c842-221">You can use the DatabaseGenerated annotation to flag those properties in your class along with the Computed enum.</span></span> <span data-ttu-id="2c842-222">其他枚举都是无和标识。</span><span class="sxs-lookup"><span data-stu-id="2c842-222">Other enums are None and Identity.</span></span>

``` csharp
    [DatabaseGenerated(DatabaseGenerationOption.Computed)]
    public DateTime DateCreated { get; set; }
```

<span data-ttu-id="2c842-223">可以使用代码首先生成数据库时生成字节或时间戳列上的数据库，否则您才应使用此时指向现有数据库，因为代码首先将无法确定的公式计算所得的列。</span><span class="sxs-lookup"><span data-stu-id="2c842-223">You can use database generated on byte or timestamp columns when code first is generating the database, otherwise you should only use this when pointing to existing databases because code first won't be able to determine the formula for the computed column.</span></span>

<span data-ttu-id="2c842-224">默认情况下，读取上面是一个整数键属性将成为在数据库中的标识键。</span><span class="sxs-lookup"><span data-stu-id="2c842-224">You read above that by default, a key property that is an integer will become an identity key in the database.</span></span> <span data-ttu-id="2c842-225">这将是与将 DatabaseGenerated 设为 DatabaseGenerationOption.Identity 相同。</span><span class="sxs-lookup"><span data-stu-id="2c842-225">That would be the same as setting DatabaseGenerated to DatabaseGenerationOption.Identity.</span></span> <span data-ttu-id="2c842-226">如果不希望其成为标识键，您可以将值设置为 DatabaseGenerationOption.None。</span><span class="sxs-lookup"><span data-stu-id="2c842-226">If you do not want it to be an identity key, you can set the value to DatabaseGenerationOption.None.</span></span>

 

## <a name="index"></a><span data-ttu-id="2c842-227">索引</span><span class="sxs-lookup"><span data-stu-id="2c842-227">Index</span></span>

> [!NOTE]
> <span data-ttu-id="2c842-228">**EF6.1 及更高版本仅**-Entity Framework 6.1 中引入了索引属性。</span><span class="sxs-lookup"><span data-stu-id="2c842-228">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="2c842-229">如果您使用的是早期版本不适用于此部分中的信息。</span><span class="sxs-lookup"><span data-stu-id="2c842-229">If you are using an earlier version the information in this section does not apply.</span></span>

<span data-ttu-id="2c842-230">可以使用的一个或多个列创建索引**IndexAttribute**。</span><span class="sxs-lookup"><span data-stu-id="2c842-230">You can create an index on one or more columns using the **IndexAttribute**.</span></span> <span data-ttu-id="2c842-231">将特性添加到一个或多个属性将导致 EF 在数据库中创建相应的索引时自动创建该数据库，或创建相应的基架**CreateIndex**调用，如果使用 Code First 迁移。</span><span class="sxs-lookup"><span data-stu-id="2c842-231">Adding the attribute to one or more properties will cause EF to create the corresponding index in the database when it creates the database, or scaffold the corresponding **CreateIndex** calls if you are using Code First Migrations.</span></span>

<span data-ttu-id="2c842-232">例如，下面的代码将导致在创建索引**评级**的列**帖子**数据库表中的。</span><span class="sxs-lookup"><span data-stu-id="2c842-232">For example, the following code will result in an index being created on the **Rating** column of the **Posts** table in the database.</span></span>

``` csharp
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        [Index]
        public int Rating { get; set; }
        public int BlogId { get; set; }
    }
```

<span data-ttu-id="2c842-233">默认情况下，将名为索引**IX\_&lt;属性名称&gt;** (IX\_评级在上面的示例)。</span><span class="sxs-lookup"><span data-stu-id="2c842-233">By default, the index will be named **IX\_&lt;property name&gt;** (IX\_Rating in the above example).</span></span> <span data-ttu-id="2c842-234">但还可以指定索引的名称。</span><span class="sxs-lookup"><span data-stu-id="2c842-234">You can also specify a name for the index though.</span></span> <span data-ttu-id="2c842-235">下面的示例指定该索引，应将命名为**PostRatingIndex**。</span><span class="sxs-lookup"><span data-stu-id="2c842-235">The following example specifies that the index should be named **PostRatingIndex**.</span></span>

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

<span data-ttu-id="2c842-236">默认情况下，索引是非唯一的但你可以使用**IsUnique**名为参数来指定应该唯一索引。</span><span class="sxs-lookup"><span data-stu-id="2c842-236">By default, indexes are non-unique, but you can use the **IsUnique** named parameter to specify that an index should be unique.</span></span> <span data-ttu-id="2c842-237">下面的示例上引入了唯一索引**用户**的登录名。</span><span class="sxs-lookup"><span data-stu-id="2c842-237">The following example introduces a unique index on a **User**'s login name.</span></span>

``` csharp
    public class User
    {
        public int UserId { get; set; }

        [Index(IsUnique = true)]
        [StringLength(200)]
        public string Username { get; set; }

        public string DisplayName { get; set; }
    }
```

### <a name="multiple-column-indexes"></a><span data-ttu-id="2c842-238">多列索引</span><span class="sxs-lookup"><span data-stu-id="2c842-238">Multiple-Column Indexes</span></span>

<span data-ttu-id="2c842-239">跨多个列的索引指定为给定的表在多个索引批注中使用相同的名称。</span><span class="sxs-lookup"><span data-stu-id="2c842-239">Indexes that span multiple columns are specified by using the same name in multiple Index annotations for a given table.</span></span> <span data-ttu-id="2c842-240">创建多列索引时，需要在索引中指定列的顺序。</span><span class="sxs-lookup"><span data-stu-id="2c842-240">When you create multi-column indexes, you need to specify an order for the columns in the index.</span></span> <span data-ttu-id="2c842-241">例如，以下代码将创建在多列索引**评级**并**BlogId**调用**IX\_BlogAndRating**。</span><span class="sxs-lookup"><span data-stu-id="2c842-241">For example, the following code creates a multi-column index on **Rating** and **BlogId** called **IX\_BlogAndRating**.</span></span> <span data-ttu-id="2c842-242">**BlogId**是在索引中的第一列和**评级**是第二个。</span><span class="sxs-lookup"><span data-stu-id="2c842-242">**BlogId** is the first column in the index and **Rating** is the second.</span></span>

``` csharp
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        [Index("IX_BlogIdAndRating", 2)]
        public int Rating { get; set; }
        [Index("IX_BlogIdAndRating", 1)]
        public int BlogId { get; set; }
    }
```

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a><span data-ttu-id="2c842-243">关系属性： InverseProperty 和 ForeignKey</span><span class="sxs-lookup"><span data-stu-id="2c842-243">Relationship Attributes: InverseProperty and ForeignKey</span></span>

> [!NOTE]
> <span data-ttu-id="2c842-244">此页提供了有关设置关系中使用数据注释在 Code First 模型的信息。</span><span class="sxs-lookup"><span data-stu-id="2c842-244">This page provides information about setting up relationships in your Code First model using Data Annotations.</span></span> <span data-ttu-id="2c842-245">有关在 EF 和如何访问和处理数据使用关系的关系的常规信息，请参阅[关系和导航属性](~/ef6/fundamentals/relationships.md)。 \*</span><span class="sxs-lookup"><span data-stu-id="2c842-245">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).\*</span></span>

<span data-ttu-id="2c842-246">第一个代码约定将负责在模型中最常见的关系，但有某些情况下，应帮助在位置。</span><span class="sxs-lookup"><span data-stu-id="2c842-246">Code first convention will take care of the most common relationships in your model, but there are some cases where it needs help.</span></span>

<span data-ttu-id="2c842-247">更改造成的问题及其与文章的博客类中的键属性的名称。</span><span class="sxs-lookup"><span data-stu-id="2c842-247">Changing the name of the key property in the Blog class created a problem with its relationship to Post.</span></span> 

<span data-ttu-id="2c842-248">在生成数据库时，代码将首先看到在开机自检类的 BlogId 属性，并识别它，通过它与类名以及"Id"，匹配作为博客类的外键的约定。</span><span class="sxs-lookup"><span data-stu-id="2c842-248">When generating the database, code first sees the BlogId property in the Post class and recognizes it, by the convention that it matches a class name plus “Id”, as a foreign key to the Blog class.</span></span> <span data-ttu-id="2c842-249">但在博客类中没有任何 BlogId 属性。</span><span class="sxs-lookup"><span data-stu-id="2c842-249">But there is no BlogId property in the blog class.</span></span> <span data-ttu-id="2c842-250">此问题的解决方案是在 Post 中创建导航属性并使用外 DataAnnotation 帮助代码首先了解如何生成两个类之间的关系，使用 Post.BlogId 属性，以及如何指定中的约束数据库。</span><span class="sxs-lookup"><span data-stu-id="2c842-250">The solution for this is to create a navigation property in the Post and use the Foreign DataAnnotation to help code first understand how to build the relationship between the two classes —using the Post.BlogId property — as well as how to specify constraints in the database.</span></span>

``` csharp
    public class Post
    {
            public int Id { get; set; }
            public string Title { get; set; }
            public DateTime DateCreated { get; set; }
            public string Content { get; set; }
            public int BlogId { get; set; }
            [ForeignKey("BlogId")]
            public Blog Blog { get; set; }
            public ICollection<Comment> Comments { get; set; }
    }
```

<span data-ttu-id="2c842-251">在数据库中的约束演示 InternalBlogs.PrimaryTrackingKey 和 Posts.BlogId 之间的关系。</span><span class="sxs-lookup"><span data-stu-id="2c842-251">The constraint in the database shows a relationship between InternalBlogs.PrimaryTrackingKey and Posts.BlogId.</span></span> 

![jj591583_figure09](~/ef6/media/jj591583-figure09.png)

<span data-ttu-id="2c842-253">有多个类之间的关系时，使用 InverseProperty。</span><span class="sxs-lookup"><span data-stu-id="2c842-253">The InverseProperty is used when you have multiple relationships between classes.</span></span>

<span data-ttu-id="2c842-254">在发布类中，你可能想要跟踪谁编写了一篇博客文章的以及谁对其进行编辑。</span><span class="sxs-lookup"><span data-stu-id="2c842-254">In the Post class, you may want to keep track of who wrote a blog post as well as who edited it.</span></span> <span data-ttu-id="2c842-255">以下是 Post 类的两个新的导航属性。</span><span class="sxs-lookup"><span data-stu-id="2c842-255">Here are two new navigation properties for the Post class.</span></span>

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

<span data-ttu-id="2c842-256">此外需要引用这些属性的 Person 类中添加。</span><span class="sxs-lookup"><span data-stu-id="2c842-256">You’ll also need to add in the Person class referenced by these properties.</span></span> <span data-ttu-id="2c842-257">Person 类具有导航属性返回到 Post，一个用于所有的人，一个用于更新由该作者的文章的所有文章。</span><span class="sxs-lookup"><span data-stu-id="2c842-257">The Person class has navigation properties back to the Post, one for all of the posts written by the person and one for all of the posts updated by that person.</span></span>

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

<span data-ttu-id="2c842-258">代码首先不能在其自己的两个类中的属性匹配。</span><span class="sxs-lookup"><span data-stu-id="2c842-258">Code first is not able to match up the properties in the two classes on its own.</span></span> <span data-ttu-id="2c842-259">文章的数据库表的 CreatedBy 人员应有一个外键和一个用于 UpdatedBy 人但代码首先将创建四个将外键属性： Person\_Id、 人\_Id1、 CreatedBy\_Id 和UpdatedBy\_id。</span><span class="sxs-lookup"><span data-stu-id="2c842-259">The database table for Posts should have one foreign key for the CreatedBy person and one for the UpdatedBy person but code first will create four will foreign key properties: Person\_Id, Person\_Id1, CreatedBy\_Id and UpdatedBy\_Id.</span></span>

![jj591583_figure10](~/ef6/media/jj591583-figure10.png)

<span data-ttu-id="2c842-261">若要解决这些问题，可以使用 InverseProperty 批注指定属性的对齐方式。</span><span class="sxs-lookup"><span data-stu-id="2c842-261">To fix these problems, you can use the InverseProperty annotation to specify the alignment of the properties.</span></span>

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

<span data-ttu-id="2c842-262">亲自 PostsWritten 属性就知道，这是指 Post 类型，因为它将生成与 Post.CreatedBy 之间的关系。</span><span class="sxs-lookup"><span data-stu-id="2c842-262">Because the PostsWritten property in Person knows that this refers to the Post type, it will build the relationship to Post.CreatedBy.</span></span> <span data-ttu-id="2c842-263">同样，PostsUpdated 将连接到 Post.UpdatedBy。</span><span class="sxs-lookup"><span data-stu-id="2c842-263">Similarly, PostsUpdated will be connected to Post.UpdatedBy.</span></span> <span data-ttu-id="2c842-264">且代码首先会创建额外外键。</span><span class="sxs-lookup"><span data-stu-id="2c842-264">And code first will not create the extra foreign keys.</span></span>

![jj591583_figure11](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a><span data-ttu-id="2c842-266">总结</span><span class="sxs-lookup"><span data-stu-id="2c842-266">Summary</span></span>

<span data-ttu-id="2c842-267">DataAnnotations 不仅允许您描述客户端和服务器端验证在您的代码的第一个类，但它们还使你可以增强和甚至更正代码首先将使有关基于其约定类的假设。</span><span class="sxs-lookup"><span data-stu-id="2c842-267">DataAnnotations not only let you describe client and server side validation in your code first classes, but they also allow you to enhance and even correct the assumptions that code first will make about your classes based on its conventions.</span></span> <span data-ttu-id="2c842-268">使用 DataAnnotations，可不只让数据库架构生成，但还可以将您的代码的第一类映射到预先存在的数据库。</span><span class="sxs-lookup"><span data-stu-id="2c842-268">With DataAnnotations you can not only drive database schema generation, but you can also map your code first classes to a pre-existing database.</span></span>

<span data-ttu-id="2c842-269">尽管它们非常灵活，请记住，DataAnnotations 提供仅大多数通常所需的配置更改可以使您的代码的第一个类。</span><span class="sxs-lookup"><span data-stu-id="2c842-269">While they are very flexible, keep in mind that DataAnnotations provide only the most commonly needed configuration changes you can make on your code first classes.</span></span> <span data-ttu-id="2c842-270">若要配置您的类的一些边缘情况，您应查找的备用配置机制，Code First Fluent API。</span><span class="sxs-lookup"><span data-stu-id="2c842-270">To configure your classes for some of the edge cases, you should look to the alternate configuration mechanism, Code First’s Fluent API .</span></span>
