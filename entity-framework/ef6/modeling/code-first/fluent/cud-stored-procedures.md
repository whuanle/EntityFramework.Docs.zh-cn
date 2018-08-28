---
title: 代码的第一个 Insert、 Update 和 Delete 存储的过程-EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 9a7ae7f9-4072-4843-877d-506dd7eef576
ms.openlocfilehash: a0448fb44dabb2e03b2358aa7b4f69d92cffa15a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994568"
---
# <a name="code-first-insert-update-and-delete-stored-procedures"></a><span data-ttu-id="a9301-102">代码的第一个 Insert、 Update 和 Delete 存储的过程</span><span class="sxs-lookup"><span data-stu-id="a9301-102">Code First Insert, Update, and Delete Stored Procedures</span></span>
> [!NOTE]
> <span data-ttu-id="a9301-103">**仅限 EF6 及更高版本** - 此页面中讨论的功能、API 等已引入实体框架 6。</span><span class="sxs-lookup"><span data-stu-id="a9301-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="a9301-104">如果使用的是早期版本，则部分或全部信息不适用。</span><span class="sxs-lookup"><span data-stu-id="a9301-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="a9301-105">默认情况下，Code First 将配置所有实体，以执行插入、 更新和删除命令使用表的直接访问。</span><span class="sxs-lookup"><span data-stu-id="a9301-105">By default, Code First will configure all entities to perform insert, update and delete commands using direct table access.</span></span> <span data-ttu-id="a9301-106">开始在 EF6 中可以配置 Code First 模型的存储的过程用于在模型中的某些或所有实体。</span><span class="sxs-lookup"><span data-stu-id="a9301-106">Starting in EF6 you can configure your Code First model to use stored procedures for some or all entities in your model.</span></span>  

## <a name="basic-entity-mapping"></a><span data-ttu-id="a9301-107">基本实体映射</span><span class="sxs-lookup"><span data-stu-id="a9301-107">Basic Entity Mapping</span></span>  

<span data-ttu-id="a9301-108">您可以选择使用存储的过程为插入、 更新和删除使用 Fluent API。</span><span class="sxs-lookup"><span data-stu-id="a9301-108">You can opt into using stored procedures for insert, update and delete using the Fluent API.</span></span>  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

<span data-ttu-id="a9301-109">执行此操作将导致代码优先，一些约定用于生成的数据库中的存储过程预期的形状。</span><span class="sxs-lookup"><span data-stu-id="a9301-109">Doing this will cause Code First to use some conventions to build the expected shape of the stored procedures in the database.</span></span>  

- <span data-ttu-id="a9301-110">三个存储过程名为 **\<type_name\>（_i)**，  **\<type_name\>（_u)** 并**\<得以名称\>（_d)** （例如，Blog_Insert、 Blog_Update 和 Blog_Delete）。</span><span class="sxs-lookup"><span data-stu-id="a9301-110">Three stored procedures named **\<type_name\>_Insert**, **\<type_name\>_Update** and **\<type_name\>_Delete** (for example, Blog_Insert, Blog_Update and Blog_Delete).</span></span>  
- <span data-ttu-id="a9301-111">参数名称对应于属性名称。</span><span class="sxs-lookup"><span data-stu-id="a9301-111">Parameter names correspond to the property names.</span></span>  
  > [!NOTE]
  > <span data-ttu-id="a9301-112">如果使用 HasColumnName() 或列属性设置为给定属性列重命名此名称用于而不是属性名称的参数。</span><span class="sxs-lookup"><span data-stu-id="a9301-112">If you use HasColumnName() or the Column attribute to rename the column for a given property then this name is used for parameters instead of the property name.</span></span>  
