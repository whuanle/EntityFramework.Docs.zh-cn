---
title: "从 EF6 移植到 EF 核心技术-移植基于代码的模型"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: a0fa4f9a7028f56666fb993185cb03eddb9a2cd1
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2017
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a>移植到 EF 核心 EF6 基于代码的模型

如果你已阅读所有警告，并且就可以准备移植，以下是一些帮助你开始准则。

## <a name="install-ef-core-nuget-packages"></a>安装 EF 核心 NuGet 包

若要使用 EF 核心，请为你想要使用的数据库提供程序安装 NuGet 包。 例如，如果目标 SQL Server，你将安装`Microsoft.EntityFrameworkCore.SqlServer`。 请参阅[的数据库提供程序](../../core/providers/index.md)有关详细信息。

如果你打算使用迁移，则还应安装`Microsoft.EntityFrameworkCore.Tools`包。

最好将从 EF6 NuGet 包 (EntityFramework) 安装，因为 EF 核心和 ef6 更高版本可以是在同一应用程序并行使用。 但是，如果你不想要在你的应用程序的任何区域中使用 ef6 更高版本，然后卸载程序包将有助于为提供上的代码片段，需要注意的编译错误。

## <a name="swap-namespaces"></a>交换命名空间

你使用 ef6 更高版本中的大多数 Api 位于`System.Data.Entity`命名空间 （和相关子命名空间）。 第一个代码更改是切换到`Microsoft.EntityFrameworkCore`命名空间。 你将通常从你派生的上下文的代码文件开始，然后制定据此，在发生解决编译错误。

## <a name="context-configuration-connection-etc"></a>上下文配置 （连接等。）

中所述[确保 EF 核心将工作为应用程序](ensure-requirements.md)，EF 核心具有较少幻解决检测要连接到的数据库。 你将需要重写`OnConfiguring`派生的上下文和使用的数据库提供程序特定的 API 来设置连接到数据库上的方法。

大多数 ef6 更高版本应用程序将连接字符串存储在应用程序`App/Web.config`文件。 在 EF 核，读取此连接字符串使用`ConfigurationManager`API。 你可能需要添加对的引用`System.Configuration`framework 程序集要能够使用此 API。

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="update-your-code"></a>更新你的代码

此时，它是一种解决编译错误，以及评审代码，以查看是否行为更改将影响你。

## <a name="existing-migrations"></a>现有的迁移

实际上，没有端口现有 ef6 更高版本迁移到 EF 核心的可行方法。

如果可能，最好假定从 ef6 更高版本的所有以前迁移已应用到数据库，然后从，迁移架构的开始点使用 EF 核心。 若要执行此操作，你将使用`Add-Migration`命令添加迁移后模型移植到 EF 核心。 然后，你将删除的所有代码`Up`和`Down`方法的基架的迁移。 该初始迁移已基架时，后续迁移将对模型进行比较。

## <a name="test-the-port"></a>测试端口

只是因为你的应用程序将编译，并不意味着它成功移植到 EF 核心。 你将需要测试应用程序，以确保任何行为更改产生负面影响你的应用程序的所有区域。
