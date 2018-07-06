---
title: 从 EF 升级核心到 RTM-1.0 RC2 EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
ms.technology: entity-framework-core
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 4bb4c5736708413f6581cad250b089b7bc22a559
ms.sourcegitcommit: 90139dbd6f485473afda0788a5a314c9aa601ea0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2018
ms.locfileid: "30151035"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a><span data-ttu-id="9993f-102">从 EF Core 1.0 RC2 升级到 RTM</span><span class="sxs-lookup"><span data-stu-id="9993f-102">Upgrading from EF Core 1.0 RC2 to RTM</span></span>

<span data-ttu-id="9993f-103">本文提供了将用到 1.0.0 RC2 包生成应用程序迁移指南 RTM。</span><span class="sxs-lookup"><span data-stu-id="9993f-103">This article provides guidance for moving an application built with the RC2 packages to 1.0.0 RTM.</span></span>

## <a name="package-versions"></a><span data-ttu-id="9993f-104">包版本</span><span class="sxs-lookup"><span data-stu-id="9993f-104">Package Versions</span></span>

<span data-ttu-id="9993f-105">你通常将安装到应用程序的最上层包的名称未 RC2 和 RTM 之间发生更改。</span><span class="sxs-lookup"><span data-stu-id="9993f-105">The names of the top level packages that you would typically install into an application did not change between RC2 and RTM.</span></span>

<span data-ttu-id="9993f-106">**你需要升级到 RTM 版本，安装的程序包：**</span><span class="sxs-lookup"><span data-stu-id="9993f-106">**You need to upgrade the installed packages to the RTM versions:**</span></span>

* <span data-ttu-id="9993f-107">运行时包 (例如`Microsoft.EntityFrameworkCore.SqlServer`) 更改，不再是`1.0.0-rc2-final`到`1.0.0`。</span><span class="sxs-lookup"><span data-stu-id="9993f-107">Runtime packages (e.g. `Microsoft.EntityFrameworkCore.SqlServer`) changed from `1.0.0-rc2-final` to `1.0.0`.</span></span>

* <span data-ttu-id="9993f-108">`Microsoft.EntityFrameworkCore.Tools`包更改从`1.0.0-preview1-final`到`1.0.0-preview2-final`。</span><span class="sxs-lookup"><span data-stu-id="9993f-108">The `Microsoft.EntityFrameworkCore.Tools` package changed from `1.0.0-preview1-final` to `1.0.0-preview2-final`.</span></span> <span data-ttu-id="9993f-109">请注意，工具仍预发行版。</span><span class="sxs-lookup"><span data-stu-id="9993f-109">Note that tooling is still pre-release.</span></span>

## <a name="existing-migrations-may-need-maxlength-added"></a><span data-ttu-id="9993f-110">现有的迁移可能需要添加的 maxLength</span><span class="sxs-lookup"><span data-stu-id="9993f-110">Existing migrations may need maxLength added</span></span>

<span data-ttu-id="9993f-111">在 RC2，在迁移中的列定义如下所示`table.Column<string>(nullable: true)`和列的长度在我们将存储迁移背后的代码在某些元数据中进行查找。</span><span class="sxs-lookup"><span data-stu-id="9993f-111">In RC2, the column definition in a migration looked like `table.Column<string>(nullable: true)` and the length of the column was looked up in some metadata we store in the code behind the migration.</span></span> <span data-ttu-id="9993f-112">在 RTM，长度现在包含在基架代码`table.Column<string>(maxLength: 450, nullable: true)`。</span><span class="sxs-lookup"><span data-stu-id="9993f-112">In RTM, the length is now included in the scaffolded code `table.Column<string>(maxLength: 450, nullable: true)`.</span></span>

<span data-ttu-id="9993f-113">已在使用 RTM 之前基架的任何现有迁移不会`maxLength`指定参数。</span><span class="sxs-lookup"><span data-stu-id="9993f-113">Any existing migrations that were scaffolded prior to using RTM will not have the `maxLength` argument specified.</span></span> <span data-ttu-id="9993f-114">这意味着将使用数据库支持的最大长度 (`nvarchar(max)` SQL Server 上)。</span><span class="sxs-lookup"><span data-stu-id="9993f-114">This means the maximum length supported by the database will be used (`nvarchar(max)` on SQL Server).</span></span> <span data-ttu-id="9993f-115">这可能是相当不错的某些列，但键外, 键的一部分的列或索引需要更新，以包含最大长度。</span><span class="sxs-lookup"><span data-stu-id="9993f-115">This may be fine for some columns, but columns that are part of a key, foreign key, or index need to be updated to include a maximum length.</span></span> <span data-ttu-id="9993f-116">按照约定，450 是最大长度用于密钥外, 键和索引列。</span><span class="sxs-lookup"><span data-stu-id="9993f-116">By convention, 450 is the maximum length used for keys, foreign keys, and indexed columns.</span></span> <span data-ttu-id="9993f-117">如果模型中，具有显式配置长度，然后应改为使用该长度。</span><span class="sxs-lookup"><span data-stu-id="9993f-117">If you have explicitly configured a length in the model, then you should use that length instead.</span></span>

<span data-ttu-id="9993f-118">**ASP.NET 标识**</span><span class="sxs-lookup"><span data-stu-id="9993f-118">**ASP.NET Identity**</span></span>

