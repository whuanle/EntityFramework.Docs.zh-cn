---
title: 存储过程具有多个结果集的 EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 1b3797f9-cd3d-4752-a55e-47b84b399dc1
ms.openlocfilehash: bb104ac5f584d26d279259a173de9afe3f018968
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996170"
---
# <a name="stored-procedures-with-multiple-result-sets"></a><span data-ttu-id="6b394-102">具有多个结果集的存储的过程</span><span class="sxs-lookup"><span data-stu-id="6b394-102">Stored Procedures with Multiple Result Sets</span></span>
<span data-ttu-id="6b394-103">有时，当使用存储时设置过程将需要返回多个结果。</span><span class="sxs-lookup"><span data-stu-id="6b394-103">Sometimes when using stored procedures you will need to return more than one result set.</span></span> <span data-ttu-id="6b394-104">此方案通常用于减少数据库的所需编写单个屏幕之间的往返。</span><span class="sxs-lookup"><span data-stu-id="6b394-104">This scenario is commonly used to reduce the number of database round trips required to compose a single screen.</span></span> <span data-ttu-id="6b394-105">在 EF5 之前, Entity Framework 将允许要调用的存储的过程，但将仅返回设置为调用代码的第一个结果。</span><span class="sxs-lookup"><span data-stu-id="6b394-105">Prior to EF5, Entity Framework would allow the stored procedure to be called but would only return the first result set to the calling code.</span></span>

<span data-ttu-id="6b394-106">本文将说明可用于从 Entity Framework 中的存储过程访问多个结果集的两种方法。</span><span class="sxs-lookup"><span data-stu-id="6b394-106">This article will show you two ways that you can use to access more than one result set from a stored procedure in Entity Framework.</span></span> <span data-ttu-id="6b394-107">一个使用只是代码，并首先适用于这两个代码和 EF 设计器，仅可与 EF 设计器使用。</span><span class="sxs-lookup"><span data-stu-id="6b394-107">One that uses just code and works with both Code first and the EF Designer and one that only works with the EF Designer.</span></span> <span data-ttu-id="6b394-108">工具和 API 的支持在将来多应提高 Entity Framework 版本。</span><span class="sxs-lookup"><span data-stu-id="6b394-108">The tooling and API support for this should improve in future versions of Entity Framework.</span></span>

## <a name="model"></a><span data-ttu-id="6b394-109">模型</span><span class="sxs-lookup"><span data-stu-id="6b394-109">Model</span></span>

<span data-ttu-id="6b394-110">这篇文章中的示例使用一个基本博客和所属单一博客文章模型一个博客，其中有许多文章和 post。</span><span class="sxs-lookup"><span data-stu-id="6b394-110">The examples in this article use a basic Blog and Posts model where a blog has many posts and a post belongs to a single blog.</span></span> <span data-ttu-id="6b394-111">返回所有博客和文章，如下所示在数据库中，我们将使用存储的过程：</span><span class="sxs-lookup"><span data-stu-id="6b394-111">We will use a stored procedure in the database that returns all blogs and posts, something like this:</span></span>

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a><span data-ttu-id="6b394-112">使用代码访问多个结果集</span><span class="sxs-lookup"><span data-stu-id="6b394-112">Accessing Multiple Result Sets with Code</span></span>

<span data-ttu-id="6b394-113">我们可以执行使用代码以发出原始的 SQL 命令以执行存储的过程。</span><span class="sxs-lookup"><span data-stu-id="6b394-113">We can execute use code to issue a raw SQL command to execute our stored procedure.</span></span> <span data-ttu-id="6b394-114">此方法的优点是，它适用于这两个代码首先和 EF 设计器。</span><span class="sxs-lookup"><span data-stu-id="6b394-114">The advantage of this approach is that it works with both Code first and the EF Designer.</span></span>

<span data-ttu-id="6b394-115">若要获取多个结果设置的工作，我们需要放置到 ObjectContext API 由使用 IObjectContextAdapter 接口。</span><span class="sxs-lookup"><span data-stu-id="6b394-115">In order to get multiple result sets working we need to drop to the ObjectContext API by using the IObjectContextAdapter interface.</span></span>

<span data-ttu-id="6b394-116">一旦我们有 ObjectContext，则我们可以使用转换方法来将我们存储过程的结果转换为可以跟踪并像平常一样在 EF 中使用的实体。</span><span class="sxs-lookup"><span data-stu-id="6b394-116">Once we have an ObjectContext then we can use the Translate method to translate the results of our stored procedure into entities that can be tracked and used in EF as normal.</span></span> <span data-ttu-id="6b394-117">下面的代码示例演示此操作在操作中。</span><span class="sxs-lookup"><span data-stu-id="6b394-117">The following code sample demonstrates this in action.</span></span>

