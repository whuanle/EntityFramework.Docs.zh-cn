---
title: 预生成的映射视图-EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 917ba9c8-6ddf-4631-ab8c-c4fb378c2fcd
ms.openlocfilehash: 397569ef374cb44d4938f9e201b588a26c408f6e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996467"
---
# <a name="pre-generated-mapping-views"></a><span data-ttu-id="94508-102">预生成的映射视图</span><span class="sxs-lookup"><span data-stu-id="94508-102">Pre-generated mapping views</span></span>
<span data-ttu-id="94508-103">实体框架可以执行查询或将更改保存到数据源之前，它必须生成一组映射视图来访问数据库。</span><span class="sxs-lookup"><span data-stu-id="94508-103">Before the Entity Framework can execute a query or save changes to the data source, it must generate a set of mapping views to access the database.</span></span> <span data-ttu-id="94508-104">这些映射视图是一组以抽象方式表示的数据库的实体 SQL 语句，并且每个应用程序域缓存的元数据的一部分。</span><span class="sxs-lookup"><span data-stu-id="94508-104">These mapping views are a set of Entity SQL statement that represent the database in an abstract way, and are part of the metadata which is cached per application domain.</span></span> <span data-ttu-id="94508-105">如果在同一应用程序域中创建相同的上下文的多个实例，这些实例将重用缓存元数据，而无需重新生成它们，从映射视图。</span><span class="sxs-lookup"><span data-stu-id="94508-105">If you create multiple instances of the same context in the same application domain, they will reuse mapping views from the cached metadata rather than regenerating them.</span></span> <span data-ttu-id="94508-106">映射视图生成是执行第一个查询的总成本的重要组成部分，实体框架，可预生成映射视图并将其包含在已编译的项目中。</span><span class="sxs-lookup"><span data-stu-id="94508-106">Because mapping view generation is a significant part of the overall cost of executing the first query, the Entity Framework enables you to pre-generate mapping views and include them in the compiled project.</span></span> <span data-ttu-id="94508-107">有关详细信息，请参阅[性能注意事项 （实体框架）](~/ef6/fundamentals/performance/perf-whitepaper.md)。</span><span class="sxs-lookup"><span data-stu-id="94508-107">For more information, see  [Performance Considerations (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span></span>

## <a name="generating-mapping-views-with-the-ef-power-tools"></a><span data-ttu-id="94508-108">生成映射与 EF Power Tools 的视图</span><span class="sxs-lookup"><span data-stu-id="94508-108">Generating Mapping Views with the EF Power Tools</span></span>

<span data-ttu-id="94508-109">预生成视图的最简单方法是使用[EF Power Tools](http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d)。</span><span class="sxs-lookup"><span data-stu-id="94508-109">The easiest way to pre-generate views is to use the [EF Power Tools](http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d).</span></span> <span data-ttu-id="94508-110">已安装 Power 工具将后，到生成的视图，如下所示的菜单选项。</span><span class="sxs-lookup"><span data-stu-id="94508-110">Once you have the Power Tools installed you will have a menu option to Generate Views, as below.</span></span>

-   <span data-ttu-id="94508-111">有关**Code First**模型右键单击包含您的 DbContext 类的代码文件。</span><span class="sxs-lookup"><span data-stu-id="94508-111">For **Code First** models right-click on the code file that contains your DbContext class.</span></span>
-   <span data-ttu-id="94508-112">有关**EF 设计器**模型右键单击 EDMX 文件。</span><span class="sxs-lookup"><span data-stu-id="94508-112">For **EF Designer** models right-click on your EDMX file.</span></span>

![generateViews](~/ef6/media/generateviews.png)

<span data-ttu-id="94508-114">该过程完成后将具有类似于以下生成的类</span><span class="sxs-lookup"><span data-stu-id="94508-114">Once the process is finished you will have a class similar to the following generated</span></span>

![generatedViews](~/ef6/media/generatedviews.png)

<span data-ttu-id="94508-116">现在，在运行时 EF 应用程序将使用此类加载所需的视图。</span><span class="sxs-lookup"><span data-stu-id="94508-116">Now when you run your application EF will use this class to load views as required.</span></span> <span data-ttu-id="94508-117">如果模型更改，但不重新生成此类 EF 将引发异常。</span><span class="sxs-lookup"><span data-stu-id="94508-117">If your model changes and you do not re-generate this class then EF will throw an exception.</span></span>

## <a name="generating-mapping-views-from-code---ef6-onwards"></a><span data-ttu-id="94508-118">生成映射代码的 EF6 及更高版本中的视图</span><span class="sxs-lookup"><span data-stu-id="94508-118">Generating Mapping Views from Code - EF6 Onwards</span></span>

<span data-ttu-id="94508-119">生成视图的其他方法是使用 EF 提供的 Api。</span><span class="sxs-lookup"><span data-stu-id="94508-119">The other way to generate views is to use the APIs that EF provides.</span></span> <span data-ttu-id="94508-120">使用此方法时您可以自由地序列化视图，但是，但您还需要自行加载视图。</span><span class="sxs-lookup"><span data-stu-id="94508-120">When using this method you have the freedom to serialize the views however you like, but you also need to load the views yourself.</span></span>

> [!NOTE]
> <span data-ttu-id="94508-121">**EF6 及更高版本仅**-Entity Framework 6 中引入了在本部分中所示的 Api。</span><span class="sxs-lookup"><span data-stu-id="94508-121">**EF6 Onwards Only** - The APIs shown in this section were introduced in Entity Framework 6.</span></span> <span data-ttu-id="94508-122">如果使用此信息不适用于较早版本。</span><span class="sxs-lookup"><span data-stu-id="94508-122">If you are using an earlier version this information does not apply.</span></span>

### <a name="generating-views"></a><span data-ttu-id="94508-123">生成视图</span><span class="sxs-lookup"><span data-stu-id="94508-123">Generating Views</span></span>

<span data-ttu-id="94508-124">若要生成的视图的 Api 是 System.Data.Entity.Core.Mapping.StorageMappingItemCollection 类上。</span><span class="sxs-lookup"><span data-stu-id="94508-124">The APIs to generate views are on the System.Data.Entity.Core.Mapping.StorageMappingItemCollection class.</span></span> <span data-ttu-id="94508-125">您可以使用 ObjectContext 的 MetadataWorkspace StorageMappingCollection 检索上下文。</span><span class="sxs-lookup"><span data-stu-id="94508-125">You can retrieve a StorageMappingCollection for a Context by using the MetadataWorkspace of an ObjectContext.</span></span> <span data-ttu-id="94508-126">如果你使用较新的 DbContext API，则可以使用访问这 IObjectContextAdapter 如下所示，我们在此代码中具有名为 dbContext 派生 DbContext 的实例：</span><span class="sxs-lookup"><span data-stu-id="94508-126">If you are using the newer DbContext API then you can access this by using the IObjectContextAdapter like below, in this code we have an instance of your derived DbContext called dbContext:</span></span>

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

<span data-ttu-id="94508-127">一旦你有 StorageMappingItemCollection 您可获得 GenerateViews 和 ComputeMappingHashValue 方法的访问权限。</span><span class="sxs-lookup"><span data-stu-id="94508-127">Once you have the StorageMappingItemCollection then you can get access to the GenerateViews and ComputeMappingHashValue methods.</span></span>

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

<span data-ttu-id="94508-128">第一种方法创建容器映射中每个视图的条目的字典。</span><span class="sxs-lookup"><span data-stu-id="94508-128">The first method creates a dictionary with an entry for each view in the container mapping.</span></span> <span data-ttu-id="94508-129">第二种方法计算单个容器映射的哈希值，并用于在运行时验证，该模型后未发生更改的预生成视图。</span><span class="sxs-lookup"><span data-stu-id="94508-129">The second method computes a hash value for the single container mapping and is used at runtime to validate that the model has not changed since the views were pre-generated.</span></span> <span data-ttu-id="94508-130">对于复杂的方案涉及多个容器映射提供两种方法的重写。</span><span class="sxs-lookup"><span data-stu-id="94508-130">Overrides of the two methods are provided for complex scenarios involving multiple container mappings.</span></span>

<span data-ttu-id="94508-131">生成视图时将调用 GenerateViews 方法并写出生成 EntitySetBase 和 DbMappingView。</span><span class="sxs-lookup"><span data-stu-id="94508-131">When generating views you will call the GenerateViews method and then write out the resulting EntitySetBase and DbMappingView.</span></span> <span data-ttu-id="94508-132">您还需要用于存储由 ComputeMappingHashValue 方法生成的哈希。</span><span class="sxs-lookup"><span data-stu-id="94508-132">You will also need to store the hash generated by the ComputeMappingHashValue method.</span></span>

### <a name="loading-views"></a><span data-ttu-id="94508-133">正在加载视图</span><span class="sxs-lookup"><span data-stu-id="94508-133">Loading Views</span></span>

<span data-ttu-id="94508-134">若要加载的 GenerateViews 方法生成的视图，可以从 DbMappingViewCache 抽象类继承的类提供 EF。</span><span class="sxs-lookup"><span data-stu-id="94508-134">In order to load the views generated by the GenerateViews method, you can provide EF with a class that inherits from the DbMappingViewCache abstract class.</span></span> <span data-ttu-id="94508-135">DbMappingViewCache 指定必须实现的两个方法：</span><span class="sxs-lookup"><span data-stu-id="94508-135">DbMappingViewCache specifies two methods that you must implement:</span></span>

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

<span data-ttu-id="94508-136">MappingHashValue 属性必须返回 ComputeMappingHashValue 方法生成的哈希。</span><span class="sxs-lookup"><span data-stu-id="94508-136">The MappingHashValue property must return the hash generated by the ComputeMappingHashValue method.</span></span> <span data-ttu-id="94508-137">EF 时将它寻求视图首先将生成并与此属性返回的哈希值进行比较的模型哈希值。</span><span class="sxs-lookup"><span data-stu-id="94508-137">When EF is going to ask for views it will first generate and compare the hash value of the model with the hash returned by this property.</span></span> <span data-ttu-id="94508-138">如果它们不匹配，EF 将引发 EntityCommandCompilationException 异常。</span><span class="sxs-lookup"><span data-stu-id="94508-138">If they do not match then EF will throw an EntityCommandCompilationException exception.</span></span>

<span data-ttu-id="94508-139">GetView 方法将接受 EntitySetBase 并且您需要返回包含已生成的 EntitySql DbMappingVIew 已给定 EntitySetBase GenerateViews 方法生成字典中与相关联。</span><span class="sxs-lookup"><span data-stu-id="94508-139">The GetView method will accept an EntitySetBase and you need to return a DbMappingVIew containing the EntitySql that was generated for that was associated with the given EntitySetBase in the dictionary generated by the GenerateViews method.</span></span> <span data-ttu-id="94508-140">如果 EF 要求提供，你没有然后 GetView 的视图应返回 null。</span><span class="sxs-lookup"><span data-stu-id="94508-140">If EF asks for a view that you do not have then GetView should return null.</span></span>

<span data-ttu-id="94508-141">以下是上文所述，在其中我们可以看到存储和检索所需 EntitySql 的一种方法将生成带有 Power Tools DbMappingViewCache 年期间提取。</span><span class="sxs-lookup"><span data-stu-id="94508-141">The following is an extract from the DbMappingViewCache that is generated with the Power Tools as described above, in it we see one way to store and retrieve the EntitySql required.</span></span>

``` csharp
    public override string MappingHashValue
    {
        get { return "a0b843f03dd29abee99789e190a6fb70ce8e93dc97945d437d9a58fb8e2afd2e"; }
    }

    public override DbMappingView GetView(EntitySetBase extent)
    {
        if (extent == null)
        {
            throw new ArgumentNullException("extent");
        }

        var extentName = extent.EntityContainer.Name + "." + extent.Name;

        if (extentName == "BlogContext.Blogs")
        {
            return GetView2();
        }

        if (extentName == "BlogContext.Posts")
        {
            return GetView3();
        }

        return null;
    }

    private static DbMappingView GetView2()
    {
        return new DbMappingView(@"
            SELECT VALUE -- Constructing Blogs
            [BlogApp.Models.Blog](T1.Blog_BlogId, T1.Blog_Test, T1.Blog_title, T1.Blog_Active, T1.Blog_SomeDecimal)
            FROM (
            SELECT
                T.BlogId AS Blog_BlogId,
                T.Test AS Blog_Test,
                T.title AS Blog_title,
                T.Active AS Blog_Active,
                T.SomeDecimal AS Blog_SomeDecimal,
                True AS _from0
            FROM CodeFirstDatabase.Blog AS T
            ) AS T1");
    }
```

<span data-ttu-id="94508-142">若要让 EF 使用您添加您 DbMappingViewCache 使用 DbMappingViewCacheTypeAttribute，指定为创建的上下文。</span><span class="sxs-lookup"><span data-stu-id="94508-142">To have EF use your DbMappingViewCache you add use the DbMappingViewCacheTypeAttribute, specifying the context that it was created for.</span></span> <span data-ttu-id="94508-143">下面的代码中我们将与 MyMappingViewCache 类关联 BlogContext。</span><span class="sxs-lookup"><span data-stu-id="94508-143">In the code below we associate the BlogContext with the MyMappingViewCache class.</span></span>

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

<span data-ttu-id="94508-144">对于更复杂的方案，可以通过指定映射视图缓存工厂提供映射视图缓存实例。</span><span class="sxs-lookup"><span data-stu-id="94508-144">For more complex scenarios, mapping view cache instances can be provided by specifying a mapping view cache factory.</span></span> <span data-ttu-id="94508-145">这可以通过实现抽象类 System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory。</span><span class="sxs-lookup"><span data-stu-id="94508-145">This can be done by implementing the abstract class System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory.</span></span> <span data-ttu-id="94508-146">可以检索或设置使用 StorageMappingItemCollection.MappingViewCacheFactoryproperty 映射视图缓存工厂使用的实例。</span><span class="sxs-lookup"><span data-stu-id="94508-146">The instance of the mapping view cache factory that is used can be retrieved or set using the StorageMappingItemCollection.MappingViewCacheFactoryproperty.</span></span>
