---
title: 从 EF 升级核心到 RTM-1.0 RC2 EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 1b95b2ab1943dfb541b3a7c873cff3cb4c16d9c1
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998314"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a><span data-ttu-id="fb28a-102">从 EF Core 1.0 RC2 升级到 RTM</span><span class="sxs-lookup"><span data-stu-id="fb28a-102">Upgrading from EF Core 1.0 RC2 to RTM</span></span>

<span data-ttu-id="fb28a-103">本文介绍了使用为 1.0.0 RC2 包生成应用程序迁移指南 RTM。</span><span class="sxs-lookup"><span data-stu-id="fb28a-103">This article provides guidance for moving an application built with the RC2 packages to 1.0.0 RTM.</span></span>

## <a name="package-versions"></a><span data-ttu-id="fb28a-104">包版本</span><span class="sxs-lookup"><span data-stu-id="fb28a-104">Package Versions</span></span>

<span data-ttu-id="fb28a-105">RC2 和 RTM 之间未更改通常会安装到的应用程序的最高级别包的名称。</span><span class="sxs-lookup"><span data-stu-id="fb28a-105">The names of the top level packages that you would typically install into an application did not change between RC2 and RTM.</span></span>

<span data-ttu-id="fb28a-106">**需要升级到 RTM 版本的已安装的包：**</span><span class="sxs-lookup"><span data-stu-id="fb28a-106">**You need to upgrade the installed packages to the RTM versions:**</span></span>

* <span data-ttu-id="fb28a-107">运行时包 (例如， `Microsoft.EntityFrameworkCore.SqlServer`) 从更改`1.0.0-rc2-final`到`1.0.0`。</span><span class="sxs-lookup"><span data-stu-id="fb28a-107">Runtime packages (for example, `Microsoft.EntityFrameworkCore.SqlServer`) changed from `1.0.0-rc2-final` to `1.0.0`.</span></span>

* <span data-ttu-id="fb28a-108">`Microsoft.EntityFrameworkCore.Tools`包中更改`1.0.0-preview1-final`到`1.0.0-preview2-final`。</span><span class="sxs-lookup"><span data-stu-id="fb28a-108">The `Microsoft.EntityFrameworkCore.Tools` package changed from `1.0.0-preview1-final` to `1.0.0-preview2-final`.</span></span> <span data-ttu-id="fb28a-109">请注意，工具仍预发行版。</span><span class="sxs-lookup"><span data-stu-id="fb28a-109">Note that tooling is still pre-release.</span></span>

## <a name="existing-migrations-may-need-maxlength-added"></a><span data-ttu-id="fb28a-110">现有迁移可能需要添加的 maxLength</span><span class="sxs-lookup"><span data-stu-id="fb28a-110">Existing migrations may need maxLength added</span></span>

<span data-ttu-id="fb28a-111">在 RC2 中，在迁移中的列定义看起来像`table.Column<string>(nullable: true)`和中我们将存储在代码隐藏迁移一些元数据中列的长度进行查找。</span><span class="sxs-lookup"><span data-stu-id="fb28a-111">In RC2, the column definition in a migration looked like `table.Column<string>(nullable: true)` and the length of the column was looked up in some metadata we store in the code behind the migration.</span></span> <span data-ttu-id="fb28a-112">在 RTM，长度现在包含在已搭建基架代码`table.Column<string>(maxLength: 450, nullable: true)`。</span><span class="sxs-lookup"><span data-stu-id="fb28a-112">In RTM, the length is now included in the scaffolded code `table.Column<string>(maxLength: 450, nullable: true)`.</span></span>

<span data-ttu-id="fb28a-113">已在使用 RTM 之前的基架的任何现有迁移不会`maxLength`指定的参数。</span><span class="sxs-lookup"><span data-stu-id="fb28a-113">Any existing migrations that were scaffolded prior to using RTM will not have the `maxLength` argument specified.</span></span> <span data-ttu-id="fb28a-114">这意味着，将使用数据库支持的最大长度 (`nvarchar(max)` SQL Server 上)。</span><span class="sxs-lookup"><span data-stu-id="fb28a-114">This means the maximum length supported by the database will be used (`nvarchar(max)` on SQL Server).</span></span> <span data-ttu-id="fb28a-115">这可能是相当不错的某些列，但包含一个键外, 键列或索引需要更新，以包含最大长度。</span><span class="sxs-lookup"><span data-stu-id="fb28a-115">This may be fine for some columns, but columns that are part of a key, foreign key, or index need to be updated to include a maximum length.</span></span> <span data-ttu-id="fb28a-116">按照约定，450 是最大长度用于密钥外, 键和索引列。</span><span class="sxs-lookup"><span data-stu-id="fb28a-116">By convention, 450 is the maximum length used for keys, foreign keys, and indexed columns.</span></span> <span data-ttu-id="fb28a-117">如果在模型中，具有显式配置长度，然后应改为使用该长度。</span><span class="sxs-lookup"><span data-stu-id="fb28a-117">If you have explicitly configured a length in the model, then you should use that length instead.</span></span>

<span data-ttu-id="fb28a-118">**ASP.NET 标识**</span><span class="sxs-lookup"><span data-stu-id="fb28a-118">**ASP.NET Identity**</span></span>