``` csharp
    using (var db = new BloggingContext())
    {
        // If using Code First we need to make sure the model is built before we open the connection
        // This isn't required for models created with the EF Designer
        db.Database.Initialize(force: false);

        // Create a SQL command to execute the sproc
        var cmd = db.Database.Connection.CreateCommand();
        cmd.CommandText = "[dbo].[GetAllBlogsAndPosts]";

        try
        {

            db.Database.Connection.Open();
            // Run the sproc
            var reader = cmd.ExecuteReader();

            // Read Blogs from the first result set
            var blogs = ((IObjectContextAdapter)db)
                .ObjectContext
                .Translate<Blog>(reader, "Blogs", MergeOption.AppendOnly);   


            foreach (var item in blogs)
            {
                Console.WriteLine(item.Name);
            }        

            // Move to second result set and read Posts
            reader.NextResult();
            var posts = ((IObjectContextAdapter)db)
                .ObjectContext
                .Translate<Post>(reader, "Posts", MergeOption.AppendOnly);


            foreach (var item in posts)
            {
                Console.WriteLine(item.Title);
            }
        }
        finally
        {
            db.Database.Connection.Close();
        }
    }
```

<span data-ttu-id="6b394-118">Translate 方法接受我们执行该过程，一个实体集的名称和 MergeOption 时我们收到的读取器。</span><span class="sxs-lookup"><span data-stu-id="6b394-118">The Translate method accepts the reader that we received when we executed the procedure, an EntitySet name, and a MergeOption.</span></span> <span data-ttu-id="6b394-119">EntitySet 名称将是派生上下文的 DbSet 属性相同。</span><span class="sxs-lookup"><span data-stu-id="6b394-119">The EntitySet name will be the same as the DbSet property on your derived context.</span></span> <span data-ttu-id="6b394-120">MergeOption 枚举控制如果同一个实体已存在于内存中处理结果的方式。</span><span class="sxs-lookup"><span data-stu-id="6b394-120">The MergeOption enum controls how results are handled if the same entity already exists in memory.</span></span>

<span data-ttu-id="6b394-121">此处我们循环访问集合的博客然后我们调用 NextResult，这一点很重要给定上面的代码中因为移到下一个结果集之前必须使用第一个结果集。</span><span class="sxs-lookup"><span data-stu-id="6b394-121">Here we iterate through the collection of blogs before we call NextResult, this is important given the above code because the first result set must be consumed before moving to the next result set.</span></span>

<span data-ttu-id="6b394-122">后两个转换，然后在博客和文章实体被 EF 跟踪方式与任何其他实体相同，因此可以修改或删除并另存为正常调用方法。</span><span class="sxs-lookup"><span data-stu-id="6b394-122">Once the two translate methods are called then the Blog and Post entities are tracked by EF the same way as any other entity and so can be modified or deleted and saved as normal.</span></span>

>[!NOTE]
> <span data-ttu-id="6b394-123">创建使用转换方法的实体时，EF 不会考虑任何映射。</span><span class="sxs-lookup"><span data-stu-id="6b394-123">EF does not take any mapping into account when it creates entities using the Translate method.</span></span> <span data-ttu-id="6b394-124">它只需将匹配的结果集的属性名称与您的类中的列名称。</span><span class="sxs-lookup"><span data-stu-id="6b394-124">It will simply match column names in the result set with property names on your classes.</span></span>

>[!NOTE]
> <span data-ttu-id="6b394-125">如果已启用，延迟加载访问文章属性上的博客实体之一中的 EF 将连接到数据库延迟加载所有发布的文章，即使我们已加载所有这些也是如此。</span><span class="sxs-lookup"><span data-stu-id="6b394-125">That if you have lazy loading enabled, accessing the posts property on one of the blog entities then EF will connect to the database to lazily load all posts, even though we have already loaded them all.</span></span> <span data-ttu-id="6b394-126">这是因为 EF 无法知道已加载所有帖子或如果有多个数据库中。</span><span class="sxs-lookup"><span data-stu-id="6b394-126">This is because EF cannot know whether or not you have loaded all posts or if there are more in the database.</span></span> <span data-ttu-id="6b394-127">如果你想要避免这种情况将需要禁用延迟加载。</span><span class="sxs-lookup"><span data-stu-id="6b394-127">If you want to avoid this then you will need to disable lazy loading.</span></span>