- <span data-ttu-id="a9301-113">**Insert 存储过程**将具有每个属性，除了那些已标记为存储生成的参数 (标识或计算)。</span><span class="sxs-lookup"><span data-stu-id="a9301-113">**The insert stored procedure** will have a parameter for every property, except for those marked as store generated (identity or computed).</span></span> <span data-ttu-id="a9301-114">存储的过程应返回包含每个生成的存储属性的列的结果集。</span><span class="sxs-lookup"><span data-stu-id="a9301-114">The stored procedure should return a result set with a column for each store generated property.</span></span>  
- <span data-ttu-id="a9301-115">**更新存储过程**将具有每个属性，除了那些标有 Computed 生成的存储模式的参数。</span><span class="sxs-lookup"><span data-stu-id="a9301-115">**The update stored procedure** will have a parameter for every property, except for those marked with a store generated pattern of 'Computed'.</span></span> <span data-ttu-id="a9301-116">一些并发标记的原始值需要一个参数，请参阅*并发标记*部分获取详细信息。</span><span class="sxs-lookup"><span data-stu-id="a9301-116">Some concurrency tokens require a parameter for the original value, see the *Concurrency Tokens* section below for details.</span></span> <span data-ttu-id="a9301-117">存储的过程应返回包含每个计算属性的列的结果集。</span><span class="sxs-lookup"><span data-stu-id="a9301-117">The stored procedure should return a result set with a column for each computed property.</span></span>  
- <span data-ttu-id="a9301-118">**删除存储过程**应具有的键值的实体 （或者如果该实体具有复合键的多个参数） 的参数。</span><span class="sxs-lookup"><span data-stu-id="a9301-118">**The delete stored procedure** should have a parameter for the key value of the entity (or multiple parameters if the entity has a composite key).</span></span> <span data-ttu-id="a9301-119">此外，删除过程还应为所有独立关联中的外键的参数对目标表 （没有声明的实体中的相应外键属性的关系）。</span><span class="sxs-lookup"><span data-stu-id="a9301-119">Additionally, the delete procedure should also have parameters for any independent association foreign keys on the target table (relationships that do not have corresponding foreign key properties declared in the entity).</span></span> <span data-ttu-id="a9301-120">一些并发标记的原始值需要一个参数，请参阅*并发标记*部分获取详细信息。</span><span class="sxs-lookup"><span data-stu-id="a9301-120">Some concurrency tokens require a parameter for the original value, see the *Concurrency Tokens* section below for details.</span></span>  

<span data-ttu-id="a9301-121">例如，使用以下类：</span><span class="sxs-lookup"><span data-stu-id="a9301-121">Using the following class as an example:</span></span>  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
}
```  

<span data-ttu-id="a9301-122">默认值是存储的过程：</span><span class="sxs-lookup"><span data-stu-id="a9301-122">The default stored procedures would be:</span></span>  

``` SQL
CREATE PROCEDURE [dbo].[Blog_Insert]  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
BEGIN
  INSERT INTO [dbo].[Blogs] ([Name], [Url])
  VALUES (@Name, @Url)

  SELECT SCOPE_IDENTITY() AS BlogId
END
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId;
CREATE PROCEDURE [dbo].[Blog_Delete]  
  @BlogId int  
AS  
  DELETE FROM [dbo].[Blogs]
  WHERE BlogId = @BlogId
```  

### <a name="overriding-the-defaults"></a><span data-ttu-id="a9301-123">重写默认值</span><span class="sxs-lookup"><span data-stu-id="a9301-123">Overriding the Defaults</span></span>  

<span data-ttu-id="a9301-124">您可以重写部分或全部默认情况下已配置的内容。</span><span class="sxs-lookup"><span data-stu-id="a9301-124">You can override part or all of what was configured by default.</span></span>  

<span data-ttu-id="a9301-125">您可以更改一个或多个存储过程的名称。</span><span class="sxs-lookup"><span data-stu-id="a9301-125">You can change the name of one or more stored procedures.</span></span> <span data-ttu-id="a9301-126">此示例将仅更新存储过程。</span><span class="sxs-lookup"><span data-stu-id="a9301-126">This example renames the update stored procedure only.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")));
```  

<span data-ttu-id="a9301-127">此示例将所有三个存储的过程。</span><span class="sxs-lookup"><span data-stu-id="a9301-127">This example renames all three stored procedures.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog"))  
     .Delete(d => d.HasName("delete_blog"))  
     .Insert(i => i.HasName("insert_blog")));
```  

<span data-ttu-id="a9301-128">在这些示例中调用链接在一起，但您也可以使用 lambda 块语法。</span><span class="sxs-lookup"><span data-stu-id="a9301-128">In these examples the calls are chained together, but you can also use lambda block syntax.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    {  
      s.Update(u => u.HasName("modify_blog"));  
      s.Delete(d => d.HasName("delete_blog"));  
      s.Insert(i => i.HasName("insert_blog"));  
    });
```  

<span data-ttu-id="a9301-129">此示例将更新存储过程的 BlogId 属性参数。</span><span class="sxs-lookup"><span data-stu-id="a9301-129">This example renames the parameter for the BlogId property on the update stored procedure.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