<span data-ttu-id="9993f-119">此更改会影响使用 ASP.NET 标识，并且已从预先创建的项目的 RTM 项目模板。</span><span class="sxs-lookup"><span data-stu-id="9993f-119">This change impacts projects that use ASP.NET Identity and were created from a pre-RTM project template.</span></span> <span data-ttu-id="9993f-120">项目模板包括用于创建数据库的迁移。</span><span class="sxs-lookup"><span data-stu-id="9993f-120">The project template includes a migration used to create the database.</span></span> <span data-ttu-id="9993f-121">必须编辑此迁移，以指定最大长度为`256`为以下列。</span><span class="sxs-lookup"><span data-stu-id="9993f-121">This migration must be edited to specify a maximum length of `256` for the following columns.</span></span>

*  <span data-ttu-id="9993f-122">**AspNetRoles**</span><span class="sxs-lookup"><span data-stu-id="9993f-122">**AspNetRoles**</span></span>

    * <span data-ttu-id="9993f-123">名称</span><span class="sxs-lookup"><span data-stu-id="9993f-123">Name</span></span>

    * <span data-ttu-id="9993f-124">NormalizedName</span><span class="sxs-lookup"><span data-stu-id="9993f-124">NormalizedName</span></span>

*  <span data-ttu-id="9993f-125">**AspNetUsers**</span><span class="sxs-lookup"><span data-stu-id="9993f-125">**AspNetUsers**</span></span>

   * <span data-ttu-id="9993f-126">电子邮件</span><span class="sxs-lookup"><span data-stu-id="9993f-126">Email</span></span>

   * <span data-ttu-id="9993f-127">NormalizedEmail</span><span class="sxs-lookup"><span data-stu-id="9993f-127">NormalizedEmail</span></span>

   * <span data-ttu-id="9993f-128">NormalizedUserName</span><span class="sxs-lookup"><span data-stu-id="9993f-128">NormalizedUserName</span></span>

   * <span data-ttu-id="9993f-129">UserName</span><span class="sxs-lookup"><span data-stu-id="9993f-129">UserName</span></span>

<span data-ttu-id="9993f-130">若要进行此更改将导致以下异常时初始迁移已应用到数据库。</span><span class="sxs-lookup"><span data-stu-id="9993f-130">Failure to make this change will result in the following exception when the initial migration is applied to a database.</span></span>

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a><span data-ttu-id="9993f-131">.NET Core： project.json 中删除"导入"</span><span class="sxs-lookup"><span data-stu-id="9993f-131">.NET Core: Remove "imports" in project.json</span></span>

<span data-ttu-id="9993f-132">如果你已面向.NET Core 与 RC2，需要添加`imports`到临时的解决方法不支持.NET 标准的 EF Core 依赖关系的某些 project.json。</span><span class="sxs-lookup"><span data-stu-id="9993f-132">If you were targeting .NET Core with RC2, you needed to add `imports` to project.json as a temporary workaround for some of EF Core's dependencies not supporting .NET Standard.</span></span> <span data-ttu-id="9993f-133">现在即可移除这些。</span><span class="sxs-lookup"><span data-stu-id="9993f-133">These can now be removed.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

> [!NOTE]  
> <span data-ttu-id="9993f-134">截至版本 1.0 RTM， [.NET Core SDK](https://www.microsoft.com/net/download/core)不再支持`project.json`或开发.NET Core 应用程序使用 Visual Studio 2015。</span><span class="sxs-lookup"><span data-stu-id="9993f-134">As of version 1.0 RTM, the [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or developing .NET Core applications using Visual Studio 2015.</span></span> <span data-ttu-id="9993f-135">我们建议你[从 project.json 迁移到 csproj](https://docs.microsoft.com/dotnet/articles/core/migration/)。</span><span class="sxs-lookup"><span data-stu-id="9993f-135">We recommend you [migrate from project.json to csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span></span> <span data-ttu-id="9993f-136">如果使用 Visual Studio，我们建议你升级到[Visual Studio 2017](https://www.visualstudio.com/downloads/)。</span><span class="sxs-lookup"><span data-stu-id="9993f-136">If you are using Visual Studio, we recommend you upgrade to [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

## <a name="uwp-add-binding-redirects"></a><span data-ttu-id="9993f-137">UWP： 添加绑定重定向</span><span class="sxs-lookup"><span data-stu-id="9993f-137">UWP: Add binding redirects</span></span>

<span data-ttu-id="9993f-138">尝试运行 EF 命令在通用 Windows 平台 (UWP) 项目导致以下错误：</span><span class="sxs-lookup"><span data-stu-id="9993f-138">Attempting to run EF commands on Universal Windows Platform (UWP) projects results in the following error:</span></span>

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

<span data-ttu-id="9993f-139">你需要手动将绑定重定向添加到 UWP 项目。</span><span class="sxs-lookup"><span data-stu-id="9993f-139">You need to manually add binding redirects to the UWP project.</span></span> <span data-ttu-id="9993f-140">创建名为的文件`App.config`在项目根文件夹并将重定向添加到正确的程序集版本。</span><span class="sxs-lookup"><span data-stu-id="9993f-140">Create a file named `App.config` in the project root folder and add redirects to the correct assembly versions.</span></span>

``` xml
<configuration>
 <runtime>
   <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
     <dependentAssembly>
       <assemblyIdentity name="System.IO.FileSystem.Primitives"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
     <dependentAssembly>
       <assemblyIdentity name="System.Threading.Overlapped"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
   </assemblyBinding>
 </runtime>
</configuration>
```
