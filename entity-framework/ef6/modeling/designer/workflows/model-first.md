---
title: 模型优先的 EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
caps.latest.revision: 3
ms.openlocfilehash: e7876776ed0dee764d5ced97b863a3580e02e20b
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/08/2018
ms.locfileid: "39120074"
---
# <a name="model-first"></a><span data-ttu-id="49110-102">模型优先</span><span class="sxs-lookup"><span data-stu-id="49110-102">Model First</span></span>
<span data-ttu-id="49110-103">此视频和分步演练会提供使用实体框架模型优先开发的简介。</span><span class="sxs-lookup"><span data-stu-id="49110-103">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="49110-104">模型首先允许你创建使用实体框架设计器的新模型，然后从模型生成数据库架构。</span><span class="sxs-lookup"><span data-stu-id="49110-104">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="49110-105">模型存储在 EDMX 文件 （扩展名为.edmx） 并可以查看和编辑实体框架设计器中。</span><span class="sxs-lookup"><span data-stu-id="49110-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="49110-106">从 EDMX 文件自动生成与交互应用程序中的类。</span><span class="sxs-lookup"><span data-stu-id="49110-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="49110-107">观看视频</span><span class="sxs-lookup"><span data-stu-id="49110-107">Watch the video</span></span>
<span data-ttu-id="49110-108">此视频和分步演练会提供使用实体框架模型优先开发的简介。</span><span class="sxs-lookup"><span data-stu-id="49110-108">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="49110-109">模型首先允许你创建使用实体框架设计器的新模型，然后从模型生成数据库架构。</span><span class="sxs-lookup"><span data-stu-id="49110-109">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="49110-110">模型存储在 EDMX 文件 （扩展名为.edmx） 并可以查看和编辑实体框架设计器中。</span><span class="sxs-lookup"><span data-stu-id="49110-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="49110-111">从 EDMX 文件自动生成与交互应用程序中的类。</span><span class="sxs-lookup"><span data-stu-id="49110-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="49110-112">**主讲人**：[Rowan Miller](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="49110-112">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="49110-113">**视频**: [WMV](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span><span class="sxs-lookup"><span data-stu-id="49110-113">**Video**: [WMV](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="49110-114">系统必备组件</span><span class="sxs-lookup"><span data-stu-id="49110-114">Pre-Requisites</span></span>

<span data-ttu-id="49110-115">将需要安装 Visual Studio 2010 或 Visual Studio 2012 安装来完成本演练。</span><span class="sxs-lookup"><span data-stu-id="49110-115">You will need to have Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="49110-116">如果使用 Visual Studio 2010，您还需要具有[NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)安装。</span><span class="sxs-lookup"><span data-stu-id="49110-116">If you are using Visual Studio 2010, you will also need to have [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="49110-117">1.创建应用程序</span><span class="sxs-lookup"><span data-stu-id="49110-117">1. Create the Application</span></span>

<span data-ttu-id="49110-118">为了简单起见，我们要生成使用模型优先执行数据访问的基本的控制台应用程序：</span><span class="sxs-lookup"><span data-stu-id="49110-118">To keep things simple we’re going to build a basic console application that uses the Model First to perform data access:</span></span>

-   <span data-ttu-id="49110-119">打开 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="49110-119">Open Visual Studio</span></span>
-   <span data-ttu-id="49110-120">**文件-&gt;新增-&gt;项目...**</span><span class="sxs-lookup"><span data-stu-id="49110-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="49110-121">选择**Windows**左侧的菜单和**控制台应用程序**</span><span class="sxs-lookup"><span data-stu-id="49110-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="49110-122">输入**ModelFirstSample**作为名称</span><span class="sxs-lookup"><span data-stu-id="49110-122">Enter **ModelFirstSample** as the name</span></span>
-   <span data-ttu-id="49110-123">选择“确定”</span><span class="sxs-lookup"><span data-stu-id="49110-123">Select **OK**</span></span>

## <a name="2-create-model"></a><span data-ttu-id="49110-124">2.创建模型</span><span class="sxs-lookup"><span data-stu-id="49110-124">2. Create Model</span></span>

<span data-ttu-id="49110-125">我们将使用实体框架设计器，它是作为 Visual Studio 的一部分，若要创建我们的模型。</span><span class="sxs-lookup"><span data-stu-id="49110-125">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="49110-126">**项目-&gt;添加新项...**</span><span class="sxs-lookup"><span data-stu-id="49110-126">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="49110-127">选择**数据**左侧的菜单，然后**ADO.NET 实体数据模型**</span><span class="sxs-lookup"><span data-stu-id="49110-127">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="49110-128">输入**BloggingModel**作为名称，然后单击**确定**，这将启动实体数据模型向导</span><span class="sxs-lookup"><span data-stu-id="49110-128">Enter **BloggingModel** as the name and click **OK**, this launches the Entity Data Model Wizard</span></span>
-   <span data-ttu-id="49110-129">选择**空模型**单击**完成**</span><span class="sxs-lookup"><span data-stu-id="49110-129">Select **Empty Model** and click **Finish**</span></span>

    ![CreateEmptyModel](~/ef6/media/createemptymodel.png)

<span data-ttu-id="49110-131">实体框架设计器将打开具有空模型。</span><span class="sxs-lookup"><span data-stu-id="49110-131">The Entity Framework Designer is opened with a blank model.</span></span> <span data-ttu-id="49110-132">现在我们可以开始向模型添加实体、 属性和关联。</span><span class="sxs-lookup"><span data-stu-id="49110-132">Now we can start adding entities, properties and associations to the model.</span></span>

-   <span data-ttu-id="49110-133">右键单击设计图面，然后选择**属性**</span><span class="sxs-lookup"><span data-stu-id="49110-133">Right-click on the design surface and select **Properties**</span></span>
-   <span data-ttu-id="49110-134">在属性窗口中更改**实体容器名称**到**bloggingcontext**
    *这是将您上下文生成派生上下文的名称表示与数据库中，使我们能够查询和保存数据的会话*</span><span class="sxs-lookup"><span data-stu-id="49110-134">In the Properties window change the **Entity Container Name** to **BloggingContext**
*This is the name of the derived context that will be generated for you, the context represents a session with the database, allowing us to query and save data*</span></span>
-   <span data-ttu-id="49110-135">右键单击设计图面，然后选择**添加新的&gt;实体...**</span><span class="sxs-lookup"><span data-stu-id="49110-135">Right-click on the design surface and select **Add New -&gt; Entity…**</span></span>
-   <span data-ttu-id="49110-136">输入**博客**作为实体名称并**BlogId**作为键名称，然后单击**确定**</span><span class="sxs-lookup"><span data-stu-id="49110-136">Enter **Blog** as the entity name and **BlogId** as the key name and click **OK**</span></span>

    ![AddBlogEntity](~/ef6/media/addblogentity.png)

-   <span data-ttu-id="49110-138">右键单击设计图面，然后选择新的实体**添加新的&gt;标量属性**，输入**名称**作为属性的名称。</span><span class="sxs-lookup"><span data-stu-id="49110-138">Right-click on the new entity on the design surface and select **Add New -&gt; Scalar Property**, enter **Name** as the name of the property.</span></span>
-   <span data-ttu-id="49110-139">重复此过程以添加**Url**属性。</span><span class="sxs-lookup"><span data-stu-id="49110-139">Repeat this process to add a **Url** property.</span></span>
-   <span data-ttu-id="49110-140">右键单击**Url**设计图面并选择属性**属性**，在属性窗口中更改**Nullable**设置为**True**
    *这使我们能够将一个博客保存到数据库而不将它分配一个 Url*</span><span class="sxs-lookup"><span data-stu-id="49110-140">Right-click on the **Url** property on the design surface and select **Properties**, in the Properties window change the **Nullable** setting to **True**
*This allows us to save a Blog to the database without assigning it a Url*</span></span>
-   <span data-ttu-id="49110-141">使用你只需了解到的技术将添加**Post**具有实体**PostId**键属性</span><span class="sxs-lookup"><span data-stu-id="49110-141">Using the techniques you just learnt, add a **Post** entity with a **PostId** key property</span></span>
-   <span data-ttu-id="49110-142">添加**标题**并**内容**到标量属性**Post**实体</span><span class="sxs-lookup"><span data-stu-id="49110-142">Add **Title** and **Content** scalar properties to the **Post** entity</span></span>

<span data-ttu-id="49110-143">现在，我们有几个实体，就可以添加关联 （或关系） 它们之间。</span><span class="sxs-lookup"><span data-stu-id="49110-143">Now that we have a couple of entities, it’s time to add an association (or relationship) between them.</span></span>

-   <span data-ttu-id="49110-144">右键单击设计图面，然后选择**添加新的&gt;关联...**</span><span class="sxs-lookup"><span data-stu-id="49110-144">Right-click on the design surface and select **Add New -&gt; Association…**</span></span>
-   <span data-ttu-id="49110-145">使一个关系一端的指向**博客**的重数**一个**和到其他终结点**Post**重数为**许多** 
    *这意味着博客中发布了许多文章和文章属于一个博客*</span><span class="sxs-lookup"><span data-stu-id="49110-145">Make one end of the relationship point to **Blog** with a multiplicity of **One** and the other end point to **Post** with a multiplicity of **Many**
*This means that a Blog has many Posts and a Post belongs to one Blog*</span></span>
-   <span data-ttu-id="49110-146">请确保**将外键属性添加到 Post 实体**框为选中状态，并单击**确定**</span><span class="sxs-lookup"><span data-stu-id="49110-146">Ensure the **Add foreign key properties to 'Post' Entity** box is checked and click **OK**</span></span>

    ![AddAssociationMF](~/ef6/media/addassociationmf.png)

<span data-ttu-id="49110-148">现在，我们有一个我们可以生成从数据库和用于读取和写入数据的简单模型。</span><span class="sxs-lookup"><span data-stu-id="49110-148">We now have a simple model that we can generate a database from and use to read and write data.</span></span>

![ModelInitial](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="49110-150">Visual Studio 2010 中的其他步骤</span><span class="sxs-lookup"><span data-stu-id="49110-150">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="49110-151">如果您正在一些其他步骤，您需要按照升级到 Entity Framework 的最新版本的 Visual Studio 2010 中。</span><span class="sxs-lookup"><span data-stu-id="49110-151">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="49110-152">升级非常重要，因为它可让你访问的改进了的 API 图面，为更易于使用，以及最新 bug 修补程序。</span><span class="sxs-lookup"><span data-stu-id="49110-152">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="49110-153">首先，我们需要从 NuGet 获取实体框架的最新版本。</span><span class="sxs-lookup"><span data-stu-id="49110-153">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="49110-154">**项目 –&gt;管理 NuGet 包...** 
    *如果你没有**管理 NuGet 包...** 选项应该安装[最新版本的 NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span><span class="sxs-lookup"><span data-stu-id="49110-154">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="49110-155">选择**Online**选项卡</span><span class="sxs-lookup"><span data-stu-id="49110-155">Select the **Online** tab</span></span>
-   <span data-ttu-id="49110-156">选择**EntityFramework**包</span><span class="sxs-lookup"><span data-stu-id="49110-156">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="49110-157">单击**安装**</span><span class="sxs-lookup"><span data-stu-id="49110-157">Click **Install**</span></span>

<span data-ttu-id="49110-158">接下来，我们需要交换我们的模型来生成利用了 DbContext API，在实体框架的更高版本中引入的代码。</span><span class="sxs-lookup"><span data-stu-id="49110-158">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="49110-159">在您的模型在 EF 设计器中的某一空白点上右键单击并选择**添加代码生成项...**</span><span class="sxs-lookup"><span data-stu-id="49110-159">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="49110-160">选择**联机模板**从左侧的菜单并搜索**DbContext**</span><span class="sxs-lookup"><span data-stu-id="49110-160">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="49110-161">选择 EF **5.x DbContext 生成器 C\#**，输入**BloggingModel**作为名称，然后单击**添加**</span><span class="sxs-lookup"><span data-stu-id="49110-161">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![DbContextTemplate](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a><span data-ttu-id="49110-163">3.生成数据库</span><span class="sxs-lookup"><span data-stu-id="49110-163">3. Generating the Database</span></span>

<span data-ttu-id="49110-164">鉴于我们的模型，实体框架可以计算将使我们能够存储和检索使用模型数据的数据库架构。</span><span class="sxs-lookup"><span data-stu-id="49110-164">Given our model, Entity Framework can calculate a database schema that will allow us to store and retrieve data using the model.</span></span>

<span data-ttu-id="49110-165">随 Visual Studio 一起安装的数据库服务器是已安装的 Visual Studio 的版本而异：</span><span class="sxs-lookup"><span data-stu-id="49110-165">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="49110-166">如果您使用的 Visual Studio 2010 系统将创建一个 SQL Express 数据库。</span><span class="sxs-lookup"><span data-stu-id="49110-166">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="49110-167">如果你正在使用 Visual Studio 2012，则您将创建[LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx)数据库。</span><span class="sxs-lookup"><span data-stu-id="49110-167">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

<span data-ttu-id="49110-168">让我们继续并生成的数据库。</span><span class="sxs-lookup"><span data-stu-id="49110-168">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="49110-169">右键单击设计图面，然后选择**根据模型生成数据库...**</span><span class="sxs-lookup"><span data-stu-id="49110-169">Right-click on the design surface and select **Generate Database from Model…**</span></span>
-   <span data-ttu-id="49110-170">单击**新建连接...**</span><span class="sxs-lookup"><span data-stu-id="49110-170">Click **New Connection…**</span></span> <span data-ttu-id="49110-171">指定 LocalDB 或 SQL Express，具体取决于哪个版本的 Visual Studio 使用，并输入**ModelFirst.Blogging**作为数据库名称。</span><span class="sxs-lookup"><span data-stu-id="49110-171">and specify either LocalDB or SQL Express, depending on which version of Visual Studio you are using, enter **ModelFirst.Blogging** as the database name.</span></span>

    ![LocalDBConnectionMF](~/ef6/media/localdbconnectionmf.png)

    ![SqlExpressConnectionMF](~/ef6/media/sqlexpressconnectionmf.png)

-   <span data-ttu-id="49110-174">选择**确定**并将你想要创建新数据库，请选择要求你**是**</span><span class="sxs-lookup"><span data-stu-id="49110-174">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="49110-175">选择**下一步**和实体框架设计器会计算一个脚本来创建数据库架构</span><span class="sxs-lookup"><span data-stu-id="49110-175">Select **Next** and the Entity Framework Designer will calculate a script to create the database schema</span></span>
-   <span data-ttu-id="49110-176">该脚本显示后，单击**完成**和脚本将添加到你的项目，并打开</span><span class="sxs-lookup"><span data-stu-id="49110-176">Once the script is displayed, click **Finish** and the script will be added to your project and opened</span></span>
-   <span data-ttu-id="49110-177">右键单击脚本，并选择**Execute**，系统会提示指定要连接到的数据库指定 LocalDB 或 SQL Server Express，具体取决于哪个版本的 Visual Studio 使用</span><span class="sxs-lookup"><span data-stu-id="49110-177">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="49110-178">4.读取和写入数据</span><span class="sxs-lookup"><span data-stu-id="49110-178">4. Reading & Writing Data</span></span>

<span data-ttu-id="49110-179">现在，我们有一个模型就可以使用它来访问某些数据。</span><span class="sxs-lookup"><span data-stu-id="49110-179">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="49110-180">我们将这些类用于访问数据在自动生成的基于 EDMX 文件。</span><span class="sxs-lookup"><span data-stu-id="49110-180">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="49110-181">*此屏幕截图取自 Visual Studio 2012 中，如果您使用的 Visual Studio 2010 BloggingModel.tt 并 BloggingModel.Context.tt 文件将直接位于你的项目而不是嵌套在 EDMX 文件下。*</span><span class="sxs-lookup"><span data-stu-id="49110-181">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![GeneratedClasses](~/ef6/media/generatedclasses.png)

<span data-ttu-id="49110-183">在 Program.cs 中实现的 Main 方法，如下所示。</span><span class="sxs-lookup"><span data-stu-id="49110-183">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="49110-184">此代码将创建我们的上下文的新实例，并使用它来插入新博客。</span><span class="sxs-lookup"><span data-stu-id="49110-184">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="49110-185">然后它使用 LINQ 查询以从按标题按字母顺序排序的数据库中检索所有博客。</span><span class="sxs-lookup"><span data-stu-id="49110-185">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

<span data-ttu-id="49110-186">你现在可以运行该应用程序和它进行测试。</span><span class="sxs-lookup"><span data-stu-id="49110-186">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="49110-187">5.处理模型更改</span><span class="sxs-lookup"><span data-stu-id="49110-187">5. Dealing with Model Changes</span></span>

<span data-ttu-id="49110-188">现在就应该对我们的模型，进行一些更改，当我们进行这些更改，我们还需要更新数据库架构。</span><span class="sxs-lookup"><span data-stu-id="49110-188">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span>

<span data-ttu-id="49110-189">我们将开始通过将新的用户实体添加到我们的模型。</span><span class="sxs-lookup"><span data-stu-id="49110-189">We’ll start by adding a new User entity to our model.</span></span>

-   <span data-ttu-id="49110-190">添加一个新**用户**与实体名称**用户名**作为键名称并**字符串**作为键的属性类型</span><span class="sxs-lookup"><span data-stu-id="49110-190">Add a new **User** entity name with **Username** as the key name and **String** as the property type for the key</span></span>

    ![AddUserEntity](~/ef6/media/adduserentity.png)

-   <span data-ttu-id="49110-192">右键单击**用户名**设计图面并选择属性**属性**，在属性窗口中更改**MaxLength**设置为**50** 
    *这会限制可存储在为 50 个字符的用户名中的数据*</span><span class="sxs-lookup"><span data-stu-id="49110-192">Right-click on the **Username** property on the design surface and select **Properties**, In the Properties window change the **MaxLength** setting to **50**
*This restricts the data that can be stored in username to 50 characters*</span></span>
-   <span data-ttu-id="49110-193">添加**DisplayName**标量属性设置为**用户**实体</span><span class="sxs-lookup"><span data-stu-id="49110-193">Add a **DisplayName** scalar property to the **User** entity</span></span>

<span data-ttu-id="49110-194">我们现在有更新的模型，我们也已准备好更新数据库以容纳我们新的用户实体类型。</span><span class="sxs-lookup"><span data-stu-id="49110-194">We now have an updated model and we are ready to update the database to accommodate our new User entity type.</span></span>

-   <span data-ttu-id="49110-195">右键单击设计图面，然后选择**根据模型生成数据库...**，实体框架将计算一个脚本来重新创建基于更新后的模型架构。</span><span class="sxs-lookup"><span data-stu-id="49110-195">Right-click on the design surface and select **Generate Database from Model…**, Entity Framework will calculate a script to recreate a schema based on the updated model.</span></span>
-   <span data-ttu-id="49110-196">单击**完成**</span><span class="sxs-lookup"><span data-stu-id="49110-196">Click **Finish**</span></span>
-   <span data-ttu-id="49110-197">可能会收到警告有关覆盖现有的 DDL 脚本和模型的映射和存储部分，单击**是**上述这两个警告</span><span class="sxs-lookup"><span data-stu-id="49110-197">You may receive warnings about overwriting the existing DDL script and the mapping and storage parts of the model, click **Yes** for both these warnings</span></span>
-   <span data-ttu-id="49110-198">为您打开已更新的 SQL 脚本，以创建数据库</span><span class="sxs-lookup"><span data-stu-id="49110-198">The updated SQL script to create the database is opened for you</span></span>  
    <span data-ttu-id="49110-199">*生成脚本将删除所有现有的表，然后重新创建从零开始的架构。这可能适用于本地开发，但不是可用于将更改推送到已部署的数据库。如果你需要将更改发布到已部署的数据库，您需要编辑该脚本或使用架构比较工具来计算的迁移脚本。*</span><span class="sxs-lookup"><span data-stu-id="49110-199">*The script that is generated will drop all existing tables and then recreate the schema from scratch. This may work for local development but is not a viable for pushing changes to a database that has already been deployed. If you need to publish changes to a database that has already been deployed, you will need to edit the script or use a schema compare tool to calculate a migration script.*</span></span>
-   <span data-ttu-id="49110-200">右键单击脚本，并选择**Execute**，系统会提示指定要连接到的数据库指定 LocalDB 或 SQL Server Express，具体取决于哪个版本的 Visual Studio 使用</span><span class="sxs-lookup"><span data-stu-id="49110-200">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="summary"></a><span data-ttu-id="49110-201">总结</span><span class="sxs-lookup"><span data-stu-id="49110-201">Summary</span></span>

<span data-ttu-id="49110-202">在此演练中探讨了模型优先开发，这使我们能够在 EF 设计器中创建的模型，然后从该模型生成数据库。</span><span class="sxs-lookup"><span data-stu-id="49110-202">In this walkthrough we looked at Model First development, which allowed us to create a model in the EF Designer and then generate a database from that model.</span></span> <span data-ttu-id="49110-203">然后，使用模型来读取和从数据库中写入一些数据。</span><span class="sxs-lookup"><span data-stu-id="49110-203">We then used the model to read and write some data from the database.</span></span> <span data-ttu-id="49110-204">最后，我们更新该模型，然后重新创建数据库架构以匹配模型。</span><span class="sxs-lookup"><span data-stu-id="49110-204">Finally, we updated the model and then recreated the database schema to match the model.</span></span>
