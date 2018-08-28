---
title: 从 EF6 移植到 EF Core技术-移植基于代码的模型
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 2484b681d71ae8711b1b3a59bc274a0b2e403294
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997044"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="51368-102">移植到 EF Core EF6 基于代码的模型</span><span class="sxs-lookup"><span data-stu-id="51368-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="51368-103">如果已读取所有需要注意的问题，您就可以到端口，以下是一些准则，以帮助您入门。</span><span class="sxs-lookup"><span data-stu-id="51368-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="51368-104">安装 EF Core NuGet 包</span><span class="sxs-lookup"><span data-stu-id="51368-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="51368-105">若要使用 EF Core，请为你想要使用的数据库提供程序安装 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="51368-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="51368-106">例如，当目标 SQL Server，则会安装`Microsoft.EntityFrameworkCore.SqlServer`。</span><span class="sxs-lookup"><span data-stu-id="51368-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="51368-107">请参阅[数据库提供程序](../../core/providers/index.md)有关详细信息。</span><span class="sxs-lookup"><span data-stu-id="51368-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="51368-108">如果想要使用迁移，则还应安装`Microsoft.EntityFrameworkCore.Tools`包。</span><span class="sxs-lookup"><span data-stu-id="51368-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="51368-109">最好将从 EF6 NuGet 包 (EntityFramework) 安装，因为 EF Core 和 ef6 更高版本可以是在同一应用程序并行使用。</span><span class="sxs-lookup"><span data-stu-id="51368-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="51368-110">但是，如果不打算在您的应用程序的任何区域中使用 EF6，然后卸载包将有助于为提供的代码片段，需要注意的编译错误。</span><span class="sxs-lookup"><span data-stu-id="51368-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="51368-111">交换命名空间</span><span class="sxs-lookup"><span data-stu-id="51368-111">Swap namespaces</span></span>

<span data-ttu-id="51368-112">在 EF6 中使用的大多数 Api 尚处于`System.Data.Entity`命名空间 （和相关子命名空间）。</span><span class="sxs-lookup"><span data-stu-id="51368-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="51368-113">第一个代码更改是切换到`Microsoft.EntityFrameworkCore`命名空间。</span><span class="sxs-lookup"><span data-stu-id="51368-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="51368-114">将使用派生的上下文文件通常会启动，然后计算出在这里，解决了在发生编译错误。</span><span class="sxs-lookup"><span data-stu-id="51368-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="51368-115">上下文配置 （连接等。）</span><span class="sxs-lookup"><span data-stu-id="51368-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="51368-116">中所述[确保 EF Core将工作为应用程序](ensure-requirements.md)，EF Core具有较少幻解决检测要连接到的数据库。</span><span class="sxs-lookup"><span data-stu-id="51368-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="51368-117">将需要重写`OnConfiguring`派生的上下文和使用数据库提供程序特定 API 来设置连接到数据库的方法。</span><span class="sxs-lookup"><span data-stu-id="51368-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="51368-118">大多数 EF6 应用程序的应用程序中存储连接字符串`App/Web.config`文件。</span><span class="sxs-lookup"><span data-stu-id="51368-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="51368-119">在 EF Core 中，阅读此连接字符串使用`ConfigurationManager`API。</span><span class="sxs-lookup"><span data-stu-id="51368-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="51368-120">可能需要添加对引用`System.Configuration`framework 程序集，以便能够使用此 API。</span><span class="sxs-lookup"><span data-stu-id="51368-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="update-your-code"></a><span data-ttu-id="51368-121">更新你的代码</span><span class="sxs-lookup"><span data-stu-id="51368-121">Update your code</span></span>

<span data-ttu-id="51368-122">在此情况下，这是解决编译错误和检查代码，若要查看的行为更改会影响您的问题。</span><span class="sxs-lookup"><span data-stu-id="51368-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="51368-123">现有迁移</span><span class="sxs-lookup"><span data-stu-id="51368-123">Existing migrations</span></span>

<span data-ttu-id="51368-124">实际上，没有端口现有 ef6 更高版本迁移到 EF Core的可行方法。</span><span class="sxs-lookup"><span data-stu-id="51368-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="51368-125">如果可能，最好假定从 ef6 更高版本的所有以前迁移已应用到数据库，然后从，迁移架构的开始点使用 EF Core。</span><span class="sxs-lookup"><span data-stu-id="51368-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="51368-126">若要执行此操作，你将使用`Add-Migration`命令添加迁移后模型移植到 EF Core。</span><span class="sxs-lookup"><span data-stu-id="51368-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="51368-127">然后，你需要删除所有码`Up`和`Down`的基架生成的迁移方法。</span><span class="sxs-lookup"><span data-stu-id="51368-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="51368-128">当该初始迁移时已搭建基架时，以后的迁移将与模型进行比较。</span><span class="sxs-lookup"><span data-stu-id="51368-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="51368-129">测试端口</span><span class="sxs-lookup"><span data-stu-id="51368-129">Test the port</span></span>

<span data-ttu-id="51368-130">只是因为你的应用程序将编译，并不意味着它成功移植到 EF Core。</span><span class="sxs-lookup"><span data-stu-id="51368-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="51368-131">将需要测试应用程序，确保没有行为更改产生负面影响你的应用程序中的所有区域。</span><span class="sxs-lookup"><span data-stu-id="51368-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