## <a name="multiple-result-sets-with-configured-in-edmx"></a><span data-ttu-id="6b394-128">使用 EDMX 时配置中的多个结果集</span><span class="sxs-lookup"><span data-stu-id="6b394-128">Multiple Result Sets with Configured in EDMX</span></span>

>[!NOTE]
> <span data-ttu-id="6b394-129">必须针对.NET Framework 4.5，以便能够在 EDMX 中配置多个结果集。</span><span class="sxs-lookup"><span data-stu-id="6b394-129">You must target .NET Framework 4.5 to be able to configure multiple result sets in EDMX.</span></span> <span data-ttu-id="6b394-130">如果你正面向.NET 4.0，可以使用在上一部分中所示的基于代码的方法。</span><span class="sxs-lookup"><span data-stu-id="6b394-130">If you are targeting .NET 4.0 you can use the code-based method shown in the previous section.</span></span>

<span data-ttu-id="6b394-131">如果使用 EF 设计器，您还可以修改您的模型，以便它了解将返回不同的结果集。</span><span class="sxs-lookup"><span data-stu-id="6b394-131">If you are using the EF Designer, you can also modify your model so that it knows about the different result sets that will be returned.</span></span> <span data-ttu-id="6b394-132">一件事之前，此工具不是多个结果指针是要了解设置注意，因此将需要手动编辑 edmx 文件。</span><span class="sxs-lookup"><span data-stu-id="6b394-132">One thing to know before hand is that the tooling is not multiple result set aware, so you will need to manually edit the edmx file.</span></span> <span data-ttu-id="6b394-133">此操作才有效，但它还会在 VS 中中断模型的验证，请编辑 edmx 文件。</span><span class="sxs-lookup"><span data-stu-id="6b394-133">Editing the edmx file like this will work but it will also break the validation of the model in VS.</span></span> <span data-ttu-id="6b394-134">因此如果您验证您的模型，始终会收到错误。</span><span class="sxs-lookup"><span data-stu-id="6b394-134">So if you validate your model you will always get errors.</span></span>

-   <span data-ttu-id="6b394-135">若要执行此操作需要将存储的过程添加到您的模型，就像为单个结果集查询。</span><span class="sxs-lookup"><span data-stu-id="6b394-135">In order to do this you need to add the stored procedure to your model as you would for a single result set query.</span></span>
-   <span data-ttu-id="6b394-136">当您获得此则需要您在模型上右键单击并选择**打开方式...**</span><span class="sxs-lookup"><span data-stu-id="6b394-136">Once you have this then you need to right click on your model and select **Open With..**</span></span> <span data-ttu-id="6b394-137">然后**Xml**</span><span class="sxs-lookup"><span data-stu-id="6b394-137">then **Xml**</span></span>

    ![OpenAs](~/ef6/media/openas.png)

<span data-ttu-id="6b394-139">后的模型以 XML 形式打开，则需要执行以下步骤：</span><span class="sxs-lookup"><span data-stu-id="6b394-139">Once you have the model opened as XML then you need to do the following steps:</span></span>

-   <span data-ttu-id="6b394-140">在您的模型中找到的复杂类型和函数导入：</span><span class="sxs-lookup"><span data-stu-id="6b394-140">Find the complex type and function import in your model:</span></span>

``` xml
    <!-- CSDL content -->
    <edmx:ConceptualModels>

    ...

      <FunctionImport Name="GetAllBlogsAndPosts" ReturnType="Collection(BlogModel.GetAllBlogsAndPosts_Result)" />

    ...

      <ComplexType Name="GetAllBlogsAndPosts_Result">
        <Property Type="Int32" Name="BlogId" Nullable="false" />
        <Property Type="String" Name="Name" Nullable="false" MaxLength="255" />
        <Property Type="String" Name="Description" Nullable="true" />
      </ComplexType>

    ...

    </edmx:ConceptualModels>
```

 

-   <span data-ttu-id="6b394-141">删除复杂类型</span><span class="sxs-lookup"><span data-stu-id="6b394-141">Remove the complex type</span></span>
-   <span data-ttu-id="6b394-142">更新函数导入，以便它映射到实体，在本例中它看起来如下所示：</span><span class="sxs-lookup"><span data-stu-id="6b394-142">Update the function import so that it maps to your entities, in our case it will look like the following:</span></span>

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

<span data-ttu-id="6b394-143">这将告知该模型的存储的过程将返回两个集合，一个博客条目和文章条目之一。</span><span class="sxs-lookup"><span data-stu-id="6b394-143">This tells the model that the stored procedure will return two collections, one of blog entries and one of post entries.</span></span>

-   <span data-ttu-id="6b394-144">查找函数映射元素：</span><span class="sxs-lookup"><span data-stu-id="6b394-144">Find the function mapping element:</span></span>