<span data-ttu-id="a9301-130">这些调用是所有可链接和可组合。</span><span class="sxs-lookup"><span data-stu-id="a9301-130">These calls are all chainable and composable.</span></span> <span data-ttu-id="a9301-131">下面是示例重命名所有三个存储的过程和其参数。</span><span class="sxs-lookup"><span data-stu-id="a9301-131">Here is an example that renames all three stored procedures and their parameters.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")  
                   .Parameter(b => b.BlogId, "blog_id")  
                   .Parameter(b => b.Name, "blog_name")  
                   .Parameter(b => b.Url, "blog_url"))  
     .Delete(d => d.HasName("delete_blog")  
                   .Parameter(b => b.BlogId, "blog_id"))  
     .Insert(i => i.HasName("insert_blog")  
                   .Parameter(b => b.Name, "blog_name")  
                   .Parameter(b => b.Url, "blog_url")));
```  

<span data-ttu-id="a9301-132">此外可以更改中包含的数据库生成值的结果集列的名称。</span><span class="sxs-lookup"><span data-stu-id="a9301-132">You can also change the name of the columns in the result set that contains database generated values.</span></span>  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures(s =>
    s.Insert(i => i.Result(b => b.BlogId, "generated_blog_identity")));
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Insert]  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
BEGIN
  INSERT INTO [dbo].[Blogs] ([Name], [Url])
  VALUES (@Name, @Url)

  SELECT SCOPE_IDENTITY() AS generated_blog_id
END
```  

## <a name="relationships-without-a-foreign-key-in-the-class-independent-associations"></a><span data-ttu-id="a9301-133">而无需类 （独立关联） 中的外键关系</span><span class="sxs-lookup"><span data-stu-id="a9301-133">Relationships Without a Foreign Key in the Class (Independent Associations)</span></span>  

<span data-ttu-id="a9301-134">当在类定义中包括外键属性时，可以为任何其他属性相同的方式重命名的相应参数。</span><span class="sxs-lookup"><span data-stu-id="a9301-134">When a foreign key property is included in the class definition, the corresponding parameter can be renamed in the same way as any other property.</span></span> <span data-ttu-id="a9301-135">当没有外键属性的类中存在的关系时，默认参数名称是 **\<navigation_property_name\>_\<primary_key_name\>**。</span><span class="sxs-lookup"><span data-stu-id="a9301-135">When a relationship exists without a foreign key property in the class, the default parameter name is **\<navigation_property_name\>_\<primary_key_name\>**.</span></span>  

<span data-ttu-id="a9301-136">例如，以下类定义将导致存储过程以插入和更新文章中所预期的 Blog_BlogId 参数。</span><span class="sxs-lookup"><span data-stu-id="a9301-136">For example, the following class definitions would result in a Blog_BlogId parameter being expected in the stored procedures to insert and update Posts.</span></span>  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }

  public List<Post> Posts { get; set; }  
}  

public class Post  
{  
  public int PostId { get; set; }  
  public string Title { get; set; }  
  public string Content { get; set; }  

