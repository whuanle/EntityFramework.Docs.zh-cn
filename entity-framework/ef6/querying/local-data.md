---
title: 本地数据-EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 2eda668b-1e5d-487d-9a8c-0e3beef03fcb
caps.latest.revision: 3
ms.openlocfilehash: 79f0d2175199780d41b43088832bab808ab2fff0
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/08/2018
ms.locfileid: "39120082"
---
# <a name="local-data"></a><span data-ttu-id="484c8-102">本地数据</span><span class="sxs-lookup"><span data-stu-id="484c8-102">Local Data</span></span>
<span data-ttu-id="484c8-103">直接对 DbSet 运行 LINQ 查询将始终将查询发送到数据库，但是您可以访问的数据的当前内存中使用的 DbSet.Local 属性。</span><span class="sxs-lookup"><span data-stu-id="484c8-103">Running a LINQ query directly against a DbSet will always send a query to the database, but you can access the data that is currently in-memory using the DbSet.Local property.</span></span> <span data-ttu-id="484c8-104">有关你使用的 DbContext.Entry 和 DbContext.ChangeTracker.Entries 方法的实体，还可以访问 EF 跟踪的额外信息。</span><span class="sxs-lookup"><span data-stu-id="484c8-104">You can also access the extra information EF is tracking about your entities using the DbContext.Entry and DbContext.ChangeTracker.Entries methods.</span></span> <span data-ttu-id="484c8-105">本主题所介绍的方法同样适用于查询使用 Code First 和 EF 设计器创建的模型。</span><span class="sxs-lookup"><span data-stu-id="484c8-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="using-local-to-look-at-local-data"></a><span data-ttu-id="484c8-106">使用本地查看本地数据</span><span class="sxs-lookup"><span data-stu-id="484c8-106">Using Local to look at local data</span></span>  

<span data-ttu-id="484c8-107">DbSet 的本地属性提供对实体集的当前由上下文跟踪和未标记为已删除的简单访问。</span><span class="sxs-lookup"><span data-stu-id="484c8-107">The Local property of DbSet provides simple access to the entities of the set that are currently being tracked by the context and have not been marked as Deleted.</span></span> <span data-ttu-id="484c8-108">访问本地属性永远不会导致查询发送到数据库。</span><span class="sxs-lookup"><span data-stu-id="484c8-108">Accessing the Local property never causes a query to be sent to the database.</span></span> <span data-ttu-id="484c8-109">这意味着，它通常用于执行查询后。</span><span class="sxs-lookup"><span data-stu-id="484c8-109">This means that it is usually used after a query has already been performed.</span></span> <span data-ttu-id="484c8-110">可以使用 Load 扩展方法来执行查询，以便将上下文跟踪结果。</span><span class="sxs-lookup"><span data-stu-id="484c8-110">The Load extension method can be used to execute a query so that the context tracks the results.</span></span> <span data-ttu-id="484c8-111">例如：</span><span class="sxs-lookup"><span data-stu-id="484c8-111">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs from the database into the context
    context.Blogs.Load();

    // Add a new blog to the context
    context.Blogs.Add(new Blog { Name = "My New Blog" });

    // Mark one of the existing blogs as Deleted
    context.Blogs.Remove(context.Blogs.Find(1));

    // Loop over the blogs in the context.
    Console.WriteLine("In Local: ");
    foreach (var blog in context.Blogs.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            blog.BlogId,  
            blog.Name,
            context.Entry(blog).State);
    }

    // Perform a query against the database.
    Console.WriteLine("\nIn DbSet query: ");
    foreach (var blog in context.Blogs)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            blog.BlogId,  
            blog.Name,
            context.Entry(blog).State);
    }
}
```  

<span data-ttu-id="484c8-112">如果我们有两个博客数据库的 ADO.NET 博客与 1 BlogId-和 Visual Studio 博客与 2 BlogId 中我们可以看到以下输出：</span><span class="sxs-lookup"><span data-stu-id="484c8-112">If we had two blogs in the database - 'ADO.NET Blog' with a BlogId of 1 and 'The Visual Studio Blog' with a BlogId of 2 - we could expect the following output:</span></span>  

```  
In Local:
Found 0: My New Blog with state Added
Found 2: The Visual Studio Blog with state Unchanged

