---
title: 使用 WinForms-EF6 进行数据绑定
author: divega
ms.date: 2016-10-23
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: 7ceb8e85fe3d8f5ab9a5e58ef9c84599585d8f77
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994524"
---
# <a name="databinding-with-winforms"></a><span data-ttu-id="ee10f-102">使用 WinForms 进行数据绑定</span><span class="sxs-lookup"><span data-stu-id="ee10f-102">Databinding with WinForms</span></span>
<span data-ttu-id="ee10f-103">此分步演练说明如何将 POCO 类型绑定到"母版-详细信息"窗体中的 Window 窗体 (WinForms) 控件。</span><span class="sxs-lookup"><span data-stu-id="ee10f-103">This step-by-step walkthrough shows how to bind POCO types to Window Forms (WinForms) controls in a “master-detail" form.</span></span> <span data-ttu-id="ee10f-104">应用程序使用实体框架来填充数据库中的数据对象、 跟踪更改，然后将数据保存到数据库。</span><span class="sxs-lookup"><span data-stu-id="ee10f-104">The application uses Entity Framework to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="ee10f-105">模型定义一个对多关系中参与的两种类型： 类别 (主体\\master) 和产品 (依赖\\详细信息)。</span><span class="sxs-lookup"><span data-stu-id="ee10f-105">The model defines two types that participate in one-to-many relationship: Category (principal\\master) and Product (dependent\\detail).</span></span> <span data-ttu-id="ee10f-106">然后，Visual Studio 工具用于绑定到 WinForms 控件模型中定义的类型。</span><span class="sxs-lookup"><span data-stu-id="ee10f-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WinForms controls.</span></span> <span data-ttu-id="ee10f-107">WinForms 数据绑定框架，相关对象之间导航： 在母版视图中选择行导致要使用相应的子数据更新的详细信息视图。</span><span class="sxs-lookup"><span data-stu-id="ee10f-107">The WinForms data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="ee10f-108">从 Visual Studio 2013 创建屏幕快照和在本演练中的代码列表，但你可以完成此演练使用 Visual Studio 2012 或 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="ee10f-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="ee10f-109">系统必备组件</span><span class="sxs-lookup"><span data-stu-id="ee10f-109">Pre-Requisites</span></span>

