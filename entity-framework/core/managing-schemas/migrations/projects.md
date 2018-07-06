---
title: 多个项目的 EF Core 的迁移
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 3684e86cce0005056380d89604d038c734054d14
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/22/2017
ms.locfileid: "27161222"
---
<a name="using-a-separate-project"></a>使用一个单独的项目
========================
你可能想要比另一个包含其他程序集中存储迁移你`DbContext`。 此外可以使用此策略，维护多个组迁移，例如，一个用于开发，另一个版本的升级。

若要执行此操作...

1. 创建一个新的类库。

2. 添加到 DbContext 程序集的引用。

3. 将迁移和模型快照文件移动到类库。
   * 如果你尚未添加任何，添加一个到 DbContext 项目，然后将其移。

4. 配置迁移程序集：

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. 添加到迁移程序集中的启动程序集的引用。
   * 如果这导致循环依赖关系，更新类库的输出路径：

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStarupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

如果你这样做的所有内容正确，你应能够向项目添加新的迁移。

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