In DbSet query:
Found 1: ADO.NET Blog with state Deleted
Found 2: The Visual Studio Blog with state Unchanged
```  

<span data-ttu-id="484c8-113">这说明了三个点：</span><span class="sxs-lookup"><span data-stu-id="484c8-113">This illustrates three points:</span></span>  

- <span data-ttu-id="484c8-114">即使它具有尚未保存到数据库，将本地集合中包含新的博客我的新博客。</span><span class="sxs-lookup"><span data-stu-id="484c8-114">The new blog 'My New Blog' is included in the Local collection even though it has not yet been saved to the database.</span></span> <span data-ttu-id="484c8-115">此博客具有零的主键，因为数据库不生成的实体的实际密钥。</span><span class="sxs-lookup"><span data-stu-id="484c8-115">This blog has a primary key of zero because the database has not yet generated a real key for the entity.</span></span>  
- <span data-ttu-id="484c8-116">尽管仍正在由上下文跟踪，ADO.NET 博客不包括在本地集合中。</span><span class="sxs-lookup"><span data-stu-id="484c8-116">The 'ADO.NET Blog' is not included in the local collection even though it is still being tracked by the context.</span></span> <span data-ttu-id="484c8-117">这是因为我们从从而将其标记为删除 DbSet 中删除它。</span><span class="sxs-lookup"><span data-stu-id="484c8-117">This is because we removed it from the DbSet thereby marking it as deleted.</span></span>  
- <span data-ttu-id="484c8-118">DbSet 用于执行查询时在结果中包含标记为删除 （ADO.NET 博客） 的博客和尚未保存到数据库的新博客 （我的新博客） 不包括在结果中。</span><span class="sxs-lookup"><span data-stu-id="484c8-118">When DbSet is used to perform a query the blog marked for deletion (ADO.NET Blog) is included in the results and the new blog (My New Blog) that has not yet been saved to the database is not included in the results.</span></span> <span data-ttu-id="484c8-119">这是因为 DbSet 正在执行对数据库进行查询，并且始终返回的结果反映什么是数据库中。</span><span class="sxs-lookup"><span data-stu-id="484c8-119">This is because DbSet is performing a query against the database and the results returned always reflect what is in the database.</span></span>  

## <a name="using-local-to-add-and-remove-entities-from-the-context"></a><span data-ttu-id="484c8-120">使用本地添加和删除实体的上下文</span><span class="sxs-lookup"><span data-stu-id="484c8-120">Using Local to add and remove entities from the context</span></span>  

<span data-ttu-id="484c8-121">DbSet 上的本地属性返回[ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx)与事件挂钩，以便它与保持同步的上下文的内容。</span><span class="sxs-lookup"><span data-stu-id="484c8-121">The Local property on DbSet returns an [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) with events hooked up such that it stays in sync with the contents of the context.</span></span> <span data-ttu-id="484c8-122">这意味着可以添加或从本地集合或 DbSet 中删除实体。</span><span class="sxs-lookup"><span data-stu-id="484c8-122">This means that entities can be added or removed from either the Local collection or the DbSet.</span></span> <span data-ttu-id="484c8-123">这还意味着到上下文中将新的实体的查询会提供这些实体来更新本地集合。</span><span class="sxs-lookup"><span data-stu-id="484c8-123">It also means that queries that bring new entities into the context will result in the Local collection being updated with those entities.</span></span> <span data-ttu-id="484c8-124">例如：</span><span class="sxs-lookup"><span data-stu-id="484c8-124">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load some posts from the database into the context
    context.Posts.Where(p => p.Tags.Contains("entity-framework").Load();  

    // Get the local collection and make some changes to it
    var localPosts = context.Posts.Local;
    localPosts.Add(new Post { Name = "What's New in EF" });
    localPosts.Remove(context.Posts.Find(1));  

    // Loop over the posts in the context.
    Console.WriteLine("In Local after entity-framework query: ");
    foreach (var post in context.Posts.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            post.Id,  
            post.Title,
            context.Entry(post).State);
    }

    var post1 = context.Posts.Find(1);
    Console.WriteLine(
        "State of post 1: {0} is {1}",
        post1.Name,  
        context.Entry(post1).State);  

    // Query some more posts from the database
    context.Posts.Where(p => p.Tags.Contains("asp.net").Load();  

    // Loop over the posts in the context again.
    Console.WriteLine("\nIn Local after asp.net query: ");
    foreach (var post in context.Posts.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            post.Id,  
            post.Title,
            context.Entry(post).State);
    }
}
```  

