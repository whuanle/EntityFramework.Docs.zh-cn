---
title: .NET Framework 入门 - 新数据库 - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: bd7054c6834ae11bfdc352d63654e4304771e432
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/26/2018
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a>使用新数据库在 .NET Framework 上开始使用 EF Core

在本演练中，你将构建一个控制台应用程序，该程序使用 Entity Framework 对 Microsoft SQL Server 数据库执行基本数据访问。 你将使用迁移功能从模型创建数据库。

> [!TIP]  
> 可在 GitHub 上查看此文章的[示例](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb)。

## <a name="prerequisites"></a>系统必备

完成本演练需要以下先决条件：

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)

* [最新版本的 NuGet 包管理器](https://dist.nuget.org/index.html)

* [最新版本的 Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

## <a name="create-a-new-project"></a>创建新项目

* 打开 Visual Studio

* “文件”>“新建”>“项目...”

* 从左侧菜单中选择“模板”>“Visual C#”>“Windows 经典桌面”

* 选择“控制台应用(.NET Framework)”项目模板

* 确保面向 .NET Framework 4.5.1 或更高版本

* 为项目提供名称，然后单击“确定”

## <a name="install-entity-framework"></a>安装 Entity Framework

要使用 EF Core，请为要作为目标对象的数据库提供程序安装程序包。 本演练使用 SQL Server。 有关可用提供程序的列表，请参阅[数据库提供程序](../../providers/index.md)。

* “工具”>“NuGet 包管理器”>“包管理器控制台”

* 运行 `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

在本演练的后面部分，我们还将一些 Entity Framework Tools 用于维护数据库。 因此，我们也会安装此工具包。

* 运行 `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="create-your-model"></a>创建你自己的模型

现在是时候定义构成模型的上下文和实体类了。

* “项目”>“添加类...”

* 输入“Model.cs”作为名称，然后单击“确定”

* 将此文件的内容替换为以下代码

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }

        public List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
}
```

> [!TIP]  
> 在实际的应用程序中，你可以将每个类放在单独的文件中，并将连接字符串放在 `App.Config` 文件中，然后使用 `ConfigurationManager` 将其读出。 为简单起见，在本教程中，我们将所有内容都放在单个代码文件中。

## <a name="create-your-database"></a>创建数据库

现在你已经有了模型，可以使用迁移为自己创建数据库。

* “工具”–>“NuGet 包管理器”–>“包管理器控制台”

* 运行 `Add-Migration MyFirstMigration` 来为迁移搭建基架，从而为模型创建一组初始表。

* 运行 `Update-Database` 以将新迁移应用到数据库。 由于数据库尚不存在，因此将在应用迁移之前进行创建。

> [!TIP]  
> 如果将来对模型进行更改，则可以使用 `Add-Migration` 命令为新迁移搭建基架，以便对数据库进行相应的架构更改。 检查已搭建基架的代码（并进行了必要更改）后，就可以使用 `Update-Database` 命令将更改应用到数据库。
>
>EF 使用数据库中的 `__EFMigrationsHistory` 表来跟踪哪些迁移已经应用到数据库。

## <a name="use-your-model"></a>使用模型

你现在可以使用模型执行数据访问。

* 打开 Program.cs

* 将此文件的内容替换为以下代码

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blogs)
                {
                    Console.WriteLine(" - {0}", blog.Url);
                }
            }
        }
    }
}
```

* “调试”>“开始执行(不调试)”

你将看到，把一个博客保存到数据库后，所有博客的详细信息都将显示在控制台中。

![图像](_static/output-new-db.png)