``` xml
    <!-- C-S mapping content -->
    <edmx:Mappings>

    ...

      <FunctionImportMapping FunctionImportName="GetAllBlogsAndPosts" FunctionName="BlogModel.Store.GetAllBlogsAndPosts">
        <ResultMapping>
          <ComplexTypeMapping TypeName="BlogModel.GetAllBlogsAndPosts_Result">
            <ScalarProperty Name="BlogId" ColumnName="BlogId" />
            <ScalarProperty Name="Name" ColumnName="Name" />
            <ScalarProperty Name="Description" ColumnName="Description" />
          </ComplexTypeMapping>
        </ResultMapping>
      </FunctionImportMapping>

    ...

    </edmx:Mappings>
```

-   <span data-ttu-id="6b394-145">将结果映射为与另一个用于每个实体返回，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6b394-145">Replace the result mapping with one for each entity being returned, such as the following:</span></span>

``` xml
    <ResultMapping>
      <EntityTypeMapping TypeName ="BlogModel.Blog">
        <ScalarProperty Name="BlogId" ColumnName="BlogId" />
        <ScalarProperty Name="Name" ColumnName="Name" />
        <ScalarProperty Name="Description" ColumnName="Description" />
      </EntityTypeMapping>
    </ResultMapping>
    <ResultMapping>
      <EntityTypeMapping TypeName="BlogModel.Post">
        <ScalarProperty Name="BlogId" ColumnName="BlogId" />
        <ScalarProperty Name="PostId" ColumnName="PostId"/>
        <ScalarProperty Name="Title" ColumnName="Title" />
        <ScalarProperty Name="Text" ColumnName="Text" />
      </EntityTypeMapping>
    </ResultMapping>
```

<span data-ttu-id="6b394-146">还有可能要将结果集映射到复杂类型，例如默认情况下创建一个。</span><span class="sxs-lookup"><span data-stu-id="6b394-146">It is also possible to map the result sets to complex types, such as the one created by default.</span></span> <span data-ttu-id="6b394-147">若要执行此操作可以创建新的复杂类型，而不是删除它们，并使用复杂类型无处不在您获得上面的示例中使用的实体名称。</span><span class="sxs-lookup"><span data-stu-id="6b394-147">To do this you create a new complex type, instead of removing them, and use the complex types everywhere that you had used the entity names in the examples above.</span></span>

<span data-ttu-id="6b394-148">一旦这些映射已更改，则可以将模型保存并执行以下代码以使用存储的过程：</span><span class="sxs-lookup"><span data-stu-id="6b394-148">Once these mappings have been changed then you can save the model and execute the following code to use the stored procedure:</span></span>

``` csharp
    using (var db = new BlogEntities())
    {
        var results = db.GetAllBlogsAndPosts();

        foreach (var result in results)
        {
            Console.WriteLine("Blog: " + result.Name);
        }

        var posts = results.GetNextResult<Post>();

        foreach (var result in posts)
        {
            Console.WriteLine("Post: " + result.Title);
        }

        Console.ReadLine();
    }
```

>[!NOTE]
> <span data-ttu-id="6b394-149">如果你手动编辑 edmx 文件为您的模型将被覆盖，如果曾经重新生成数据库模型。</span><span class="sxs-lookup"><span data-stu-id="6b394-149">If you manually edit the edmx file for your model it will be overwritten if you ever regenerate the model from the database.</span></span>

## <a name="summary"></a><span data-ttu-id="6b394-150">总结</span><span class="sxs-lookup"><span data-stu-id="6b394-150">Summary</span></span>

<span data-ttu-id="6b394-151">此处我们已经演示了使用实体框架访问多个结果的两个不同的方法集。</span><span class="sxs-lookup"><span data-stu-id="6b394-151">Here we have shown two different methods of accessing multiple result sets using Entity Framework.</span></span> <span data-ttu-id="6b394-152">这两个文件都同样有效，具体取决于您的具体情况和首选项和您应选择一种似乎是最适合您的环境。</span><span class="sxs-lookup"><span data-stu-id="6b394-152">Both of them are equally valid depending on your situation and preferences and you should choose the one that seems best for your circumstances.</span></span> <span data-ttu-id="6b394-153">计划对多个结果集将会支持在将来改进 Entity Framework 版本和，执行本文档中的步骤将不再有必要。</span><span class="sxs-lookup"><span data-stu-id="6b394-153">It is planned that the support for multiple result sets will be improved in future versions of Entity Framework and that performing the steps in this document will no longer be necessary.</span></span>