<span data-ttu-id="484c8-125">假设我们有几个帖子标记为实体框架和 asp.net 输出可能如下所示：</span><span class="sxs-lookup"><span data-stu-id="484c8-125">Assuming we had a few posts tagged with 'entity-framework' and 'asp.net' the output may look something like this:</span></span>  

```  
In Local after entity-framework query:
Found 3: EF Designer Basics with state Unchanged
Found 5: EF Code First Basics with state Unchanged
Found 0: What's New in EF with state Added
State of post 1: EF Beginners Guide is Deleted

In Local after asp.net query:
Found 3: EF Designer Basics with state Unchanged
Found 5: EF Code First Basics with state Unchanged
Found 0: What's New in EF with state Added
Found 4: ASP.NET Beginners Guide with state Unchanged
```  

<span data-ttu-id="484c8-126">这说明了三个点：</span><span class="sxs-lookup"><span data-stu-id="484c8-126">This illustrates three points:</span></span>  

- <span data-ttu-id="484c8-127">新文章 What's New in EF 已添加到本地集合将成为由上下文跟踪处于已添加状态。</span><span class="sxs-lookup"><span data-stu-id="484c8-127">The new post 'What's New in EF' that was added to the Local collection becomes tracked by the context in the Added state.</span></span> <span data-ttu-id="484c8-128">它将因此插入到数据库时调用 SaveChanges。</span><span class="sxs-lookup"><span data-stu-id="484c8-128">It will therefore be inserted into the database when SaveChanges is called.</span></span>  
- <span data-ttu-id="484c8-129">已从本地集合 （EF 初学者指南） 中删除的文章现在标记为已删除的上下文中。</span><span class="sxs-lookup"><span data-stu-id="484c8-129">The post that was removed from the Local collection (EF Beginners Guide) is now marked as deleted in the context.</span></span> <span data-ttu-id="484c8-130">它将因此从数据库中删除时调用 SaveChanges。</span><span class="sxs-lookup"><span data-stu-id="484c8-130">It will therefore be deleted from the database when SaveChanges is called.</span></span>  
- <span data-ttu-id="484c8-131">加载到上下文中与第二个查询的其他文章 （ASP.NET 初学者指南） 自动添加到本地集合。</span><span class="sxs-lookup"><span data-stu-id="484c8-131">The additional post (ASP.NET Beginners Guide) loaded into the context with the second query is automatically added to the Local collection.</span></span>  

<span data-ttu-id="484c8-132">最后一件要注意有关本地的事情是，因为它是 ObservableCollection 性能不适合大量实体。</span><span class="sxs-lookup"><span data-stu-id="484c8-132">One final thing to note about Local is that because it is an ObservableCollection performance is not great for large numbers of entities.</span></span> <span data-ttu-id="484c8-133">因此如果您的上下文中正在处理数千个实体可能不会建议使用本地。</span><span class="sxs-lookup"><span data-stu-id="484c8-133">Therefore if you are dealing with thousands of entities in your context it may not be advisable to use Local.</span></span>  

## <a name="using-local-for-wpf-data-binding"></a><span data-ttu-id="484c8-134">使用 WPF 数据绑定的本地</span><span class="sxs-lookup"><span data-stu-id="484c8-134">Using Local for WPF data binding</span></span>  