  public Blog Blog { get; set; }  
}
```  

### <a name="overriding-the-defaults"></a><span data-ttu-id="a9301-137">重写默认值</span><span class="sxs-lookup"><span data-stu-id="a9301-137">Overriding the Defaults</span></span>  

<span data-ttu-id="a9301-138">可以更改参数的情况下提供对参数方法主键属性的路径不包括在类中的外键。</span><span class="sxs-lookup"><span data-stu-id="a9301-138">You can change parameters for foreign keys that are not included in the class by supplying the path to the primary key property to the Parameter method.</span></span>  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

<span data-ttu-id="a9301-139">如果依赖实体 （即没有导航属性</span><span class="sxs-lookup"><span data-stu-id="a9301-139">If you don’t have a navigation property on the dependent entity (i.e</span></span> <span data-ttu-id="a9301-140">没有 Post.Blog 属性） 则可以使用关联方法来确定此关系的另一端，然后配置参数对应于每个键属性。</span><span class="sxs-lookup"><span data-stu-id="a9301-140">no Post.Blog property) then you can use the Association method to identify the other end of the relationship and then configure the parameters that correspond to each of the key property(s).</span></span>  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a><span data-ttu-id="a9301-141">并发标记</span><span class="sxs-lookup"><span data-stu-id="a9301-141">Concurrency Tokens</span></span>  

<span data-ttu-id="a9301-142">Update 和 delete 存储过程可能还需要并发处理：</span><span class="sxs-lookup"><span data-stu-id="a9301-142">Update and delete stored procedures may also need to deal with concurrency:</span></span>  

- <span data-ttu-id="a9301-143">如果该实体包含并发标记，该存储的过程可以具有一个输出参数，它返回的行更新/删除 （受影响的行） 数。</span><span class="sxs-lookup"><span data-stu-id="a9301-143">If the entity contains concurrency tokens, the stored procedure can optionally have an output parameter that returns the number of rows updated/deleted (rows affected).</span></span> <span data-ttu-id="a9301-144">此类参数必须使用 RowsAffectedParameter 方法进行配置。</span><span class="sxs-lookup"><span data-stu-id="a9301-144">Such a parameter must be configured using the RowsAffectedParameter method.</span></span>  
<span data-ttu-id="a9301-145">默认情况下 EF 使用 ExecuteNonQuery 的返回值来确定受影响行数。</span><span class="sxs-lookup"><span data-stu-id="a9301-145">By default EF uses the return value from ExecuteNonQuery to determine how many rows were affected.</span></span> <span data-ttu-id="a9301-146">指定受影响的行的输出参数非常有用，如果你将导致 ExecuteNonQuery 正在 （从 EF 的角度来看） 不正确的返回值的存储过程中执行的任何逻辑执行结束时。</span><span class="sxs-lookup"><span data-stu-id="a9301-146">Specifying a rows affected output parameter is useful if you perform any logic in your sproc that would result in the return value of ExecuteNonQuery being incorrect (from EF's perspective) at the end of execution.</span></span>  
- <span data-ttu-id="a9301-147">对于每个并发令牌存在将名为的参数 **\<property_name\>_Original** (例如，Timestamp_Original)。</span><span class="sxs-lookup"><span data-stu-id="a9301-147">For each concurrency token there will be a parameter named **\<property_name\>_Original** (for example, Timestamp_Original ).</span></span> <span data-ttu-id="a9301-148">这将传递此属性-值时从数据库中查询的原始值。</span><span class="sxs-lookup"><span data-stu-id="a9301-148">This will be passed the original value of this property – the value when queried from the database.</span></span>  
    - <span data-ttu-id="a9301-149">由数据库，– 例如，时间戳 – 计算的并发标记将只能有一个原始值参数。</span><span class="sxs-lookup"><span data-stu-id="a9301-149">Concurrency tokens that are computed by the database – such as timestamps – will only have an original value parameter.</span></span>  
    - <span data-ttu-id="a9301-150">非计算的属性设置为并发标记还必须在更新过程中的新值的参数。</span><span class="sxs-lookup"><span data-stu-id="a9301-150">Non-computed properties that are set as concurrency tokens will also have a parameter for the new value in the update procedure.</span></span> <span data-ttu-id="a9301-151">这将使用已讨论过的新值的命名约定。</span><span class="sxs-lookup"><span data-stu-id="a9301-151">This uses the naming conventions already discussed for new values.</span></span> <span data-ttu-id="a9301-152">此类令牌的一个示例将使用博客的 URL 作为并发标记，新值是必需的因为这可以为新值更新，你的代码 （与不同的时间戳标记仅更新一次数据库）。</span><span class="sxs-lookup"><span data-stu-id="a9301-152">An example of such a token would be using a Blog's URL as a concurrency token, the new value is required because this can be updated to a new value by your code (unlike a Timestamp token which is only updated by the database).</span></span>  

<span data-ttu-id="a9301-153">这是一个示例类，并使用时间戳并发标记更新存储的过程。</span><span class="sxs-lookup"><span data-stu-id="a9301-153">This is an example class and update stored procedure with a timestamp concurrency token.</span></span>  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
  [Timestamp]
  public byte[] Timestamp { get; set; }
}
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max),
  @Timestamp_Original rowversion  
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId AND [Timestamp] = @Timestamp_Original
```  

