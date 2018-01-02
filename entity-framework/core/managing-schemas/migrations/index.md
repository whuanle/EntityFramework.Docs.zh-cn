---
title: "迁移 - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 3e75947e65b8e7707292fec8d74b170aff191395
ms.sourcegitcommit: c72d85805db0aa95f980514a18381fdc5e17c786
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2017
---
<a name="migrations"></a><span data-ttu-id="ee589-102">迁移</span><span class="sxs-lookup"><span data-stu-id="ee589-102">Migrations</span></span>
==========
<span data-ttu-id="ee589-103">通过迁移能够以递增方式将架构更改应用到数据库，使其与 EF Core 模型保持同步，同时保留数据库中的现有数据。</span><span class="sxs-lookup"><span data-stu-id="ee589-103">Migrations provide a way to incrementally apply schema changes to the database to keep it in sync with your EF Core model while preserving existing data in the database.</span></span>

<a name="creating-the-database"></a><span data-ttu-id="ee589-104">创建数据库</span><span class="sxs-lookup"><span data-stu-id="ee589-104">Creating the database</span></span>
---------------------
<span data-ttu-id="ee589-105">[定义初始模型][1]后，即应创建数据库。</span><span class="sxs-lookup"><span data-stu-id="ee589-105">After you've [defined your initial model][1], it's time to create the database.</span></span> <span data-ttu-id="ee589-106">为此，需要添加初始迁移。</span><span class="sxs-lookup"><span data-stu-id="ee589-106">To do this, add an initial migration.</span></span>
<span data-ttu-id="ee589-107">请安装 [EF Core 工具][2]并运行相应命令。</span><span class="sxs-lookup"><span data-stu-id="ee589-107">Install the [EF Core Tools][2] and run the appropriate command.</span></span>

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="ee589-108">向“迁移”目录下的项目添加以下三个文件：</span><span class="sxs-lookup"><span data-stu-id="ee589-108">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="ee589-109">**00000000000000_InitialCreate.cs**--主迁移文件。</span><span class="sxs-lookup"><span data-stu-id="ee589-109">**00000000000000_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="ee589-110">包含应用迁移所需的操作（在 `Up()` 中）和还原迁移所需的操作（在 `Down()` 中）。</span><span class="sxs-lookup"><span data-stu-id="ee589-110">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="ee589-111">**00000000000000_InitialCreate.Designer.cs**--迁移元数据文件。</span><span class="sxs-lookup"><span data-stu-id="ee589-111">**00000000000000_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="ee589-112">包含 EF 所用的信息。</span><span class="sxs-lookup"><span data-stu-id="ee589-112">Contains information used by EF.</span></span>
* <span data-ttu-id="ee589-113">**MyContextModelSnapshot.cs**--当前模型的快照。</span><span class="sxs-lookup"><span data-stu-id="ee589-113">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="ee589-114">用于确定添加下一迁移时的更改内容。</span><span class="sxs-lookup"><span data-stu-id="ee589-114">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="ee589-115">文件名中的时间戳有助于保持文件按时间顺序排列，以便你可以查看更改进展。</span><span class="sxs-lookup"><span data-stu-id="ee589-115">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="ee589-116">可以自由移动“迁移”文件并更改其命名空间。</span><span class="sxs-lookup"><span data-stu-id="ee589-116">You are free to move Migrations files and change their namespace.</span></span> <span data-ttu-id="ee589-117">创建的新迁移属于上个迁移的同级。</span><span class="sxs-lookup"><span data-stu-id="ee589-117">New migrations are created as siblings of the last migration.</span></span>

<span data-ttu-id="ee589-118">接下来，将迁移应用到数据库以创建架构。</span><span class="sxs-lookup"><span data-stu-id="ee589-118">Next, apply the migration to the database to create the schema.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="adding-another-migration"></a><span data-ttu-id="ee589-119">添加另一个迁移</span><span class="sxs-lookup"><span data-stu-id="ee589-119">Adding another migration</span></span>
------------------------
<span data-ttu-id="ee589-120">更改 EF Core 模型后，数据库架构将不同步。为使其保持最新，请再添加一个迁移。</span><span class="sxs-lookup"><span data-stu-id="ee589-120">After making changes to your EF Core model, the database schema will be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="ee589-121">迁移名称的用途与版本控制系统中的提交消息类似。</span><span class="sxs-lookup"><span data-stu-id="ee589-121">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="ee589-122">例如，如果我作出更改并保存客户对产品的评论，则可以选择类似于 AddProductReviews 的内容。</span><span class="sxs-lookup"><span data-stu-id="ee589-122">For example, if I made changes to save customer reviews of products, I might choose something like *AddProductReviews*.</span></span>

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