<span data-ttu-id="484c8-135">DbSet 上的本地属性可直接用于 WPF 应用程序中的数据绑定，因为它是 ObservableCollection 实例。</span><span class="sxs-lookup"><span data-stu-id="484c8-135">The Local property on DbSet can be used directly for data binding in a WPF application because it is an instance of ObservableCollection.</span></span> <span data-ttu-id="484c8-136">这意味着它会自动将前面部分中所述的上下文的内容与保持同步和上下文的内容将自动保持与其同步。</span><span class="sxs-lookup"><span data-stu-id="484c8-136">As described in the previous sections this means that it will automatically stay in sync with the contents of the context and the contents of the context will automatically stay in sync with it.</span></span> <span data-ttu-id="484c8-137">请注意，您需要预填充的数据才会将绑定到，由于本地永远不会导致数据库查询的任何内容的本地集合。</span><span class="sxs-lookup"><span data-stu-id="484c8-137">Note that you do need to pre-populate the Local collection with data for there to be anything to bind to since Local never causes a database query.</span></span>  

<span data-ttu-id="484c8-138">这不是完整的 WPF 数据绑定示例的合适位置，但的关键要素是：</span><span class="sxs-lookup"><span data-stu-id="484c8-138">This is not an appropriate place for a full WPF data binding sample but the key elements are:</span></span>  

- <span data-ttu-id="484c8-139">设置绑定源</span><span class="sxs-lookup"><span data-stu-id="484c8-139">Setup a binding source</span></span>  
- <span data-ttu-id="484c8-140">将其绑定到你的集的本地属性</span><span class="sxs-lookup"><span data-stu-id="484c8-140">Bind it to the Local property of your set</span></span>  
- <span data-ttu-id="484c8-141">填充本地使用对数据库查询。</span><span class="sxs-lookup"><span data-stu-id="484c8-141">Populate Local using a query to the database.</span></span>  

## <a name="wpf-binding-to-navigation-properties"></a><span data-ttu-id="484c8-142">WPF 绑定到导航属性</span><span class="sxs-lookup"><span data-stu-id="484c8-142">WPF binding to navigation properties</span></span>  

<span data-ttu-id="484c8-143">如果你正在母版/详细信息数据绑定您可能想要绑定到一个实体的导航属性的详细信息视图。</span><span class="sxs-lookup"><span data-stu-id="484c8-143">If you are doing master/detail data binding you may want to bind the detail view to a navigation property of one of your entities.</span></span> <span data-ttu-id="484c8-144">实现此目的的简单方法是使用一个 ObservableCollection 的导航属性。</span><span class="sxs-lookup"><span data-stu-id="484c8-144">An easy way to make this work is to use an ObservableCollection for the navigation property.</span></span> <span data-ttu-id="484c8-145">例如：</span><span class="sxs-lookup"><span data-stu-id="484c8-145">For example:</span></span>  

``` csharp
public class Blog
{
    private readonly ObservableCollection<Post> _posts =
        new ObservableCollection<Post>();

    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual ObservableCollection<Post> Posts
    {
        get { return _posts; }
    }
}
```  

## <a name="using-local-to-clean-up-entities-in-savechanges"></a><span data-ttu-id="484c8-146">使用本地清理 SaveChanges 中的实体</span><span class="sxs-lookup"><span data-stu-id="484c8-146">Using Local to clean up entities in SaveChanges</span></span>  

<span data-ttu-id="484c8-147">在大多数情况下从导航属性中移除的实体将不会自动标记为已删除的上下文中。</span><span class="sxs-lookup"><span data-stu-id="484c8-147">In most cases entities removed from a navigation property will not be automatically marked as deleted in the context.</span></span> <span data-ttu-id="484c8-148">例如，如果删除 Post 对象从 Blog.Posts 集合，文章将不会自动删除调用 SaveChanges 的时候。</span><span class="sxs-lookup"><span data-stu-id="484c8-148">For example, if you remove a Post object from the Blog.Posts collection then that post will not be automatically deleted when SaveChanges is called.</span></span> <span data-ttu-id="484c8-149">如果您需要它来删除然后可能需要找到这些无关联的实体并标记为已删除，然后再调用 SaveChanges 或重写 SaveChanges 的一部分。</span><span class="sxs-lookup"><span data-stu-id="484c8-149">If you need it to be deleted then you may need to find these dangling entities and mark them as deleted before calling SaveChanges or as part of an overridden SaveChanges.</span></span> <span data-ttu-id="484c8-150">例如：</span><span class="sxs-lookup"><span data-stu-id="484c8-150">For example:</span></span>  

