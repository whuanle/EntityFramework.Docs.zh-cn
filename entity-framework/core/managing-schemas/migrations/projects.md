---
title: 多个项目的 EF Core 的迁移
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 30a6afad1488e74ce2585be3d780186311379a97
ms.sourcegitcommit: ad1bdea58ed35d0f19791044efe9f72f94189c18
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/28/2018
ms.locfileid: "47447139"
---
<a name="using-a-separate-project"></a>使用一个单独的项目
========================
你可能想要比另一个包含其他程序集中存储迁移你`DbContext`。 此外可以使用此策略，即维护多个集的迁移，例如，一个用于开发，另一个发行版本升级。

若要执行此操作...

1. 创建一个新的类库。

2. 添加到 DbContext 程序集的引用。

3. 将迁移和模型快照文件移动到类库。
   > [!TIP]
   > 如果您不具有现有迁移，将生成一个包含在 DbContext 的项目中，然后将其移动。 这非常重要，因为如果迁移程序集不包含现有的迁移，将找不到 DbContext Add-migration 命令。

4. 配置迁移程序集：

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. 添加到迁移程序集从启动程序集的引用。
   * 如果此操作导致循环依赖项，更新的类库的输出路径：

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

如果您这样做的所有内容正确，您应能够向项目添加新迁移。

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