<span data-ttu-id="fb28a-119">此更改会影响项目的早期版本中创建和使用 ASP.NET 标识的 RTM 项目模板。</span><span class="sxs-lookup"><span data-stu-id="fb28a-119">This change impacts projects that use ASP.NET Identity and were created from a pre-RTM project template.</span></span> <span data-ttu-id="fb28a-120">项目模板包括用于创建数据库的迁移。</span><span class="sxs-lookup"><span data-stu-id="fb28a-120">The project template includes a migration used to create the database.</span></span> <span data-ttu-id="fb28a-121">必须编辑此迁移，以指定最大长度为`256`为以下列。</span><span class="sxs-lookup"><span data-stu-id="fb28a-121">This migration must be edited to specify a maximum length of `256` for the following columns.</span></span>

*  <span data-ttu-id="fb28a-122">**AspNetRoles**</span><span class="sxs-lookup"><span data-stu-id="fb28a-122">**AspNetRoles**</span></span>

    * <span data-ttu-id="fb28a-123">name</span><span class="sxs-lookup"><span data-stu-id="fb28a-123">Name</span></span>

    * <span data-ttu-id="fb28a-124">NormalizedName</span><span class="sxs-lookup"><span data-stu-id="fb28a-124">NormalizedName</span></span>

*  <span data-ttu-id="fb28a-125">**AspNetUsers**</span><span class="sxs-lookup"><span data-stu-id="fb28a-125">**AspNetUsers**</span></span>

   * <span data-ttu-id="fb28a-126">电子邮件</span><span class="sxs-lookup"><span data-stu-id="fb28a-126">Email</span></span>

   * <span data-ttu-id="fb28a-127">NormalizedEmail</span><span class="sxs-lookup"><span data-stu-id="fb28a-127">NormalizedEmail</span></span>

   * <span data-ttu-id="fb28a-128">NormalizedUserName</span><span class="sxs-lookup"><span data-stu-id="fb28a-128">NormalizedUserName</span></span>

   * <span data-ttu-id="fb28a-129">UserName</span><span class="sxs-lookup"><span data-stu-id="fb28a-129">UserName</span></span>

<span data-ttu-id="fb28a-130">初始迁移应用到数据库时，无法进行此更改将导致出现以下异常。</span><span class="sxs-lookup"><span data-stu-id="fb28a-130">Failure to make this change will result in the following exception when the initial migration is applied to a database.</span></span>

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a><span data-ttu-id="fb28a-131">.NET Core： project.json 中删除"导入"</span><span class="sxs-lookup"><span data-stu-id="fb28a-131">.NET Core: Remove "imports" in project.json</span></span>

<span data-ttu-id="fb28a-132">如果你已面向.NET Core 与 RC2，需要添加`imports`到临时的解决方法不支持.NET 标准的 EF Core 依赖关系的某些 project.json。</span><span class="sxs-lookup"><span data-stu-id="fb28a-132">If you were targeting .NET Core with RC2, you needed to add `imports` to project.json as a temporary workaround for some of EF Core's dependencies not supporting .NET Standard.</span></span> <span data-ttu-id="fb28a-133">可以立即删除这些。</span><span class="sxs-lookup"><span data-stu-id="fb28a-133">These can now be removed.</span></span>

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
> <span data-ttu-id="fb28a-134">截至版本 1.0 RTM， [.NET Core SDK](https://www.microsoft.com/net/download/core)不再支持`project.json`或开发.NET Core 应用程序使用 Visual Studio 2015。</span><span class="sxs-lookup"><span data-stu-id="fb28a-134">As of version 1.0 RTM, the [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or developing .NET Core applications using Visual Studio 2015.</span></span> <span data-ttu-id="fb28a-135">我们建议你[从 project.json 迁移到 csproj](https://docs.microsoft.com/dotnet/articles/core/migration/)。</span><span class="sxs-lookup"><span data-stu-id="fb28a-135">We recommend you [migrate from project.json to csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span></span> <span data-ttu-id="fb28a-136">如果使用的 Visual Studio，我们建议升级到[Visual Studio 2017](https://www.visualstudio.com/downloads/)。</span><span class="sxs-lookup"><span data-stu-id="fb28a-136">If you are using Visual Studio, we recommend you upgrade to [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

## <a name="uwp-add-binding-redirects"></a><span data-ttu-id="fb28a-137">UWP： 添加绑定重定向</span><span class="sxs-lookup"><span data-stu-id="fb28a-137">UWP: Add binding redirects</span></span>

<span data-ttu-id="fb28a-138">尝试在以下的错误导致的通用 Windows 平台 (UWP) 项目上运行 EF 命令：</span><span class="sxs-lookup"><span data-stu-id="fb28a-138">Attempting to run EF commands on Universal Windows Platform (UWP) projects results in the following error:</span></span>

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

<span data-ttu-id="fb28a-139">您需要将绑定重定向手动添加到 UWP 项目。</span><span class="sxs-lookup"><span data-stu-id="fb28a-139">You need to manually add binding redirects to the UWP project.</span></span> <span data-ttu-id="fb28a-140">创建名为`App.config`在项目根文件夹并将重定向添加到正确的程序集版本。</span><span class="sxs-lookup"><span data-stu-id="fb28a-140">Create a file named `App.config` in the project root folder and add redirects to the correct assembly versions.</span></span>

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
