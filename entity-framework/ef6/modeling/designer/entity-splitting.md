---
title: 设计器的实体拆分的 EF6
author: divega
ms.date: 2016-10-23
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: 06199be977276cd3656e2550df79bac24276ec51
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250591"
---
# <a name="designer-entity-splitting"></a><span data-ttu-id="d1d4a-102">设计器的实体拆分</span><span class="sxs-lookup"><span data-stu-id="d1d4a-102">Designer Entity Splitting</span></span>
<span data-ttu-id="d1d4a-103">本演练演示如何通过修改具有实体框架设计器 （EF 设计器） 的模型的实体类型映射到两个表。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-103">This walkthrough shows how to map an entity type to two tables by modifying a model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="d1d4a-104">如果多个表共享同一个键，则可以将一个实体映射到这些表。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-104">You can map an entity to multiple tables when the tables share a common key.</span></span> <span data-ttu-id="d1d4a-105">适用于将一个实体类型映射到两个表的概念，可以轻松扩展为将一个实体类型映射到两个以上的表。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-105">The concepts that apply to mapping an entity type to two tables are easily extended to mapping an entity type to more than two tables.</span></span>

<span data-ttu-id="d1d4a-106">下图显示了使用 EF 设计器时使用的主窗口。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF 设计器](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="d1d4a-108">系统必备</span><span class="sxs-lookup"><span data-stu-id="d1d4a-108">Prerequisites</span></span>

<span data-ttu-id="d1d4a-109">Visual Studio 2012 或 Visual Studio 2010、 Ultimate、 Premium、 Professional 或 Web Express edition。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-109">Visual Studio 2012 or Visual Studio 2010, Ultimate, Premium, Professional, or Web Express edition.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="d1d4a-110">创建数据库</span><span class="sxs-lookup"><span data-stu-id="d1d4a-110">Create the Database</span></span>

<span data-ttu-id="d1d4a-111">随 Visual Studio 一起安装的数据库服务器是已安装的 Visual Studio 的版本而异：</span><span class="sxs-lookup"><span data-stu-id="d1d4a-111">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="d1d4a-112">如果使用 Visual Studio 2012，则您将创建 LocalDB 数据库。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-112">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="d1d4a-113">如果您使用的 Visual Studio 2010 系统将创建一个 SQL Express 数据库。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-113">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="d1d4a-114">首先我们将使用我们要合并到单个实体的两个表创建一个数据库。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-114">First we'll create a database with two tables that we are going to combine into a single entity.</span></span>

-   <span data-ttu-id="d1d4a-115">打开 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d1d4a-115">Open Visual Studio</span></span>
-   <span data-ttu-id="d1d4a-116">**视图-&gt;服务器资源管理器**</span><span class="sxs-lookup"><span data-stu-id="d1d4a-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="d1d4a-117">右键单击**数据连接-&gt;添加连接...**</span><span class="sxs-lookup"><span data-stu-id="d1d4a-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="d1d4a-118">如果你尚未连接到数据库服务器资源管理器之前将需要选择**Microsoft SQL Server**作为数据源</span><span class="sxs-lookup"><span data-stu-id="d1d4a-118">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="d1d4a-119">连接到 LocalDB 或 SQL Express，具体取决于哪一个已安装</span><span class="sxs-lookup"><span data-stu-id="d1d4a-119">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="d1d4a-120">输入**EntitySplitting**作为数据库名称</span><span class="sxs-lookup"><span data-stu-id="d1d4a-120">Enter **EntitySplitting** as the database name</span></span>
-   <span data-ttu-id="d1d4a-121">选择**确定**并将你想要创建新数据库，请选择要求你**是**</span><span class="sxs-lookup"><span data-stu-id="d1d4a-121">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="d1d4a-122">新的数据库现在将出现在服务器资源管理器</span><span class="sxs-lookup"><span data-stu-id="d1d4a-122">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="d1d4a-123">如果你正在使用 Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="d1d4a-123">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="d1d4a-124">右键单击服务器资源管理器中的数据库，然后选择**新查询**</span><span class="sxs-lookup"><span data-stu-id="d1d4a-124">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="d1d4a-125">将下面的 SQL 复制到新的查询，然后右键单击查询并选择**Execute**</span><span class="sxs-lookup"><span data-stu-id="d1d4a-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="d1d4a-126">如果您使用的 Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="d1d4a-126">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="d1d4a-127">选择**数据-&gt; Transact SQL 编辑器-&gt;新建查询连接...**</span><span class="sxs-lookup"><span data-stu-id="d1d4a-127">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="d1d4a-128">输入 **。\\SQLEXPRESS**作为服务器名称，然后单击**确定**</span><span class="sxs-lookup"><span data-stu-id="d1d4a-128">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="d1d4a-129">选择**EntitySplitting**数据库从下拉列表顶部的查询编辑器</span><span class="sxs-lookup"><span data-stu-id="d1d4a-129">Select the **EntitySplitting** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="d1d4a-130">将下面的 SQL 复制到新的查询，然后右键单击查询并选择**执行 SQL**</span><span class="sxs-lookup"><span data-stu-id="d1d4a-130">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

``` SQL
CREATE TABLE [dbo].[Person] (
[PersonId] INT IDENTITY (1, 1) NOT NULL,
[FirstName] NVARCHAR (200) NULL,
[LastName] NVARCHAR (200) NULL,
CONSTRAINT [PK_Person] PRIMARY KEY CLUSTERED ([PersonId] ASC)
);

CREATE TABLE [dbo].[PersonInfo] (
[PersonId] INT NOT NULL,
[Email] NVARCHAR (200) NULL,
[Phone] NVARCHAR (50) NULL,
CONSTRAINT [PK_PersonInfo] PRIMARY KEY CLUSTERED ([PersonId] ASC),
CONSTRAINT [FK_Person_PersonInfo] FOREIGN KEY ([PersonId]) REFERENCES [dbo].[Person] ([PersonId]) ON DELETE CASCADE
);
```

## <a name="create-the-project"></a><span data-ttu-id="d1d4a-131">创建项目</span><span class="sxs-lookup"><span data-stu-id="d1d4a-131">Create the Project</span></span>

-   <span data-ttu-id="d1d4a-132">在 **“文件”** 菜单上，指向 **“新建”**，然后单击 **“项目”**。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-132">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="d1d4a-133">在左窗格中，单击**Visual C\#**，然后选择**控制台应用程序**模板。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-133">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="d1d4a-134">输入**MapEntityToTablesSample**作为名称的项目并单击**确定**。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-134">Enter **MapEntityToTablesSample** as the name of the project and click **OK**.</span></span>
-   <span data-ttu-id="d1d4a-135">单击**否**如果提示您保存第一个部分中创建的 SQL 查询。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-135">Click **No** if prompted to save the SQL query created in the first section.</span></span>

## <a name="create-a-model-based-on-the-database"></a><span data-ttu-id="d1d4a-136">创建基于数据库的模型</span><span class="sxs-lookup"><span data-stu-id="d1d4a-136">Create a Model based on the Database</span></span>

-   <span data-ttu-id="d1d4a-137">右键单击解决方案资源管理器中的项目名称，指向**外**，然后单击**新项**。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-137">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="d1d4a-138">选择**数据**从左侧的菜单，然后选择**ADO.NET 实体数据模型**模板窗格中。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-138">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="d1d4a-139">输入**MapEntityToTablesModel.edmx**以及该文件的名称，然后单击**添加**。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-139">Enter **MapEntityToTablesModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="d1d4a-140">在选择模型内容对话框中，选择**从数据库生成**，然后单击**下一步。**</span><span class="sxs-lookup"><span data-stu-id="d1d4a-140">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="d1d4a-141">选择**EntitySplitting**从删除的连接，并单击**下一步**。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-141">Select the **EntitySplitting** connection from the drop down and click **Next**.</span></span>
-   <span data-ttu-id="d1d4a-142">在选择数据库对象对话框中，旁边的复选框**表**节点。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-142">In the Choose Your Database Objects dialog box, check the box next to the **Tables** node.</span></span>
    <span data-ttu-id="d1d4a-143">这将添加来自所有表**EntitySplitting**到模型数据库。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-143">This will add all the tables from the **EntitySplitting** database to the model.</span></span>
-   <span data-ttu-id="d1d4a-144">单击 **“完成”**。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-144">Click **Finish**.</span></span>

<span data-ttu-id="d1d4a-145">提供用于编辑模型的设计图面，实体设计器随即出现。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-145">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-an-entity-to-two-tables"></a><span data-ttu-id="d1d4a-146">将一个实体映射到两个表</span><span class="sxs-lookup"><span data-stu-id="d1d4a-146">Map an Entity to Two Tables</span></span>

<span data-ttu-id="d1d4a-147">在此步骤中我们将更新**人员**实体类型将数据从合并**人员**并**PersonInfo**表。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-147">In this step we will update the **Person** entity type to combine data from the **Person** and **PersonInfo** tables.</span></span>

-   <span data-ttu-id="d1d4a-148">选择**电子邮件**并**电话**的属性 \* \* PersonInfo \* \* 实体，然后按**Ctrl + X**密钥。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-148">Select the **Email** and **Phone** properties of the \*\*PersonInfo \*\*entity and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="d1d4a-149">选择 \* \* 人员 \* \* 实体，然后按**Ctrl + V**密钥。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-149">Select the \*\*Person \*\*entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="d1d4a-150">在设计图面上，选择**PersonInfo**实体，然后按**删除**键盘上的按钮。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-150">On the design surface, select the **PersonInfo** entity and press **Delete** button on the keyboard.</span></span>
-   <span data-ttu-id="d1d4a-151">单击**否**当系统询问是否要删除**PersonInfo**表从模型中，我们正准备将其映射到**人员**实体。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-151">Click **No** when asked if you want to remove the **PersonInfo** table from the model, we are about to map it to the **Person** entity.</span></span>

    ![删除表](~/ef6/media/deletetables.png)

<span data-ttu-id="d1d4a-153">下一步骤要求**映射详细信息**窗口。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-153">The next steps require the **Mapping Details** window.</span></span> <span data-ttu-id="d1d4a-154">如果您无法查看此窗口中，右键单击设计图面并选择**映射详细信息**。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-154">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="d1d4a-155">选择**Person**实体类型，然后单击**&lt;添加表或视图&gt;** 中**映射详细信息**窗口。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-155">Select the **Person** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="d1d4a-156">选择 \* \* PersonInfo \* \* 从下拉列表。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-156">Select \*\*PersonInfo \*\* from the drop-down list.</span></span>
    <span data-ttu-id="d1d4a-157">**映射详细信息**窗口将会更新默认列映射，这些是适用于我们的方案。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-157">The **Mapping Details** window is updated with default column mappings, these are fine for our scenario.</span></span>

<span data-ttu-id="d1d4a-158">**Person**实体类型现在所映射到**人员**并**PersonInfo**表。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-158">The **Person** entity type is now mapped to the **Person** and **PersonInfo** tables.</span></span>

![映射 2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a><span data-ttu-id="d1d4a-160">使用模型</span><span class="sxs-lookup"><span data-stu-id="d1d4a-160">Use the Model</span></span>

-   <span data-ttu-id="d1d4a-161">以下代码粘贴到 Main 方法中。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-161">Paste the following code in the Main method.</span></span>

``` csharp
    using (var context = new EntitySplittingEntities())
    {
        var person = new Person
        {
            FirstName = "John",
            LastName = "Doe",
            Email = "john@example.com",
            Phone = "555-555-5555"
        };

        context.People.Add(person);
        context.SaveChanges();

        foreach (var item in context.People)
        {
            Console.WriteLine(item.FirstName);
        }
    }
```

-   <span data-ttu-id="d1d4a-162">编译并运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-162">Compile and run the application.</span></span>

<span data-ttu-id="d1d4a-163">针对因运行此应用程序而数据库执行以下 T-SQL 语句。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-163">The following T-SQL statements were executed against the database as a result of running this application.</span></span> 

-   <span data-ttu-id="d1d4a-164">以下两个**插入**语句执行因执行上下文。Savechanges （)。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-164">The following two **INSERT** statements were executed as a result of executing context.SaveChanges().</span></span> <span data-ttu-id="d1d4a-165">它们采用的数据**Person**实体并将其之间拆分**人员**并**PersonInfo**表。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-165">They take the data from the **Person** entity and split it between the **Person** and **PersonInfo** tables.</span></span>

    ![插入 1](~/ef6/media/insert1.png)

    ![插入 2](~/ef6/media/insert2.png)
-   <span data-ttu-id="d1d4a-168">以下**选择**由于枚举在数据库中的人员执行。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-168">The following **SELECT** was executed as a result of enumerating the people in the database.</span></span> <span data-ttu-id="d1d4a-169">它将从数据相结合**Person**并**PersonInfo**表。</span><span class="sxs-lookup"><span data-stu-id="d1d4a-169">It combines the data from the **Person** and **PersonInfo** table.</span></span>

    ![选择](~/ef6/media/select.png)