``` csharp
public override int SaveChanges()
{
    foreach (var post in this.Posts.Local.ToList())
    {
        if (post.Blog == null)
        {
            this.Posts.Remove(post);
        }
    }

    return base.SaveChanges();
}
```  

<span data-ttu-id="484c8-151">上面的代码使用本地集合来查找所有文章和标记为已删除不具有的博客引用任何的。</span><span class="sxs-lookup"><span data-stu-id="484c8-151">The code above uses the Local collection to find all posts and marks any that do not have a blog reference as deleted.</span></span> <span data-ttu-id="484c8-152">ToList 调用是必需的因为删除将否则修改该集合正在枚举时调用。</span><span class="sxs-lookup"><span data-stu-id="484c8-152">The ToList call is required because otherwise the collection will be modified by the Remove call while it is being enumerated.</span></span> <span data-ttu-id="484c8-153">在大多数其他情况下可以查询直接针对本地属性而无需先使用 ToList。</span><span class="sxs-lookup"><span data-stu-id="484c8-153">In most other situations you can query directly against the Local property without using ToList first.</span></span>  

## <a name="using-local-and-tobindinglist-for-windows-forms-data-binding"></a><span data-ttu-id="484c8-154">使用本地和 ToBindingList 为 Windows 窗体数据绑定</span><span class="sxs-lookup"><span data-stu-id="484c8-154">Using Local and ToBindingList for Windows Forms data binding</span></span>  

<span data-ttu-id="484c8-155">Windows 窗体不支持直接使用 ObservableCollection 完全保真的数据绑定。</span><span class="sxs-lookup"><span data-stu-id="484c8-155">Windows Forms does not support full fidelity data binding using ObservableCollection directly.</span></span> <span data-ttu-id="484c8-156">但是，仍可以使用数据绑定的 DbSet 本地属性以获取前面各节中所述的所有权益。</span><span class="sxs-lookup"><span data-stu-id="484c8-156">However, you can still use the DbSet Local property for data binding to get all the benefits described in the previous sections.</span></span> <span data-ttu-id="484c8-157">这通过 ToBindingList 扩展方法创建[IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx)实现支持的本地 ObservableCollection。</span><span class="sxs-lookup"><span data-stu-id="484c8-157">This is achieved through the ToBindingList extension method which creates an [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) implementation backed by the Local ObservableCollection.</span></span>  

<span data-ttu-id="484c8-158">这不是完整的 Windows 窗体数据绑定示例的合适位置，但的关键要素是：</span><span class="sxs-lookup"><span data-stu-id="484c8-158">This is not an appropriate place for a full Windows Forms data binding sample but the key elements are:</span></span>  

- <span data-ttu-id="484c8-159">设置对象绑定源</span><span class="sxs-lookup"><span data-stu-id="484c8-159">Setup an object binding source</span></span>  
- <span data-ttu-id="484c8-160">将其绑定到使用 Local.ToBindingList() 集的本地属性</span><span class="sxs-lookup"><span data-stu-id="484c8-160">Bind it to the Local property of your set using Local.ToBindingList()</span></span>  
- <span data-ttu-id="484c8-161">填充本地使用对数据库查询</span><span class="sxs-lookup"><span data-stu-id="484c8-161">Populate Local using a query to the database</span></span>  

## <a name="getting-detailed-information-about-tracked-entities"></a><span data-ttu-id="484c8-162">获取有关跟踪的实体的详细的信息</span><span class="sxs-lookup"><span data-stu-id="484c8-162">Getting detailed information about tracked entities</span></span>  

<span data-ttu-id="484c8-163">许多本系列中的示例使用入口方法以返回实体的 DbEntityEntry 实例。</span><span class="sxs-lookup"><span data-stu-id="484c8-163">Many of the examples in this series use the Entry method to return a DbEntityEntry instance for an entity.</span></span> <span data-ttu-id="484c8-164">此条目对象然后充当用于收集有关其当前状态，例如实体的信息以及在如显式加载相关的实体的实体上执行操作的起始点。</span><span class="sxs-lookup"><span data-stu-id="484c8-164">This entry object then acts as the starting point for gathering information about the entity such as its current state, as well as for performing operations on the entity such as explicitly loading a related entity.</span></span>  