<span data-ttu-id="ee10f-110">需要具有 Visual Studio 2013，Visual Studio 2012 或 Visual Studio 2010 安装来完成本演练。</span><span class="sxs-lookup"><span data-stu-id="ee10f-110">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="ee10f-111">如果使用 Visual Studio 2010，您还必须安装 NuGet。</span><span class="sxs-lookup"><span data-stu-id="ee10f-111">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="ee10f-112">有关详细信息，请参阅[安装 NuGet](http://docs.nuget.org/docs/start-here/installing-nuget)。</span><span class="sxs-lookup"><span data-stu-id="ee10f-112">For more information, see [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span></span>

## <a name="create-the-application"></a><span data-ttu-id="ee10f-113">创建应用程序</span><span class="sxs-lookup"><span data-stu-id="ee10f-113">Create the Application</span></span>

-   <span data-ttu-id="ee10f-114">打开 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ee10f-114">Open Visual Studio</span></span>
-   <span data-ttu-id="ee10f-115">**文件-&gt;新增-&gt;项目...**</span><span class="sxs-lookup"><span data-stu-id="ee10f-115">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="ee10f-116">选择**Windows**的左窗格中并**Windows FormsApplication**右窗格中</span><span class="sxs-lookup"><span data-stu-id="ee10f-116">Select **Windows** in the left pane and **Windows FormsApplication** in the right pane</span></span>
-   <span data-ttu-id="ee10f-117">输入**WinFormswithEFSample**作为名称</span><span class="sxs-lookup"><span data-stu-id="ee10f-117">Enter **WinFormswithEFSample** as the name</span></span>
-   <span data-ttu-id="ee10f-118">选择“确定”</span><span class="sxs-lookup"><span data-stu-id="ee10f-118">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="ee10f-119">安装 Entity Framework NuGet 包</span><span class="sxs-lookup"><span data-stu-id="ee10f-119">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="ee10f-120">在解决方案资源管理器，右键单击**WinFormswithEFSample**项目</span><span class="sxs-lookup"><span data-stu-id="ee10f-120">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="ee10f-121">选择**管理 NuGet 包...**</span><span class="sxs-lookup"><span data-stu-id="ee10f-121">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="ee10f-122">在管理 NuGet 包对话框中，选择**联机**选项卡，选择**EntityFramework**包</span><span class="sxs-lookup"><span data-stu-id="ee10f-122">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="ee10f-123">单击**安装**</span><span class="sxs-lookup"><span data-stu-id="ee10f-123">Click **Install**</span></span>  
    > [!NOTE]
    > <span data-ttu-id="ee10f-124">除了 EntityFramework 程序集也添加到 System.ComponentModel.DataAnnotations 的引用。</span><span class="sxs-lookup"><span data-stu-id="ee10f-124">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="ee10f-125">如果项目具有对 System.Data.Entity 的引用，然后它将删除在安装 EntityFramework 包。</span><span class="sxs-lookup"><span data-stu-id="ee10f-125">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="ee10f-126">System.data.entity 的引用程序集不能再用于 Entity Framework 6 应用程序。</span><span class="sxs-lookup"><span data-stu-id="ee10f-126">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="implementing-ilistsource-for-collections"></a><span data-ttu-id="ee10f-127">为集合实现 IListSource</span><span class="sxs-lookup"><span data-stu-id="ee10f-127">Implementing IListSource for Collections</span></span>

<span data-ttu-id="ee10f-128">集合属性必须实现 IListSource 接口以启用与排序时使用 Windows 窗体的双向数据绑定。</span><span class="sxs-lookup"><span data-stu-id="ee10f-128">Collection properties must implement the IListSource interface to enable two-way data binding with sorting when using Windows Forms.</span></span> <span data-ttu-id="ee10f-129">若要执行此操作，我们将要扩展 ObservableCollection 添加 IListSource 功能。</span><span class="sxs-lookup"><span data-stu-id="ee10f-129">To do this we are going to extend ObservableCollection to add IListSource functionality.</span></span>

-   <span data-ttu-id="ee10f-130">添加**ObservableListSource**到项目的类：</span><span class="sxs-lookup"><span data-stu-id="ee10f-130">Add an **ObservableListSource** class to the project:</span></span>
    -   <span data-ttu-id="ee10f-131">右键单击项目名称</span><span class="sxs-lookup"><span data-stu-id="ee10f-131">Right-click on the project name</span></span>
    -   <span data-ttu-id="ee10f-132">选择**添加-&gt;新项**</span><span class="sxs-lookup"><span data-stu-id="ee10f-132">Select **Add -&gt; New Item**</span></span>
    -   <span data-ttu-id="ee10f-133">选择**类**并输入**ObservableListSource**的类名</span><span class="sxs-lookup"><span data-stu-id="ee10f-133">Select **Class** and enter **ObservableListSource** for the class name</span></span>
-   <span data-ttu-id="ee10f-134">默认情况下，使用以下代码生成的代码替换为：</span><span class="sxs-lookup"><span data-stu-id="ee10f-134">Replace the code generated by default with the following code:</span></span>

<span data-ttu-id="ee10f-135">*此类支持双向数据绑定，以及排序。类派生自 ObservableCollection&lt;T&gt;并添加 IListSource 的显式实现。实现 IListSource getlist （） 方法以返回与 ObservableCollection 保持同步的 IBindingList 实现。生成的 ToBindingList IBindingList 实现支持排序。ToBindingList 扩展方法定义中的 EntityFramework 程序集。*</span><span class="sxs-lookup"><span data-stu-id="ee10f-135">*This class enables two-way data binding as well as sorting. The class derives from ObservableCollection&lt;T&gt; and adds an explicit implementation of IListSource. The GetList() method of IListSource is implemented to return an IBindingList implementation that stays in sync with the ObservableCollection. The IBindingList implementation generated by ToBindingList supports sorting. The ToBindingList extention method is defined in the EntityFramework assembly.*</span></span>

``` csharp
    using System.Collections;
    using System.Collections.Generic;
    using System.Collections.ObjectModel;
    using System.ComponentModel;
    using System.Diagnostics.CodeAnalysis;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public class ObservableListSource<T> : ObservableCollection<T>, IListSource
            where T : class
        {
            private IBindingList _bindingList;

            bool IListSource.ContainsListCollection { get { return false; } }

            IList IListSource.GetList()
            {
                return _bindingList  (_bindingList = this.ToBindingList());
            }
        }
    }
```

## <a name="define-a-model"></a><span data-ttu-id="ee10f-136">定义模型</span><span class="sxs-lookup"><span data-stu-id="ee10f-136">Define a Model</span></span>

<span data-ttu-id="ee10f-137">在本演练中你可以选择实现使用 Code First 或 EF 设计器的模型。</span><span class="sxs-lookup"><span data-stu-id="ee10f-137">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="ee10f-138">完成以下两个部分之一。</span><span class="sxs-lookup"><span data-stu-id="ee10f-138">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="ee10f-139">选项 1： 定义使用 Code First 模型</span><span class="sxs-lookup"><span data-stu-id="ee10f-139">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="ee10f-140">本部分演示如何创建模型和其关联的数据库使用 Code First。</span><span class="sxs-lookup"><span data-stu-id="ee10f-140">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="ee10f-141">请跳到下一节 (**选项 2： 定义模型使用 Database First)** 如果您更愿意使用 Database First 进行反向工程使用 EF 设计器从数据库模型</span><span class="sxs-lookup"><span data-stu-id="ee10f-141">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="ee10f-142">使用 Code First 开发时通常首先编写.NET Framework 类定义概念 （域） 模型。</span><span class="sxs-lookup"><span data-stu-id="ee10f-142">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="ee10f-143">添加一个新**产品**到项目的类</span><span class="sxs-lookup"><span data-stu-id="ee10f-143">Add a new **Product** class to project</span></span>
-   <span data-ttu-id="ee10f-144">默认情况下，使用以下代码生成的代码替换为：</span><span class="sxs-lookup"><span data-stu-id="ee10f-144">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }
```

-   <span data-ttu-id="ee10f-145">添加**类别**到项目的类。</span><span class="sxs-lookup"><span data-stu-id="ee10f-145">Add a **Category** class to the project.</span></span>
-   <span data-ttu-id="ee10f-146">默认情况下，使用以下代码生成的代码替换为：</span><span class="sxs-lookup"><span data-stu-id="ee10f-146">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Category
        {
            private readonly ObservableListSource<Product> _products =
                    new ObservableListSource<Product>();

            public int CategoryId { get; set; }
            public string Name { get; set; }
            public virtual ObservableListSource<Product> Products { get { return _products; } }
        }
    }
```