<span data-ttu-id="ee589-123">搭建迁移基架后，应检查迁移的准确性，并添加正确应用迁移所需的任何其他操作。</span><span class="sxs-lookup"><span data-stu-id="ee589-123">Once the migration is scaffolded, you should review it for accuracy and add any additional operations required to apply it correctly.</span></span> <span data-ttu-id="ee589-124">例如，迁移可能包含以下操作：</span><span class="sxs-lookup"><span data-stu-id="ee589-124">For example, your migration might contain the following operations:</span></span>

``` csharp
migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");

migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);
```

<span data-ttu-id="ee589-125">虽然这些操作可使数据库架构兼容，但是它们不会保留现有客户姓名。</span><span class="sxs-lookup"><span data-stu-id="ee589-125">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="ee589-126">为了进行改善，请按如下所示进行重写。</span><span class="sxs-lookup"><span data-stu-id="ee589-126">To make it better, rewrite it as follows.</span></span>

``` csharp
migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);

migrationBuilder.Sql(
@"
    UPDATE Customer
    SET Name = FirstName + ' ' + LastName;
");

migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");
```

> [!TIP]
> <span data-ttu-id="ee589-127">如果在基架化某一操作时添加新的迁移，系统会提醒你此举可能导致数据丢失（例如删除某列）。</span><span class="sxs-lookup"><span data-stu-id="ee589-127">Adding a new migration warns when an operation is scaffolded that may result in data loss (like dropping a column).</span></span> <span data-ttu-id="ee589-128">请务必仔细检查这些迁移的准确性。</span><span class="sxs-lookup"><span data-stu-id="ee589-128">Be sure to especially review these migrations for accuracy.</span></span>

<span data-ttu-id="ee589-129">使用相应命令将迁移应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="ee589-129">Apply the migration to the database using the appropriate command.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="removing-a-migration"></a><span data-ttu-id="ee589-130">删除迁移</span><span class="sxs-lookup"><span data-stu-id="ee589-130">Removing a migration</span></span>
--------------------
<span data-ttu-id="ee589-131">有时，你可能在添加迁移后意识到需要在应用迁移前对 EF Core 模型作出其他更改。</span><span class="sxs-lookup"><span data-stu-id="ee589-131">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span>
<span data-ttu-id="ee589-132">要删除上个迁移，请使用如下命令。</span><span class="sxs-lookup"><span data-stu-id="ee589-132">To remove the last migration, use this command.</span></span>

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

<span data-ttu-id="ee589-133">删除迁移后，可对模型作出其他更改，然后再次添加迁移。</span><span class="sxs-lookup"><span data-stu-id="ee589-133">After removing it, you can make the additional model changes and add it again.</span></span>

<a name="reverting-a-migration"></a><span data-ttu-id="ee589-134">还原迁移</span><span class="sxs-lookup"><span data-stu-id="ee589-134">Reverting a migration</span></span>
---------------------
<span data-ttu-id="ee589-135">如果已对数据库应用一个迁移（或多个迁移），但需要将其复原，则可使用应用迁移的相同命令并指定要回退的迁移名称。</span><span class="sxs-lookup"><span data-stu-id="ee589-135">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="empty-migrations"></a><span data-ttu-id="ee589-136">空迁移</span><span class="sxs-lookup"><span data-stu-id="ee589-136">Empty migrations</span></span>
----------------
<span data-ttu-id="ee589-137">有时添加迁移而不进行任何模型更改很有用处。</span><span class="sxs-lookup"><span data-stu-id="ee589-137">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="ee589-138">在这种情况下，添加新迁移会创建一个空迁移。</span><span class="sxs-lookup"><span data-stu-id="ee589-138">In this case, adding a new migration creates an empty one.</span></span> <span data-ttu-id="ee589-139">可以自定义此迁移，执行与 EF Core 模型不直接相关的操作。</span><span class="sxs-lookup"><span data-stu-id="ee589-139">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span>
<span data-ttu-id="ee589-140">可能希望通过此方式管理的一些事项包括：</span><span class="sxs-lookup"><span data-stu-id="ee589-140">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="ee589-141">全文搜索</span><span class="sxs-lookup"><span data-stu-id="ee589-141">Full-Text Search</span></span>
* <span data-ttu-id="ee589-142">函数</span><span class="sxs-lookup"><span data-stu-id="ee589-142">Functions</span></span>
* <span data-ttu-id="ee589-143">存储过程</span><span class="sxs-lookup"><span data-stu-id="ee589-143">Stored procedures</span></span>
* <span data-ttu-id="ee589-144">触发器</span><span class="sxs-lookup"><span data-stu-id="ee589-144">Triggers</span></span>
* <span data-ttu-id="ee589-145">视图</span><span class="sxs-lookup"><span data-stu-id="ee589-145">Views</span></span>
* <span data-ttu-id="ee589-146">等。</span><span class="sxs-lookup"><span data-stu-id="ee589-146">etc.</span></span>

