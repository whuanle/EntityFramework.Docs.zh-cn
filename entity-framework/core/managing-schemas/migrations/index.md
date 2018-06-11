---
title: 迁移 - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: dd164125c053497af94773011127853ad10d27a6
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2018
ms.locfileid: "34754504"
---
<a name="migrations"></a>迁移
==========
通过迁移能够以递增方式将架构更改应用到数据库，使其与 EF Core 模型保持同步，同时保留数据库中的现有数据。

<a name="creating-the-database"></a>创建数据库
---------------------
[定义初始模型][1]后，即应创建数据库。 为此，需要添加初始迁移。
请安装 [EF Core 工具][2]并运行相应命令。

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

向“迁移”目录下的项目添加以下三个文件：

* **00000000000000_InitialCreate.cs**--主迁移文件。 包含应用迁移所需的操作（在 `Up()` 中）和还原迁移所需的操作（在 `Down()` 中）。
* **00000000000000_InitialCreate.Designer.cs**--迁移元数据文件。 包含 EF 所用的信息。
* **MyContextModelSnapshot.cs**--当前模型的快照。 用于确定添加下一迁移时的更改内容。

文件名中的时间戳有助于保持文件按时间顺序排列，以便你可以查看更改进展。

> [!TIP]
> 可以自由移动“迁移”文件并更改其命名空间。 创建的新迁移属于上个迁移的同级。

接下来，将迁移应用到数据库以创建架构。

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="adding-another-migration"></a>添加另一个迁移
------------------------
更改 EF Core 模型后，数据库架构将不同步。为使其保持最新，请再添加一个迁移。 迁移名称的用途与版本控制系统中的提交消息类似。 例如，如果我作出更改并保存客户对产品的评论，则可以选择类似于 AddProductReviews 的内容。

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

搭建迁移基架后，应检查迁移的准确性，并添加正确应用迁移所需的任何其他操作。 例如，迁移可能包含以下操作：

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

虽然这些操作可使数据库架构兼容，但是它们不会保留现有客户姓名。 为了进行改善，请按如下所示进行重写。

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
> 如果在基架化某一操作时添加新的迁移，系统会提醒你此举可能导致数据丢失（例如删除某列）。 请务必仔细检查这些迁移的准确性。

使用相应命令将迁移应用到数据库。

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="removing-a-migration"></a>删除迁移
--------------------
有时，你可能在添加迁移后意识到需要在应用迁移前对 EF Core 模型作出其他更改。
要删除上个迁移，请使用如下命令。

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

删除迁移后，可对模型作出其他更改，然后再次添加迁移。

<a name="reverting-a-migration"></a>还原迁移
---------------------
如果已对数据库应用一个迁移（或多个迁移），但需要将其复原，则可使用应用迁移的相同命令并指定要回退的迁移名称。

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="empty-migrations"></a>空迁移
----------------
有时添加迁移而不进行任何模型更改很有用处。 在这种情况下，添加新迁移会创建一个空迁移。 可以自定义此迁移，执行与 EF Core 模型不直接相关的操作。
可能希望通过此方式管理的一些事项包括：

* 全文搜索
* 函数
* 存储过程
* 触发器
* 视图
* 等等

<a name="generating-a-sql-script"></a>生成 SQL 脚本
-----------------------
调试迁移或将其部署到生产数据库时，生成一个 SQL 脚本很有帮助。 之后可进一步检查该脚本的准确性，并对其作出调整以满足生产数据库的需求。 该脚本还可与部署技术结合使用。 基本命令如下。

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

此命令有几个选项。

from 迁移应是运行该脚本前应用到数据库的最后一个迁移。 如果未应用任何迁移，请指定 `0`（默认值）。

to 迁移是运行该脚本后应用到数据库的最后一个迁移。 它默认为项目中的最后一个迁移。

可以选择生成 idempotent 脚本。 此脚本仅会应用尚未应用到数据库的迁移。 如果不确知应用到数据库的最后一个迁移或需要部署到多个可能分别处于不同迁移的数据库，此脚本非常有用。

<a name="applying-migrations-at-runtime"></a>在运行时应用迁移
------------------------------
启动或首次运行期间，一些应用可能需要在运行时应用迁移。 为此，请使用 `Migrate()` 方法。

请注意，此方法并不适合所有人。 尽管此方法非常适合具有本地数据库的应用，但是大多数应用程序需要更可靠的部署策略，例如生成 SQL 脚本。

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> 请勿在 `Migrate()` 前调用 `EnsureCreated()`。 `EnsureCreated()` 会绕过迁移创建架构，这会导致 `Migrate()` 失败。

> [!NOTE]
> 此方法构建于 `IMigrator` 服务之上，该服务可用于更多高级方案。 请使用 `DbContext.GetService<IMigrator>()` 进行访问。


  [1]: ../../modeling/index.md
  [2]: ../../miscellaneous/cli/index.md