<span data-ttu-id="ee10f-147">除了定义实体，还需要定义一个类，派生自**DbContext** ，并公开**DbSet&lt;TEntity&gt;** 属性。</span><span class="sxs-lookup"><span data-stu-id="ee10f-147">In addition to defining entities, you need to define a class that derives from **DbContext** and exposes **DbSet&lt;TEntity&gt;** properties.</span></span> <span data-ttu-id="ee10f-148">**DbSet**属性使知道你想要在模型中包含哪些的类型的上下文。</span><span class="sxs-lookup"><span data-stu-id="ee10f-148">The **DbSet** properties let the context know which types you want to include in the model.</span></span> <span data-ttu-id="ee10f-149">**DbContext**并**DbSet**类型定义中的 EntityFramework 程序集。</span><span class="sxs-lookup"><span data-stu-id="ee10f-149">The **DbContext** and **DbSet** types are defined in the EntityFramework assembly.</span></span>

<span data-ttu-id="ee10f-150">在运行时，其中包括填充数据库中的数据的对象期间将 DbContext 派生类型的实例管理的实体对象，到数据库更改跟踪，并保留数据。</span><span class="sxs-lookup"><span data-stu-id="ee10f-150">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="ee10f-151">添加一个新**ProductContext**到项目的类。</span><span class="sxs-lookup"><span data-stu-id="ee10f-151">Add a new **ProductContext** class to the project.</span></span>
-   <span data-ttu-id="ee10f-152">默认情况下，使用以下代码生成的代码替换为：</span><span class="sxs-lookup"><span data-stu-id="ee10f-152">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;
    using System.Text;

    namespace WinFormswithEFSample
    {
        public class ProductContext : DbContext
        {
            public DbSet<Category> Categories { get; set; }
            public DbSet<Product> Products { get; set; }
        }
    }
```

<span data-ttu-id="ee10f-153">编译该项目。</span><span class="sxs-lookup"><span data-stu-id="ee10f-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="ee10f-154">选项 2： 定义模型使用 Database First</span><span class="sxs-lookup"><span data-stu-id="ee10f-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="ee10f-155">本部分演示如何使用第一个数据库进行反向工程，使用 EF 设计器从数据库模型。</span><span class="sxs-lookup"><span data-stu-id="ee10f-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="ee10f-156">如果你在完成上一节 (**选项 1： 使用 Code First 定义模型)**，然后跳过此部分并直接转到**延迟加载**部分。</span><span class="sxs-lookup"><span data-stu-id="ee10f-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="ee10f-157">创建现有数据库</span><span class="sxs-lookup"><span data-stu-id="ee10f-157">Create an Existing Database</span></span>

<span data-ttu-id="ee10f-158">通常当你面向的现有数据库，则将已创建，但我们需要在本演练中创建的数据库访问。</span><span class="sxs-lookup"><span data-stu-id="ee10f-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="ee10f-159">随 Visual Studio 一起安装的数据库服务器是已安装的 Visual Studio 的版本而异：</span><span class="sxs-lookup"><span data-stu-id="ee10f-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="ee10f-160">如果您使用的 Visual Studio 2010 系统将创建一个 SQL Express 数据库。</span><span class="sxs-lookup"><span data-stu-id="ee10f-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="ee10f-161">如果你正在使用 Visual Studio 2012，则您将创建[LocalDB](https://msdn.microsoft.com/library/hh510202.aspx)数据库。</span><span class="sxs-lookup"><span data-stu-id="ee10f-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="ee10f-162">让我们继续并生成的数据库。</span><span class="sxs-lookup"><span data-stu-id="ee10f-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="ee10f-163">**视图-&gt;服务器资源管理器**</span><span class="sxs-lookup"><span data-stu-id="ee10f-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="ee10f-164">右键单击**数据连接-&gt;添加连接...**</span><span class="sxs-lookup"><span data-stu-id="ee10f-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="ee10f-165">如果你尚未连接到数据库服务器资源管理器之前将需要选择 Microsoft SQL Server 作为数据源</span><span class="sxs-lookup"><span data-stu-id="ee10f-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![ChangeDataSource](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="ee10f-167">连接到 LocalDB 或 SQL Express，具体取决于哪一个已安装，并输入**产品**作为数据库名称</span><span class="sxs-lookup"><span data-stu-id="ee10f-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![AddConnectionLocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![AddConnectionExpress](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="ee10f-170">选择**确定**并将你想要创建新数据库，请选择要求你**是**</span><span class="sxs-lookup"><span data-stu-id="ee10f-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![CreateDatabase](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="ee10f-172">新数据库现在将出现在服务器资源管理器，右键单击它并选择**新查询**</span><span class="sxs-lookup"><span data-stu-id="ee10f-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="ee10f-173">将下面的 SQL 复制到新的查询，然后右键单击查询并选择**Execute**</span><span class="sxs-lookup"><span data-stu-id="ee10f-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
    CREATE TABLE [dbo].[Categories] (
        [CategoryId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        CONSTRAINT [PK_dbo.Categories] PRIMARY KEY ([CategoryId])
    )

    CREATE TABLE [dbo].[Products] (
        [ProductId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        [CategoryId] [int] NOT NULL,
        CONSTRAINT [PK_dbo.Products] PRIMARY KEY ([ProductId])
    )

    CREATE INDEX [IX_CategoryId] ON [dbo].[Products]([CategoryId])

    ALTER TABLE [dbo].[Products] ADD CONSTRAINT [FK_dbo.Products_dbo.Categories_CategoryId] FOREIGN KEY ([CategoryId]) REFERENCES [dbo].[Categories] ([CategoryId]) ON DELETE CASCADE
```