<span data-ttu-id="484c8-165">条目方法返回多个或所有实体正在由上下文跟踪的 DbEntityEntry 对象。</span><span class="sxs-lookup"><span data-stu-id="484c8-165">The Entries methods return DbEntityEntry objects for many or all entities being tracked by the context.</span></span> <span data-ttu-id="484c8-166">这允许你收集的信息或执行对多个实体而不是只需单个条目的操作。</span><span class="sxs-lookup"><span data-stu-id="484c8-166">This allows you to gather information or perform operations on many entities rather than just a single entry.</span></span> <span data-ttu-id="484c8-167">例如：</span><span class="sxs-lookup"><span data-stu-id="484c8-167">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load some entities into the context
    context.Blogs.Load();
    context.Authors.Load();
    context.Readers.Load();

    // Make some changes
    context.Blogs.Find(1).Title = "The New ADO.NET Blog";
    context.Blogs.Remove(context.Blogs.Find(2));
    context.Authors.Add(new Author { Name = "Jane Doe" });
    context.Readers.Find(1).Username = "johndoe1987";

    // Look at the state of all entities in the context
    Console.WriteLine("All tracked entities: ");
    foreach (var entry in context.ChangeTracker.Entries())
    {
        Console.WriteLine(
            "Found entity of type {0} with state {1}",
            ObjectContext.GetObjectType(entry.Entity.GetType()).Name,
            entry.State);
    }

    // Find modified entities of any type
    Console.WriteLine("\nAll modified entities: ");
    foreach (var entry in context.ChangeTracker.Entries()
                              .Where(e => e.State == EntityState.Modified))
    {
        Console.WriteLine(
            "Found entity of type {0} with state {1}",
            ObjectContext.GetObjectType(entry.Entity.GetType()).Name,
            entry.State);
    }

    // Get some information about just the tracked blogs
    Console.WriteLine("\nTracked blogs: ");
    foreach (var entry in context.ChangeTracker.Entries<Blog>())
    {
        Console.WriteLine(
            "Found Blog {0}: {1} with original Name {2}",
            entry.Entity.BlogId,  
            entry.Entity.Name,
            entry.Property(p => p.Name).OriginalValue);
    }

    // Find all people (author or reader)
    Console.WriteLine("\nPeople: ");
    foreach (var entry in context.ChangeTracker.Entries<IPerson>())
    {
        Console.WriteLine("Found Person {0}", entry.Entity.Name);
    }
}
```  

<span data-ttu-id="484c8-168">您会注意到，我们引入的作者和读者类到示例-这两种类均实现 IPerson 接口。</span><span class="sxs-lookup"><span data-stu-id="484c8-168">You'll notice we are introducing a Author and Reader class into the example - both of these classes implement the IPerson interface.</span></span>  

``` csharp
public class Author : IPerson
{
    public int AuthorId { get; set; }
    public string Name { get; set; }
    public string Biography { get; set; }
}

public class Reader : IPerson
{
    public int ReaderId { get; set; }
    public string Name { get; set; }
    public string Username { get; set; }
}

public interface IPerson
{
    string Name { get; }
}
```  

<span data-ttu-id="484c8-169">让我们假设我们在数据库中具有以下数据：</span><span class="sxs-lookup"><span data-stu-id="484c8-169">Let's assume we have the following data in the database:</span></span>

<span data-ttu-id="484c8-170">使用 BlogId 博客 = 1 且名称 = ADO.NET 博客</span><span class="sxs-lookup"><span data-stu-id="484c8-170">Blog with BlogId = 1 and Name = 'ADO.NET Blog'</span></span>  
<span data-ttu-id="484c8-171">使用 BlogId 博客 = 2，名称 = Visual Studio 博客</span><span class="sxs-lookup"><span data-stu-id="484c8-171">Blog with BlogId = 2 and Name = 'The Visual Studio Blog'</span></span>  
<span data-ttu-id="484c8-172">使用 BlogId 博客 = 3 且名称 =.NET Framework 博客</span><span class="sxs-lookup"><span data-stu-id="484c8-172">Blog with BlogId = 3 and Name = '.NET Framework Blog'</span></span>  
<span data-ttu-id="484c8-173">作者与作者 Id = 1 且名称 = ' Joe Bloggs</span><span class="sxs-lookup"><span data-stu-id="484c8-173">Author with AuthorId = 1 and Name = 'Joe Bloggs'</span></span>  
<span data-ttu-id="484c8-174">读取器与 ReaderId = 1 且名称 = John Doe</span><span class="sxs-lookup"><span data-stu-id="484c8-174">Reader with ReaderId = 1 and Name = 'John Doe'</span></span>  

<span data-ttu-id="484c8-175">将运行代码的输出：</span><span class="sxs-lookup"><span data-stu-id="484c8-175">The output from running the code would be:</span></span>  

```  
All tracked entities:
Found entity of type Blog with state Modified
Found entity of type Blog with state Deleted
Found entity of type Blog with state Unchanged
Found entity of type Author with state Unchanged
Found entity of type Author with state Added
Found entity of type Reader with state Modified