<a name="generating-a-sql-script"></a><span data-ttu-id="ee589-147">生成 SQL 脚本</span><span class="sxs-lookup"><span data-stu-id="ee589-147">Generating a SQL script</span></span>
-----------------------
<span data-ttu-id="ee589-148">调试迁移或将其部署到生产数据库时，生成一个 SQL 脚本很有帮助。</span><span class="sxs-lookup"><span data-stu-id="ee589-148">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="ee589-149">之后可进一步检查该脚本的准确性，并对其作出调整以满足生产数据库的需求。</span><span class="sxs-lookup"><span data-stu-id="ee589-149">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="ee589-150">该脚本还可与部署技术结合使用。</span><span class="sxs-lookup"><span data-stu-id="ee589-150">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="ee589-151">基本命令如下。</span><span class="sxs-lookup"><span data-stu-id="ee589-151">The basic command is as follows.</span></span>

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

<span data-ttu-id="ee589-152">此命令有几个选项。</span><span class="sxs-lookup"><span data-stu-id="ee589-152">There are several options to this command.</span></span>

<span data-ttu-id="ee589-153">from 迁移应是运行该脚本前应用到数据库的最后一个迁移。</span><span class="sxs-lookup"><span data-stu-id="ee589-153">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="ee589-154">如果未应用任何迁移，请指定 `0`（默认值）。</span><span class="sxs-lookup"><span data-stu-id="ee589-154">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="ee589-155">to 迁移是运行该脚本后应用到数据库的最后一个迁移。</span><span class="sxs-lookup"><span data-stu-id="ee589-155">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="ee589-156">它默认为项目中的最后一个迁移。</span><span class="sxs-lookup"><span data-stu-id="ee589-156">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="ee589-157">可以选择生成 idempotent 脚本。</span><span class="sxs-lookup"><span data-stu-id="ee589-157">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="ee589-158">此脚本仅会应用尚未应用到数据库的迁移。</span><span class="sxs-lookup"><span data-stu-id="ee589-158">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="ee589-159">如果不确知应用到数据库的最后一个迁移或需要部署到多个可能分别处于不同迁移的数据库，此脚本非常有用。</span><span class="sxs-lookup"><span data-stu-id="ee589-159">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

<a name="applying-migrations-at-runtime"></a><span data-ttu-id="ee589-160">在运行时应用迁移</span><span class="sxs-lookup"><span data-stu-id="ee589-160">Applying migrations at runtime</span></span>
------------------------------
<span data-ttu-id="ee589-161">启动或首次运行期间，一些应用可能需要在运行时应用迁移。</span><span class="sxs-lookup"><span data-stu-id="ee589-161">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="ee589-162">为此，请使用 `Migrate()` 方法。</span><span class="sxs-lookup"><span data-stu-id="ee589-162">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="ee589-163">请注意，此方法并不适合所有人。</span><span class="sxs-lookup"><span data-stu-id="ee589-163">Caution, this approach isn't for everyone.</span></span> <span data-ttu-id="ee589-164">尽管此方法非常适合具有本地数据库的应用，但是大多数应用程序需要更可靠的部署策略，例如生成 SQL 脚本。</span><span class="sxs-lookup"><span data-stu-id="ee589-164">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> <span data-ttu-id="ee589-165">请勿在 `Migrate()` 前调用 `EnsureCreated()`。</span><span class="sxs-lookup"><span data-stu-id="ee589-165">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="ee589-166">`EnsureCreated()` 会绕过迁移创建架构并导致 `Migrate()` 失败。</span><span class="sxs-lookup"><span data-stu-id="ee589-166">`EnsureCreated()` bypasses Migrations to create the schema and cause `Migrate()` to fail.</span></span>

> [!NOTE]
> <span data-ttu-id="ee589-167">此方法构建于 `IMigrator` 服务之上，该服务可用于更多高级方案。</span><span class="sxs-lookup"><span data-stu-id="ee589-167">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="ee589-168">请使用 `DbContext.GetService<IMigrator>()` 进行访问。</span><span class="sxs-lookup"><span data-stu-id="ee589-168">Use `DbContext.GetService<IMigrator>()` to access it.</span></span>


  [1]: ../../modeling/index.md
  [2]: ../../miscellaneous/cli/index.md