#### <a name="reverse-engineer-model"></a><span data-ttu-id="ee10f-174">反向工程模型</span><span class="sxs-lookup"><span data-stu-id="ee10f-174">Reverse Engineer Model</span></span>

<span data-ttu-id="ee10f-175">我们将使用实体框架设计器，它是作为 Visual Studio 的一部分，若要创建我们的模型。</span><span class="sxs-lookup"><span data-stu-id="ee10f-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="ee10f-176">**项目-&gt;添加新项...**</span><span class="sxs-lookup"><span data-stu-id="ee10f-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="ee10f-177">选择**数据**左侧的菜单，然后**ADO.NET 实体数据模型**</span><span class="sxs-lookup"><span data-stu-id="ee10f-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="ee10f-178">输入**ProductModel**作为名称，然后单击**确定**</span><span class="sxs-lookup"><span data-stu-id="ee10f-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="ee10f-179">这将启动**实体数据模型向导**</span><span class="sxs-lookup"><span data-stu-id="ee10f-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="ee10f-180">选择**从数据库生成**单击**下一步**</span><span class="sxs-lookup"><span data-stu-id="ee10f-180">Select **Generate from Database** and click **Next**</span></span>

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="ee10f-182">选择连接到第一个部分中创建的数据库中，输入**ProductContext**作为名称的连接字符串和单击**下一步**</span><span class="sxs-lookup"><span data-stu-id="ee10f-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![ChooseYourConnection](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="ee10f-184">单击表导入的所有表并单击完成旁边的复选框</span><span class="sxs-lookup"><span data-stu-id="ee10f-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![ChooseYourObjects](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="ee10f-186">反向工程过程完成后的新模型添加到你的项目，并打开，以便在实体框架设计器中查看。</span><span class="sxs-lookup"><span data-stu-id="ee10f-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="ee10f-187">此外具有已 App.config 文件添加到你的项目数据库的连接详细信息。</span><span class="sxs-lookup"><span data-stu-id="ee10f-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="ee10f-188">Visual Studio 2010 中的其他步骤</span><span class="sxs-lookup"><span data-stu-id="ee10f-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="ee10f-189">如果你正在 Visual Studio 2010 中将需要更新 EF 设计器使用 EF6 代码生成。</span><span class="sxs-lookup"><span data-stu-id="ee10f-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="ee10f-190">在您的模型在 EF 设计器中的某一空白点上右键单击并选择**添加代码生成项...**</span><span class="sxs-lookup"><span data-stu-id="ee10f-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="ee10f-191">选择**联机模板**从左侧的菜单并搜索**DbContext**</span><span class="sxs-lookup"><span data-stu-id="ee10f-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="ee10f-192">选择**EF 6.x DbContext Generator 于 c 语言\#，** 输入**ProductsModel**作为名称并单击添加</span><span class="sxs-lookup"><span data-stu-id="ee10f-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="ee10f-193">更新数据绑定的代码生成</span><span class="sxs-lookup"><span data-stu-id="ee10f-193">Updating code generation for data binding</span></span>

<span data-ttu-id="ee10f-194">EF 从您的模型使用 T4 模板生成代码。</span><span class="sxs-lookup"><span data-stu-id="ee10f-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="ee10f-195">随 Visual Studio 或从 Visual Studio 库中下载的模板适用于常规用途使用。</span><span class="sxs-lookup"><span data-stu-id="ee10f-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="ee10f-196">这意味着从这些模板生成的实体具有简单 ICollection&lt;T&gt;属性。</span><span class="sxs-lookup"><span data-stu-id="ee10f-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="ee10f-197">但是，在执行数据绑定时，需要具有实现 IListSource 的集合属性。</span><span class="sxs-lookup"><span data-stu-id="ee10f-197">However, when doing data binding it is desirable to have collection properties that implement IListSource.</span></span> <span data-ttu-id="ee10f-198">这是为什么我们创建了上面的 ObservableListSource 类，而且我们现在将要修改的模板，以使此类的使用。</span><span class="sxs-lookup"><span data-stu-id="ee10f-198">This is why we created the ObservableListSource class above and we are now going to modify the templates to make use of this class.</span></span>

-   <span data-ttu-id="ee10f-199">打开**解决方案资源管理器**并找到**ProductModel.edmx**文件</span><span class="sxs-lookup"><span data-stu-id="ee10f-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="ee10f-200">查找**ProductModel.tt**将嵌套在 ProductModel.edmx 文件下的文件</span><span class="sxs-lookup"><span data-stu-id="ee10f-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![ProductModelTemplate](~/ef6/media/productmodeltemplate.png)

-   <span data-ttu-id="ee10f-202">双击 ProductModel.tt 文件以在 Visual Studio 编辑器中打开它</span><span class="sxs-lookup"><span data-stu-id="ee10f-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="ee10f-203">查找和替换的两个实例"**ICollection**"with"**ObservableListSource**"。</span><span class="sxs-lookup"><span data-stu-id="ee10f-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="ee10f-204">这些是位于大约行 296 和 484。</span><span class="sxs-lookup"><span data-stu-id="ee10f-204">These are located at approximately lines 296 and 484.</span></span>
-   <span data-ttu-id="ee10f-205">查找和替换第一个匹配项"**HashSet**"with"**ObservableListSource**"。</span><span class="sxs-lookup"><span data-stu-id="ee10f-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="ee10f-206">此匹配项位于大约第 50 行。</span><span class="sxs-lookup"><span data-stu-id="ee10f-206">This occurrence is located at approximately line 50.</span></span> <span data-ttu-id="ee10f-207">**不这样做**替换 HashSet 更高版本的代码中找到的第二个匹配项。</span><span class="sxs-lookup"><span data-stu-id="ee10f-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="ee10f-208">保存 ProductModel.tt 文件。</span><span class="sxs-lookup"><span data-stu-id="ee10f-208">Save the ProductModel.tt file.</span></span> <span data-ttu-id="ee10f-209">这应该会导致重新生成的实体的代码。</span><span class="sxs-lookup"><span data-stu-id="ee10f-209">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="ee10f-210">如果不自动重新生成代码，然后 ProductModel.tt 右键单击并选择"运行自定义工具"。</span><span class="sxs-lookup"><span data-stu-id="ee10f-210">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="ee10f-211">如果现在打开 Category.cs 文件 （其中嵌套在 ProductModel.tt 下），则你应看到产品集合具有类型**ObservableListSource&lt;产品&gt;**。</span><span class="sxs-lookup"><span data-stu-id="ee10f-211">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableListSource&lt;Product&gt;**.</span></span>

<span data-ttu-id="ee10f-212">编译该项目。</span><span class="sxs-lookup"><span data-stu-id="ee10f-212">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="ee10f-213">延迟加载</span><span class="sxs-lookup"><span data-stu-id="ee10f-213">Lazy Loading</span></span>

<span data-ttu-id="ee10f-214">**产品**上的属性**类别**类并**类别**属性**产品**类是导航属性。</span><span class="sxs-lookup"><span data-stu-id="ee10f-214">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="ee10f-215">在实体框架中，导航属性提供一种导航两个实体类型之间的关系方法。</span><span class="sxs-lookup"><span data-stu-id="ee10f-215">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="ee10f-216">EF 提供了一个选项的相关的实体从数据库中加载自动首次访问导航属性。</span><span class="sxs-lookup"><span data-stu-id="ee10f-216">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="ee10f-217">使用此类型的加载 （称为延迟加载），注意，访问每个导航属性的第一次一个单独的查询时将执行针对数据库内容尚不在上下文中。</span><span class="sxs-lookup"><span data-stu-id="ee10f-217">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="ee10f-218">使用 POCO 实体类型时，EF 可以通过在运行时期间创建派生的代理类型的实例，然后重写虚拟属性添加加载挂钩在类中的实现延迟加载。</span><span class="sxs-lookup"><span data-stu-id="ee10f-218">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="ee10f-219">若要获取相关对象的延迟加载，您必须声明导航属性 getter 作为**公共**和**虚拟**(**Overridable**在 Visual Basic 中)，而且不能为您的类**密封**(**NotOverridable**在 Visual Basic 中)。</span><span class="sxs-lookup"><span data-stu-id="ee10f-219">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="ee10f-220">使用数据库时将自动进行虚拟若要启用延迟加载第一个导航属性。</span><span class="sxs-lookup"><span data-stu-id="ee10f-220">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="ee10f-221">在代码优先部分中我们所做的导航属性虚拟出于同样的原因</span><span class="sxs-lookup"><span data-stu-id="ee10f-221">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="ee10f-222">将对象绑定到控件</span><span class="sxs-lookup"><span data-stu-id="ee10f-222">Bind Object to Controls</span></span>

<span data-ttu-id="ee10f-223">添加此 WinForms 应用程序的数据源作为模型中定义的类。</span><span class="sxs-lookup"><span data-stu-id="ee10f-223">Add the classes that are defined in the model as data sources for this WinForms application.</span></span>

-   <span data-ttu-id="ee10f-224">从主菜单中，选择**项目-&gt;添加新数据源...**</span><span class="sxs-lookup"><span data-stu-id="ee10f-224">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="ee10f-225">(需要在 Visual Studio 2010 中，选择要**数据-&gt;添加新数据源...**)</span><span class="sxs-lookup"><span data-stu-id="ee10f-225">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="ee10f-226">在选择数据源类型窗口中，选择**对象**单击**下一步**</span><span class="sxs-lookup"><span data-stu-id="ee10f-226">In the Choose a Data Source Type window, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="ee10f-227">在选择数据对象对话框中，展开**WinFormswithEFSample**两次，然后选择**类别**没有不需要选择产品数据源，因为我们将通过该产品的获取到它类别数据源的属性。</span><span class="sxs-lookup"><span data-stu-id="ee10f-227">In the Select the Data Objects dialog, unfold the **WinFormswithEFSample** two times and select **Category** There is no need to select the Product data source, because we will get to it through the Product’s property on the Category data source.</span></span>

    ![DataSource](~/ef6/media/datasource.png)

-   <span data-ttu-id="ee10f-229">单击**完成。**
    *如果未显示数据源窗口，选择 * * * 视图-&gt;其他 Windows-&gt;数据源**</span><span class="sxs-lookup"><span data-stu-id="ee10f-229">Click **Finish.**
*If the Data Sources window is not showing up, select***View -&gt; Other Windows-&gt; Data Sources**</span></span>
-   <span data-ttu-id="ee10f-230">按固定图标，以便数据源窗口不会不会自动隐藏。</span><span class="sxs-lookup"><span data-stu-id="ee10f-230">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="ee10f-231">您可能需要按刷新按钮，如果窗口已可见。</span><span class="sxs-lookup"><span data-stu-id="ee10f-231">You may need to hit the refresh button if the window was already visible.</span></span>

    ![DataSource2](~/ef6/media/datasource2.png)

-   <span data-ttu-id="ee10f-233">在解决方案资源管理器中双击**Form1.cs**文件以在设计器中打开主窗体。</span><span class="sxs-lookup"><span data-stu-id="ee10f-233">In Solution Explorer, double-click the **Form1.cs** file to open the main form in designer.</span></span>
-   <span data-ttu-id="ee10f-234">选择**类别**数据源，并将其拖动窗体上。</span><span class="sxs-lookup"><span data-stu-id="ee10f-234">Select the **Category** data source and drag it on the form.</span></span> <span data-ttu-id="ee10f-235">默认情况下，新 DataGridView (**categoryDataGridView**) 和导航工具栏控件添加到设计器。</span><span class="sxs-lookup"><span data-stu-id="ee10f-235">By default, a new DataGridView (**categoryDataGridView**) and Navigation toolbar controls are added to the designer.</span></span> <span data-ttu-id="ee10f-236">这些控件绑定到 BindingSource (**categoryBindingSource**) 和绑定的导航器 (**categoryBindingNavigator**) 也创建的组件。</span><span class="sxs-lookup"><span data-stu-id="ee10f-236">These controls are bound to the BindingSource (**categoryBindingSource**) and Binding Navigator (**categoryBindingNavigator**) components that are created as well.</span></span>
-   <span data-ttu-id="ee10f-237">在编辑的列**categoryDataGridView**。</span><span class="sxs-lookup"><span data-stu-id="ee10f-237">Edit the columns on the **categoryDataGridView**.</span></span> <span data-ttu-id="ee10f-238">我们想要设置**CategoryId**列添加到只读的。</span><span class="sxs-lookup"><span data-stu-id="ee10f-238">We want to set the **CategoryId** column to read-only.</span></span> <span data-ttu-id="ee10f-239">值**CategoryId**我们保存数据后，由数据库生成的属性。</span><span class="sxs-lookup"><span data-stu-id="ee10f-239">The value for the **CategoryId** property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="ee10f-240">右键单击在 DataGridView 控件，并选择编辑列...</span><span class="sxs-lookup"><span data-stu-id="ee10f-240">Right-click the DataGridView control and select Edit Columns…</span></span>
    -   <span data-ttu-id="ee10f-241">选择类别 id 列，然后将 ReadOnly 设置为 True</span><span class="sxs-lookup"><span data-stu-id="ee10f-241">Select the CategoryId column and set ReadOnly to True</span></span>
    -   <span data-ttu-id="ee10f-242">按确定</span><span class="sxs-lookup"><span data-stu-id="ee10f-242">Press OK</span></span>
-   <span data-ttu-id="ee10f-243">从类别的数据源下选择产品，并将其拖动窗体上。</span><span class="sxs-lookup"><span data-stu-id="ee10f-243">Select Products from under the Category data source and drag it on the form.</span></span> <span data-ttu-id="ee10f-244">则 productDataGridView 和 productBindingSource 都会添加到窗体。</span><span class="sxs-lookup"><span data-stu-id="ee10f-244">The productDataGridView and productBindingSource are added to the form.</span></span>
-   <span data-ttu-id="ee10f-245">编辑 productDataGridView 上的列。</span><span class="sxs-lookup"><span data-stu-id="ee10f-245">Edit the columns on the productDataGridView.</span></span> <span data-ttu-id="ee10f-246">我们想要隐藏的类别 Id 和类别列并将产品 id 设置为只读的。</span><span class="sxs-lookup"><span data-stu-id="ee10f-246">We want to hide the CategoryId and Category columns and set ProductId to read-only.</span></span> <span data-ttu-id="ee10f-247">我们将保存数据后，将由数据库生成 ProductId 属性的值。</span><span class="sxs-lookup"><span data-stu-id="ee10f-247">The value for the ProductId property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="ee10f-248">右键单击在 DataGridView 控件，然后选择**编辑列...**.</span><span class="sxs-lookup"><span data-stu-id="ee10f-248">Right-click the DataGridView control and select **Edit Columns...**.</span></span>
    -   <span data-ttu-id="ee10f-249">选择**ProductId**列并设置**ReadOnly**到**True**。</span><span class="sxs-lookup"><span data-stu-id="ee10f-249">Select the **ProductId** column and set **ReadOnly** to **True**.</span></span>
    -   <span data-ttu-id="ee10f-250">选择**CategoryId**列，然后按**删除**按钮。</span><span class="sxs-lookup"><span data-stu-id="ee10f-250">Select the **CategoryId** column and press the **Remove** button.</span></span> <span data-ttu-id="ee10f-251">执行具有相同的操作**类别**列。</span><span class="sxs-lookup"><span data-stu-id="ee10f-251">Do the same with the **Category** column.</span></span>
    -   <span data-ttu-id="ee10f-252">按**确定**。</span><span class="sxs-lookup"><span data-stu-id="ee10f-252">Press **OK**.</span></span>

    <span data-ttu-id="ee10f-253">到目前为止，我们可以我们 DataGridView 控件与 BindingSource 组件在设计器中。</span><span class="sxs-lookup"><span data-stu-id="ee10f-253">So far, we associated our DataGridView controls with BindingSource components in the designer.</span></span> <span data-ttu-id="ee10f-254">在下一部分中我们会将代码添加到代码隐藏将 categoryBindingSource.DataSource 设置为当前跟踪通过 DbContext 的实体的集合。</span><span class="sxs-lookup"><span data-stu-id="ee10f-254">In the next section we will add code to the code behind to set categoryBindingSource.DataSource to the collection of entities that are currently tracked by DbContext.</span></span> <span data-ttu-id="ee10f-255">当从 WinForms 类别下拖动和删除产品谨慎 productsBindingSource.DataSource 属性，并为产品 categoryBindingSource 和 productsBindingSource.DataMember 属性设置。</span><span class="sxs-lookup"><span data-stu-id="ee10f-255">When we dragged-and-dropped Products from under the Category, the WinForms took care of setting up the productsBindingSource.DataSource property to categoryBindingSource and productsBindingSource.DataMember property to Products.</span></span> <span data-ttu-id="ee10f-256">由于此绑定将中 productDataGridView 显示仅属于当前所选类别的产品。</span><span class="sxs-lookup"><span data-stu-id="ee10f-256">Because of this binding, only the products that belong to the currently selected Category will be displayed in the productDataGridView.</span></span>
-   <span data-ttu-id="ee10f-257">启用**保存**通过单击鼠标右键并选择导航工具栏上的按钮**已启用**。</span><span class="sxs-lookup"><span data-stu-id="ee10f-257">Enable the **Save** button on the Navigation toolbar by clicking the right mouse button and selecting **Enabled**.</span></span>

    ![Form1 设计器](~/ef6/media/form1-designer.png)

-   <span data-ttu-id="ee10f-259">添加保存的事件处理程序通过双击该按钮的按钮。</span><span class="sxs-lookup"><span data-stu-id="ee10f-259">Add the event handler for the save button by double-clicking on the button.</span></span> <span data-ttu-id="ee10f-260">这将添加事件处理程序，并转到代码隐藏窗体。</span><span class="sxs-lookup"><span data-stu-id="ee10f-260">This will add the event handler and bring you to the code behind for the form.</span></span> <span data-ttu-id="ee10f-261">代码**categoryBindingNavigatorSaveItem\_单击**将在下一节中添加事件处理程序。</span><span class="sxs-lookup"><span data-stu-id="ee10f-261">The code for the **categoryBindingNavigatorSaveItem\_Click** event handler will be added in the next section.</span></span>

## <a name="add-the-code-that-handles-data-interaction"></a><span data-ttu-id="ee10f-262">添加用于处理数据交互的代码</span><span class="sxs-lookup"><span data-stu-id="ee10f-262">Add the Code that Handles Data Interaction</span></span>

<span data-ttu-id="ee10f-263">我们现在将添加代码以使用 ProductContext 执行数据访问。</span><span class="sxs-lookup"><span data-stu-id="ee10f-263">We'll now add the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="ee10f-264">更新主窗体窗口的代码，如下所示。</span><span class="sxs-lookup"><span data-stu-id="ee10f-264">Update the code for the main form window as shown below.</span></span>

<span data-ttu-id="ee10f-265">该代码声明了 ProductContext 的长时间运行实例。</span><span class="sxs-lookup"><span data-stu-id="ee10f-265">The code declares a long-running instance of ProductContext.</span></span> <span data-ttu-id="ee10f-266">ProductContext 对象用于查询并将数据保存到数据库。</span><span class="sxs-lookup"><span data-stu-id="ee10f-266">The ProductContext object is used to query and save data to the database.</span></span> <span data-ttu-id="ee10f-267">从重写的 OnClosing 方法然后调用 ProductContext 实例上的 dispose （） 方法。</span><span class="sxs-lookup"><span data-stu-id="ee10f-267">The Dispose() method on the ProductContext instance is then called from the overridden OnClosing method.</span></span> <span data-ttu-id="ee10f-268">代码注释提供了有关代码的作用的详细信息。</span><span class="sxs-lookup"><span data-stu-id="ee10f-268">The code comments provide details about what the code does.</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.ComponentModel;
    using System.Data;
    using System.Drawing;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Windows.Forms;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public partial class Form1 : Form
        {
            ProductContext _context;
            public Form1()
            {
                InitializeComponent();
            }

            protected override void OnLoad(EventArgs e)
            {
                base.OnLoad(e);
                _context = new ProductContext();

                // Call the Load method to get the data for the given DbSet
                // from the database.
                // The data is materialized as entities. The entities are managed by
                // the DbContext instance.
                _context.Categories.Load();

                // Bind the categoryBindingSource.DataSource to
                // all the Unchanged, Modified and Added Category objects that
                // are currently tracked by the DbContext.
                // Note that we need to call ToBindingList() on the
                // ObservableCollection<TEntity> returned by
                // the DbSet.Local property to get the BindingList<T>
                // in order to facilitate two-way binding in WinForms.
                this.categoryBindingSource.DataSource =
                    _context.Categories.Local.ToBindingList();
            }

            private void categoryBindingNavigatorSaveItem_Click(object sender, EventArgs e)
            {
                this.Validate();

                // Currently, the Entity Framework doesn’t mark the entities
                // that are removed from a navigation property (in our example the Products)
                // as deleted in the context.
                // The following code uses LINQ to Objects against the Local collection
                // to find all products and marks any that do not have
                // a Category reference as deleted.
                // The ToList call is required because otherwise
                // the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can do LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                // Save the changes to the database.
                this._context.SaveChanges();

                // Refresh the controls to show the values         
                // that were generated by the database.
                this.categoryDataGridView.Refresh();
                this.productsDataGridView.Refresh();
            }

            protected override void OnClosing(CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }
    }
```

## <a name="test-the-windows-forms-application"></a><span data-ttu-id="ee10f-269">测试 Windows 窗体应用程序</span><span class="sxs-lookup"><span data-stu-id="ee10f-269">Test the Windows Forms Application</span></span>

-   <span data-ttu-id="ee10f-270">编译和运行应用程序，而且可以测试功能。</span><span class="sxs-lookup"><span data-stu-id="ee10f-270">Compile and run the application and you can test out the functionality.</span></span>

    ![Form1BeforeSave](~/ef6/media/form1beforesave.png)

-   <span data-ttu-id="ee10f-272">保存后生成的存储密钥显示在屏幕上。</span><span class="sxs-lookup"><span data-stu-id="ee10f-272">After saving the store generated keys are shown on the screen.</span></span>

    ![Form1AfterSave](~/ef6/media/form1aftersave.png)

-   <span data-ttu-id="ee10f-274">如果使用 Code First，那么您还会看到**WinFormswithEFSample.ProductContext**为您创建数据库。</span><span class="sxs-lookup"><span data-stu-id="ee10f-274">If you used Code First, then you will also see that a **WinFormswithEFSample.ProductContext** database is created for you.</span></span>

    ![ServerObjExplorer](~/ef6/media/serverobjexplorer.png)