All modified entities:
Found entity of type Blog with state Modified
Found entity of type Reader with state Modified

Tracked blogs:
Found Blog 1: The New ADO.NET Blog with original Name ADO.NET Blog
Found Blog 2: The Visual Studio Blog with original Name The Visual Studio Blog
Found Blog 3: .NET Framework Blog with original Name .NET Framework Blog

People:
Found Person John Doe
Found Person Joe Bloggs
Found Person Jane Doe
```  

<span data-ttu-id="484c8-176">这些示例阐释了几个点：</span><span class="sxs-lookup"><span data-stu-id="484c8-176">These examples illustrate several points:</span></span>  

- <span data-ttu-id="484c8-177">条目方法返回的所有状态，包括已删除的实体的条目。</span><span class="sxs-lookup"><span data-stu-id="484c8-177">The Entries methods return entries for entities in all states, including Deleted.</span></span> <span data-ttu-id="484c8-178">与此比较不包括本地删除的实体。</span><span class="sxs-lookup"><span data-stu-id="484c8-178">Compare this to Local which excludes Deleted entities.</span></span>  
- <span data-ttu-id="484c8-179">使用非泛型条目方法时，则返回所有实体类型的项。</span><span class="sxs-lookup"><span data-stu-id="484c8-179">Entries for all entity types are returned when the non-generic Entries method is used.</span></span> <span data-ttu-id="484c8-180">使用泛型条目方法时则仅会返回的实体是泛型类型的实例的项。</span><span class="sxs-lookup"><span data-stu-id="484c8-180">When the generic entries method is used entries are only returned for entities that are instances of the generic type.</span></span> <span data-ttu-id="484c8-181">这用于上面获取的所有博客条目。</span><span class="sxs-lookup"><span data-stu-id="484c8-181">This was used above to get entries for all blogs.</span></span> <span data-ttu-id="484c8-182">它还用于获得实现 IPerson 的所有实体的条目。</span><span class="sxs-lookup"><span data-stu-id="484c8-182">It was also used to get entries for all entities that implement IPerson.</span></span> <span data-ttu-id="484c8-183">此示例演示泛型类型不必是实际的实体类型。</span><span class="sxs-lookup"><span data-stu-id="484c8-183">This demonstrates that the generic type does not have to be an actual entity type.</span></span>  
- <span data-ttu-id="484c8-184">LINQ 到对象可用来筛选返回的结果。</span><span class="sxs-lookup"><span data-stu-id="484c8-184">LINQ to Objects can be used to filter the results returned.</span></span> <span data-ttu-id="484c8-185">这用于上面查找任何类型的实体，只要进行修改。</span><span class="sxs-lookup"><span data-stu-id="484c8-185">This was used above to find entities of any type as long as they are modified.</span></span>  

<span data-ttu-id="484c8-186">请注意 DbEntityEntry 实例始终包含一个非 null 的实体。</span><span class="sxs-lookup"><span data-stu-id="484c8-186">Note that DbEntityEntry instances always contain a non-null Entity.</span></span> <span data-ttu-id="484c8-187">这样就无需这些筛选器作为 DbEntityEntry 实例不表示关系条目与存根 （stub） 条目。</span><span class="sxs-lookup"><span data-stu-id="484c8-187">Relationship entries and stub entries are not represented as DbEntityEntry instances so there is no need to filter for these.</span></span>