<span data-ttu-id="a9301-154">下面是一个示例类，并使用非计算并发标记更新存储的过程。</span><span class="sxs-lookup"><span data-stu-id="a9301-154">Here is an example class and update stored procedure with non-computed concurrency token.</span></span>  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  [ConcurrencyCheck]
  public string Url { get; set; }  
}
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max),
  @Url_Original nvarchar(max),
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId AND [Url] = @Url_Original
```  

### <a name="overriding-the-defaults"></a><span data-ttu-id="a9301-155">重写默认值</span><span class="sxs-lookup"><span data-stu-id="a9301-155">Overriding the Defaults</span></span>  

<span data-ttu-id="a9301-156">您可以根据需要引入受影响的行数参数。</span><span class="sxs-lookup"><span data-stu-id="a9301-156">You can optionally introduce a rows affected parameter.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

<span data-ttu-id="a9301-157">对计算的数据库并发令牌 – 其中仅的原始值将传递 – 只需可以使用重命名机制的标准参数重命名的原始值的参数。</span><span class="sxs-lookup"><span data-stu-id="a9301-157">For database computed concurrency tokens – where only the original value is passed – you can just use the standard parameter renaming mechanism to rename the parameter for the original value.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

<span data-ttu-id="a9301-158">为 – 个同时的原始和新值传递 – 非计算并发令牌可以使用可用于提供每个参数的名称的参数的重载。</span><span class="sxs-lookup"><span data-stu-id="a9301-158">For non-computed concurrency tokens – where both the original and new value are passed – you can use an overload of Parameter that allows you to supply a name for each parameter.</span></span>  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a><span data-ttu-id="a9301-159">多对多关系</span><span class="sxs-lookup"><span data-stu-id="a9301-159">Many to Many Relationships</span></span>  

<span data-ttu-id="a9301-160">例如，在本部分中，我们将使用以下类。</span><span class="sxs-lookup"><span data-stu-id="a9301-160">We’ll use the following classes as an example in this section.</span></span>  

``` csharp
public class Post  
{  
  public int PostId { get; set; }  
  public string Title { get; set; }  
  public string Content { get; set; }  

  public List<Tag> Tags { get; set; }  
}  

public class Tag  
{  
  public int TagId { get; set; }  
  public string TagName { get; set; }  

  public List<Post> Posts { get; set; }  
}
```  

<span data-ttu-id="a9301-161">多对多个关系可以映射到具有以下语法的存储过程。</span><span class="sxs-lookup"><span data-stu-id="a9301-161">Many to many relationships can be mapped to stored procedures with the following syntax.</span></span>  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

<span data-ttu-id="a9301-162">如果不提供任何其他配置，则默认情况下使用以下存储的过程形式。</span><span class="sxs-lookup"><span data-stu-id="a9301-162">If no other configuration is supplied then the following stored procedure shape is used by default.</span></span>  

- <span data-ttu-id="a9301-163">两个存储过程名为**\<类型 — —\>\<type_two\>（_i)** 并**\<类型 — —\>\<type_two\>（_d)** （例如，PostTag_Insert 和 PostTag_Delete）。</span><span class="sxs-lookup"><span data-stu-id="a9301-163">Two stored procedures named **\<type_one\>\<type_two\>_Insert** and **\<type_one\>\<type_two\>_Delete** (for example, PostTag_Insert and PostTag_Delete).</span></span>  
- <span data-ttu-id="a9301-164">参数将为每种类型的密钥值。</span><span class="sxs-lookup"><span data-stu-id="a9301-164">The parameters will be the key value(s) for each type.</span></span> <span data-ttu-id="a9301-165">每个参数的名称**\<type_name\>_\<property_name\>** （例如，Post_PostId 和 Tag_TagId）。</span><span class="sxs-lookup"><span data-stu-id="a9301-165">The name of each parameter being **\<type_name\>_\<property_name\>** (for example, Post_PostId and Tag_TagId).</span></span>

<span data-ttu-id="a9301-166">下面是示例插入和更新存储的过程。</span><span class="sxs-lookup"><span data-stu-id="a9301-166">Here are example insert and update stored procedures.</span></span>  

``` SQL
CREATE PROCEDURE [dbo].[PostTag_Insert]  
  @Post_PostId int,  
  @Tag_TagId int  
AS  
  INSERT INTO [dbo].[Post_Tags] (Post_PostId, Tag_TagId)   
  VALUES (@Post_PostId, @Tag_TagId)
CREATE PROCEDURE [dbo].[PostTag_Delete]  
  @Post_PostId int,  
  @Tag_TagId int  
AS  
  DELETE FROM [dbo].[Post_Tags]    
  WHERE Post_PostId = @Post_PostId AND Tag_TagId = @Tag_TagId
```  

### <a name="overriding-the-defaults"></a><span data-ttu-id="a9301-167">重写默认值</span><span class="sxs-lookup"><span data-stu-id="a9301-167">Overriding the Defaults</span></span>  

<span data-ttu-id="a9301-168">可以在类似于实体存储过程中配置的过程和参数名称。</span><span class="sxs-lookup"><span data-stu-id="a9301-168">The procedure and parameter names can be configured in a similar way to entity stored procedures.</span></span>  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.HasName("add_post_tag")  
                   .LeftKeyParameter(p => p.PostId, "post_id")  
                   .RightKeyParameter(t => t.TagId, "tag_id"))  
     .Delete(d => d.HasName("remove_post_tag")  
                   .LeftKeyParameter(p => p.PostId, "post_id")  
                   .RightKeyParameter(t => t.TagId, "tag_id")));
```  
