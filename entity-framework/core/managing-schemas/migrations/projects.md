---
title: 多个项目的 EF Core 的迁移
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 76e88dd486b1c53dc69a24e35710511bf9cb673b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997982"
---
<a name="using-a-separate-project"></a><span data-ttu-id="d11a2-102">使用一个单独的项目</span><span class="sxs-lookup"><span data-stu-id="d11a2-102">Using a Separate Project</span></span>
========================
<span data-ttu-id="d11a2-103">你可能想要比另一个包含其他程序集中存储迁移你`DbContext`。</span><span class="sxs-lookup"><span data-stu-id="d11a2-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="d11a2-104">此外可以使用此策略，即维护多个集的迁移，例如，一个用于开发，另一个发行版本升级。</span><span class="sxs-lookup"><span data-stu-id="d11a2-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="d11a2-105">若要执行此操作...</span><span class="sxs-lookup"><span data-stu-id="d11a2-105">To do this...</span></span>

1. <span data-ttu-id="d11a2-106">创建一个新的类库。</span><span class="sxs-lookup"><span data-stu-id="d11a2-106">Create a new class library.</span></span>

2. <span data-ttu-id="d11a2-107">添加到 DbContext 程序集的引用。</span><span class="sxs-lookup"><span data-stu-id="d11a2-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="d11a2-108">将迁移和模型快照文件移动到类库。</span><span class="sxs-lookup"><span data-stu-id="d11a2-108">Move the migrations and model snapshot files to the class library.</span></span>
   > [!TIP]
   > <span data-ttu-id="d11a2-109">如果您不具有现有迁移，将生成一个包含在 DbContext 的项目中，然后将其移动。</span><span class="sxs-lookup"><span data-stu-id="d11a2-109">If you have no existing migrations, generate one in the project containing the DbContext then move it.</span></span> <span data-ttu-id="d11a2-110">这非常重要，因为如果迁移程序集不包含现有的迁移，将找不到 DbContext Add-migration 命令。</span><span class="sxs-lookup"><span data-stu-id="d11a2-110">This is important because if the migrations assembly does not contain an existing migration, the Add-Migration command will be unable to find the DbContext.</span></span>

4. <span data-ttu-id="d11a2-111">配置迁移程序集：</span><span class="sxs-lookup"><span data-stu-id="d11a2-111">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="d11a2-112">添加到迁移程序集从启动程序集的引用。</span><span class="sxs-lookup"><span data-stu-id="d11a2-112">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="d11a2-113">如果此操作导致循环依赖项，更新的类库的输出路径：</span><span class="sxs-lookup"><span data-stu-id="d11a2-113">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="d11a2-114">如果您这样做的所有内容正确，您应能够向项目添加新迁移。</span><span class="sxs-lookup"><span data-stu-id="d11a2-114">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
